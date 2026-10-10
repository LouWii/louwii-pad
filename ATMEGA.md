# ATmega flashing

Blank/virgin ATmega might be missing a few things in order to run properly as a macro pad with a QMK firmware.

## Setup

### ISP programmer and avrdude

The Louwii Pad exposes ISP pins of the ATmega in order to program it via an ISP programmer. You can use an Arduino as an ISP programmer ([see official doc](https://www.arduino.cc/en/tutorial/arduinoISP#toc2)), or buy a dedicated one like an USBasp ([see official page](https://www.fischl.de/usbasp/)).

There's no other way to program your ATmega32u4 if it came without a bootloader (in this particular case with a Louwii Pad).

We're going to use the [avrdude](https://www.nongnu.org/avrdude/) command tool to set the fuses and flash the bootloader.

### Bootloader

There are various bootloaders you can use on an ATmega32u4, and we need to pick one to install on our bare ATmega32u:

* [Atmel DFU link](https://www.microchip.com/en-us/product/ATmega32U4#documenttable-caption--1286044838) - Official bootloader; Scroll down to the _Embedded Software_ section and download the bootloaders zip file. It contains `ATMega32U4-usbdevice_dfu-1_0_0.hex`, the bootloader ready to be flashed. **Update** It doesn't seem to be available on their website anymore, but you can find it by searching `ATMega32U4-usbdevice_dfu-1_0_0.hex` on the internet.
* [QMK DFU link](https://docs.qmk.fm/#/flashing?id=qmk-dfu) - QMK DFU contains some customizations compared to the Atmel DFU that cam be handy, but we're not using them really. It can be generated via `make` commands in the QMK repo: `make planck/rev4:default:bootloader`
* [Arduino Caterina](https://github.com/arduino/ArduinoCore-avr/blob/master/bootloaders/caterina/Caterina-Micro.hex) - Caterina is the default bootloader for Arduino Micro controllers, that are based on an ATmega32u4.
* [Sparkfun's version of Caterina](https://github.com/sparkfun/Arduino_Boards/tree/master/sparkfun/avr/bootloaders/caterina) - That one requires a double press on the reset button to get the chip to stay in bootloader mode for a moment.

[QMK's bootloaders documentation](https://docs.qmk.fm/isp_flashing_guide#bootloader-firmware) contains a good list of bootloaders along with more explanation.

I personally went for the Sparkfun version. But you can pick any as long as it's compatible with QMK.

### ATmega Fuses

The fuses need to be set depending on the bootloader you picked. In my case:

* **Low:** 0xFF (we use an external 16Mhz resonator)
* **High:** 0xD8 (set bootloader flash size, boot reset, SPI enabled) -- Note: we could disable SPI once we're done, but it's nice to have them if we mess up
* **Extended:** 0xFB (Brown-out set to 2.6V)

We're not touching the lock fuse, we don't need to. It should be set to 0xFF by default, allowing us to do anything we want.

[Great fuse calculator for AVR chips](https://eleccelerator.com/fusecalc/fusecalc.php?chip=atmega32u4&LOW=FF&HIGH=D9&EXTENDED=C0&LOCKBIT=FF) - [And another one](https://www.engbedded.com/fusecalc/)


### `avrdude` on Windows

Using `WSL` (`Windows Subsystem for Linux`) is probably the easiest.

* Install WSL
* Install `avrdude` command in your WSL terminal (`sudo apt install avrdude` should do the trick if using Ubuntu WSL).
* Install `usbipd` Windows command - See [github repo](https://github.com/dorssel/usbipd-win) - This is so that the USBasp device can be accessed from within WSL.
* In Windows, open an admin command prompt, run `usbipd list` to list all USB peripherals. Note the bus ID of the USBasp (looks like `1-7`)
* Still in Windows command prompt, run `usbipd bind --busid 1-7` (replace the bus ID with the correct one)
* Still in Windows command prompt, run `usbipd attach --wsl --busid 1-7`
* You should be able to use the USB device in WSL now. Run the `avrdude` command in a WSL command prompt

## Steps

### 1. Check that the ATmega is detected

To see if your programmer detects the ATmega, run

```
sudo avrdude -c usbasp-clone -p m32u4
```

Remove `sudo` if you're running the command in Windows.

In my case, I'm using a usbasp clone (`usbasp-clone`). But you can change it to `usbasp` or `arduino` or any other programmer you use.

This should give you something like this

```
$ sudo avrdude -c usbasp-clone -p m32u4

avrdude: warning: cannot set sck period. please check for usbasp firmware update.
avrdude: AVR device initialized and ready to accept instructions

Reading | ################################################## | 100% 0.00s

avrdude: Device signature = 0x1e9587 (probably m32u4)

avrdude: safemode: Fuses OK (E:FF, H:D9, L:C0)

avrdude done.  Thank you.
```

If your fuses are different, that's fine, we're going to set them up properly.

**If it's not working**, your ATmega might not be configured properly yet. Either talk to your provider, or check out the section about the unknown clock further down.

### 2. Set the fuses

```
sudo avrdude -c usbasp-clone -p m32u4 -U lfuse:w:0xFF:m -U hfuse:w:0xD8:m -U efuse:w:0xFB:m
```

**If you were not using the 16Mhz quartz yet, unplug the board and solder it in, the ATmega is now set to use it.** Without it, you won't be able to communicate with the chip anymore.


### 3. Flash the bootloader

```
sudo avrdude -c usbasp-clone -p m32u4 -U flash:w:ATMega32U4-usbdevice_dfu-1_0_0.hex
```

Once the bootloader is flashed, the ATmega will boot up in bootloader mode, ready to flash a program on it.

## The unknown clock problem

`avrdude` wasn't happy:

```
device signature = 0x000000
device signature always changing
avrdude: initialization failed, rc=-1
```

An ATmega32u4 can be configured different ways to set its clock. If you're lucky, your seller/provider will tell you how it is configured when you purchase it. Here's a very basic breakdown of the options:
- internal clock - you don't need a crystal connect to the chip to connect to it through SPI
- external clock 8MHZ - you need an 8Mhz crystal (or something producing 8Mhz) connected to connect to it through SPI
- external clock 16Mhz - you need a 16Mhz crystal (or something producing 16Mhz) connected to connect to it through SPI; Louwii Pad uses a 16Mhz crystal

When I received my ATmega chips, I had no idea how they were configured. That's a hard to solve problem: the ATmega isn't responding when trying to be contacted via the SPI pins, and there's no way to know or change the fuses. To add insult to injury, I had no idea the ATmega had to be configured to use your expected clock. At the time, I had soldered the 16Mhz crystal and thought it would just work.

After many hours of research, trial and error, I finally found a way to connect to the ATmega: by generating an approximate 8Mhz square wave signal via an Arduino to be used as a crystal. Turns out my chips are configured to use an external 8Mhz crystal.

I had to remove the 16Mhz quartz from the PCB. Then I connected the pin 9 or my Arduino Leonardo to the XTAL1 pin of the ATmega32u4 (the one on the left side on X1). Powered on the Arduino. Connected the ATmega to the programmer. Ran the test command, crossed fingers very hard. And the ATmega showed up. I was so happy.

The arduino program I used:

```
#ifdef __AVR_ATmega2560__
  const byte CLOCKOUT = 11;  // Mega 2560
#else
  const byte CLOCKOUT = 9;   // Uno, Duemilanove, etc.
#endif

void setup ()
{
  // set up 8 MHz timer on CLOCKOUT (OC1A)
  pinMode (CLOCKOUT, OUTPUT);
  // set up Timer 1
  TCCR1A = bit (COM1A0);  // toggle OC1A on Compare Match
  TCCR1B = bit (WGM12) | bit (CS10);   // CTC, no prescaling
  OCR1A =  0;       // output every cycle
}

void loop ()
{
  // whatever
}
```
