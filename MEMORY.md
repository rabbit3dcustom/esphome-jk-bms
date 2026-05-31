# Project Memory

## Purpose

This repository provides custom ESPHome components for monitoring JK-BMS PB 15.x devices over RS-485, with support for multi-node RS-485 networks and Home Assistant integration.

The implementation is centered on a shared RS-485 sniffer/hub and one or more per-BMS device instances bound to RS-485 addresses.

## Current Documentation Baseline

- `README.md` is a high-level entry point and links to the external manual/tutorial repository.
- `README.md` still highlights historical release notes up to `V1.6`, so it is not the best source for the latest implementation state.
- `CHANGELOG.md` is the most reliable in-repo source for recent behavior changes. The latest entry at the time of this memory is `2026-05-22`.

## Repository Shape

- `components/jk_rs485_sniffer/`
  - Shared UART/RS-485 component.
  - Owns protocol selection, RX timeout, optional talk pin, and publish throttling intervals.
- `components/jk_rs485_bms/`
  - Per-device component registered against one sniffer instance.
  - Owns decoding, entity publication, online-state tracking, and BMS-address-specific behavior.
  - Includes Python schema glue plus C++ runtime logic.
- `components/jk_rs485_bms/number/`
  - Number entities and write-back controls.
- `components/jk_rs485_bms/switch/`
  - Switch entities and write-back controls.
- Top-level YAML files
  - Local configuration, development, and protocol-analysis fixtures.
  - Several `faker_*.yaml` and `juanmi_*.yml` files appear to be capture or simulation assets used during protocol work.
- `logs/`
  - Local diagnostic output storage.

## Effective Architecture

### `jk_rs485_sniffer`

Main configuration responsibilities inferred from `components/jk_rs485_sniffer/__init__.py`:

- Depends on `uart`.
- Supports multiple instances (`MULTI_CONF = True`).
- Requires `protocol_version`.
- Accepts optional:
  - `master_fw_version`
  - `talk_pin`
  - `rx_timeout`
  - `sensor_publish_interval`
  - `number_publish_interval`
  - `text_publish_interval`
- Default publish intervals are `10s`.

This component acts as the transport-level coordinator and rate-limiting source for child BMS devices.

### `jk_rs485_bms`

Main configuration responsibilities inferred from `components/jk_rs485_bms/__init__.py`:

- Depends on a parent `jk_rs485_sniffer`.
- Supports multiple instances (`MULTI_CONF = True`).
- Requires `rs485_address` in the range `0..15`.
- Uses a polling interval of `5s`.

This component represents a logical BMS node attached to the shared sniffer and publishes sensors, binary sensors, text sensors, numbers, and switches.

## Important Recent Behavior

Based on `CHANGELOG.md`, the current implementation priorities are stability and API responsiveness rather than raw publish immediacy.

Key recent themes:

- Publish throttling was introduced and then moved to `jk_rs485_sniffer` configuration.
- Non-binary publications are intentionally delayed to reduce Home Assistant/API pressure.
- Binary sensors and alarm publications remain immediate.
- `network_nodes_available` required a follow-up fix so its latest value is preserved while text sensor throttling is active.
- Decoder hardening was added for short frames, invalid cell counts, optional entities, parent guards, and buffer/parser safety.
- `cell_count_settings` was made optional for online detection.
- Address `0x00` is now allowed by validation.

## Observed Runtime Concerns

The changelog and code search point to a few sensitive areas that deserve extra care in future work:

- RS-485 timing and UART loop blocking.
- Home Assistant API disconnects caused by aggressive publishing.
- Online-state lifecycle, which depends on frame arrival and valid cell metadata.
- Text/sensor/number throttling interactions with state freshness.
- Frame parsing robustness when protocol captures are incomplete or malformed.

## Maintenance Notes

- The project mixes ESPHome Python schema definitions with C++ runtime code. Changes usually need review in both layers.
- The repository contains active development fixtures and debug-oriented YAML assets; avoid treating them as dead files without confirming their workflow role.
- `setup.cfg` shows flake8 docstring rules intentionally disabled, but the repository-level working preference should still be to add useful documentation when touching important logic.
- There was no dedicated project memory file before this one.

## Suggested Source Priority For Future Sessions

When reconstructing project intent, prefer this order:

1. `CHANGELOG.md` for latest behavior changes.
2. `components/jk_rs485_sniffer/__init__.py` and `components/jk_rs485_bms/__init__.py` for effective config surface.
3. C++ runtime files under `components/` for actual behavior and edge-case handling.
4. `README.md` for project overview and external documentation entry points.

## Snapshot Date

This memory was created on `2026-05-31` from the current repository state.
