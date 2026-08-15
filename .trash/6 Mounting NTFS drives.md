#### 1 - Install ntfs-3g

	sudo pacman -S ntfs-3g

#### 2 - Identify the drive

	lsblk -f

#### 3 - Mount

	sudo mkdir -p /mnt/externalhdd
	sudo mount /dev/sdX1 /mnt/externalhdd

___

#### Issue - "wrong fs type, bad option, bad superblock"

Checked dmesg right after the failed mount:

	sudo dmesg | tail -10

Showed:

	ntfs3(sdX1): It is recommended to use chkdsk.
	ntfs3(sdX1): volume is dirty and "force" flag is not set!

Drive was left in a dirty state, likely from Windows Fast Startup/hibernation or not being ejected cleanly.

#### Fix - force mount

	sudo mount -t ntfs3 -o force /dev/sdX1 /mnt/externalhdd

> [!note]
> This bypasses the dirty check rather than fixing it. Proper fix is running `chkdsk D: /f` from Windows next time to clear the dirty flag at the source, so future mounts don't need `force`.
