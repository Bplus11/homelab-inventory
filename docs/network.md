# Network Topology

## Overview

The homelab network is segmented into VLANs to isolate traffic by function. All inter-VLAN routing goes through the primary router/firewall.

## VLAN Layout

| VLAN | Name        | Subnet              | Purpose                                      |
|------|-------------|---------------------|----------------------------------------------|
| 10   | Management  | 192.168.10.0/24     | Hypervisor management, infrastructure hosts  |
| 20   | Servers     | 192.168.20.0/24     | Production VMs and services                  |
| 30   | Security Lab| 192.168.30.0/24     | Isolated lab — AD, Security Onion, attack VMs|
| 40   | IoT         | 192.168.40.0/24     | Smart home devices, isolated from main LAN   |
| 99   | Guest       | 192.168.99.0/24     | Guest WiFi, internet-only                    |

## DNS

- Primary: 192.168.10.10 (Windows Server DC)
- Secondary: 1.1.1.1 (Cloudflare fallback)
- Internal domain: lab.local

## Design decisions

- Security Lab VLAN (30) has no route to Management or Servers — attack VMs stay contained
- IoT VLAN (40) can reach internet but not LAN — prevents lateral movement from compromised devices
- Guest VLAN (99) is completely isolated, internet-only
