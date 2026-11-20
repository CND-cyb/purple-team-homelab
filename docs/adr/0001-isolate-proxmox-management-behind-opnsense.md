# 0001 — Isolate Proxmox management behind OPNsense, with emergency access through a direct cable

- **Date:** 2026-11-20
- **Status:** Accepted
- **Author:** Benjamin LOTH

## Context

The homelab runs on a single mini-PC with two Ethernet ports and 16 GB of RAM. It sits behind the home internet router, and the house must not depend on the lab, if I break something in the lab, the internet has to keep working for everyone.

To isolate the lab, OPNsense runs in a VM on Proxmox and acts as the firewall between the lab and the house. This creates a problem: OPNsense runs on Proxmox, but if I put Proxmox management behind OPNsense, I can only reach it through OPNsense. What do I do if OPNsense does not start?

## Options

### A — Manage Proxmox from the home network, filtered by the Proxmox firewall

- **Pro:** simple to set up, no VPN needed to administer it.
- **Con:** the Proxmox interface has an address on the home network. Any device in the house (phone, TV, a guest's laptop) can reach its login page. If one of these devices is compromised, the attacker can go after the hypervisor, which controls the whole lab. The Proxmox firewall reduces this risk but does not remove it.

### B — Manage Proxmox only from a dedicated VLAN (VLAN 10) behind OPNsense, with emergency access through a cable on the second port

- **Pro:** Proxmox no longer has any address on the home network. It is invisible from the house, it can only be reached through the VPN or the emergency cable.
- **Con:** if OPNsense fails, I have to be in front of the mini-PC to fix it.

### C — Expose the Proxmox interface to the internet

- **Pro:** access from anywhere, without a VPN.
- **Con:** the hypervisor becomes a target for every automated scan on the internet. Rejected immediately.

## Decision

I choose option B, because it is the only one where Proxmox cannot be reached from the home network.

I accept that I will have to intervene physically if OPNsense fails, this is a lab, it does not need to be available 24/7, and the mini-PC is at home.

## Consequences

### Positive

- The lab is more secure, the hypervisor can no longer be reached from the house at all.

### Negative and risks

- If OPNsense fails, administration is less convenient, I have to be next to the machine with a cable.
  - **Mitigation:** keep a cable and a laptop ready, and back up the OPNsense configuration so it can be restored quickly.
- The biggest risk is locking myself out during the migration, if OPNsense is misconfigured when I move Proxmox management to VLAN 10, I lose all access.
  - **Mitigation:** the emergency access is configured and tested before the migration, not after.

### Next steps, in this order

1. Configure the second port with a static address, on a separate bridge.
2. Test the emergency access with a laptop connected by cable.
3. Install OPNsense and create VLAN 10.
4. Move Proxmox management to VLAN 10.
5. Check access to Proxmox through the VPN.
6. Remove the Proxmox address from the home network.
