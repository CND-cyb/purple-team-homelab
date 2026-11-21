| Risk | Consequence | Action |
| --- | --- | --- |
| Locking myself out during the migration | No access to Proxmox | Tested fallback access before the migration |
| OPNsense failure | Lab inaccessible remotely | Backup cable + OPNsense configuration backup |
| Sharing secrets on GitHub | Passwords and keys become public | Set up a .gitignore file for secret files, use a secret scanner that blocks commits before they’re pushed (gitleaks); if a leak occurs, change the secret immediately |
| Vulnerable machine outside the network | Can be hacked and used as a relay to devices in the home | Isolated virtual network, with no access to the home or the Internet, with a single authorized data flow (SOC logs) and verification through testing |
| Failing mini-PC disk | Loss of all VMs and all configurations | Storage monitoring and alerts when disk usage reaches 80% capacity, plus backups stored off the mini-PC with restoration testing |
