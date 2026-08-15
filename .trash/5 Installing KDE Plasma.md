#### 1 - Install Xorg

	sudo pacman -S xorg

#### 2 - Install Plasma and SDDM

	sudo pacman -S plasma sddm

#### 3 - Install KDE applications (optional but recommended)

	sudo pacman -S kde-applications

#### 4 - Enable SDDM

	sudo systemctl enable sddm

#### 5 - Reboot

	sudo reboot

___

> [!warning]
> At the SDDM login screen, check the Session dropdown - it can default to "Plasma Bigscreen" (TV/couch mode) instead of the normal desktop. Switch it to "Plasma" before logging in.

> [!note]
> Plasma shell restarts (`kquitapp6 plasmashell` + `plasmashell &`) can reset display config that persists fine across a normal logout/login - primary monitor and refresh rate both reset to wrong values after a shell restart during this session. Prefer a full logout/login over a manual shell restart when troubleshooting display issues.
