 
# Revisions

## Revision 1

First rev made. Got PCBs manufactured by JLCPCB for cheap.

Everything is working ok, not issues in the wiring, which is good. The ATmega32u4 is not easy to solder, but I was expecting that. It's doable though, with some patience and some [flux](https://en.wikipedia.org/wiki/Flux_(metallurgy)).

However, there are a few points that can be improved:

* The OLED screen placement is not the best. Might be better to put it between the rotary encoder rows. Or increase the spacing between the encoders and the switches.
* The mini-usb socket soldering is not easy. If not done right, it causes disconnections and other problems. The way it's supposed to be attached is not great either, and the socket can be ripped off easily. The socket that I got adds even more issues. The cable wiggles a lot when plugged in, also causing random disconnections. **Lots of issues for a single part.** I would probably change it for a micro-usb through-hole socket, which would be sturdier and easier to solder. I like mini-USB, I find it less fragile that micro-usb, and never really had issues with it before (a lot of specialized mechanical keyboards still them), that's why I chose that in the first place.
* Flash space on the ATmega32u4 is a bit small when using QMK firmware with all the features enabled (well, features that we need). I know some keyboards use an ATmega328p which is available in a DIP format (easier to solder), I wonder if we could use a 644 (or 644P). Might be worth digging.
