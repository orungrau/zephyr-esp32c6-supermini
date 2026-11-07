# ESP32-C6 Super Mini for Zephyr

This module adds board support for the ESP32-C6 Super Mini to Zephyr 4.4.0. It targets boards with an ESP32-C6FH4, 4 MiB of flash, and one WS2812 RGB LED on GPIO8.

| Board target | Use | Console |
| --- | --- | --- |
| `esp32c6_supermini/esp32c6/hpcore` | Main application, Bluetooth LE, Wi-Fi, and RGB LED | USB Serial/JTAG over USB-C |
| `esp32c6_supermini/esp32c6/lpcore` | Low-power core applications | LP UART |

## Install with west

Add the following entry to the `manifest.projects` list in your workspace's `west.yml`.

```yaml
- name: esp32c6-supermini-bsp
  url: https://github.com/orungrau/zephyr-esp32c6-supermini.git
  revision: main
  path: modules/boards/esp32c6-supermini-bsp
```

The example tracks the `main` branch. For reproducible builds, set `revision` to a specific commit SHA or release tag.

Fetch the module and check that west lists the board.

```shell
west update esp32c6-supermini-bsp
west boards --board esp32c6_supermini
```

Zephyr discovers the board directory automatically. For Wi-Fi or Bluetooth LE, include `hal_espressif` in your workspace and fetch its binary blobs with `west blobs fetch hal_espressif`.

## Run the RGB LED sample

Connect the board with a USB-C data cable. From the Zephyr source directory, build the LED strip sample for the high-performance core and flash it to the board.

```shell
west build -b esp32c6_supermini/esp32c6/hpcore -d build/supermini-rgb samples/drivers/led/led_strip
west flash -d build/supermini-rgb
```

The onboard LED cycles through red, green, and blue. To control it from your application, enable `CONFIG_LED_STRIP=y` and get the device with `DEVICE_DT_GET(DT_ALIAS(led_strip))`. The board's devicetree defines a single pixel with GRB channel order.

## High-performance core pinout

| Function | Pins |
| --- | --- |
| WS2812 RGB LED | GPIO8 (I2S data output) |
| BOOT button | GPIO9 (active low) |
| USB Serial/JTAG | GPIO12 (D−), GPIO13 (D+) |
| UART0 | GPIO16 (TX), GPIO17 (RX) |
| I²C0 | GPIO20 (SDA), GPIO19 (SCL) |

[Board hardware reference](boards/others/esp32c6_supermini/doc/index.rst) · [Apache 2.0 license](LICENSE)
