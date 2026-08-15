# Nvidia Drivers

#### Symptom

Monitor refresh rate capped at 240Hz instead of the panel's rated 400Hz. 

NVIDIA card was running on `nouveau` (open-source fallback driver), not the proprietary driver.

#### 1 - Install the NVIDIA driver

	sudo pacman -S nvidia-open-dkms nvidia-utils nvidia-settings

> [!note]
> `nvidia-open-dkms` over `nvidia-open` - DKMS rebuilds the kernel module automatically on kernel updates, avoiding the driver breaking after a kernel upgrade.

#### 2 - Regenerate initramfs and reboot

	sudo mkinitcpio -P
	sudo reboot

___

#### Issue after first reboot

Only one monitor detected, apps failed to open. Checked the journal:

	journalctl -b -p err --no-pager | grep -i nvidia

`Failed to find module 'nvidia_uvm'` - kernel module didn't build correctly.

#### Fix

	pacman -Qs linux-headers

linux-headers was missing as it required DKMS to build the module.

	sudo pacman -S linux-headers
	sudo dkms autoinstall
	sudo mkinitcpio -P
	sudo reboot

#### Verify

	lspci -k | grep -A 3 -E "(VGA|3D)"

NVIDIA line should show `nvidia` as the driver in use, not `nouveau`.

	xrandr

400Hz should now be listed as an available refresh rate.


