**📓 [notes-network](https://github.com/bmedicke/notes-network) 🕸️**

---

# raspberry-notes

* [Raspberry Pi in 100 seconds](https://www.youtube.com/watch?v=eZ74x6dVYes)

## toc

<!-- vim-markdown-toc Redcarpet -->

* [hardware](#hardware)
  * [vcgencmd](#vcgencmd)
    * [under-voltage and other throttling reasons](#under-voltage-and-other-throttling-reasons)
  * [SSD1306 OLED (SPI version)](#ssd1306-oled-spi-version)
  * [automatic 8188eu based Wi-Fi driver install](#automatic-8188eu-based-wi-fi-driver-install)
    * [managing networks with wicd](#managing-networks-with-wicd)
* [install docker](#install-docker)

<!-- vim-markdown-toc -->

# hardware

## vcgencmd

`vcgencmd` is a tool by Broadcom that can be used to query
the VideoCore processor in a Raspberry Pi.
VideoCore is a low-power mobile multimedia processor.
### under-voltage and other throttling reasons

```sh
vcgencmd commands
man vcgencmd
```

* query throttle state with `vcgencmd get_throttled`
* this returns a bit pattern with the following meaning:

```sh
              Bit   Meaning
              ────  ────────────────────────────────────
         0    Under-voltage detected
         1    Arm frequency capped
         2    Currently throttled
         3    Soft temperature limit active
        16    Under-voltage has occurred
        17    Arm frequency capping has occurred
        18    Throttling has occurred
        19    Soft temperature limit has occurred
```

example:

```sh
vcgencmd get_throttled
# returns 0x50000

python -c 'print(bin(0x500000))'
# returns 0b1010000000000000000

# counting from the right (zero-indexed):
# bit 16 and 18 are set.

# according to the table above:
# 17    Arm frequency capping has occurred
# 19    Soft temperature limit has occurred
```

## SSD1306 OLED (SPI version)

__prerequisites__

- Raspberry Pi running [Raspbian](https://www.raspberrypi.org/downloads/raspbian/), not tested with [raspbian ua netinst](https://github.com/debian-pi/raspbian-ua-netinst).
- enabled [SPI](https://en.wikipedia.org/wiki/Serial_Peripheral_Interface_Bus) (via `raspi-config`, advanced options)

__pinout__

| oled module | raspberry pi   | meaning                       |
| ---         | ---            | ---                           |
| GND         | GND            | ground                        |
| VCC         | 3.3V or 5V     | Voltage Collector Collector   |
| SCL         | SCLK, SPI CLK  | SPI clock                     |
| SDA         | MISO, SPI MISO | SPI data, Master In Slave Out |
| RST         | GPIO_RST       | reset                         |
| D/C         | GPIO_DC        | data/command                  |

`GPIO_RST` and `GPIO_DC` can be any two GPIO pins.

See [interactive pinout diagram](https://pinout.xyz) for the Raspberry Pi.

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

__usage__

```sh
# GPIO_RST: 23
# GPIO_DC: 24
cd luma.examples/examples
python3 terminal.py -i spi --gpio-data-command 24 --gpio-reset 23
```

## automatic 8188eu based Wi-Fi driver install

https://www.raspberrypi.org/forums/viewtopic.php?f=28&t=62371

```sh
sudo wget http://www.fars-robotics.net/install-wifi -O /usr/bin/install-wifi
sudo chmod +x /usr/bin/install-wifi
```

### managing networks with wicd

```sh
sudo apt-get install wicd-curses
sudo wicd-curses
```

# install docker

```sh
curl https://get.docker.com | sh
sudo systemctl enable docker
```
