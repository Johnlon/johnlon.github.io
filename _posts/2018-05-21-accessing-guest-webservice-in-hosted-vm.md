---
layout: post
title: "Accessing a Guest Webservice in a Hosted VM"
date: 2018-05-21
tags: [devops, networking, virtualbox, centos]
blogger_url: https://johnlonergan.blogspot.com/2018/05/accessing-guest-webservice-in-hosted-vm.html
---

A web service running on a CentOS guest VM remained unreachable from the Windows 10 host, even after SSH and port forwarding were configured correctly.

## Diagnosis

Curl with verbose output showed connections initiating but no response arriving. The culprit: the guest firewall was blocking the port, not a routing issue.

## Quick Fix — Disable Firewall Temporarily (CentOS iptables)

```bash
iptables -P INPUT ACCEPT
iptables -P OUTPUT ACCEPT
iptables -P FORWARD ACCEPT
iptables -F
```

## Permanent Fix — Open the Port (CentOS firewalld)

```bash
firewall-cmd --zone=home --add-port=8000/tcp --permanent
```

## Working Network Configurations

Three approaches all work once the firewall is sorted:

**1. NAT with port forwarding (VirtualBox)**
- Guest listens on `0.0.0.0:8000`
- VirtualBox: forward host port `8888` → guest port `8000`
- Access from host via `localhost:8888`

**2. Bridged networking with DHCP (VirtualBox)**
- Guest gets its own IP on the local network
- Access directly via that IP on port `8000`

**3. Bridged networking (VMware Workstation Player)**
- Same as above — guest gets a DHCP address on the local network

The key thing in all cases: the firewall on the guest needs to allow the port, regardless of how the host-to-guest routing is set up.
