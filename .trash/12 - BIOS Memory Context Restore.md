Firmware-level setting (MSI Click BIOS 5) that skips RAM re-training on every boot, using cached timings from a previous successful boot instead. Noticeably speeds up boot time.

#### Where to find it

BIOS > Settings > Advanced > usually under Overclocking or DRAM-related settings, labeled "Memory Context Restore".

#### Notes

- Runs entirely during POST, before GRUB or Windows Boot Manager load - does not touch the dual-boot setup, GRUB config, or either OS in any way
- Safe with XMP enabled (no manual OC beyond XMP) - well within normal use case for this feature
- If RAM is ever reseated/changed, most boards auto-detect and retrain once; if not, disable the setting once to force a full retrain, then re-enable
- If instability shows up after enabling, same fix - disable once, let it fully retrain, re-enable
