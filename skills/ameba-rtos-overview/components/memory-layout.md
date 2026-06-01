---
name: ameba-rtos-memory-layout
description: Memory and flash layout quirks for ameba-rtos SDK. Use when modifying flash partitions, adjusting RAM regions, configuring PSRAM, or hitting memory-related build/runtime issues.
---

# Memory Layout — Quirks & Undocumented

> For chip-specific flash/RAM layout tables, use docs MCP: `"flash layout RTL8730E"` (replace chip name).

## Changing Flash Layout Requires Reflashing Bootloader

After editing OTA partition addresses in `ameba_flashcfg.c`, the bootloader must also be reflashed — it reads OTA1/OTA2 addresses at compile time. Flashing only the app image with mismatched bootloader will cause boot failure.

## All Partition Addresses Must Be 4KB-Aligned

Addresses in `ameba_flashcfg.c` (IMG_APP_OTA1, IMG_APP_OTA2, etc.) must be multiples of `0x1000`. Unaligned addresses cause silent flash corruption.

## Verify PSRAM Integration Before Enabling PSRAM Options

Menuconfig PSRAM options (`IMG2 on PSRAM`, `PSRAM AS HEAP`) assume PSRAM is physically present and wired. Enabling these on a board without PSRAM causes boot failure. Confirm with hardware schematics first.

## ImageTool Must Match ameba_flashcfg.c

When changing OTA1 partition addresses, also update the **Start Addr / End Addr** of `app.bin` in ImageTool to match the new `IMG_APP_OTA1` values. Mismatch = image flashed to wrong address.

## Flash Layout File Location

`{SDK}/component/soc/usrcfg/amebaxxx/ameba_flashcfg.c`

## RAM Layout File Location

`{SDK}/component/soc/amebaxxx/project/ameba_layout.ld`
