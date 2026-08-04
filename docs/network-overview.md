# Network Overview

## What is this lab?

A physical home lab built around a Cisco ASA 5505 firewall, a Catalyst 
3650-24PD switch, and a Cisco 891 router. Core devices are real hardware 
configured entirely from CLI. GNS3 is used alongside the physical gear 
to simulate additional network segments and virtual hosts where needed.

## Goal

I hold a CCNA and I am working toward CCNP Enterprise. This lab is 
where I build and validate the hands-on skills that go beyond 
certification — designing real network segmentation, configuring 
firewall policies, implementing routing protocols, and eventually 
automating repetitive tasks with Ansible.

The target is a Network Engineer role. This lab is how I get there.

## Roadmap

- Perimeter firewall design — ASA zones, ACLs, NAT/PAT
- Network segmentation — VLANs and trunking on the Catalyst 3650
- Dynamic routing — OSPF between the 891 router and switch
- Site-to-site VPN — IKEv1 on ASA 8.2
- Centralized logging — syslog from all devices into a free SIEM
- Network automation — Ansible playbooks for configuration management

