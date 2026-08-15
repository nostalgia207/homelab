All steps below run inside the chroot.

#### 1 - Timezone

	ln -sf /usr/share/zoneinfo/Europe/Lisbon /etc/localtime
	hwclock --systohc

#### 2 - Locale

Uncomment en_US.UTF-8 UTF-8 in /etc/locale.gen, then:

	locale-gen
	echo "LANG=en_US.UTF-8" > /etc/locale.conf

#### 3 - Hostname

	echo "XXXXXXX" > /etc/hostname

#### 4 - Hosts file

/etc/hosts:

	127.0.0.1   localhost
	::1         localhost
	127.0.1.1   XXXXXXX.localdomain   XXXXXXX

#### 5 - Root password

	passwd

___

#### 6 - Create regular user

	useradd -m -G wheel -s /bin/bash XXXXXXX
	passwd XXXXXXX

#### 7 - Enable sudo for wheel group

	pacman -S sudo
	EDITOR=vi visudo

Uncomment:

	%wheel ALL=(ALL:ALL) ALL

___

#### 8 - Networking

	pacman -S networkmanager
	systemctl enable NetworkManager

___

#### 9 - Keyboard layout (Portuguese)

Console (current session):

	loadkeys pt-latin1

Console (permanent) - /etc/vconsole.conf:

	KEYMAP=pt-latin1

X11 / graphical session (set after first boot, as regular user):

	sudo localectl set-x11-keymap pt

> [!note]
> Console keymap and graphical (X11) keymap are separate settings - both need to be set independently.
