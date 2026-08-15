# GRUB

#### 1 - Install GRUB

	pacman -S grub efibootmgr

#### 2 - Install to the EFI partition

	grub-install --target=x86_64-efi --efi-directory=/boot --bootloader-id=GRUB

#### 3 - Generate config

	grub-mkconfig -o /boot/grub/grub.cfg

___

#### os-prober did not detect Windows automatically

Tried the standard route first:
- installing os-prober
- enabling GRUB_DISABLE_OS_PROBER=false in /etc/default/grub.

> [!note]
> os-prober was not finding the Windows install on its own.

#### Fix - manual custom GRUB entry

Edited /etc/grub.d/40_custom directly and added:

	menuentry "Windows 11" {
		insmod ntfs
		set root=(sda,1,2,3,4)
		chainloader +1
	}

Then regenerated the config:

	grub-mkconfig -o /boot/grub/grub.cfg

#### Result

Rebooted, selected "Windows 11" from the GRUB menu, booted into Windows successfully.

	sudo reboot
