One feature of the Mk3 is a single NeoPixel multi-colour LED adjacent to
the red power/charge LED.

## Faulty manufacture

Unfortunately, during the original manufacture of the badges the
NeoPixel was mounted 180 degrees out of orientation. As originally
delivered it will not function, being mounted with the triangular notch
facing the outer part of the badge and the chip part of the LED (the
brown spot under the transparent plastic) facing the charge LED. The
correct position is with the triangular notch facing inward and the chip
part facing the "D1" label of the silkscreen.

### Correcting the faulty mount

The best solution should be to use soldering tweezers take the LED off,
clean the pads, turn it, and put it on but this is a delicate operation
that will probably destroy the LED. The trick is to be quick, to use a
fairly high temperature (350-370 degrees) and add some new solder to the
pads to increase the heat transfer.

You can possibly use two soldering irons at the same time, one on each
side of the LED.

Avoid hot air at all costs, the badge is packed and you don't really
want to risk blowing the charging LED away or even worse, damaging the
LCD.

## Programming the LED

See this:
<https://github.com/mayhem/tilda-mk3-led-demo/blob/master/main.py>

Or NeoPixelTest in the App Library:
<http://api.badge.emfcamp.org/app/emuboy/neopixtest>