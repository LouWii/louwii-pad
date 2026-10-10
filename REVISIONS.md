 
# Revisions

## Revision 1

First rev made. Got PCBs manufactured by JLCPCB for cheap.

Everything is working ok, not issues in the wiring, which is good. The ATmega32u4 is not easy to solder, but I was expecting that. It's doable though, with some patience and some [flux](https://en.wikipedia.org/wiki/Flux_(metallurgy)).

However, there are a few points that can be improved:

* **The OLED screen placement is not the best.** Might be better to put it between the rotary encoder rows. Or increase the spacing between the encoders and the switches. I did solder a socket to insert the OLED screen pins into, which brings it up a bit and make it better, but it's not ideal.
* **The SMD mini-USB socket was a mistake.** At the time, mini-USB was used on some custom keyboards. And although USB cables usually sit firmly in micro-USB sockets, those socket looked more flimsy to me compared to the larger mini-USB ones. USB-C was just starting to push through at the time, and I deemed not worth it to look into it. In retrospect, I should have. There are USB-C sockets available with a through-hole design. It does require patience and precision to be soldered, but it'll be so much better than the mini-USB I picked. And soldering a mini-USB or micro-USB is already a pain anyway, so might as well pick the best socket of them all. What's even worse is that the mini-USB sockets I have are very loose, the USB cable has way too much wiggle room when plugged-in, which can cause accidental disconnects when moved.
* **Flash space on the ATmega32u4 is a bit small** when enabling MIDI features within QMK, along with RGB for underglow and OLED screen. Granted those features are technically all optional, the macropad will work fine without RGB and/or the OLED screen. I know some keyboards use an ATmega328p which is available in a DIP format (easier to solder), I wonder if we could use a 644 (or 644P). Not sure if it's worth digging into it.

### USB Socket Hack

I managed to find cheap USB-C sockets USB2.0 breakout boards that can fit perfectly onto the Louwii Pad PCB. It's janky, but it does work, and it does solve the biggest issue with the revision 1, the mini-USB socket. Just need to solder 4 wires from the breakout board to the PCB and you're done. It's also so much easier than soldering the mini-USB socket.

Here's where you can connect the USB lines onto the PCB:

![Louwii Pad USB Lines](https://private-user-images.githubusercontent.com/2750789/669349800-294756d2-7d55-480f-8a3f-26ddc6bd0d9d.jpg?jwt=eyJ0eXAiOiJKV1QiLCJhbGciOiJIUzI1NiJ9.eyJpc3MiOiJnaXRodWIuY29tIiwiYXVkIjoicmF3LmdpdGh1YnVzZXJjb250ZW50LmNvbSIsImtleSI6ImtleTUiLCJleHAiOjE3OTE1MjE1NzMsIm5iZiI6MTc5MTUyMTI3MywicGF0aCI6Ii8yNzUwNzg5LzY2OTM0OTgwMC0yOTQ3NTZkMi03ZDU1LTQ4MGYtOGEzZi0yNmRkYzZiZDBkOWQuanBnP1gtQW16LUFsZ29yaXRobT1BV1M0LUhNQUMtU0hBMjU2JlgtQW16LUNyZWRlbnRpYWw9QUtJQVZDT0RZTFNBNTNQUUs0WkElMkYyMDI2MTAwOSUyRnVzLWVhc3QtMSUyRnMzJTJGYXdzNF9yZXF1ZXN0JlgtQW16LURhdGU9MjAyNjEwMDlUMDQ0NzUzWiZYLUFtei1FeHBpcmVzPTMwMCZYLUFtei1TaWduYXR1cmU9YTQ5YTA2MjM0MDUzZWRmMzEwZWRhYzAyYzljMjIzM2E1YmQ2ZGY0MmQ1NzdlM2Y1ZDJlN2ViMTE3YTE5NGM3YSZYLUFtei1TaWduZWRIZWFkZXJzPWhvc3QmcmVzcG9uc2UtY29udGVudC10eXBlPWltYWdlJTJGanBlZyJ9.lRFe1Hw-mfFjcLSlArhJohu_8ucx2ohaxsucVHxzfCA)