---
name: ameba-quickstart-rmesh
description: |
  Interactive quickstart for configuring and building Wi-Fi R-MESH firmware on Ameba IoT SDK.
  Guides the user through IC selection, R-MESH feature configuration (basic mesh, RNAT, socket,
  OTA, TDMA, BLE provisioning), wificfg.c patching, Kconfig configuration, and build.
  Use this skill whenever the user mentions R-MESH, wifi_rpp, Wi-Fi mesh networking,
  "配置R-Mesh", "编译R-Mesh", "R-Mesh quickstart", "rmesh setup", "mesh build", "wifi tunnel",
  "wtn_en", or wants to get started with R-MESH networking on any Ameba SoC. Always invoke
  this skill rather than manually walking through the steps.
---

## ⚠️ 语言匹配规则（必读）

**所有引导文字、AskUserQuestion 的问题/选项/描述、提示与汇总，都要镜像用户的对话语言——不限于中英文。**

- **判定**：以用户之前消息的主要自然语言为准（zh-CN / zh-TW / en / ja / ko / de / fr / es / …）。
- **覆盖范围**：所有面向用户的输出——引导说明、AskUserQuestion 的 `question` / `header` / `option label` / `description`、确认提示、错误信息、最终汇总——一律用用户语言呈现。
- **不要把模板原串直接抛给非中文用户**。下方中文内容仅为**内容模板**；agent 必须在调用时实时翻译为用户语言后再输出。
- **单次 AskUserQuestion 调用内的所有文字必须语言一致**，不得多语言混排。
- **保持原样不翻译**：参数名、Kconfig 选项 / 宏（`CONFIG_*`）、命令、文件路径、代码标识符、芯片型号——这些是技术标识符，与自然语言无关。

---

# Ameba Wi-Fi R-MESH 快速入门 / Quickstart

本 skill 逐步引导用户完成 R-MESH 固件的配置与编译：
选择芯片 → 选择功能 → 修改 wificfg.c → 生成 prj.conf → 编译。

## R-MESH 简介

Wi-Fi R-MESH（内部称为 "wifi_tunnel" / wtn）是一种工作在 Wi-Fi 驱动层的树形拓扑 Wi-Fi Mesh，对上层应用完全透明。支持的主要芯片：

| 芯片       | usrcfg 目录     | 根节点容量 |
|------------|----------------|------------|
| RTL8721Dx  | amebadplus     | 4          |
| RTL8721F   | amebagreen2    | 32         |

> **根节点容量**：单个根节点下最多可连接的节点数量。

> 注意：SDK 中存在其他芯片（RTL8720E、RTL8730E 等），但 R-MESH 仅在
> RTL8721Dx 和 RTL8721F 上官方支持。若用户选择其他芯片，给出警告，
> 但如用户坚持则继续执行。

## 第 1 步 — 确认 SDK 根目录

当前工作目录应为 SDK 根目录（包含 `ameba.py` 和 `env.sh` 的目录）。执行以下命令确认：
```bash
ls env.sh ameba.py 2>/dev/null && echo "SDK root OK" || echo "请先 cd 进入 sdk/ 目录"
```
若不在 SDK 根目录，请用户先切换目录后再继续。

> **重要**：始终使用 `python3 ameba.py` 调用构建工具（而非直接使用 `ameba.py`）。
> 在非交互式 Bash 子进程中 source `env.sh` 不会跨工具调用持久化 PATH，
> 因此 `source env.sh && ameba.py` 的方式不可靠。

## 第 2 步 — 通过 AskUserQuestion 选择芯片

> 🌐 **语言**：question / header / 选项标签 / 描述全部镜像用户语言（参见文首规则，不限于中英）。

请用户选择目标芯片，清晰呈现以下选项：

- **RTL8721Dx** — Wi-Fi 4 双频，根节点容量：4，超低功耗（推荐）
- **RTL8721F** — Wi-Fi 6 双频，根节点容量：32，适合大型组网
- **其他** — 用户可指定；警告 R-MESH 仅在 RTL8721Dx 和 RTL8721F 上官方支持

将所选芯片映射到对应的 `SOC_FOLDER`：
- RTL8721Dx → `amebadplus`
- RTL8721F → `amebagreen2`

wificfg 文件路径为：`component/soc/usrcfg/<SOC_FOLDER>/ameba_wificfg.c`

## 第 3 步 — 通过 AskUserQuestion 选择 R-MESH 功能

> 🌐 **语言**：下面 3 个问题都镜像用户语言呈现（参见文首规则）。
>
> **AskUserQuestion 限制**：每个问题最多 4 个选项，使用**三个独立问题**。

**问题 1**（多选，最多 3 个选项）：
- Socket — Gravitation 拓扑可视化工具；若同时需要 OTA 则必须勾选
- OTA — 通过 Mesh 进行固件更新（依赖 Socket）
- TDMA — 减少密集部署（>8 个相邻节点）中的 Beacon 冲突

**问题 2** — RNAT 角色（单选，3 个选项）。
展示选项前，简要说明：RNAT 允许 Mesh 网络突破上游 AP 的 STA 数量限制，
通过让一个节点充当其余节点的 NAT 网关来实现扩展。
- RNAT 网关 — 本节点充当 NAT 网关；设置 `wtn_rnat_en=1`、`wtn_fixed_rnat_node=1`、`CONFIG_IP_NAT_MENU=y`
- 仅连接到 RNAT — 本节点仅连接到 RNAT 组节点；设置 `wtn_connect_only_to_rnat=1`
- 不使用 RNAT — 所有 RNAT 字段保持默认值 0

**问题 3**（单选 是/否）：
- 是否启用 BLE WiFiMate 配网？（允许 MGravitation 手机 App 通过 BLE 为节点配网）
- 是 → 在配置文件中添加 Feature F 的 Kconfig 条目；否 → 不添加 BT 相关条目

### 功能参数说明

#### Feature A：基础 R-MESH（始终必须启用）
- `CONFIG_RMESH_EN=y`
- 设置 `wifi_user_config.wtn_en = 1`

#### Feature B：RNAT（可选，三种角色互斥）
- **RNAT 网关**：`wtn_rnat_en=1`、`wtn_fixed_rnat_node=1`、`CONFIG_IP_NAT_MENU=y`
- **仅连接到 RNAT**：`wtn_connect_only_to_rnat=1`
- **不使用 RNAT**：三个字段均保持 0（默认）

#### Feature C：R-MESH Socket（可选，Gravitation 可视化工具依赖此功能）
- `CONFIG_RMESH_SOCKET_EN=y`

#### Feature D：R-MESH OTA（可选，依赖 Socket）
- `CONFIG_RMESH_OTA_EN=y`
- 需同时启用 Feature C（Socket）

#### Feature E：TDMA（可选）
- `wifi_user_config.wtn_tdma_en = 1`

#### Feature F：BLE WiFiMate 配网（可选，用于 wifi_rpp 示例）
- `CONFIG_BT_MENU=y`
- `CONFIG_BT_EXAMPLE_DEMO_MENU=y`
- `CONFIG_BT_WIFIMATE_DEVICE=y`
- `CONFIG_BT_WIFIMATE_CONFIGURATOR=y`
- 设置 `wifi_user_config.fast_reconnect_en = 0`（BLE 配网必须关闭快速重连）

## 第 4 步 — 高级参数（可选）

> 🌐 **语言**：参数名（代码标识符）保持原样不翻译，说明文字和选项标签镜像用户语言。

先询问用户是否需要调整高级 wificfg 参数。如不需要，直接跳至下一步。

如需要，使用单次 AskUserQuestion 调用，将下表所有四个参数作为独立问题一次性询问
（每个参数必须询问，不得省略）：

| 参数                         | 默认值（amebadplus） | 默认值（amebagreen2） | 说明 |
|------------------------------|---------------------|----------------------|------|
| `wtn_max_node_num`           | 15                  | 32                   | Mesh 最大节点数（影响 Beacon 窗口大小） |
| `wtn_strong_rssi_thresh`     | -50                 | -50                  | AP RSSI 高于此值时直接连接 AP（dBm） |
| `wtn_father_refresh_timeout` | 3000                | 3000                 | 超过 N ms 未收到 Beacon 则切换父节点（ms） |
| `wtn_child_refresh_timeout`  | 4000                | 4000                 | 超过 N ms 未收到 Beacon 则断开子节点（ms） |

每个参数提供"保持默认值"选项，并通过"其他"接受自定义值。
仅将与默认值不同的参数写入 wificfg.c。

## 第 5 步 — 修改 wificfg.c

读取 `component/soc/usrcfg/<SOC_FOLDER>/ameba_wificfg.c` 中的当前内容。
使用 Edit 工具进行精确修改，仅改动必要部分。典型修改如下：

```c
wifi_user_config.wtn_en = 1;              // 启用 R-MESH（始终）
// 若启用 BLE 配网（Feature F）：
wifi_user_config.fast_reconnect_en = 0;
// 若为 RNAT 网关（Feature B）：
wifi_user_config.wtn_rnat_en = 1;
wifi_user_config.wtn_fixed_rnat_node = 1;
// 若仅连接到 RNAT（Feature B）：
wifi_user_config.wtn_connect_only_to_rnat = 1;
// 若启用 TDMA（Feature E）：
wifi_user_config.wtn_tdma_en = 1;
// 高级参数（若用户修改了默认值）：
wifi_user_config.wtn_max_node_num = <value>;
```

注意：使用 Edit 工具进行精确修改，**不要**覆盖无关配置项。

## 第 6 步 — 通过 menuconfig -f 进行配置

**不要**修改 `example/wifi/wifi_rpp/prj.conf`。将所有 Kconfig 选项汇总到
一个临时 conf 文件中，一次性应用。

### 6a — 选择目标芯片

```bash
python3 ameba.py soc RTL8721Dx   # 针对 RTL8721Dx
python3 ameba.py soc RTL8721F    # 针对 RTL8721F
```

> **芯片名称 vs SOC_FOLDER**：`SOC_FOLDER`（amebadplus / amebagreen2）仅用于
> wificfg.c 的文件路径。`ameba.py soc` 命令接受芯片名称（RTL8721Dx / RTL8721F）。

### 6b — 生成并应用配置

在 SDK 根目录（当前目录）生成 `rmesh_quick_start_config.conf`，内容根据用户选择决定：

| 条件 | 配置条目 |
|------|---------|
| 始终（Feature A） | `CONFIG_RMESH_EN=y` |
| 选择 RNAT（Feature B） | `CONFIG_IP_NAT_MENU=y` |
| 选择 Socket（Feature C） | `CONFIG_RMESH_SOCKET_EN=y` |
| 选择 OTA（Feature D） | `CONFIG_RMESH_OTA_EN=y`、`CONFIG_RMESH_SOCKET_EN=y` |
| BLE 配网 是（Feature F） | `CONFIG_BT_MENU=y`、`CONFIG_BT_EXAMPLE_DEMO_MENU=y`、`CONFIG_BT_WIFIMATE_DEVICE=y`、`CONFIG_BT_WIFIMATE_CONFIGURATOR=y` |

（Feature E / TDMA 无对应 Kconfig 条目，完全通过 wificfg.c 处理。）

使用 Write 工具写入文件（只写入上表中适用的条目）。
若 `rmesh_quick_start_config.conf` 已存在，写入前须先用 Read 工具读取——
Write 工具要求对已有文件进行过先读取操作：

然后应用：
```bash
python3 ameba.py menuconfig -f rmesh_quick_start_config.conf
```

此命令以 `default.conf` 为基础，一步应用您的选项覆盖当前 `.config`。
若 menuconfig 方式不可用，向用户说明如何在 menuconfig 图形界面中手动启用
对应选项（参见下方参考说明）。

## 第 7 步 — 编译

- **已启用 BLE 配网（Feature F）：**
  ```bash
  python3 ameba.py build -a wifi_rpp
  ```
- **其他所有情况：**
  ```bash
  python3 ameba.py build
  ```

由于第 6 步已生成 `.config`，编译时将直接使用该配置，
不会再从示例目录重新应用任何 prj.conf。

编译完成后，输出镜像（`app.bin`、`boot.bin`、`ota_all.bin`）位于 `build_<SOC>/`
目录下（例如 `build_RTL8721F/`）。编译成功后向用户显示此路径。

## 第 8 步 — 编译后汇总

> 🌐 **语言**：整段汇总镜像用户语言输出（参见文首规则）。

编译成功后，输出以下汇总信息：
- 目标芯片型号
- 已启用的功能列表
- 镜像输出路径
- 下一步操作：使用 `python3 ameba.py flash` 烧录，或使用 Ameba Image Tool
- 配网说明：若启用 BLE 配网，使用 `tools/R-Mesh_Demo_Tool/MGravitation.apk`
  中的 MGravitation App 为节点配网，其余节点随后通过 ZRPP 自动入网。

## Menuconfig 手动导航参考（prj.conf 失败时使用）

```
CONFIG WIFI → Enable Wifi →
   [*] Enable R-mesh
   [*]     Enable R-NAT            （若启用 RNAT）
   [*]     Enable R-mesh Socket    （若启用 Socket 或 Gravitation）
   [*]         Enable R-mesh OTA   （若启用 OTA，需先启用 Socket）

CONFIG LWIP →
   [*] Enable NAT REPEATER          （若启用 RNAT）

CONFIG BT → BT Example Demo →
   [*] BLE WiFiMate Device          （若启用 BLE 配网）
   [*] BLE WiFiMate Configurator    （若启用 BLE 配网）
```

## 错误处理

- 若 `python3 ameba.py soc <IC>` 失败：执行 `python3 ameba.py soc` 查看可用芯片名称（使用芯片名如 RTL8721Dx，而非目录名如 amebadplus）
- 若编译因缺少 BT 库失败：BLE 功能需要项目中存在 BT 库文件
- 若修改 wificfg.c 时发现结构不同：先读取文件内容，再调整编辑方式
- 若未找到 RNAT + lwIP NAT 选项：该选项名称可能不同，在 Kconfig 中搜索 `NAT`
