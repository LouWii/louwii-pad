# ATmega flashing

Blank/virgin ATmega might be missing a few things in order to run properly as a macro pad with a QMK firmware.

## Setup

### ISP programmer and avrdude

The Louwii-pad exposes ISP pins of the ATmega in order to program it via an ISP programmer. You can use an Arduino as an ISP programmer ([see official doc](https://www.arduino.cc/en/tutorial/arduinoISP#toc2)), or buy a dedicated one like an USBasp ([see official page](https://www.fischl.de/usbasp/)).

There's no other way to program your ATmega32u4 if it came without a bootloader (in case of a Louwii-pad).

We're going to use the [avrdude](https://www.nongnu.org/avrdude/) command tool to set the fuses and flash the bootloader.

### ATmega Fuses

The fuses are used to set some parameters to the ATmega, defining how it runs. We need to set them according to our use.

* **Low:** 0xFF (we use an external 16Mhz resonator)
* **High:** 0xD9 (default flash size etc..., SPI enabled) -- Note: we could disable SPI once we're done, but it's nice to have them if we mess up
* **Extended:** 0xC0 (Brown-out set to 4.3V, Hardware boot enabled) -- Note: We might set the brown-out down to 3.5V, in case of a voltage drop: 0xC1

We're not touching the lock fuse, we don't need to. It should be set to 0xFF by default, allowing us to do anything we want.

[See more about fuses](https://eleccelerator.com/fusecalc/fusecalc.php?chip=atmega32u4&LOW=FF&HIGH=D9&EXTENDED=C0&LOCKBIT=FF)

### Bootloader

You can either use Atmel DFU bootloader or the QMK fork QMK DFU.

* [Atmel DFU link](https://www.microchip.com/wwwproducts/en/ATmega32U4#additional-features) - Go in the _Documents_ tab, scroll down to _Software Libraries/Firmware_ and download the bootloaders zip file. It contains `ATMega32U4-usbdevice_dfu-1_0_0.hex`, the bootloader ready to be flashed.
* [QMK DFU link](https://docs.qmk.fm/#/flashing?id=qmk-dfu) - QMK DFU contains some customizations compared to the Atmel DFU that cam be handy, but we're not using them really. It can be generated via `make` commands in the QMK repo: `make planck/rev4:default:bootloader`

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

### 2. Set the fuses

```
sudo avrdude -c usbasp-clone -p m32u4 -U lfuse:w:0xFF:m -U hfuse:w:0xD9:m -U efuse:w:0xC0:m
```

If you were not using the 16Mhz quartz yet, unplug the board and solder it in, the ATmega is now set to use it.


### 3. Flash the bootloader

```
sudo avrdude -c usbasp-clone -p m32u4 -U flash:w:ATMega32U4-usbdevice_dfu-1_0_0.hex
```

Once the bootloader is flashed, the ATmega will boot up in bootloader mode, ready to flash a program on it.

## The external resonator problem

```
device signature = 0x000000
device signature always changing
avrdude: initialization failed, rc=-1
```

When I received my ATmega chips, the fuses were set in such a way that the ATmega was expecting a "low speed" external resonator to be set on the XTAL1 pin. That's a hard to track problem: the ATmega wasn't responding when trying to be contacted via the SPI pins, and there's no way to know or change the fuses. After many hours of research, try and fail, I found 1 solution that worked: generating an approximate 8Mhz square wave signal via an Arduino.

What I did was: removing the 16Mhz quartz from the PCB. Connect the pin 9 or my Arduino Leonardo to the XTAL1 pin of the ATmega32u4 (the one on the left side on X1). Power on the Arduino. Connect the ATmega to the programmer. Run the test command, cross fingers. And the ATmega showed up.

The arduino program:

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

The ATmega can also use its internal clock, so it might be able to respond to the avrdude command without any resonator wired in.
