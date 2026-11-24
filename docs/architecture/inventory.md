# Inventory

Last updated: 2026-11-24

## Hosts

| Host | Hardware | OS / version | Role |
| --- | --- | --- | --- |
| pve01 | Mini-PC, Ryzen 7 5800U, 16 GB RAM, 500 GB disk | Proxmox VE 9.2.32, kernel 7.0.2-6-pve | Hypervisor |

## Network interfaces (pve01)

| Interface | Type | Used by | Address | Notes |
| --- | --- | --- | --- | --- |
| nic0 | Ethernet | vmbr0 | 192.168.1.250/24 | Temporary management, home network |
| nic1 | Ethernet | vmbr1 | 172.16.99.1/30 | Break-glass access |
| wlp3s0 | Wi-Fi | — | — | Unused |
