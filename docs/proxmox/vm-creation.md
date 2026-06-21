# Creating a VM

## Downloading an ISO

![Proxmox ISO download from URL](../../Images/Proxmox_Iso_Download_from_URL.png)
1. From the target node, download an ISO directly via URL
2. Paste the URL, press **Query URL**, then **Download**

## Creating the VM

1. Select the target node and assign a unique VM ID
2. Select the storage containing the ISO, and choose the image
3. Enable the **QEMU Guest Agent**. This is a helper daemon installed in the guest that exchanges information between host and guest and allows command execution in the guest
   - Windows VMs will also add a TPM + EFI disk by default. Keep this enabled and use Local LVM storage
   - Linux VMs can ignore this
4. Enable **Discard** if the underlying storage is SSD-backed
5. Set CPU cores - stay under the node's total core count
6. Set memory - generally more is better for faster processes
7. Network: defaults are fine unless separating the management network from the VM network is needed
8. Proceed with a normal OS install
