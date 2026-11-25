# Proxmox hardening log

Security measures applied to the hypervisor (pve01), with the date and the reference that justifies each one.

| Measure | Date | Reference |
| --- | --- | --- |
| "No-subscription" repository enabled, enterprise repositories disabled, system fully updated | 2026-11-23 | Proxmox VE Administration Guide, "Package Repositories" |
| Two-factor authentication (TOTP) enabled on `root@pam`, recovery keys stored offline | 2026-11-25 | Proxmox VE Administration Guide, "Two-Factor Authentication" |
| Personal administrator account created in the PVE realm, protected by TOTP | 2026-11-25 | Proxmox VE Administration Guide, "Authentication Realms", ANSSI, *Recommandations relatives à l'administration sécurisée des systèmes d'information* |
| `root@pam` kept only for operations that require it | 2026-11-25 | ANSSI, *Recommandations relatives à l'administration sécurisée des systèmes d'information* |
| Break-glass access on a dedicated bridge (`vmbr1`), tested | 2026-11-24 | [ADR 0001](../adr/0001-isolate-proxmox-management-behind-opnsense.md), [break-glass runbook](../runbooks/break-glass-access.md) |

## Notes

- The administrator role is granted on the whole datacenter (`/`). This is acceptable for a single-operator lab, with several people and different responsibilities, permissions would be split according to the principle of least privilege.
