---
name: ameba-rtos-setup
description: Environment setup quirks for ameba-rtos SDK on Linux and Windows. Use when dealing with setup errors, toolchain issues, or environment activation problems.
---

# Setup — Quirks & Undocumented

> For full setup steps, use docs MCP: `"ameba-rtos environment setup Linux"` or `"ameba-rtos environment setup Windows"`.

## Do NOT Add env.sh to .bashrc

`source env.sh` activates a Python venv **only intended for SDK work**. Adding it to `.bashrc` activates it in every terminal, breaking other Python environments. Always run it manually per terminal.

## Common Errors

| Error | Fix |
|-------|-----|
| `command 'python' not found` | `sudo ln -s /usr/bin/python3 /usr/bin/python` |
| Python DLL errors on Windows | Delete `.venv` folder in SDK root, re-run `env.bat` |
| `ameba.py: command not found` | `source env.sh` not run, or not in SDK root |
| Build fails on first run (no toolchain) | Check internet; or use `-D USE_ALIYUN_URL=True` for Aliyun mirror |

## Windows: Two Ways to Activate

- **CMD / PowerShell**: `env.bat`
- **Git Bash / MSYS2**: `source env.sh`

Do not mix — use the one matching your shell.
