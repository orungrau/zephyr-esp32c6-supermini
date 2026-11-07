.. SPDX-License-Identifier: Apache-2.0

ESP32-C6 Super Mini
###################

This board definition targets the ESP32-C6FH4 Super Mini with 4 MiB flash
and a single WS2812 RGB LED on GPIO8. It uses Zephyr's ESP32-C6 SoC support.

Targets
*******

* ``esp32c6_supermini/esp32c6/hpcore``: main application, native USB console,
  GPIO, BOOT button, UART0, I2C, RGB LED and wireless peripherals.
* ``esp32c6_supermini/esp32c6/lpcore``: low-power core, LP UART on GPIO4/5.

Connections
***********

==================  ==========================
Function            GPIO
==================  ==========================
USB Serial/JTAG     12 (D-), 13 (D+)
BOOT button         9, active low with pull-up
WS2812 RGB LED      8, I2S data output
UART0               16 (TX), 17 (RX)
I2C0                20 (SDA), 19 (SCL)
==================  ==========================

The RESET button is connected to EN, not to a software GPIO.

RGB LED
*******

The HP-core devicetree exposes one GRB pixel through the ``rgb0`` and
``led-strip`` aliases, using ``worldsemi,ws2812-i2s`` with DMA channel 3.
Applications use Zephyr's LED strip API (``led_strip_update_rgb``) and enable
``CONFIG_LED_STRIP`` when they need the LED. The driver selects I2S and DMA.
The board does not enable the LED driver in its defconfig.

There is no GPIO ``led0`` alias: the WS2812 requires serial pixel data.
Some Super Mini revisions also have a discrete LED on GPIO15; that optional
LED is not assigned here because its presence and polarity vary by board.
GPIO8 and GPIO9 are strapping pins; GPIO12/13 are reserved for native USB
while USB Serial/JTAG is in use.

Location
********

This is an out-of-tree board under ``boards/others/esp32c6_supermini``.
Its board root is the repository root (the directory containing ``boards``).
Zephyr discovers this root through the module metadata in zephyr/module.yml.

References
**********

* `Super Mini pinout and specifications
  <https://mischianti.org/esp32-c6-supermini-high-resolution-pinout-datasheet-schema-and-specs/>`_
* `Board schematic (ESP32-C6FH4)
  <https://mischianti.org/wp-content/uploads/2026/04/esp32-c6-supermini-board-schematic.pdf>`_
