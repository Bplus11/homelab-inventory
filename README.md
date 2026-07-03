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

## Future plans

- Pull live VM status from Proxmox API at build time (Netlify build hook)
- Add service inventory (what's running in Docker, etc.)
- Hardware health / uptime tracking
