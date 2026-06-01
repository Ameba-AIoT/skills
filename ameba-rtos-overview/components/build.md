---
name: ameba-rtos-build
description: Build system quirks and undocumented behavior for ameba-rtos SDK. Use when dealing with build failures, clean operations, toolchain issues, or SoC selection.
---

# Build — Quirks & Undocumented

> For general build commands and flow, use docs MCP: `"ameba.py build commands"` or see CLAUDE.md.

## Critical: `-c` vs `-p` vs `build`

| Command | Removes | Keeps `.config` (menuconfig)? |
|---------|---------|-------------------------------|
| `ameba.py build` | Nothing — incremental | Yes |
| `ameba.py build -c` | Build artifacts (obj, bin) | **Yes** |
| `ameba.py build -p` | Build artifacts + `.config` | **No — config is gone** |

Use `-c` to rebuild cleanly while keeping menuconfig settings. Use `-p` only for a full reset.

## `clean` vs `cleansoc`

| Command | Removes | Keeps |
|---------|---------|-------|
| `ameba.py clean` | `build_RTLxxx/build/` + `*.bin` | menuconfig, `soc_info.json` |
| `ameba.py cleansoc` | Entire `build_RTLxxx/` + `soc_info.json` | Nothing |

`cleansoc` also deletes the SoC selection — you must re-run `ameba.py soc` afterward.

## Toolchain Auto-Download

First build downloads toolchain automatically (needs internet). For China mainland, use Aliyun mirror:
```bash
ameba.py build -D USE_ALIYUN_URL=True
```
Different chips may require different toolchain versions — the build system checks and updates automatically.

## SoC Selection Persistence

`soc_info.json` stores the active chip. All `ameba.py` commands read it automatically. Deleting this file (via `cleansoc`) means you must re-select the SoC.

## env.sh Must Run Per Terminal

`source env.sh` activates a Python venv scoped to SDK work. It must be run in every new terminal. **Do not add to `.bashrc`** — it will pollute all terminals with the SDK venv.
