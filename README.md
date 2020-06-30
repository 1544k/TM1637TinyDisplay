# TM1637 Tiny Display # 
[![Build Status](https://travis-ci.org/jasonacox/TM1637TinyDisplay.svg?branch=master)](https://travis-ci.org/github/jasonacox/TM1637TinyDisplay)

Arduino Library for the TM1637 Based LED Display Module

## Description
This is an Arduino library for 4-digit 7-segment display modules based on the TM1637 chip. Connect the display's CLK and DIO pins to your Arduino GPIO pins, include this library, initialize TM1637TinyDisplay and call easy to use functions like showNumber(), showString(), showLevel() and showAnimation(). Runs well on many tiny controllers including the ATtiny85.

## Hardware 
![TM1637](examples/tm1637.png)

Display modules based on the TM1637 chip are available from [HiLetgo](https://www.amazon.com/gp/product/B01DKISMXK/ref=ppx_yo_dt_b_search_asin_title?ie=UTF8&psc=1), [DX](https://dx.com/p/0-36-led-4-digit-display-module-for-arduino-black-blue-works-with-official-arduino-boards-254978) and [SeeedStudio](https://www.digikey.com/products/en?keywords=tm1637). 

![TM1637](examples/tm1637back.png)

The display has four connectors:
* CLK - Clock - attach to any GPIO output on the Arduino
* DIO - Data  - attach to any GPIO output on the Arduino
* VCC - Power 5v
* GND - Ground

Power Note: Steady clean power is important for circuit stability. If you are seeing display artifacts during high frequency updates or animation sequences, you may be experiencing power fluctuations that are impacting signal timing and communication with the TM1637. This is especially true with standalone microprocessor applications that lack any power conditioning (e.g. ATtiny85). A polarized 100uF electrolytic capacitor inserted across VCC and GND can help smooth out the spikes.

## Installation
Copy this repository into your Arduino library folder as set by the Arduino IDE "sketchbook location" preferences (e.g. `~/Documents/Arduino/libraries`).  

## Usage
The library provides a single class named TM1637TinyDisplay with the following functions:

* `showNumber` - Display an integer number (positive or negative)
* `showNumberDec` - Display a number with ability to set decimal points or colon
* `showNumberHex` - Display a number in hexadecimal format and set decimal point or colon
* `showString` - Display a ASCII string of text with optional scrolling  
* `showLevel` - Use display LEDs to simulate a level indicator (vertical or horizontal)  
* `showAnimation` - Display a sequence of frames to render an animation
* `setSegments` - Directly set the value of the LED segments in each digit
* `setBrightness` - Sets the brightness of the display

```cpp
#include <Arduino.h>
#include <TM1637TinyDisplay.h>

// Define Digital Pins
#define CLK 1
#define DIO 2

// Initialize TM1637TinyDisplay
TM1637TinyDisplay display(CLK, DIO);

void setup() {
  display.setBrightness(0x0f);
}

void loop() {
  // Say Hello
  display.showString("HELLO");
  delay(500);

  // Clear Screen
  display.clear();

  // We can count!
  for (int x = -100; x <= 100; x++) {
    display.showNumber(x);
  }

  // Level indicator
  for (int x = 0; x <= 100; x = x + 10) {
    display.showLevel(x, false);
    delay(20);
  }
  for (int x = 100; x >= 0; x = x - 10) {
    display.showLevel(x, false);
    delay(20);
  }

  // Split screen for temperature
  display.showString("\xB0", 1, 3);        // Degree Mark, length=1, position=3 (right)
  for (int x = -90; x < 200; x++) {
    display.showNumber(x, false, 3, 0);    // Number, length=3, position=0 (left)
    delay(10);
  }

  // The end
  display.showString("End");
  delay(1000);
}
```

Refer to [TM1637TinyDisplay.h](TM1637TinyDisplay.h) for information on available functions. See also the [TM1637Test sketch](examples) for a demonstration including animation.

## References and Credit
* This library is based on the great work by Avishay Orpaz - https://github.com/avishorp/TM1637
* SevenSegmentTM1637 Arduino Library by Bram Harmsen - https://github.com/bremme/arduino-tm1637 
* Arduino - https://playground.arduino.cc/Main/TM1637/

