#### 1 - Check network

	ping 8.8.8.8

#### 2 - Update mirrorlist

	reflector --country Portugal --age 12 --protocol https --sort rate --save /etc/pacman.d/mirrorlist

#### 3 - Install base system

	pacstrap -K /mnt base linux linux-firmware

#### 4 - Generate fstab

	genfstab -U /mnt >> /mnt/etc/fstab
	cat /mnt/etc/fstab

> [!warning]
> Run from the live ISO shell, before chrooting - not from inside the chroot.

#### 5 - Chroot into the new install

	arch-chroot /mnt
