# Troubleshooting Outbound NAT on the FortiGate

## Starting point

I had a static default route on the FortiGate (0.0.0.0/0 via 192.168.1.1 out
WAN1), NAT enabled on my Internal → Outside policy, and I could ping the
FortiGate itself and my home router from inside the lab. My assumption was
that meant NAT was working.

That assumption was wrong. Both of those pings are on-link cases — the home
router sits on the same subnet as WAN1 (192.168.1.0/24), so it can reply
directly to whatever source IP shows up, NAT or no NAT. Neither test actually
proves anything about reaching the real internet.

## Finding the real problem

The actual test is pinging something past the home router — I tried
`ping 8.8.8.8` and got "destination unreachable." That's a different signal
than a timeout: something actively replied that it had no route, which pointed
at routing, not policy.

Checking Network > Static Routes on the FortiGate confirmed a default route
did exist (0.0.0.0/0 via 192.168.1.1, out WAN1), along with a return route back
to the internal VLANs. So routing wasn't the gap either — the "unreachable"
turned out to be DNS resolution failing (FortiGate's default FortiGuard DNS
servers, 96.45.45.45/96.45.46.46, aren't reachable on this unregistered unit).
Swapping DNS to 8.8.8.8 changed the symptom from "unreachable" to a plain
timeout on the ping itself, which meant the packet was now routing correctly
but still not getting a reply.

## Sniffing the actual traffic

Rather than keep guessing, I ran a packet capture on the FortiGate while
pinging 8.8.8.8 from a VLAN host:

```
diagnose sniffer packet any 'icmp' 4
```

The echo request showed up arriving on Internal2 — and then nothing. No
retransmit out WAN1, no NAT translation, nothing. That ruled out a routing
problem; this was the firewall dropping the packet before it ever got to NAT
or an egress interface, which only happens on a policy mismatch (implicit
deny).

## The actual bug

My Internal → Outside policy had its **destination** field set to the
"Outside" address object — 192.168.1.0/24. That's the home router's subnet,
not the internet. 8.8.8.8 doesn't fall inside that range, so the policy never
matched, and the packet hit the implicit deny silently. PING and DNS had
happened to work earlier because of previous testing against addresses that
did fall in-scope, which is exactly the kind of false positive that makes this
bug easy to miss.

The fix: change the policy's destination from the "Outside" object to `all`,
keep NAT on with the outgoing interface address (overload), and leave the
source scoped to the internal supernet. The firewall only needs to know
traffic is leaving *this* network, not which specific external host it's
going to.

## Return traffic and the second policy

I'd also built a separate outside-to-inside policy, reasoning that return
traffic needed its own NAT translation back to the originating host. That
reasoning was backwards. FortiGate is stateful — it tracks the outbound
session and un-NATs the reply automatically. No second policy is needed, and
no IP pool either. I deleted it; the implicit deny now correctly handles any
unsolicited inbound traffic from the WAN side.

## Result

One policy: Internal (10.10.0.0/16) → all, PING/DNS/HTTPS, NAT via outgoing
interface address. Outbound internet access confirmed from a host on the
management VLAN with Wi-Fi disabled, so the test traffic had no path out
except through the lab itself.

## Takeaways

- A successful ping doesn't prove NAT or routing works unless the destination
  is actually off-link. On-link replies don't need either.
- "Destination unreachable" and a plain timeout are different failure modes —
  worth telling apart before changing anything.
- When a policy silently drops traffic, check what it's actually matching
  against (source *and* destination objects, not just the interface pair)
  before assuming routing or NAT is the problem.
- Stateful firewalls don't need a return-path policy for sessions they
  initiated — building one is solving a problem that doesn't exist and opens
  unnecessary inbound access.
