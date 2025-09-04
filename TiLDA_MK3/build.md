## Dependencies

You can use either windows or linux, although windows will not have most
things installed by default

### git, make, python, etc

I will just assume you have this

### arm-none-eabi-gcc

<https://launchpad.net/gcc-arm-embedded/+download> You will have to make
the arm-none-eabi-gcc from the bin directory available to your system
(add to PATH, symlink or copy)

On Ubuntu you can follow these instructions:
<https://launchpad.net/~team-gcc-arm-embedded/+archive/ubuntu/ppa>

On Debian (in Stretch, maybe earlier versions too, also Raspbian Jessie)
you can \`sudo apt-get install gcc-arm-none-eabi\`

### pyusb

    sudo pip install pyusb

## USB Permissioning on Linux

On some Linux distributions it may be necessary to add additional udev
rules in order to allow the REPL to work:

copy the following text to /etc/udev/rules.d/49-tilda-mk3.rules

    # 0483:df11 - Tilda Mk3 based on Micropython board
    ATTRS{idVendor}=="0483", ATTRS{idProduct}=="df11", ENV{ID_MM_DEVICE_IGNORE}="1"
    ATTRS{idVendor}=="0483", ATTRS{idProduct}=="df11", ENV{MTP_NO_PROBE}="1"
    SUBSYSTEMS=="usb", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="df11", MODE:="0666"
    KERNEL=="ttyACM*", ATTRS{idVendor}=="0483", ATTRS{idProduct}=="df11", MODE:="0666"

And then restart the udev service:

    sudo udevadm control --reload-rules

## Flashing

    # Clone this repo
    git clone --recursive https://github.com/emfcamp/micropython.git

    # Switch to our work branch
    cd micropython
    git checkout tilda-master

    # Now we can build the firmware and flash it to the badge
    # You have to boot the badge into dfu mode by pressing down the center
    # joystick button while pressing the reset button to trigger a reboot
    make -C stmhal BOARD=STM32L475_EMFBADGE deploy

## What to do when the firmware update.py script fails

When attempting an update from linux, some users have hit timeouts
during hte mass_erase step. This leaves the badge in an ususable state.

    $ python update.py
    Hello - Welcome to the automated TiLDA firmware updater
    We found your badge. So far, so good...
    Traceback (most recent call last):
      File "update.py", line 556, in <module>
        main()
      File "update.py", line 530, in main
        mass_erase()
      File "update.py", line 114, in mass_erase
        "\x41", __TIMEOUT)
      File "/usr/local/lib/python2.7/dist-packages/usb/core.py", line 1043, in ctrl_transfer
        self.__get_timeout(timeout))
      File "/usr/local/lib/python2.7/dist-packages/usb/backend/libusb1.py", line 883, in ctrl_transfer
        timeout))
      File "/usr/local/lib/python2.7/dist-packages/usb/backend/libusb1.py", line 595, in _check
        raise USBError(_strerror(ret), ret, _libusb_errno[ret])
    usb.core.USBError: [Errno 110] Operation timed out

If you have access to a Windows PC, you can download the
STMicroelectronics DfuSe USB device firmware updater software
<http://www.st.com/content/st_com/en/products/development-tools/software-development-tools/stm32-software-development-tools/stm32-programmers/stsw-stm32080.html>
and use the 'Upgrade or Verify Action' part of the GUI to update the
firmware instead.

- Install the STMMicroelectronics util from the link above.
- Download the firmware.dfu file from
  <https://update.badge.emfcamp.org/firmware.dfu> and save it off.
- Start up DfuSeDemo.exe, and click the 'Choose' button - select the
  firmware.dfu file and then hit 'Upgrade'
- Your badge should now be un-bricked