# Louwii Pad

**Very basic USB macro pad for tinkering and experimenting.**

The main goal is to have it run a custom [QMK](https://docs.qmk.fm/) firmware, so that it can be easily programmed to be used as a macropad to help with day to day tasks. As it's running an ATmega32u4, same as the Arduino Leonardo & Micro, it can be programmed through the Arduino IDE as well, for even more customization.

![louwii-pad-rev1-small](https://private-user-images.githubusercontent.com/2750789/670247439-dfbe5354-9f28-458f-8513-f5d79d3b1d79.jpeg?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTE2MDY3MzIsIm5iZiI6MTc5MTYwNjQzMiwicGF0aCI6Ii8yNzUwNzg5LzY3MDI0NzQzOS1kZmJlNTM1NC05ZjI4LTQ1OGYtODUxMy1mNWQ3OWQzYjFkNzkuanBlZz9YLUFtei1BbGdvcml0aG09QVdTNC1ITUFDLVNIQTI1NiZYLUFtei1DcmVkZW50aWFsPUFLSUFWQ09EWUxTQTUzUFFLNFpBJTJGMjAyNjEwMTAlMkZ1cy1lYXN0LTElMkZzMyUyRmF3czRfcmVxdWVzdCZYLUFtei1EYXRlPTIwMjYxMDEwVDA0MjcxMlomWC1BbXotRXhwaXJlcz0zMDAmWC1BbXotU2lnbmF0dXJlPTJkZTdlY2E2MmEyYmFmMzFiNmU4NDI2MTk5MDRmYjJjY2RkNmYwZjc5OTA3MTJlYjgzNDFjMWMxNGFiNTI4MTAmWC1BbXotU2lnbmVkSGVhZGVycz1ob3N0JnJlc3BvbnNlLWNvbnRlbnQtdHlwZT1pbWFnZSUyRmpwZWcifQ.r9Crihq03SLz5WYOTDRxo0ubDbzw7JnOoz72aCsSmFU)

![louwii-pad-rev1-demo](https://private-user-images.githubusercontent.com/2750789/670255763-4d714bb6-b55f-40b0-9e51-5dd4ab754735.gif?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTE2MDgxOTMsIm5iZiI6MTc5MTYwNzg5MywicGF0aCI6Ii8yNzUwNzg5LzY3MDI1NTc2My00ZDcxNGJiNi1iNTVmLTQwYjAtOWU1MS01ZGQ0YWI3NTQ3MzUuZ2lmP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MTAxMCUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjEwMTBUMDQ1MTMzWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9ZWU5ZTIwNDVjZWY1OTQwN2Y3YzA2Nzk1MTAyMDU3NDMxMTI2N2RiNjA0Zjg4YjAwZTJkOWJiYTJjMjczZDJmOSZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGZ2lmIn0.mEn1UY_JDH-Clc5-bU8d7kEX9qx5xtZxYkt8m8s8BgE)

This demo is running the default QMK firmware. The bottom right knob is used to switch between layers, which get displayed on the OLED screen.

## Inspiration

* [ruiqimao's tutorial](https://github.com/ruiqimao/keyboard-pcb-guide)
* [Arisu](https://github.com/FateNozomi/arisu-pcb)
* [cpm43](https://github.com/Gtrx0/cpm43)
* [Gtrx0/Keyboard-template-atmega32u4](https://github.com/Gtrx0/Keyboard-template-atmega32u4)

[ATmega32u4 official doc](http://ww1.microchip.com/downloads/en/devicedoc/atmel-7766-8-bit-avr-atmega16u4-32u4_datasheet.pdf)

## BOM (list of components)

[See doc](BOM.md)

## Bootlader, ATmega flashing

Flash a bootloader, set the chip fuses: [See doc](ATMEGA.md)

## Firmware (QMK)

[See doc](FIRMWARE.md)

## Revisions

[See doc](REVISIONS.md)

## Third party libs

[See doc](LIBS.md)
