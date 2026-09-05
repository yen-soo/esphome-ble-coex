# esphome-ble-coex

Pinned ESPHome `esp32_ble_tracker` override that restores the pre-#16036
Wi-Fi/BT coexistence policy (revert to `ESP_COEX_PREFER_BALANCE` as soon as
transient client states clear, even while connections are ESTABLISHED).

Rationale: ESPHome PR #16036 (merged 2026-04-27) holds `ESP_COEX_PREFER_BT`
for the lifetime of any active BLE connection to fix GATT response timeouts
(status=133). On long-lived BLE sessions driven through an active
`bluetooth_proxy` (e.g. a BLE charger polled continuously from Home
Assistant), the sustained PREFER_BT starves Wi-Fi until the AP disassociates
the STA (reason='Unspecified'), which in turn kills the ESPHome API and the
proxied BLE session. See the review discussion on #16036 where this exact
regression was predicted for long-lived connections.

Source base: esphome/esphome commit 2026.8.2, component
`esphome/components/esp32_ble_tracker`, single patch on top.

License: same as upstream — C++ under GPLv3, Python under MIT (see LICENSE).
