---
name: ameba-rtos-overview
description: Use for ALL Realtek ameba-rtos SDK tasks — development, build, flash, monitor, debug, Wi-Fi/BT/USB/OTA/filesystem/peripheral integration, and code generation. Trigger on any mention of RTL8730E, RTL8721F, RTL8726E, RTL8720E, RTL8710E, RTL8713E, RTL8721Dx, ameba-rtos, ameba SDK, or any Realtek IoT/embedded development task. When the user wants to flash, monitor serial, reset a device, or write firmware — act immediately using MCP tools, don't just explain how.
---

## ⚠️ Language Matching Rule (Must Read)

**All user-facing output — guidance, AskUserQuestion fields (`question` / `header` / `option label` / `description`), confirmation prompts, error messages, and final summaries — must mirror the user's primary interaction language.** Not limited to Chinese / English.

- **Detection**: use the user's **primary interaction language** across the conversation (zh-CN / zh-TW / en / ja / ko / de / fr / es / …), with the **first user message** as the initial reference; follow the user's lead if they switch. Fall back to English when undetectable.
- **Do not** dump the English (or any other) template strings verbatim to a non-English user — translate at call time.
- All text within a single AskUserQuestion call must be in one consistent language; no mixed-language output.
- **Keep as-is, do not translate**: parameter names, Kconfig options / macros (`CONFIG_*`), shell commands, file paths, code identifiers, chip model numbers — these are technical identifiers, language-agnostic.

The English content below is a **content template**; render it in the user's language at runtime.

---

# ameba-rtos SDK Skill

## Supported Chips
RTL8730E · RTL8726E · RTL8721Dx · RTL8721F · RTL8720E · RTL8713E · RTL8710E

---

## Decision Logic

### "Flash / Monitor / Reset for me" → Act with MCP
1. Get serial port: call ameba-dev list-ports tool → present list → user confirms
2. Confirm SoC (check `soc_info.json` or ask) and build dir (`build_<CHIP>`)
3. Execute — see `mcp-actions.md` for tool details
4. After flash: **automatically** start monitor

### "Write code for X" → Generate with context
1. Query docs MCP for API details (`docs-queries.md` has patterns)
2. Read `components/<domain>.md` for quirks that affect implementation
3. Generate code
4. Offer: "Want me to flash after you build?"

### "How do I X" → Query docs first
1. `mcp__realmcu-ask-ai-docs__search_realtek_knowledge_sources` with specific query
2. Supplement with `components/<domain>.md` for undocumented behavior
3. Answer directly

### Build triggered (future build MCP)
Auto-sequence: **build → flash → monitor** — no prompts needed between steps

---

## Pre-Action Info (Collect Before Executing)

| Info | How to Get |
|------|-----------|
| Serial port | ameba-dev list-ports → present to user → confirm |
| SoC/chip | Read `soc_info.json`, or ask |
| image_dir | Always `build_<CHIP>` (e.g. `build_RTL8721F`) |
| Baudrate (flash) | Default `1500000` |
| Baudrate (monitor) | Default `1500000` |

---

## SDK Structure (Quick Reference)

```
component/
  wifi/  bluetooth/  usb/  file_system/
  lwip/  ssl/        soc/  os/(FreeRTOS)  utils/
example/    tools/    cmake/
build_<CHIP>/         ← build output
soc_info.json         ← active SoC selection
```

Config chain: `Kconfig → .config → platform_autoconf.h + .config_<MCU>`

---

## Sub-Skill Routing

| Task | File |
|------|------|
| ameba-dev MCP: flash, serial, reset | `mcp-actions.md` |
| docs MCP query patterns | `docs-queries.md` |
| Build / clean / env | `components/build.md` |
| Environment setup | `components/setup.md` |
| Wi-Fi | `components/wifi.md` |
| Bluetooth / BLE | `components/bluetooth.md` |
| USB | `components/usb.md` |
| File system / KV | `components/filesystem.md` |
| OTA | `components/ota.md` |
| Memory / flash layout | `components/memory-layout.md` |
| Menuconfig / Kconfig | `components/menuconfig.md` |
| Project structure / examples | `components/project.md` |
