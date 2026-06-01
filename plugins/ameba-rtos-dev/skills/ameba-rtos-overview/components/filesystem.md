---
name: ameba-rtos-filesystem
description: File system and KV storage quirks for ameba-rtos SDK. Use when implementing flash storage, VFS, LittleFS, FatFS, or KV operations.
---

# File System / KV — Quirks & Undocumented

> For VFS API overview and FatFS/LittleFS general usage, use docs MCP — see `docs-queries.md` File System section.

## fseek() Returns Current Offset, Not 0

`fseek()` in this SDK returns the **current file pointer offset** on success, not 0 like standard POSIX. Code that checks `if (fseek(...) == 0)` will incorrectly treat a non-zero seek as failure.

## VFS2 is Unallocated by Default

VFS1 (128 KB, LittleFS) exists by default. VFS2 has zero size — you must:
1. Allocate space in `component/soc/usrcfg/amebaxxx/ameba_flashcfg.c`
2. Register VFS2 manually in your app code

## Blank Flash Auto-Initializes VFS

If a VFS partition is all `0xFF` (blank flash), the system automatically formats it on first mount. No manual format step needed for fresh devices.

## LittleFS on by Default for VFS1

LittleFS is the default for VFS1 — no menuconfig change needed. To use FatFS (e.g., for SD card or VFS2), enable `CONFIG VFS → Enable VFS FATFS` in menuconfig.

## KV Storage Layout

KV keys become filenames stored in a `KV/` directory under VFS1. Each key is a separate file. KV and BT FTL both share VFS1 — size conflicts are possible with large KV stores.
