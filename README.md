# LCDGFX library for display controllers

[![Build Status](https://travis-ci.org/lexus2k/lcdgfx.svg?branch=master)](https://travis-ci.org/lexus2k/lcdgfx)
[![Coverage Status](https://coveralls.io/repos/github/lexus2k/lcdgfx/badge.svg?branch=master)](https://coveralls.io/github/lexus2k/lcdgfx?branch=master)


[tocstart]: # (toc start)

  * [Introduction](#introduction)
  * [Key Features](#key-features)
  * [Easy to use](#easy-to-use)
  * [Supported displays](#supported-displays)
  * [Supported platforms](#supported-platforms)
  * [Setting up](#setting-up)
  * [License](#license)

[tocend]: # (toc end)


## Introduction

lcdgfx driver is Arduino style C++ library with unicode support. The library can be compiled for plain Linux
(for example, raspberry spi), or you can use it with plain avr-gcc compiler without Arduino IDE. It supports
monochrome and RGB oleds and has debug mode, allowing to execute code on PC, using SDL2.0.
Initially the library was intended to run on very small microcontrollers (with a little of RAM). It was developed to use as
few resources as possible, but still has powerful capabilities (NanoEngine), allowing to develop nice animation.
It works on any powerful devices like raspberry pi, esp32; and can be easily ported to new platform.
This library integrates another library [canvas](https://github.com/lexus2k/canvas)

## Key Features

 * Supports color, monochrome OLED displays
 * The library has modular structure, and some modules can be excluded from compilation at all to reduce flash usage.
 * Needs very little RAM (Attiny85 with Damellis package needs minimum 25 bytes of RAM to communicate with OLED)
 * Fast implementation to provide reasonable speed on slow microcontrollers
 * Supports i2c and spi interfaces:
   * i2c (software implementation, Wire library, AVR Twi, Linux i2c-dev)
   * spi (4-wire spi via Arduino SPI library, AVR Spi, AVR USI module)
 * Primitive graphics functions (lines, rectangles, pixels, bitmaps, drawing canvas)
 * Printing text to display (using fonts of different size, you can use GLCD Font Creator to create new fonts)
 * Includes [graphics engine](https://github.com/lexus2k/lcdgfx/wiki/Using-NanoEngine-for-systems-with-low-resources2) to support
   double buffering on tiny microcontrollers.
 * Can be used for game development (bonus examples):
   * Arkanoid game ([arkanoid](examples/games/arkanoid) in old style API and [arkanoid8](examples/games/arkanoid8) in new style API)
   * Simple [Lode runner](examples/games/lode_runner) game.
   * [Snowflakes](examples/nano_engine/snowflakes)

![Image of arkanoid intro](imgs/arkanoid.png)
![Image of lode runner](imgs/lode_runner.gif)

![Image of menu example](imgs/mainmenu_top.png)
![Image of color oled](imgs/fonts.png)

The i2c pins can be changed via API functions. Please, refer to documentation. Keep in mind,
that the pins, which are allowed for i2c or spi interface, depend on the hardware.
The default spi SCLK and MOSI pins are defined by SPI library, and DC, RST, CES pins are configurable
through API.

## Easy to use

Example:

```.cpp
DisplayST7735_128x160x16_SPI display(3,{-1, 4, 5, 0,-1,-1});

void setup()
{
    display.begin();
    display.clear();
}

void loop()
{
    display.setColor(RGB_COLOR16(255,255,0));
    display.drawLine(10,30,56,96);
}
```

## Supported displays:

| **Display** | **I2C** | **SPI** | **Orientation** | **Comments** |
| :-------- |:---:|:---:|:---:|:---------|
| sh1106 128x64 | X | X |   |   |
| ssd1306 128x64 | X | X |   |   |
| ssd1306 128x32 | X | X |   |   |
| ssd1325 128x64 | X | X |   |   |
| ssd1327 128x128 | X | X |   |   |
| ssd1331 96x64 |   | X | X |   |
| ssd1351 128x128 |   | X |   |   |
| il9163 128x128 |   | X | X |   |
| st7735 128x160 |   | X | X |   |
| ili9341 240x320 |   | X | X |   |
| pcd8544 84x48 |   | X  |   | Nokia 5110 |

## Supported platforms

| **Platforms** | **I2C** | **SPI** | **Comments** |
| :-------- |:---:|:---:|:---------|
| **Arduino** |     |     |          |
| Attiny85, Attiny45  |  X  |  X  | Refer to [Damellis attiny package](https://raw.githubusercontent.com/damellis/attiny/ide-1.6.x-boards-manager/package_damellis_attiny_index.json) |
| Attiny84, Attiny44  |  X  |  X  | Refer to [Damellis attiny package](https://raw.githubusercontent.com/damellis/attiny/ide-1.6.x-boards-manager/package_damellis_attiny_index.json) |
| Atmega328p, Atmega168  |  X  |  X  |    |
| Atmega32u4  |  X  |  X  |    |
| Atmega2560  |  X  |  X  |    |
| Digispark, including PRO version  |  X  |  X  |  check [examples compatibility list](examples/Digispark_compatibility.txt)  |
| ESP8266  |  X  |  X  | check [examples compatibility list](examples/ESP8266_compatibility.txt)   |
| ESP32  |  X  |  X  | check [examples compatibility list](examples/ESP8266_compatibility.txt)   |
| STM32  |  X  |  X  | [stm32duino](https://github.com/stm32duino/wiki/wiki)  |
| Arduino Zero | X  | X  |    |
| Nordic nRF5 (nRF51, nRF52) | X | X | via Standard Arduino nRF52 boards. nRF users, enable c++11 in platform.txt `-std=gnu++11` |
| Nordic nRF5 (nRF51, nRF52) | X | X | via [Sandeep Mistry arduino-nRF5](https://github.com/sandeepmistry/arduino-nRF5) package |
| **Plain AVR** |   |     |          |
| Attiny85, Attiny45 |  X  |  X  |         |
| Atmega328p, Atmega168 |  X  |  X  |         |
| Atmega32u4  |  X  |  X  |    |
| **Plain ESP32** |   |     |          |
| ESP32 |  X  | X  |  library can be used as IDF component  |
| **Linux**  |    |     |          |
| Raspberry Pi |  X  |  X  | i2c-dev, spidev, sys/class/gpio  |
| [SDL Emulation](https://github.com/lexus2k/lcdgfx/wiki/How-to-run-emulator-mode) |  X  |  X  | demo code can be run without real OLED HW via SDL library |
| **Windows**  |    |     |          |
| [SDL Emulation](https://github.com/lexus2k/lcdgfx/wiki/How-to-run-emulator-mode) |  X  |  X  | demo code can be run without real OLED HW via MinGW32 + SDL library |

Digispark users, please check compilation options in your Arduino prior to using this library.
lcdgfx library requires at least c++11 and c99 (by default Digispark package misses the options
-std=gnu11, -std=gnu++11).

## The goals of lcdgfx library

 * To use as few RAM as possible
 * To use as few Flash as possible
 * To be as fast as possible
 * To fit [Arkanoid game example](examples/games/arkanoid) to Attiny85 microcontroller

## Setting up

*i2c Hardware setup is described [here](https://github.com/lexus2k/lcdgfx/wiki/Hardware-setup)*

*Setting up for Arduino from github sources)*
 * Download source from https://github.com/lexus2k/lcdgfx
 * Put the sources to Arduino/libraries/lcdgfx folder

*Setting up for Arduino from Arduino IDE library manager*
 * Install lcdgfx library (named lcdgfx by Alexey Dynda) via Arduino IDE library manager

*Using with plain avr-gcc:*
 * Download source from https://github.com/lexus2k/lcdgfx
 * Build the library (variant 1)
   * cd lcdgfx/src && make -f Makefile.avr MCU=<your_mcu>
   * Link library to your project (refer to [Makefile.avr](examples/Makefile.avr) in examples folder).
 * Build demo code (variant 2)
   * cd lcdgfx/tools && ./build_and_run.sh -p avr -m <your_mcu> ssd1306_demo

 *For esp32:*
  * Download source from https://github.com/lexus2k/lcdgfx
  * Put downloaded sources to components/lcdgfx/ folder.
  * Compile your project as described in ESP-IDF build system documentation

For more information about this library, please, visit https://github.com/lexus2k/lcdgfx.
Doxygen documentation can be found at [github.io site](http://lexus2k.github.io/lcdgfx).
If you found any problem or have any idea, please, report to Issues section.

## License

The library is free. If this project helps you, you can give me a cup of coffee.

MIT License

Copyright (c) 2016-2019, Alexey Dynda

Permission is hereby granted, free of charge, to any person obtaining a copy
of this software and associated documentation files (the "Software"), to deal
in the Software without restriction, including without limitation the rights
to use, copy, modify, merge, publish, distribute, sublicense, and/or sell
copies of the Software, and to permit persons to whom the Software is
furnished to do so, subject to the following conditions:

The above copyright notice and this permission notice shall be included in all
copies or substantial portions of the Software.

THE SOFTWARE IS PROVIDED "AS IS", WITHOUT WARRANTY OF ANY KIND, EXPRESS OR
IMPLIED, INCLUDING BUT NOT LIMITED TO THE WARRANTIES OF MERCHANTABILITY,
FITNESS FOR A PARTICULAR PURPOSE AND NONINFRINGEMENT. IN NO EVENT SHALL THE
AUTHORS OR COPYRIGHT HOLDERS BE LIABLE FOR ANY CLAIM, DAMAGES OR OTHER
LIABILITY, WHETHER IN AN ACTION OF CONTRACT, TORT OR OTHERWISE, ARISING FROM,
OUT OF OR IN CONNECTION WITH THE SOFTWARE OR THE USE OR OTHER DEALINGS IN THE
SOFTWARE.


