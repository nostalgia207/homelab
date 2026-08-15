Status: not yet working - to revisit.

#### Goal

Windows-style edge/corner tiling plus a "snap assist" prompt (drag a window to an edge, get shown thumbnails of other open windows to fill the remaining space) - closest match to how this works on Windows/Ubuntu.

#### What works already (built-in KWin)

Corner quarter-tiling on a single monitor works out of the box by dragging a window to a screen corner.

#### What doesn't work

- Tiling across the seam where two monitors meet (not treated as a snap zone by default)
- No "snap assist" prompt to pick another window for the remaining half/quarter

#### Attempted - plasma-snap-assistant (AUR)

	yay -S plasma-snap-assistant

Installed but never appeared under System Settings > Window Management > KWin Scripts. Package links against kwin/qt6-base directly rather than shipping as a plain .kwinscript, so it may need a different enablement method not yet figured out. Removed:

	yay -R plasma-snap-assistant

#### Next attempt - kde-snap-assist (Plasma 6 fork)

Not yet done. Plan:

	https://github.com/luisbocanegra/kde-snap-assist/tree/plasma6

1. Download repo as ZIP, rename to .kwinscript
2. System Settings > Window Management > KWin Scripts > "Install from File"
3. Full logout/login (not just a plasmashell restart) to register it
4. Enable it in the KWin Scripts list

#### Alternative if snap-assist scripts don't pan out

PlasmaZones - heavier tool, modifier-key-drag zone tiling with explicit multi-monitor support. Worth trying if the seam-tiling issue specifically persists.
