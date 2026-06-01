# Ameba IoT SDK Skills

AI skill files for Realtek Ameba IoT SDK development. Use these with **Cursor, Windsurf, Codex, or any AI coding assistant** that supports custom rules or system prompts.

> **Claude Code users:** Use the [Plugin Marketplace](https://github.com/Ameba-AIoT/ameba-plugin-marketplace) instead — it installs skills and MCP servers automatically in one step.

---

## Available Skills

| Skill | Description | Supported ICs |
|-------|-------------|---------------|
| [`ameba-rtos-overview`](./ameba-rtos-overview/SKILL.md) | Full SDK assistant: build, flash, monitor, debug, Wi-Fi / BT / USB / OTA / filesystem / peripheral integration | RTL8730E, RTL8721F, RTL8726E, RTL8720E, RTL8710E, RTL8713E, RTL8721Dx |
| [`ameba-quickstart-rmesh`](./ameba-quickstart-rmesh/SKILL.md) | Interactive quickstart for Wi-Fi R-MESH firmware — IC selection, feature config (RNAT, OTA, TDMA, BLE provisioning), `wificfg.c` patching, Kconfig, and build | All Ameba SoCs with R-MESH support |

---

## Installation

### Cursor

Copy the skill file into your project's rules directory:

```bash
mkdir -p .cursor/rules
curl -o .cursor/rules/ameba-rtos-overview.mdc \
  https://raw.githubusercontent.com/Ameba-AIoT/skills/main/ameba-rtos-overview/SKILL.md
```

Or copy manually: open the skill link above → copy raw content → paste into **Settings → Rules for AI** (global) or `.cursor/rules/<name>.mdc` (project-level).

### Windsurf

```bash
curl -o .windsurfrules \
  https://raw.githubusercontent.com/Ameba-AIoT/skills/main/ameba-rtos-overview/SKILL.md
```

Or append to an existing `.windsurfrules` file in your project root.

### Codex / OpenAI

Paste the skill content into `AGENTS.md` in your project root.

### Other AI Tools

Copy the `SKILL.md` content into whichever persistent system prompt or project-level rules file your tool supports.

---

## Notes

- Skills are **self-contained Markdown files** — the `description` field tells the AI when to activate automatically.
- Features requiring live SDK operations (build, flash, serial monitor) need the [realmcu-ask-ai-docs MCP server](https://github.com/Ameba-AIoT/ameba-plugin-marketplace). Without MCP, skills degrade gracefully — documentation queries and guided configuration steps still work.
- Skills respond in the **user's language** automatically (English, Chinese, Japanese, Korean, etc.).

---

## Issues & Contributing

[Open an issue](https://github.com/Ameba-AIoT/skills/issues) for bug reports, feature requests, or new skill proposals.
