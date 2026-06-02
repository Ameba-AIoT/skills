<div align="center">

  <img src="./Realtek_Symbol_SVG.svg" alt="Ameba-AIoT Skills" />

  <h3>AI skills for Realtek Ameba IoT SDK — Claude Code, Cursor, Windsurf, Codex and more</h3>

  <p>
    <a href="./LICENSE">
      <img src="https://img.shields.io/github/license/Ameba-AIoT/skills?style=flat-square" alt="License" />
    </a>
    <a href="https://github.com/Ameba-AIoT/skills/stargazers">
      <img src="https://img.shields.io/github/stars/Ameba-AIoT/skills?style=flat-square&color=0078af" alt="Stars" />
    </a>
    <img src="https://img.shields.io/badge/npx_skills-compatible-brightgreen?style=flat-square" alt="npx skills compatible" />
    <img src="https://img.shields.io/badge/Claude_Code-plugin_ready-8A2BE2?style=flat-square" alt="Claude Code plugin" />
    <img src="https://img.shields.io/badge/Realtek_Ameba-RTL8730E_%7C_RTL8721F_%7C_more-0078af?style=flat-square" alt="Realtek Ameba chips" />
  </p>

  <a href="./README.md">English</a>
  |
  <a href="./README_CN.md">简体中文</a>
  |
  <a href="https://github.com/Ameba-AIoT/skills/issues">Issues</a>

</div>

---

## 📦 Available Skills

| Skill | Description | Supported ICs |
|-------|-------------|---------------|
| `ameba-rtos-overview` | Full SDK assistant: build, flash, monitor, debug, Wi-Fi / BT / USB / OTA / filesystem / peripheral integration | RTL8730E, RTL8721F, RTL8726E, RTL8720E, RTL8710E, RTL8713E, RTL8721Dx |
| `ameba-quickstart-rmesh` | Interactive quickstart for Wi-Fi R-MESH firmware — IC selection, feature config (RNAT, OTA, TDMA, BLE provisioning), `wificfg.c` patching, Kconfig, and build | All Ameba SoCs with R-MESH support |

---

## 🚀 Installation

### Method 1 — npx (Cursor, Windsurf, Codex, and others)

```bash
# Install all skills
npx skills add https://github.com/Ameba-AIoT/skills

# Install a specific skill
npx skills add https://github.com/Ameba-AIoT/skills --skill ameba-rtos-overview
npx skills add https://github.com/Ameba-AIoT/skills --skill ameba-quickstart-rmesh
```

---

### Method 2 — Claude Code Plugin

**Skills only (no MCP)** — install directly from this repo:

```
/plugin marketplace add https://github.com/Ameba-AIoT/skills
/plugin install ameba-rtos-dev@ameba-aiot-skills
```

**Full experience (skills + MCP)** — install from the plugin marketplace:

```
/plugin marketplace add https://github.com/Ameba-AIoT/ameba-plugin-marketplace
/plugin install ameba-rtos-dev@ameba-aiot
```

The full install adds the `realmcu-ask-ai-docs` MCP server (Realtek documentation query) alongside the skills.

---

## 💻 Supported Chips

RTL8730E · RTL8721F · RTL8726E · RTL8720E · RTL8710E · RTL8713E · RTL8721Dx

---

## 🐛 Issues & Contributing

[Open an issue](https://github.com/Ameba-AIoT/skills/issues) for bug reports, feature requests, or new skill proposals.
