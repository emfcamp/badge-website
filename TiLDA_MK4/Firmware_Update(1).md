While many of the TiLDA apps can be updated via the Badge Store, the
base firmware of the badge must be updated over USB.

Updating will provide stability and performance improvements, as well as
fixing problems with phone calls.

**Warning:** Updating the badge firmware will wipe all apps and
settings. Please be sure to have backed up your own code before doing
this.

## What you need

- A computer running OSX, Linux, or Windows equipped with USB ports
- A micro USB cable to connect your badge
- <a href="TiLDA_MK4/tilda-tools" class="wikilink"
  title="TiLDA Tools">TiLDA Tools</a> installed
- [DFU Util](http://dfu-util.sourceforge.net/) installed

## Procedure

1.  Connect the Badge to your computer using the USB cable
2.  Put your Badge into DFU mode (Press the joystick in - it will click,
    then press the reset button, then let go of the joystick after a
    second)
3.  Run `tilda-tools firmware-update` in a terminal (in the location
    where you installed TiLDA Tools)
4.  Follow the onscreen information
5.  Don't forget to press the reset button once the firmware update was
    successful!