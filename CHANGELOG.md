# Changelog

All notable changes to this project are documented here.

The newest entries appear first.

## 2026-05-22

### Fixed

- Preserved the latest `network_nodes_available` value while keeping text sensor throttling enabled.
- Deferred publication of pending `network_nodes_available` updates until the next allowed publish window.

## 2026-05-21

### Added

- Added `CHANGELOG.md` and linked it from `README.md`.

### Changed

- Moved publish interval settings to `jk_rs485_sniffer` configuration.
- Added YAML options for `sensor_publish_interval`, `number_publish_interval`, and `text_publish_interval`.
- Kept default publish intervals at `10s` when the new options are not provided.

### Fixed

- Removed invalid `ESP_LOG_LEVEL` checks from the sniffer hot path.
- Disabled expensive buffer dumps in the sniffer hot path to reduce loop blocking.
- Improved Home Assistant API responsiveness by reducing sniffer hot-path overhead.

## 2026-05-20

### Added

- Configurable publish intervals for sensors, numbers, and text sensors from `jk_rs485_sniffer`, with `10s` defaults.

### Changed

- Delayed non-binary state publications to reduce API load and Home Assistant reconnection issues.
- Reduced UART broadcast delays in the sniffer to improve timing and responsiveness.
- Kept binary sensors and alarm publications immediate while throttling sensors, numbers, and text sensors.

### Fixed

- Made `cell_count_settings` optional for online detection logic.
- Added crash guards for short frames and invalid cell counts during `decode_jk02_cell_info_()`.
- Improved stability around API disconnects and high publish rates.

## 2026-04-07

### Fixed

- Allowed RS485 address `0x00` in `jk_rs485_bms` configuration validation.
- Cleaned offset handling in JK02 frame decoding.
- Removed unsafe `arr[1]` split handling from cell info decoding.
- Removed dead `JkRS485Bms_init()` code and replaced invalid null checks with real parent guards.
- Added null guards for optional switches, numbers, and online tracking dependencies.
- Hardened the sniffer RX buffer parser against invalid pointer reuse after buffer erases.

## 2025-11-03

### Changed

- Merged formatting compatibility updates for ESPHome 2025.11.

## 2025-11-02

### Changed

- Updated text sensor definitions.
- Updated additional schema definitions.

## 2025-11-01

### Changed

- Partially rewrote `switch` schema definitions.
- Updated `number` schema format.

### Fixed

- Fixed minor typos.

## 2025-08-25

### Changed

- Updated release version metadata.
- Merged development updates for the `v19_20250817` line.

## 2025-08-20

### Changed

- Improved traces and debugging output.

## 2025-08-19

### Changed

- Extended `decode_device_info_()` handling.

## 2025-08-18

### Changed

- Reworked frame handling and debugging during protocol analysis.
- Added temporary print/debug helpers used while decoding all frame types.
