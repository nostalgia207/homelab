# AD DS, DNS & DHCP

## Domain controller

Windows Server, running:

- **AD DS
- **DNS
- **DHCP**

The domain controller sits on the management bridge (`vmbr0`), while DHCP client VMs sit on a separate bridge (`vmbr1`). Keeping domain/management traffic logically separated from general client traffic.

Other machines on the client bridge pick up addressing via DHCP and resolve names through the domain controller's DNS.

## Notes

- This lab environment is used to practice identity/networking fundamentals, user/group management, GPOs, DNS records, and DHCP scope configuration. Skills that map directly to enterprise AD/Entra ID administration.
