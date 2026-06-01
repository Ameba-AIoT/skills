---
name: ameba-rtos-wifi
description: Wi-Fi quirks and undocumented behavior for ameba-rtos SDK. Use when implementing Wi-Fi features, troubleshooting connections, or configuring reconnect behavior.
---

# Wi-Fi — Quirks & Undocumented

> For API reference (wifi_connect, rtw_network_info, wifi_scan_networks, etc.), use docs MCP — see `docs-queries.md` Wi-Fi section.

## Known Channel → Faster Connection

If you know the AP's channel (e.g., from a prior scan), set `rtw_network_info::channel` before calling `wifi_connect()`. This skips the full channel scan and significantly reduces connection latency.

## Auto-Reconnect Defaults (Often Surprising)

Auto-reconnect is **enabled by default** with these values:

| Parameter | Default |
|-----------|---------|
| `auto_reconnect_en` | 1 (enabled) |
| `auto_reconnect_count` | 10 retries |
| `auto_reconnect_interval` | 5 seconds |

Set `auto_reconnect_count = 0xFF` for unlimited retries.

To disable: `wifi_user_config.auto_reconnect_en = 0;`

When disabled, use `example/wifi/wifi_user_reconnect/` as reference for custom reconnect logic.

## Fast Reconnect on Power-Up

After each successful connection, the device saves Wi-Fi credentials to flash (SSID, password, security type, channel). On next boot it reconnects automatically — no user code needed.

Default: **enabled**. To disable: `wifi_user_config.fast_reconnect_en = 0;`

When disabled, the app must call `wifi_connect()` explicitly after every reboot.

## Wi-Fi Init is Automatic

`wifi_init()` is called automatically inside `main()`. No explicit init code needed. Device starts in **STA mode** by default. To use SoftAP, call `wifi_start_ap()` after init completes.
