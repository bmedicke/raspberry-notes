# the raspberry notes


<!-- vim-markdown-toc Redcarpet -->

* [SSD1306 OLED (SPI version)](#ssd1306-oled-spi-version)

<!-- vim-markdown-toc -->

## SSD1306 OLED (SPI version)

__prerequisites__

- Raspbian, not tested with raspbian-ua-netinst

__pinout__

| oled module | raspberry pi | meaning                       |
| ---         | ---          | ---                           |
| GND         | GND          | ground                        |
| VCC         | 3.3V or 5V   | Voltage Collector Collector   |
| SCL         | SPI CLK      | SPI clock                     |
| SDA         | SPI MISO     | SPI data, Master In Slave Out |
| RST         | GPIO_RST     | reset                         |
| D/C         | GPIO_DC      | data/command                  |

`GPIO_RST` and `GPIO_DC` can be any two GPIO pins.

See [interactive pinout diagram](https://pinout.xyz).

__libraries__

- [luma.oled](http://github.com/rm-hull/luma.oled)
- [luma.examples](http://github.com/rm-hull/luma.examples)


__installation__

```sh
# https://github.com/rm-hull/luma.examples#installation-instructions
sudo usermod -a -G i2c,spi,gpio pi
sudo apt install python-dev python-pip libfreetype6-dev libjpeg-dev build-essential
sudo apt install libsdl-dev libportmidi-dev libsdl-ttf2.0-dev libsdl-mixer1.2-dev libsdl-image1.2-dev
sudo -H pip install --upgrade pip setuptools
sudo apt purge python-pip

git clone https://github.com/rm-hull/luma.examples.git
cd luma.examples

sudo -H pip install -e .
```

_usage_

```sh
# GPIO_RST: 23
# GPIO_DC: 24
cd luma.examples/examples
python3 terminal.py -i spi --gpio-data-command 24 --gpio-reset 23
```
