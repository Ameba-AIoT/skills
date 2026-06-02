<div align="center">

  <img src="./Realtek_Symbol_SVG.svg" alt="Ameba-AIoT Skills" />

  <h3>为 Realtek Ameba IoT SDK 开发打造的 AI 技能包 — 支持 Claude Code、Cursor、Windsurf、Codex 等</h3>

  <p>
    <a href="./LICENSE">
      <img src="https://img.shields.io/github/license/Ameba-AIoT/skills?style=flat-square" alt="License" />
    </a>
    <a href="https://github.com/Ameba-AIoT/skills/stargazers">
      <img src="https://img.shields.io/github/stars/Ameba-AIoT/skills?style=flat-square&color=0078af" alt="Stars" />
    </a>
    <img src="https://img.shields.io/badge/npx_skills-兼容-brightgreen?style=flat-square" alt="npx skills 兼容" />
    <img src="https://img.shields.io/badge/Claude_Code-插件支持-8A2BE2?style=flat-square" alt="Claude Code 插件" />
    <img src="https://img.shields.io/badge/瑞昱_Ameba-RTL8730E_%7C_RTL8721F_%7C_更多-0078af?style=flat-square" alt="瑞昱 Ameba 芯片" />
  </p>

  <a href="./README.md">English</a>
  |
  简体中文
  |
  <a href="https://github.com/Ameba-AIoT/skills/issues">反馈问题</a>

</div>

---

## 📦 技能列表

| 技能 | 描述 | 支持的芯片 |
|------|------|-----------|
| `ameba-rtos-overview` | SDK 全能助手：编译、烧录、监控、调试，以及 Wi-Fi / BT / USB / OTA / 文件系统 / 外设集成 | RTL8730E, RTL8721F, RTL8726E, RTL8720E, RTL8710E, RTL8713E, RTL8721Dx |
| `ameba-quickstart-rmesh` | Wi-Fi R-MESH 固件快速上手向导 — 芯片选型、功能配置（RNAT、OTA、TDMA、BLE 配网）、`wificfg.c` 补丁、Kconfig 配置与编译 | 所有支持 R-MESH 的 Ameba SoC |

---

## 🚀 安装方式

### 方法一 — npx（推荐，适用 Cursor / Windsurf / Codex 等所有 AI 工具）

```bash
# 安装全部技能
npx skills add https://github.com/Ameba-AIoT/skills

# 安装指定技能
npx skills add https://github.com/Ameba-AIoT/skills --skill ameba-rtos-overview
npx skills add https://github.com/Ameba-AIoT/skills --skill ameba-quickstart-rmesh
```

---

### 方法二 — Claude Code 插件

**仅技能（无 MCP）** — 从本 repo 安装：

```
/plugin marketplace add https://github.com/Ameba-AIoT/skills
/plugin install ameba-rtos-dev@ameba-aiot-skills
```

**完整体验（技能 + MCP 文档查询）** — 从插件 marketplace 安装：

```
/plugin marketplace add https://github.com/Ameba-AIoT/ameba-plugin-marketplace
/plugin install ameba-rtos-dev@ameba-aiot
```

完整安装额外注册 `realmcu-ask-ai-docs` MCP 服务器（瑞昱在线文档查询）。

---

## 💻 支持的芯片

RTL8730E · RTL8721F · RTL8726E · RTL8720E · RTL8710E · RTL8713E · RTL8721Dx

---

## 🐛 问题 & 贡献

[提交 Issue](https://github.com/Ameba-AIoT/skills/issues) 报告 Bug、提出功能请求或建议新技能。
