---
name: ameba-rtos-mcp-actions
description: ameba-dev MCP tool reference for flashing firmware, serial monitoring, device reset, and port listing in ameba-rtos projects.
---

# ameba-dev MCP: Action Reference

## ⚠️ Critical: Always Disconnect Serial

The server allows **max 10 connections**. Leaked connections block all future connections. Call `serial_disconnect_tool` when done — even on error or early exit.

---

## 1. List Serial Ports

Call the ameba-dev list-ports tool to get available ports. Present the list to the user and ask them to confirm which port to use before proceeding with flash or monitor.

---

## 2. Flash Firmware

**Tool**: `mcp__ameba-dev__flash_firmware_tool`

| Parameter | Required | Notes |
|-----------|----------|-------|
| `device` | Yes | SoC name e.g. `RTL8721F` — **must match `image_dir`** |
| `port` | Yes | e.g. `/dev/ttyUSB0`, `COM3` |
| `image_dir` | Yes | **Must be `build_<DEVICE>`** — e.g. `build_RTL8721F` |
| `baudrate` | No | Default: `1500000` |
| `memory_type` | No | Default: `nor` (options: `nor`, `nand`, `ram`) |
| `chip_erase` | No | Default: `false` |
| `remote_server` | No | IP of machine with serial port (WSL/SSH scenario) |
| `remote_port` | No | Default: `58916` |
| `remote_password` | No | If required by server |

**device → image_dir naming rule:**
```
RTL8721F  →  build_RTL8721F
RTL8730E  →  build_RTL8730E
RTL8726E  →  build_RTL8726E
```

**If flash fails: stop immediately and ask the user. Do not retry automatically.**

---

## 3. Monitor Serial Output

Always follow connect → read loop → disconnect. Never leave connections open.

```
1. serial_connect_tool(port, baudrate=115200)
   → returns connection_id

2. loop:
     serial_read_tool(connection_id, size=4096, timeout=1.0)
     → display output to user
   until: user stops, timeout, or error

3. serial_disconnect_tool(connection_id)   ← always run this
```

**serial_connect_tool parameters:**

| Parameter | Default | Notes |
|-----------|---------|-------|
| `port` | required | Serial port |
| `baudrate` | 115200 | 115200 for monitor; 1500000 is for flashing only |
| `timeout` | 1.0 | Read timeout in seconds |
| `remote_server` | — | Remote machine IP |
| `remote_port` | 58916 | |
| `remote_password` | — | |

**serial_write_tool** — send command to device:

| Parameter | Default | Notes |
|-----------|---------|-------|
| `connection_id` | required | |
| `data` | required | String to send |
| `add_crlf` | true | Appends `\r\n` automatically |

---

## 4. Reset Device

**Tool**: `mcp__ameba-dev__reset_device_tool`

Requires an active `connection_id` from `serial_connect_tool`. Resets via DTR/RTS signals.

Typical use: connect → reset → read boot log → disconnect.

---

## Full Auto Flow (Flash + Monitor)

```
list-ports → user confirms port
↓
flash_firmware_tool(device, port, image_dir)
↓ [success]
serial_connect_tool(port, baudrate=115200)
↓
serial_read loop → display to user
↓
serial_disconnect_tool
```
