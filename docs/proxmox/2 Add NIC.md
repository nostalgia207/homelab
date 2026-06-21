#### 1 - Open VM hardware settings

Select the VM -> Hardware -> Add -> Network Device

#### 2 - Configure the new NIC

- Bridge: vmbr1
- Model: VirtIO (paravirtualized)
- Firewall: unchecked


> [!NOTE] **VirtIO** is preferred over the default Intel E1000 for better performance - OPNsense and most modern Linux distros support it natively.

> [!NOTE] **Firewall** checkbox here is Proxmox's own NIC-level firewall, separate from any firewall running inside the VM (e.g. OPNsense, ufw). Left unchecked so it doesn't interfere with filtering done inside the guest.

#### 3 - Add

Click **Add**. The VM now has a second interface (e.g. `net1`) alongside the original (`net0`).

> [!TIP] Proxmox labels interfaces in the order they were added (`net0`, `net1`, ...). When the guest OS installer/console asks which interface is which, match by MAC address shown in Proxmox (VM -> Hardware) if the order isn't obvious.

---