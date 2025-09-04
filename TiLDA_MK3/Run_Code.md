This video is meant for the micropython board, but most of it works
similar on the TiLDA: <https://micropython.org/>

# Option 1: REPL

REPL stands for "[Read–eval–print
loop](https://en.wikipedia.org/wiki/Read%E2%80%93eval%E2%80%93print_loop)"
and allows you to run python code, line by line. This is a great way to
see effects of certain hardware commands instantly and to test basic
python features.

This allows you to run code, line by line, on the badge and to see the
effects instantly. Follow
<https://docs.micropython.org/en/latest/pyboard/pyboard/tutorial/repl.html>
or follow step 1-4 on the
<a href="TiLDA_MK3/Get_Started" class="wikilink" title="Get Started">Get
Started</a> guide.

<strong>Note:</strong> This will only work if you have an empty
`main.py` on your badge to stop the home screen from taking over
instantly. If an app or script is already running you will not get a
REPL that you can use to program, but simple debug output instead.
Remember to remove it afterwards if you want to use the home screen
again.

You can also press Ctrl-C to stop the currently running app and get to
the python prompt.

# Option 2: Copying files

You can also copy your code to `main.py` on your badge, safely eject and
then reset the badge via the button. To debug it you can use the serial
console (see option 1) which will show you all text printed via
`print()` and errors.

<strong>Note:</strong> It's very important to always correctly "eject"
the usb storage before pressing the reset button, otherwise your
filesystem is going to corrupt and you might have to
<a href="TiLDA_MK3/reset" class="wikilink" title="factory reset">factory
reset</a> your badge

Remember to remove your main.py afterwards if you want to use the home
screen again.

On Linux you can use this script to automatically mount the TILDA -\>
sync files from a local directory -\> safely eject:
<https://gist.github.com/IVBakker/fbab508c336397323d6c1caef8cdc46c>

# Option 3: pyboard.py

You can also use micropythons's `pyboard.py` script to automatically run
a full local file of code without having to worry about copying it
line-by-line or ejecting your device.

You can find a quick introduction on how to set it up
[here](https://badge.emfcamp.org/wiki/TiLDA_MK3/Get_Started#5._Run_a_whole_file_of_code).

This option is very useful as a build script with your editor, it allows
you to quickly test whether your app is working.

<strong>Note:</strong> While this option works even if you run the home
screen, it works slightly better if you have an empty `main.py` on your
badge.

### Sublime Text 2 build script

OSX:

    {
        "shell_cmd": "/path/to/your/pyboard.py --device /dev/tty.usbmodem* $file"
    }