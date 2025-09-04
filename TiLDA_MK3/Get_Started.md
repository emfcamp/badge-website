# Get started

Hacking your TiLDA badge is easy. We've written it down step by step.

If something goes wrong, the badge can be
<a href="TiLDA_MK3/reset" class="wikilink" title="reset">reset</a> to
its out-of-the-box state, so do not be concerned about breaking
anything.

## 1. Connect to your computer

To get started you need to connect your badge to a computer. You can use
any MicroUSB cable - the same type that charges most mobile phones
nowadays. It doesn't matter whether the batter is plugged in or not, so
don't worry about it.

## 2. Check your badge is found by your computer

Upon connecting the badge to a computer, it should appear as a "mass
storage device" just like a USB key or an external hard disk.

**Important:** Just like any other USB storage device, if you changed or
copied anything on the Badge, please make sure to "eject" or "safely
remove" before unplugging your badge or pressing the reset button.
Otherwise you might lose all your work :(

Windows will require a driver file for the serial port, which is stored
on the badge's mass storage. [More
information](https://micropython.org/resources/Micro-Python-Windows-setup.pdf)

## 3. Connect to your badge

### Windows

- Please follow the instruction under the "Windows" section on this
  page:
  <https://docs.micropython.org/en/latest/pyboard/pyboard/tutorial/repl.html>
- Once you've installed the driver and Putty and you've connected to
  your badge, press Control+C to stop the main badge app, and then you
  should be greeted by a line saying "Micropython"

### OSX

- Click the magnifying class in the top right of your screen and type
  `Terminal` followed by Enter. A white terminal window should appear.
- type `screen /dev/tty.usbmodem*` (The \* is important, so don't let it
  out) and hit enter
- hit `control+c`, if an error is displayed
- You should now be greeted by a line saying "Micropython"

### Linux

- Open a terminal and type `screen /dev/ttyACM0`
- You should now be greeted by a line saying "Micropython"

### What to do if this step doesn't work?

- Press the reset button at the back of your badge and try again
- Have a look at
  <https://docs.micropython.org/en/latest/pyboard/pyboard/tutorial/repl.html>
  for more information

## 4. Your first line of Micropython

- In your terminal (next to the \>\>\>) type `print(1 + 1)` followed by
  Enter
- You should see `2` printed on the screen. Congratulations, you've just
  written your first line of micropython code!
- You can now close the terminal window (or Putty, if you're on
  Windows). Note: some serial terminals will not close when the badge is
  removed or powered off. You should close the serial terminal before
  plugging the badge back in, otherwise the serial terminal may not
  reconnect.

## 5. Download some software you'll need

You will be downloading some stuff now. It's probably be best if you
create some folder somewhere so you don't end up with files all over
your Desktop.

Note you can copy files across from your computer to the badge, reset
the badge and run the files though the menu, however these instructions
make things a little easier.

### Windows

- Python: Go to <https://www.python.org/downloads/> and download python
  version 3.x. After the download is finished you can install it.
- Go to <https://bootstrap.pypa.io/get-pip.py> and save the file into
  your folder
- Hold shift, right click on the folder on, and click "open command
  window here"
- Now type `python get-pip.py`
- After the install was successful please type
  `pip install pyserial pyusb` followed by Enter.
- If you do not wish to install python, and would rather copy files
  across, the command line utility RoboCopy is recommended. For example:
  `Robocopy.exe ./my_app/ d:/apps/my_app`

### OSX / Linux

- Python: Open a terminal and type `python --version`. If you get a
  version number you're good to go, otherwise go to
  <https://www.python.org/downloads/> and download Python version 3.x.
  After the download is finished you can install it.
- Go to <https://bootstrap.pypa.io/get-pip.py> and save the file into
  your folder
- Use a terminal to go to the folder you created ([this is how you do
  it](http://www.macworld.com/article/2042378/master-the-command-line-navigating-files-and-folders.html))
  and type `python get-pip.py`
- Now type `python get-pip.py`
- After the install was successful please type
  `pip install pyserial pyusb` followed by Enter

### Problems

- If you have problems install python, please use google, there are lots
  of good explanations out there
- For information about how to install `pip` have a look here:
  <https://pip.pypa.io/en/stable/installing/>

## 6. (Optional) Update your badge's micropython

'You can skip this step', but since TiLDA is still a work in progress,
it's best to update regularly. So why not do it now? It's easy, just
follow the instructions here:
<a href="TiLDA_MK3/Firmware_Update" class="wikilink"
title="TiLDA_MK3/Firmware_Update">TiLDA_MK3/Firmware_Update</a>

## 5. Run a whole file of code

While you can do most things via the terminal like you just did, it's a
bit easier to work on a file that you can edit. So let's do that.

- Go to
  <https://raw.githubusercontent.com/emfcamp/micropython/tilda-master/tools/pyboard.py>
  and use your browser to save it as "pyboard.py" into your newly
  created folder
- Open a text editor. On Windows you can search for `Notepad`, on OSX
  there's one called `textedit` and if you're on Linux you can probably
  find one by searching for "edit".
- Copy the following and save it as `test.py` in your folder:

``` python
print(1 + 2)
```

- Open a terminal, navigate to the right folder (see step 5 for more
  info)
  - Windows: `python pyboard.py test.py --device=COM4` Replace COM4 with
    whatever port your badge installed as.
  - OSX: type `python pyboard.py test.py --device=/dev/tty.usbmodem*`
  - Linux: type `python pyboard.py test.py --device=/dev/ttyACM*`
- You should now see the your terminal showing `3`. Your getting good at
  this!

**Hint**: If you don't want the home app to start every time you can
create an empty `main.py` file in the USB folder. This will stop the
boot from loading the home screen and allow you do run code on your
badge undisturbed. But remember to "safely eject" the badge after
writing the file and to remove it again when you want the normal
behaviour back

## 7. Blink

So far all we've seen is the badge doing some maths and sending the
result back via usb. Let's start with one of the simplest hardware
device on the badge, a LED (which means "Light emitting diode") -- We
want to make it blink.

Create a file called `blink.py` and copy this bit of code into it:

``` python
import pyb
led = pyb.LED(1)
led.on()
pyb.delay(500)
led.off()
pyb.delay(500)
led.on()
pyb.delay(500)
led.off()
pyb.delay(500)
led.on()
```

Save it and use a terminal to execute it (See Step 5 - but make sure to
replace the filename in the command). Have a look at your badge, you
should see the tiny LED labeled "A" (It's right between the B button and
the screen) flash twice before and then staying on.

Now, let's see what the code actually does.

``` python
import pyb
```

This line tells your badge to import the "pyb" library. It allows you to
do various hardware related things with your badge (the full list is
[here](http://docs.micropython.org/en/latest/pyboard/library/pyb.html)).
This is something very important in python, you always need to import
libraries before you can use them.

``` python
led = pyb.LED(1)
```

This line uses the pyb module we've just imported and creates a LED
object. The "`1`" means we're using the first LED on the badge. Change
it to "`2`", save it and run pyboard.py again - Now the LED B should
flash in green.

The `led` bit before the `=` means that we're not only creating an LED
object, but we're also storing it for later use.

``` python
led.on()
```

Here we're using the reference to the LED object we've just created and
calling the `.on()` function on it. Most python objects have multiple
functions you can use. In this case you can look the them up here:
<http://docs.micropython.org/en/latest/pyboard/library/pyb.LED.html?highlight=led>

``` python
pyb.delay(500)
```

This line uses the `pyb.delay()` function to pause the execution of our
program for a while. The `500` is a number in milliseconds and the code
is stopping for half a second. We need this delay to give your eyes some
time to see that the LED has turned on. Try playing with different
numbers or leaving the line out altogether to get a feeling for it.

## 8. Blink forever

Since our original `blink.py` script is only blinking twice it might be
easy to miss. Let's fix this by blinking a bit longer:

``` python
import pyb
led = pyb.LED(1)
while True:
    led.toggle()
    pyb.delay(500)
```

Be careful to make sure you're copying the spaces in front of the last
two lines as well, they're very important in Python! If you want to type
them you have to use the `Tab` key on your keyboard - It's normally on
the left of your keyboard, two keys above the `Shift` key.

Save this code and run it via `pyboard.py` again. You should now see the
red LED blinking forever. So what's happening here?

``` python
while True:
    led.toggle()
    pyb.delay(500)
```

This is called a "while loop" in Python. It means that a certain bit of
code (the one indented by the Tab at the beginning of the line) will be
repeated over and over again until a certain condition is not fulfilled
anymore. In this case the condition is `True` which is the Python-way of
saying "this is always correct". Therefore the loop will repeat forever.

`led.toggle()` is similar to `led.on()` and `led.off()`, but instead of
always turning the LED on or off it switches between the states. Very
useful in our case!

This kind of "infinite loop" is quite useful for a lot of badge code,
but often you also want to have some way of leaving the loop. Try this
instead:

``` python
import pyb
import buttons
buttons.init()
led = pyb.LED(1)
while not buttons.is_pressed("BTN_MENU"):
    led.toggle()
    pyb.delay(500)
```

There are a few new things here: First we have imported a new library
called `buttons`. This library helps you with all the buttons on the
badge. If you want to use buttons in your script you also have to call
`buttons.init()`, so that's what we're doing in line 3.

We have also changed the `while` loop with a new statement
`not buttons.is_pressed("BTN_MENU")`. Like I said above, `while` loops
will go on forever while its condition is fulfilled. We have now changed
the condition to be "while the MENU button is not pressed".

Try running the code. You will again get the same blinking LED, but this
time you should be able to stop it by pressing the MENU button.

## 8. Hello Screen

Ok, now let's try to write something on the screen. TiLDA comes with a
colour screen and we can use a library called UGFX to draw on it.

Create a `main.py` file and put this in there:

``` python
import ugfx
import buttons
import pyb

ugfx.init()
buttons.init()
ugfx.clear(ugfx.YELLOW)

ugfx.text(5, 5, "Hello World", ugfx.RED)
ugfx.fill_circle(100, 100, 30, ugfx.GREEN)
ugfx.fill_circle(200, 100, 30, ugfx.GREEN)
ugfx.area(80, 150, 140, 20, ugfx.GREEN)
ugfx.area(120, 170, 60, 20, ugfx.GREEN)
```

If you run this file you should see a the screen turn yellow with some
text and a (very simple) smiley.

If you use the screen you have to make sure you're using `ugfx.init()`,
otherwise nothing will work. `ugfx.clear()` is used to clear the screen
(in this case with a special colour) and the next 5 lines are drawing
some text and a few basic shapes.

`ugfx.text(x, y, text, color)` Prints some text at position (x, y) in a
given colour. x and y are coordinates on the screen. x=0 and y=0 means
the top-left of the screen and x=320 y=240 is the bottom right corner.
Try changing the two numbers and see how the text moves around on the
screen.

`ugfx.fill_circle(x, y, radius, color)` draws a circle on the screen.

`ugfx.area(x, y, width, height, color)` draws a rectangle on the screen.

There are more basic drawing operations. You can find a list of them
here: <a href="TiLDA_MK3/ugfx" class="wikilink" title="uGFX">uGFX</a>

## 10. Your own app

Having to have your badge connected to your computer all the time is a
bit boring. Let's make it an app, so you can carry it around!

Update your `main.py` to look like this:

``` python
### Author: Your Name
### Description: Smiley!
### Category: Tutorial
### License: MIT
### Appname : Smiley

import ugfx
import buttons
import pyb

ugfx.init()
buttons.init()
ugfx.clear(ugfx.YELLOW)

ugfx.text(5, 5, "Hello World", ugfx.RED)
ugfx.fill_circle(100, 100, 30, ugfx.GREEN)
ugfx.fill_circle(200, 100, 30, ugfx.GREEN)
ugfx.area(80, 150, 140, 20, ugfx.GREEN)
ugfx.area(120, 170, 60, 20, ugfx.GREEN)

while not buttons.is_pressed("BTN_MENU"):
    pyb.wfi()
```

The bit at the top is a comment, it's not actually part of the code and
python ignores it. It does however help us categorise the app. Make sure
you update your name :)

Plug your badge in and open the USB drive. Go into the "apps" folder and
create a new folder called "hello_screen". Copy your newly updated
`main.py` into there and safely eject the device. If you now press the
reset button at the back of your badge it will restart normally. Wait
until your name appears and press the MENU button. Go all the way to the
bottom right and select "View all". Now you have to press "right" until
you find the "Smiley" app. Select it and press A - You should now see
your smiley.

## 11. Publish your app to the app library

Now that you have your own smiley app, why not share it with others?

Find your main.py and create an archive of it. Most operating systems
have something build in for that purpose. In OSX you can right click and
select "Compress main.py", under Windows you can select "New \> Zipped
folder" and drag required files into the zip; under Linux you right
click the files and use "Compress...". You should now have a new file
called `main.zip` or similar.

Go to <http://api.badge.emfcamp.org/> and sign up with your email
address and a password. In the top right there's a field called "Create
New App" - Put "Smiley" into the field next to it and then click
"Create". On the next screen click the "Choose File" button and select
your `main.zip`. Now press "Upload" - and you're done!

You now have to wait until we have reviewed your app to make sure it's
not doing something weird and then we'll hit the "publish" button and
everyone on the camp site will be able to download your newly created
app to their badge. If we're not quick enough, feel free to come to the
badge tent and we'll hurry the review up ;)

# Alternative ways to work with TiLDA

All of the python code which runs on the badge can be modified on the
mass storage device, and new apps can be added this way too. Be careful
when using text editors to modify files on the device, if the mass
storage is not 'safely removed' before removal, corruption of the file
being edited can occur.

When writing your own code, it is advised edit code on your computer,
then copy it across to the badge, wait for the red 'writing' LED to go
out, reset it, and run. This way you don't need to worry about safely
removing the badge each time.

To interactively run code on the badge, a python REPL can be accessed
via the virtual serial port. Once you have found the serial port, use
your favourite serial terminal to connect to the badge. Since the badge
will be running the main software, press Ctrl+c to stop it, and the
badge will display '\>\>\>' to indicate it is ready to receive commands.

The badge runs micropython, and as a result does not contain everything
you may expect from full python. See the micropython docs in the lniks
below for more information. The badge has additional APIs, in particular
for the LCD, Wifi and IMU, which are also listed below.