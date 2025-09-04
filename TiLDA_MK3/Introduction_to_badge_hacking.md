# Beginners Guide to Badge Hacking

We're going to create an LED torch for seeing our way around the camp.

**Soldering the LED**

You should have received an LED and a resistor with your badge. First,
grab the resistor. Bend the legs over so they fit in the holes in the
board, using the resistor lead bender.

<figure>
<img src="Hackpad.com_Rs7awClTQDH_p.554927_1470420913282_legbender.jpg"
title="Hackpad.com_Rs7awClTQDH_p.554927_1470420913282_legbender.jpg" />
<figcaption>Hackpad.com_Rs7awClTQDH_p.554927_1470420913282_legbender.jpg</figcaption>
</figure>

It doesn't matter way round the resistor goes.

<figure>
<img src="Hackpad.com_Rs7awClTQDH_p.554927_1470307053798_P1010517.jpg"
title="Hackpad.com_Rs7awClTQDH_p.554927_1470307053798_P1010517.jpg" />
<figcaption>Hackpad.com_Rs7awClTQDH_p.554927_1470307053798_P1010517.jpg</figcaption>
</figure>

Flip over your board and solder the resistor in place, then trim down
the legs to be flush against the board.

Next, the LED. It does matter which way round the LED goes - look for a
flat side on the LED and match it up with the flat side on the board.
Pull your LED out about a centimetre and bend it over a bit.

<figure>
<img src="Hackpad.com_Rs7awClTQDH_p.554927_1470307470838_P1010533.jpg"
title="Hackpad.com_Rs7awClTQDH_p.554927_1470307470838_P1010533.jpg" />
<figcaption>Hackpad.com_Rs7awClTQDH_p.554927_1470307470838_P1010533.jpg</figcaption>
</figure>

Flip the board over and solder the LED into place.

**Testing**

Plug your badge into your laptop, and use the information on
[1](https://docs.micropython.org/en/latest/pyboard/pyboard/tutorial/repl.html)
to connect to the usb-serial port. If you hit Ctrl-C, you drop out of
the badge software, and get a REPL prompt. You can tell this is working
because you'll see three \> symbols.

Type the following at the prompt. This should turn your LED on!

`import pyb`
`pin = pyb.Pin("LED_TORCH")`
`pin.high()`

Of course, thats a bit of a pain to do every time we want the LED on, so
let's make an app!

**The App**

Your badge should have shown up as a USB drive. Open the folder
**apps**, and create a folder within it called **torch.**

Create a file within this called **main.py**. This is your app's main
file. Open it in a text editor.

First, we need to tell the badge some details about your app. This is
done in a header section at the top of the file

`# Author: Your Name `
`# Description: Torch `
`# Category: Flashy `
`# License: MIT `
`# Appname : Torch `
`# Built-in: no `

You could save and run your app now, but it wont actually do anything
yet. Lets add the code:

`# Author: Your Name `
`# Description: Torch `
`# Category: Flashy `
`# License: MIT `
`# Appname : Torch `
`# Built-in: no `

`import pyb`
`pin = pyb.Pin("LED_TORCH")`
`pin.high() `
`while True:`
`  pass`

Save, and run your app. When you run it the LED should turn on, brill!

Of course, we're not showing anything on the screen. Lets have a nice
notice box to show we're in the torch instead of that while loop.

`# Author: Your Name `
`# Description: Torch `
`# Category: Flashy `
`# License: MIT `
`# Appname : Torch `
`# Built-in: no `

`import pyb`
`import ugfx`
`import dialogs`

`pin = pyb.Pin("LED_TORCH")`
`pin.high() `
`ugfx.init()`
`ugfx.clear(ugfx.html_color(0x7c1143))`
`dialogs.notice("Shine a light!", title="Torch", close_text="Exit")`

Run your app again and you should have a nice button you can press to
shine a light in the camp!

You can unplug the badge from your computer when the LED has finished
flashing, but we recommend you eject the drive first.