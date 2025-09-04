## Update your Tilda Mk3

This guide will help you to update your Tilda Mk3 badge. Be careful,
following these instructions will delete all files and setting on your
badge, so make sure you have copies of everything!

If you're having trouble following these instructions or updating your
badge, [please get in touch](mailto:badge@emfcamp.org) and we'll do our
best to help.

## Before you start

You need to put your badge into DFU mode. This activates a USB
bootloader stored in ROM on the STM32 processor.

**To enter DFU mode:** Switch on the badge. Press the center joystick
button while at the same time quickly pushing the reset button on the
back of the badge.

In some cases this seems to require a few attempts. You can also try
unplugging the battery, and holding down the center button while
plugging in USB.

## OSX

**Prerequisites needed for Mac OS X**

1\. XCode (from App Store)

2\. Python & pip

- Homebrew method

  1\. [Install Homebrew and
  Pip](http://docs.python-guide.org/en/latest/starting/install/osx/)

  2\. `brew install libusb lsusb`

  3\. `pip install libusb1 pyusb`
- MacPorts method

  1\. [Install MacPorts and
  Pip](http://johnlaudun.org/20150512-installing-and-setting-pip-with-macports/)

  2\. `sudo port install usbutils py-pip`


  usbutils provides e.g. lsusb, useful for knowing if the badge is
  connected in DFU mode

  3\. `sudo pip install libusb1 pyusb`

3\. Check that you're connected in DFU mode:


`lsusb`

Should output something like:

`Bus 020 Device 002: ID 0483:df11 STMicroelectronics STM Device in DFU Mode`

4\.
`wget `[`https://update.badge.emfcamp.org/update.py`](https://update.badge.emfcamp.org/update.py)

5\. `sudo python update.py`

6\. To get a shell: `screen /dev/tty.usbmodem*`

**Troubleshooting**

If you get the error `Error: No backend available` then python cannot
find the USB library. Please Repeat steps 2 and 3 above, see
<https://github.com/walac/pyusb/issues/120> for more details.

## Linux

Install prerequisites if required:

`sudo apt install python-usb`

Open a terminal and execute this line:

`curl --silent --show-error --retry 5 `[`https://update.badge.emfcamp.org/update.py`](https://update.badge.emfcamp.org/update.py)` | python`

If you get an error saying `ImportError: No module named core` then the
python-usb package in your distribution is too old. On Debian, you
should be able to fix it using pip instead:

`sudo apt install python-pip`

`sudo pip install --upgrade pyusb`

## Windows

**<span style="color:red">WARNING</span>: There have been reports of
badges being bricked when flashed from windows. They can be recovered
using the OSX/Linux update procedure. We are looking for alternative
ways of updating using Windows.**

On a Windows PC, you will need to download the STMicroelectronics DfuSe
USB device firmware updater software
<http://www.st.com/content/st_com/en/products/development-tools/software-development-tools/stm32-software-development-tools/stm32-programmers/stsw-stm32080.html>
and use the 'Upgrade or Verify Action' part of the GUI to update the
firmware instead.

- Install the STMMicroelectronics utility from the link above.
- Download the firmware.dfu file from
  <https://update.badge.emfcamp.org/firmware.dfu> and save it.
- With the badge plugged into the computer, hold the centre joystick
  button and press reset. The badge should now boot into the bootloader,
  and be detected as a 'STM Device in DFU Mode'.
- Start up DfuSeDemo.exe, and click the 'Choose' button - select the
  firmware.dfu file and then hit 'Upgrade'
- Your firmware will now be updated.

## Other Operating systems

To update your badge please download the following script and run it via
python: [update.py](https://update.badge.emfcamp.org/update.py)

If you know how to flash the badge yourself you can also download the
DFU binary directly:
[firmware.dfu](https://update.badge.emfcamp.org/firmware.dfu)

## Build your own version

If you want to build your own version of the firmware have a look
[here](https://badge.emfcamp.org/wiki/TiLDA_MK3/build)