# nRF52840 BLE Firmware

Bluetooth Low Energy (BLE) peripheral firmware for the **Nordic nRF52840 DK**, written in C as part of the *Microcontrollers: Interface & Programming* course at ADA University.

The projects cover the BLE peripheral life cycle: advertising, connecting, exchanging data over GATT, tuning advertising parameters, and building a minimal advertise-only broadcaster.

## Hardware and tools

| Item | Details |
|------|---------|
| Board | Nordic nRF52840 DK (ARM Cortex-M4F, 64 MHz, integrated 2.4 GHz radio) |
| Language | C |
| Configuration | `prj.conf` (Kconfig) per project |
| Debug and flash | Onboard SEGGER J-Link, USB |
| Testing | nRF Connect mobile app (scanner, connection and GATT inspection) |

## Repository contents

| Files | Description |
|-------|-------------|
| `Lab7_task1_main.c`, `Lab7_task1_prj.conf` | BLE peripheral with a button service. The board advertises as `Nordic_LBS`; pressing and releasing the on-board button is reported to the phone in nRF Connect. |
| `Lab7_task2_main.c`, `Lab7_task2_prj.conf` | Modified peripheral with a custom device name (`Murad_nRF52840`) and an environmental sensing service that reports the on-chip temperature. |
| `Lab8_main.c`, `Lab8_prj.conf` | Minimal BLE advertiser: advertises a custom name with no GATT services and no connections. |
| `Lab8_ble_adv.c`, `Lab8_ble_adv.h` | Advertising module used by the Lab 8 application (data setup, interval, TX power, start). |

## What each project demonstrates

### BLE peripheral and GATT (Lab 7)
- The board advertises on channels 37, 38 and 39 and appears in the nRF Connect scan list.
- A phone (central) connects, advertising stops, and communication moves to the data channels.
- Data is exchanged through GATT (button state, temperature notifications).
- Configuration is done in `prj.conf`, for example `CONFIG_BT=y`, `CONFIG_BT_PERIPHERAL=y` and `CONFIG_BT_DEVICE_NAME`.
- After a disconnect, the board resumes advertising automatically.

### Minimal advertiser (Lab 8)
- Advertise-only: the device is discoverable but cannot be connected to (no "Connect" button in nRF Connect).
- Advertising data contains the flags and the complete local name.
- **Advertising interval:** 500 ms. BLE uses 0.625 ms units, so 500 / 0.625 = 800 = `0x0320`.
- **TX power:** +4 dBm (`CONFIG_BT_CTLR_TX_PWR_PLUS_4=y`).
- Trade-off studied: a shorter interval makes the device appear faster in scans but uses more power.

## Build and flash

Requires the Nordic nRF Connect SDK and a connected nRF52840 DK. Each project needs its source file named `main.c` and its configuration named `prj.conf` inside a project folder, for example:

```bash
mkdir lab8 && cd lab8
cp ../Lab8_main.c src/main.c        # create src/ first if it does not exist
cp ../Lab8_ble_adv.c ../Lab8_ble_adv.h src/
cp ../Lab8_prj.conf prj.conf
west build -b nrf52840dk/nrf52840
west flash
```

Then open **nRF Connect** on a phone, scan, and look for the device name.

## Report

A detailed write-up with BLE workflow diagrams (ADV_IND, CONNECT_IND, channel hopping, TERMINATE_IND), initialization flowcharts, code walkthroughs and test screenshots is available on request.

## Author

**Murad Balashli**: Electrical & Electronics Engineering, ADA University
[GitHub](https://github.com/MuradB1) · muradbalashov7@gmail.com
