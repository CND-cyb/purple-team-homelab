# 0002 — Bootstrap access to OPNsense through the Proxmox host on VLAN 10

- **Date:** 2026-11-26
- **Status:** Accepted
- **Author:** Benjamin LOTH

## Context

OPNsense is installed and its administration interface is on VLAN 10, carried by `vmbr2`. This bridge has no physical port, so none of my machines can reach it.

The target solution is remote access through WireGuard, but WireGuard is configured from the OPNsense web interface itself. I need a temporary first access to get started.

## Options

### A — A VM acting as an administration workstation in VLAN 10

- **Pro:** no exposure to the home network.
- **Con:** a lightweight Linux VM with a browser needs about 1 to 2 GB of RAM, out of 16 GB in total, while OPNsense already uses 4 GB. It also has to be used through the Proxmox console.

### B — Give Proxmox an address (10.10.10.2/24) in VLAN 10, in addition to its current address

- **Pro:** no exposure to the home network, and it already covers part of step 4 of [ADR 0001](0001-isolate-proxmox-management-behind-opnsense.md).
- **Con:** during the transition, Proxmox has a foot in each network. An infected device in the house that compromised it would reach VLAN 10 directly.

### C — Open the OPNsense web interface on the WAN

- **Pro:** reachable from any device in the house.
- **Con:** an infected device could attack the firewall's administration interface. Rejected immediately.

## Decision

I choose option B, because it moves forward step 4 of ADR 0001 (Proxmox management in VLAN 10) without exposing any administration interface to the house.

It also costs no extra RAM, unlike option A, the mini-PC's resources are kept for the services I will deploy later.

## Consequences

### Positive

- No administration interface is exposed to the house.
- Step 4 of ADR 0001 is already half done.

### Negative and risks

- Proxmox bridges the house and VLAN 10.
  - **Mitigation:** this is temporary and ends with step 6 of ADR 0001. Check that Proxmox does not route traffic between its networks (`net.ipv4.ip_forward` must be `0`).
- SSH access to Proxmox from the house becomes an entry point.
  - **Mitigation:** key-based authentication, and password login disabled for `root`.

### Next steps, in this order

1. Add the address `10.10.10.2/24` to Proxmox on VLAN 10 of `vmbr2`, without a gateway.
2. Check that routing is disabled on Proxmox.
3. Open an SSH tunnel from the laptop to the OPNsense interface.
4. Log in, checking the certificate fingerprint.
5. Change the OPNsense root password with the right keyboard layout, and rename the machine `opn01`.
