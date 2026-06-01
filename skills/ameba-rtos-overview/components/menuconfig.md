---
name: ameba-rtos-menuconfig
description: Kconfig and menuconfig quirks for ameba-rtos SDK. Use when configuring features, dealing with prj.conf behavior, or understanding config priority.
---

# Menuconfig / Kconfig — Quirks & Undocumented

> For menuconfig command reference, use docs MCP: `"ameba menuconfig commands"`.

## `build -p` Deletes Your Config

`ameba.py build -p` (pristine) removes `.config` along with build artifacts. All menuconfig settings are lost. Use `ameba.py build -c` to clean artifacts while keeping config.

## prj.conf Only Auto-Applied on First Build

`ameba.py build -a <example>` auto-applies `prj.conf` **only when `.config` does not already exist** in the build directory. If `.config` exists from a prior build or menuconfig session, it is used as-is and `prj.conf` is silently ignored.

To force-apply prj.conf on an existing config: `ameba.py menuconfig -f <path>/prj.conf`

## Config Priority (High → Low)

```
user-specified conf files   ← highest
prj.conf (example dir)
default.conf (SoC default)
Kconfig default values      ← lowest
```

`default.conf` lives at `{SDK}/component/soc/amebaxxx/project/default.conf`. You only need to write incremental changes in your own conf.

## Save Current Config to File

```bash
ameba.py menuconfig -s myconfig.conf
```

Useful for saving a known-good config to reapply later.

## Config Outputs (What Gets Generated)

| File | Used By |
|------|---------|
| `.config` | menuconfig source of truth |
| `.config_<MCU>` | CMake build system |
| `platform_autoconf.h` | C code (`#define CONFIG_FOO`) |
