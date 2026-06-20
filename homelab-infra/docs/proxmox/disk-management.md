# Disk Management

## Resizing a VM disk

1. Select the VM → **Hardware** tab
2. Select the disk to resize → **Disk Action → Resize**
3. Specify how much additional space to add

Inside the guest, verify with `lsblk`, then expand the partition and filesystem to use the new space:

```bash
sudo growpart /dev/sda 3            # expand the partition
sudo pvresize /dev/sda3             # expand the physical volume
sudo lvextend -l +100%FREE /dev/ubuntu-vg/ubuntu-lv   # expand the logical volume
sudo resize2fs /dev/ubuntu-vg/ubuntu-lv               # expand the filesystem
```

---

## Adding a new physical disk to a node

Run from the Proxmox shell:

```bash
lsblk                                # identify the new disk (e.g. sda, sdb — nvme0n1 is usually the Proxmox disk)
umount /dev/DISKNAME 2>/dev/null     # unmount if needed
mkfs.ext4 /dev/DISKNAME              # format
mkdir -p /path/to/mountpoint         # create the mount point
mount /dev/DISKNAME /path/to/mountpoint
df -h | grep mountpoint              # verify
```

To make the mount persistent (only do this if the disk is **not** also being mounted inside a VM — avoid two machines fighting over the same disk):

```bash
blkid /dev/DISKNAME                  # get the UUID
nano /etc/fstab                      # add a line:
# UUID=YOUR-UUID-HERE  /path/to/mountpoint  ext4  defaults  0  2
```

Test before rebooting:

```bash
umount /path/to/mountpoint
mount -a
df -h | grep mountpoint
```

---

## Attaching a physical disk to a VM

> Only do this after the disk has already been mounted on the Proxmox host first.

**Proxmox shell:**

```bash
ls -l /dev/disk/by-id/ | grep sdX            # get the persistent disk ID
qm set <VMID> -scsi1 /dev/disk/by-id/<disk-id>   # attach to the VM
```

**Inside the VM:**

```bash
lsblk                                # confirm the new disk is visible
sudo mkdir -p /mnt/media             # create the mount point
sudo mount /dev/sdX1 /mnt/media      # mount it
df -h | grep media                   # verify
```

> The mount path inside the VM doesn't need to match the path used on the Proxmox host — they can differ.

Make it persistent the same way as above (`blkid` → `/etc/fstab` entry), then test with `umount` / `mount -a` before rebooting.
