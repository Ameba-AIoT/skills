---
name: ameba-rtos-ota
description: OTA firmware update quirks for ameba-rtos SDK. Use when implementing OTA, configuring boot partition strategy, or setting image version numbers.
---

# OTA — Quirks & Undocumented

> For OTA flow overview and transport examples, use docs MCP — see `docs-queries.md` OTA section.

## APP OTA is Enabled by Default

No menuconfig change needed to enable dual-partition OTA. It works out of the box.

## Equal Version Numbers → Boot from OTA1

When both OTA1 and OTA2 have the same version number, the bootloader always boots **OTA1**. Plan version increments to avoid this if you want OTA2 to be the active partition.

## "Valid Header Based" Strategy: Chip Restrictions

`Valid Header Based` OTA switch strategy is **NOT supported** on:
- RTL8726E
- RTL8713E
- RTL8720E
- RTL8710E

Use `Version Number Based` (default) for these chips.

## Valid Header Based Corrupts Old Image

When using `Valid Header Based`, the old image header is intentionally corrupted after OTA. If the new image fails, you cannot fall back — **re-flashing is required**. Use only for long-term testing without version control.

## OTP Write is Permanent

For BOOT OTA (bootloader update), the OTA2 address is written to OTP. **OTP cannot be changed or erased after writing.** Verify the address is correct before writing. Incorrect OTP = device cannot boot from intended address.

Formula for OTP value:
```
Value_OTP = (IMG_BOOT_OTA2 >> 12 & 0xFF) << 8 | (IMG_BOOT_OTA2 >> 20 & 0xFF)
```

## Version Number Location

Image version is set in: `component/soc/amebaxxx/project/manifest.json5`
```json5
"image2": { "img_ver_major": 1, "img_ver_minor": 2 }
```
Increment minor (or major) before each OTA build so new version > running version.
