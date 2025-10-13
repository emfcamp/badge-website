Should your badge stop working there are three ways to fix it. Please
make sure you have a backup of whatever you've been working on, you
might lose all the data stored on your badge.

It's best to try these in sequence, however if you just want to factory
reset your badge skip to option (3).

1.  Try deleting the file you have been working on, "safely eject" the
    badge and then press the reset button.
2.  Delete all other files apart from `boot.py`, `wifi.json` and
    `bootstrap.py`, "safely eject" the badge and then press the reset
    button.
3.  Factory reset the badge. Press and hold 'MENU' and then press the
    reset button on the back. Keep the MENU button held down, then
    release it when both the green light is on and the screen is white.

All these steps should finally get you to a screen saying "Downloading
TiLDA software". It should take less than a minute until the bare badge
functionalities are restored.

P.S. The micropython documentation on factory resetting is
[here](http://docs.micropython.org/en/latest/pyboard/pyboard/tutorial/reset.html).
On the TiLDA MK3 the USR button is the MENU button, and the orange light
is the screen going white.