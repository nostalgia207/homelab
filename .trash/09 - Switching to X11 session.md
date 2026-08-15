Plasma 6.4+ splits KWin into kwin-wayland and kwin-x11. Only Wayland ships by default now - X11 session needs manual install.

#### 1 - Install X11 session support

	sudo pacman -S plasma-x11-session

#### 2 - Reboot

	sudo reboot

#### 3 - Select session

At the SDDM login screen, Session dropdown -> "Plasma (X11)" instead of the default Wayland option.

> [!note]
> Reason for switching: Flameshot's multi-monitor selection (drag-select spanning both screens in one go) does not work correctly on Wayland - it only allows selecting one monitor at a time. Works correctly on X11.
