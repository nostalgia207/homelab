Used to replace Rufus for creating bootable USB drives from Linux. Installs once, then any ISO can be copied onto the drive directly - no re-flashing needed per ISO.

#### 1 - Install Ventoy

	wget https://github.com/ventoy/Ventoy/releases/download/v1.0.99/ventoy-1.0.99-linux.tar.gz
	tar -xzf ventoy-1.0.99-linux.tar.gz
	cd ventoy-1.0.99

#### 2 - Identify the USB drive

	lsblk

> [!warning]
> Target the whole disk (e.g. /dev/sdX), not a partition (e.g. /dev/sdX1). This wipes the drive.

#### 3 - Write Ventoy to the drive

	sudo ./Ventoy2Disk.sh -i /dev/sdX

> [!note]
> If the drive is currently mounted, unmount it first: `sudo umount /dev/sdX1`

___

#### Adding ISOs

After Ventoy is installed, the drive shows up as a normal storage device. Copy ISOs onto it directly:

	cp ~/Downloads/Windows11.iso /media/gabriel/Ventoy/
	cp ~/Downloads/archlinux-*.iso /media/gabriel/Ventoy/

Boot from the USB and Ventoy shows a menu of every ISO currently on the drive.
