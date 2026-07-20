# homelab-inventory

Structured inventory of my homelab: physical hardware, virtual machines, and network topology. Currently a static JSON snapshot; intended to evolve into a live asset tracker as the lab grows.

The data in this repo powers the [Lab Inventory](https://bplus11.com/inventory) page on my site.

## Structure

```
homelab-inventory/
├── hardware.json     # Physical hardware — servers, switches, NAS, etc.
├── vms.json          # Virtual machines — host, OS, role, resources, IP
├── network.json      # VLANs, subnets, DNS, gateway
└── docs/
    ├── network.md    # Network topology narrative and design decisions
    └── rack.md       # Rack layout diagram
```

## Data schema

### hardware.json
Each entry: `id`, `name`, `type`, `role`, `make`, `model`, `cpu`, `ram_gb`, `storage[]`, `nics`, `location`, `status`

### vms.json
Each entry: `id`, `name`, `host` (maps to hardware id), `vmid`, `os`, `role`, `vcpus`, `ram_gb`, `disk_gb`, `ip`, `vlan`, `status`, `tags[]`

### network.json
Top-level: `vlans[]`, `dns`, `gateway`, `isp_uplink`
Each VLAN: `id`, `name`, `subnet`, `description`

## Recent Implementations (not reflected in json files yet)
Integrating Firewalla SE managed switch into Home Network
Did this to allow for growth of my network and for ease of management of individual devices.
Target Physical Topology

```
Internet → Router
              ↓
        Firewalla SE Switch
        ├── Port 1: Router uplink
        ├── Port 2: AP
        ├── Port 3: Primary PC
        ├── Port 4: Mini PC (Proxmox Node 2) — management + VM traffic
        ├── Port 5: Proxmox Node 1 — management + VM traffic (NIC 1)
        └── Port 6: Proxmox Node 1 — mirror/sniff target (NIC 2)
              ↑
        Mirror policy: copy all traffic from ports 1-5 → port 6
```
Currently due to the Beta status of the Switch and software, port mirroring is NYI, so the physical topology is as follows to allow for proper monitoring by Security Onion:
```
Internet → Router → [Proxmox Node 1 - inline tap] → Firewalla SE Switch 
                                                        ├── Port 1: Proxmox Node 1 - Router uplink
                                                        ├── Port 2: AP
                                                        ├── Port 3: Mini PC (Node 2)
                                                        ├── Port 4: Primary PC

```

Added a Mini PC with plans to make it Node 2 for Proxmox cluster and primary home for the AD lab

## Future plans

- Pull live VM status from Proxmox API at build time (Netlify build hook)
- Add service inventory (what's running in Docker, etc.)
- Hardware health / uptime tracking
