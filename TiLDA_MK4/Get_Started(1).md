# Get started

Hacking your TiLDA Mk4 badge is easy. We've written it down step by
step.

If something goes wrong, the badge can be
<a href="TiLDA_MK4/reset" class="wikilink" title="reset">reset</a> to
its out-of-the-box state, so do not be concerned about breaking
anything.

## 1. Connect to your computer

To get started you need to connect your badge to a computer. You can use
any MicroUSB cable - the same type that charges most mobile phones
nowadays. It doesn't matter whether the battery is plugged in or not, so
don't worry about it.

## 2. Connect to your badge

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

- Open a terminal and type `screen /dev/xyz` (todo: please add the
  correct path)
- You should now be greeted by a line saying "Micropython"

### What to do if this step doesn't work?

- Press the reset button at the back of your badge and try again
- Have a look at
  <https://docs.micropython.org/en/latest/pyboard/pyboard/tutorial/repl.html>
  for more information

## 3. Your first line of Micropython

- In your terminal (next to the \>\>\>) type `print(1 + 1)` followed by
  Enter
- You should see `2` printed on the screen. Congratulations, you've just
  written your first line of micropython code!
- You can now close the terminal window (or Putty, if you're on
  Windows). Note: some serial terminals will not close when the badge is
  removed or powered off. You should close the serial terminal before
  plugging the badge back in, otherwise the serial terminal may not
  reconnect.

## 4. Download some software you'll need

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

## 5. tilda_tools

You need to clone our Mk4-Apps repository:
<https://github.com/emfcamp/Mk4-Apps>

You might find some tricks on how to do this here:
<https://badge.emfcamp.org/wiki/TiLDA_MK4/Badge_Store_Submissions#Submitting_your_badge_app_to_the_official_badge_store>

There's a python script calle "tilda_tools" in the repository. You can
use it to do all sorts of stuff that is a bit complicated to do with the
USB mass storage.

To test it open "adhoc.py" and add the following line at the bottom:

    ugfx.text(5, 5, "Hello World!", ugfx.RED)

and then run "./tilda_tools run adhoc.py" (or "python tilda_tools run
adhoc.py" on Windows)

Todo: test this on windows - what's the best way of running python
scripts there?

You should now see "Hello World!" on your screen.

## 5. Your first app

Let's create an app you can put on your badge.

- Open the folder you've downloaded from github - You will see other app
  folders like "badge_store" and "snake".
- Create a folder called "my_app". This is your new app.
- Open a text editor. On Windows you can search for `Notepad`, on OSX
  there's one called `textedit` and if you're on Linux you can probably
  find one by searching for "edit".
- Copy the following and save it as `main.py` in your folder:

<!-- -->

    """My first app"""

    ___title___         = "My app"
    ___license___      = "MIT"
    ___dependencies___ = ["ugfx_helper"]
    ___categories___ = ["Demo"]

    import ugfx_helper, ugfx, tilda, sleep
    from app import restart_to_default

    ugfx_helper.init()
    ugfx.clear()

    ugfx.text(5, 5, "Hello World", ugfx.RED)
    ugfx.fill_circle(100, 100, 30, ugfx.GREEN)
    ugfx.fill_circle(200, 100, 30, ugfx.GREEN)
    ugfx.area(80, 150, 140, 20, ugfx.GREEN)
    ugfx.area(120, 170, 60, 20, ugfx.GREEN)

    while tilda.Buttons.is_pressed(tilda.Buttons.BTN_Menu) is False:
        sleep.wfi() # This means sleep for a short while

    ugfx.clear()
    restart_to_default()

Now you can run "./tilda_tools app my_app" - Wait a few seconds and you
should see your app working on the screen.

It will now work even when the computer is disconnected. To load it
you'll have to open it via the Launcher or use the settings app to make
it your default app.

## 6. Publish your app to the app library

Now that you have your own smiley app, why not share it with others?

See <https://badge.emfcamp.org/wiki/TiLDA_MK4/Badge_Store_Submissions>
for more information