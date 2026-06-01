---
name: ameba-rtos-project
description: Project structure and example system quirks for ameba-rtos SDK. Use when creating examples, building specific apps, or setting up external projects.
---

# Project Structure / Examples — Quirks & Undocumented

> For build commands and external project creation, see `components/build.md` and docs MCP.

## Example Name Must Match Folder Name Exactly

`ameba.py build -a <name>` uses `GLOB_RECURSE` to find `example_<name>.c`. The `<name>` must exactly match the folder name containing `CMakeLists.txt`. A mismatch silently builds without the example or fails to find it.

```bash
ameba.py build -a wifi_chan_scan_uart   # folder: example/wifi/wifi_chan_scan_uart/
ameba.py build -a raw_i2c_dma_mode     # folder: example/peripheral/raw/raw_i2c_dma_mode/
```

## app_example() is a Weak Symbol

`main.c` declares `_WEAK void app_example(void) {}`. Each example provides a **strong** `app_example()` in `app_example.c` that overrides it at link time. Only one strong definition can be active — building multiple examples into one binary will cause a linker error.

## prj.conf Ignored When .config Already Exists

See `components/menuconfig.md` — same rule applies: if `.config` exists, `prj.conf` is not applied automatically.

## Always Read Example's README.md First

Every example has a `README.md` listing required HW wiring, necessary Kconfig items, and supported chips. Skipping it often leads to mysterious build or runtime failures.

## Component Categories (Quick Reference)

| component/ dir | Content |
|----------------|---------|
| `wifi/` | Wi-Fi driver + API |
| `bluetooth/` | BT/BLE stack |
| `usb/` | USB stack |
| `file_system/` | VFS, FatFS, LittleFS, KV |
| `lwip/` | TCP/IP stack |
| `ssl/` | mbedTLS |
| `soc/` | SoC drivers, linker scripts, usrcfg |
| `os/` | FreeRTOS wrapper |
| `utils/` | JSON, XML, log, etc. |
| `at_cmd/` | AT command interpreter |
| `audio/` | Audio framework |
