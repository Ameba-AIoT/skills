# Ameba IoT SDK Skills for AI Coding Assistants

AI-powered skills for Realtek Ameba IoT SDK development — works with Claude Code, Cursor, Windsurf, Codex, and other AI coding assistants.

---

## Available Skills

| Skill | Description | Supported ICs |
|-------|-------------|---------------|
| `ameba-rtos-overview` | Full SDK assistant: build, flash, monitor, debug, Wi-Fi / BT / USB / OTA / filesystem / peripheral integration | RTL8730E, RTL8721F, RTL8726E, RTL8720E, RTL8710E, RTL8713E, RTL8721Dx |
| `ameba-quickstart-rmesh` | Interactive quickstart for Wi-Fi R-MESH firmware — IC selection, feature config (RNAT, OTA, TDMA, BLE provisioning), `wificfg.c` patching, Kconfig, and build | All Ameba SoCs with R-MESH support |

---

## Installation

### Method 1 — Claude Code Plugin Marketplace (Recommended)

Gives you skills **and** pre-configured MCP servers in one step.

```bash
# Step 1: Add this repo as a marketplace source (one-time)
/plugin marketplace add https://github.com/Ameba-AIoT/skills

# Step 2: Install the plugin
/plugin install ameba-rtos-dev@skills
```

After installation you get:

- **2 skills** auto-loaded into Claude Code: `ameba-rtos-overview`, `ameba-quickstart-rmesh`
- **1 MCP server** auto-registered: `realmcu-ask-ai-docs` (Realtek documentation query, remote HTTP)

#### Prerequisites

| Requirement | Details |
|-------------|---------|
| `realmcu-ask-ai-docs` MCP | One-time GitHub OAuth. After install, run `/mcp` — if status shows `△ needs authentication`, follow the prompt to complete OAuth. |
| `AMEBA_SDK_ROOT` (optional) | Only needed if you use the local SDK build/flash features. Export before starting Claude Code: `export AMEBA_SDK_ROOT=<absolute path to your SDK>`. Other skills and MCP are unaffected if this is not set. |

---

### Method 2 — Manual SKILL.md Copy

For **Cursor, Windsurf, Codex, and other AI tools** that support custom system prompts or rules files.

Each skill is a single self-contained Markdown file:

| Skill | File path in this repo |
|-------|------------------------|
| `ameba-rtos-overview` | `plugins/ameba-rtos-dev/skills/ameba-rtos-overview/SKILL.md` |
| `ameba-quickstart-rmesh` | `plugins/ameba-rtos-dev/skills/ameba-quickstart-rmesh/SKILL.md` |

**How to use:**

1. Browse to the skill file above and copy its contents (or download the raw file).
2. Paste it into your AI tool's custom instructions / rules / system prompt location:

   | Tool | Where to paste |
   |------|---------------|
   | **Cursor** | `.cursor/rules/<skill-name>.mdc` in your project, or Settings → Rules for AI |
   | **Windsurf** | `.windsurfrules` in your project root |
   | **Codex** | `AGENTS.md` in your project root |
   | **Other tools** | Any persistent system prompt or project-level rules file |

3. The skill's `description` field tells the AI assistant when to automatically activate it. You can also invoke it manually by describing what you want to do.

> **Note:** Without the MCP servers, features that require live SDK operations (build, flash, serial monitor) will not work. The skills degrade gracefully — documentation queries and guided configuration steps still work.

---

## Supported Chips

RTL8730E · RTL8721F · RTL8726E · RTL8720E · RTL8710E · RTL8713E · RTL8721Dx

---

## Issues & Contributing

- **Bug reports / feature requests**: [Open an issue](https://github.com/Ameba-AIoT/skills/issues)
- **Pull requests**: Welcome for skill improvements, typo fixes, and new skill additions
