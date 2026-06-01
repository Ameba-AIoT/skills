---
name: ameba-rtos-docs-queries
description: Query patterns for the realmcu-ask-ai-docs MCP when working on ameba-rtos SDK components. Use these templates to efficiently retrieve API details, struct fields, and configuration options.
---

# docs MCP: Query Patterns

**Tool**: `mcp__realmcu-ask-ai-docs__search_realtek_knowledge_sources`

Use natural-language queries. Be specific — include struct name, function name, or chip name when known.

**Tips:**
- Add chip name when results are weak: `"wifi_connect RTL8730E"`
- For struct fields: `"<struct_name> struct fields"`
- For function signatures: `"<function_name> function parameters"`
- For examples: `"<feature> example ameba"`

---

## Wi-Fi

| Need | Query |
|------|-------|
| Connect to AP | `"wifi_connect function parameters"` |
| STA connection struct | `"rtw_network_info struct fields"` |
| Scan networks | `"wifi_scan_networks parameters"` |
| Scan result | `"rtw_scan_result fields"` |
| SoftAP start | `"wifi_start_ap parameters"` |
| SoftAP config | `"rtw_softap_info struct fields"` |
| User config (reconnect, power) | `"wifi_user_conf configuration options"` |
| Events / callbacks | `"wifi event callback RTW_EVENT"` |
| Power save | `"wifi power save mode"` |

---

## Bluetooth / BLE

| Need | Query |
|------|-------|
| BT init | `"bt_init flow ameba"` |
| BLE advertising | `"ble gap advertising start"` |
| BLE connection events | `"ble gap connection event"` |
| GATT server | `"gatt server service add attribute"` |
| BLE scan | `"ble scan start parameters"` |
| BT Classic / SPP | `"bt classic spp profile"` |

---

## USB

| Need | Query |
|------|-------|
| USB device init | `"usb device init ameba"` |
| CDC (virtual serial) | `"usb cdc acm device"` |
| HID | `"usb hid device"` |
| Mass storage | `"usb mass storage device"` |
| USB host | `"usb host init"` |

---

## Peripheral

| Need | Query |
|------|-------|
| UART | `"uart init ameba parameters"` |
| I2C master | `"i2c master init ameba"` |
| SPI | `"spi init ameba parameters"` |
| ADC | `"adc read ameba"` |
| PWM | `"pwm output ameba"` |
| GPIO | `"gpio output input ameba"` |
| Timer | `"timer init callback ameba"` |
| RTC | `"rtc ameba"` |
| WDT | `"watchdog timer ameba"` |

---

## File System / KV

| Need | Query |
|------|-------|
| VFS init / mount | `"vfs init mount ameba"` |
| LittleFS | `"littlefs vfs ameba"` |
| FatFS | `"fatfs vfs mount ameba"` |
| KV storage | `"kv set get delete ameba"` |

---

## OTA

| Need | Query |
|------|-------|
| OTA flow overview | `"ota upgrade flow ameba"` |
| HTTP OTA | `"ota http download ameba"` |
| OTA API | `"ota_update function parameters"` |

---

## Memory / Flash Layout

| Need | Query |
|------|-------|
| Flash layout | `"flash layout RTL8730E"` ← replace chip |
| RAM layout | `"ram layout RTL8721F"` |
| PSRAM config | `"psram configuration ameba"` |

---

## Network / TCP-IP / TLS

| Need | Query |
|------|-------|
| Socket API | `"socket connect send recv ameba lwip"` |
| TLS/SSL | `"mbedtls ssl client ameba"` |
| MQTT | `"mqtt client ameba"` |
| HTTP client | `"http client request ameba"` |
| DNS | `"dns lookup ameba lwip"` |
