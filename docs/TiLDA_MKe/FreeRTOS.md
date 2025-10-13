# Programming the badge in FreeRTOS

## Your first “Hello world” app

There’s a “HelloWorldApp.cpp” file in which you can play around. In
order for it to show up on the Homescreen you have to uncomment line 51
in AppManager.cpp and flash the changed code to the badge. Great app
pull requests are appreciated!

If you are still using the Arduino IDE at this point, note that it will
not let you edit the .cpp and .h files that are needed to create Apps
for the badge. To force the IDE to re-compile/re-read any files you've
edited using an external editor, make sure to go to the File -\>
Preferences dialog box, and check the "Use external editor" checkbox.

## Why are things so different from standard Arduino code?

We’re using a library called FreeRTOS that allows us to multitask -
something that’s normally not possible with standard Arduino code. This
allows us to run multiple tasks at the same time. FreeRTOS uses
preemptive scheduling to switch between the task. Due to this we have to
be very careful about how we do some things. For example we can’t just
define interrupts for buttons in every task (imagine the mess!) or write
to the serial port directly (your task might stop in the middle of the
message).

We’ve also spent quite a lot of time to make the built-in components as
easy to use as possible without having every task to write lots of
boilerplate code. If you feel like using the build-in components on the
badge, chances are we already wrote a wrapper for them that is already
used by one of the other tasks.

Have a look at the “Documentation” section in this document for a full
list of API functions. You will avoid a lot of headaches if you stick to
those.

## Code structure

- FreeRTOS has the concept of “Tasks” which work like threads. We’ve
  wrappered them in a class called “Task” (for background stuff) and
  “Apps” (for foreground, one-at-a-time things)
- Everything needs to be in the main EMF2014 folder. Subfolders are not
  allowed. This is an Arduino IDE restriction :(

## Debugging and Gotchas

- The USB serial is set up to 115200 baud. There are lots of terminals
  that can connect to them. See below for how to enable the debug
  logging.
- If you can’t revive a badge, or only get the two red programming LEDs
  when you plug it, in, do a full erase (see below)
- Avoid busy waiting, use FreeRTOS queues and Tilda::delay() instead
- Don’t use low level functions like interrupts or serial ports directly
  unless you really, really know how FreeRTOS will handle them. For
  general logging you can use Tilda::log()
- If sending code to the badge using the Arduino IDE "Upload" button
  fails, even though the /dev/ttyACM0 (linux com port) is there, just
  retry, twice if neccessary.

## Full erase

This is the failsafe process if your badge won't show up over USB.

1.  Unplug the badge
2.  Connect the Erase pins on the back together (two holes down the left
    hand side next to the battery and under the blue wireless module).
    You can use a jumper, jump wire, or the leg of a resistor for this.
3.  Turn the badge back on with the switch or plug it back in
4.  Press the Reset button on the front (bottom centre) and release
5.  Wait 15 seconds
6.  Unplug the badge or turn it off

When you plug the badge back into a computer it will come back up in
programming mode, with a different serial port to the usual one. Open
the EMF 2014 sketch in the Arduino IDE, select the Tilda v0.333 (RTOS)
programmer, and find the new serial port. The IDE console should show
something like this:

`Sketch uses 118,748 bytes (22%) of program storage space. Maximum is 524,288 bytes.`
`Erase flash`
`Write 127668 bytes to flash`

`[                              ] 0% (0/499 pages)`
`[                              ] 2% (10/499 pages)`
`[=                             ] 4% (20/499 pages)`
`[=                             ] 6% (30/499 pages)`
`...`
`[============================= ] 98% (490/499 pages)`
`[==============================] 100% (499/499 pages)`
`Verify 127668 bytes of flash`

`[                              ] 0% (0/499 pages)`
`[                              ] 2% (10/499 pages)`
`[=                             ] 4% (20/499 pages)`
`[=                             ] 6% (30/499 pages)`
`...`
`[============================= ] 98% (490/499 pages)`
`[==============================] 100% (499/499 pages)`
`Verify successful`
`Set boot flash true`
`CPU reset.`

# Useful Hacks & Non-Programming Information

## Your own wireless badge network

<a href="DIY_TiLDA_Badge_Network" class="wikilink"
title="DIY TiLDA Badge Network">DIY TiLDA Badge Network</a> has
instructions on how to setup your own private badge network using a
RaspberryPi and two Ciseco radios.

## Convert images to TiLDA bitmap format

- A Python script (via
  [@trotmaster99](https://twitter.com/trotmaster99)) that converts a
  monochrome bitmap image into a format suitable for the Tilda can be
  found [here](http://pastebin.com/8XeazQjT).
- A similar script in Perl to create TiLDA MKe fullscreen bitmaps from
  XBM: -

<div style ="height:200px;overflow-x:hidden;overflow-y:auto;border: 4px solid orange;">

**xbm2mke.pl by <a href="User:Msemtd" class="wikilink"
title="User:Msemtd">User:Msemtd</a>**

``
`#!perl -w`
`use strict;`
`# Little script to convert a regular XBM to TiLDA MKe bitmap`
`# only tested with fullscreen bitmaps!`
`# Hot file handle magic...`
`select((select(STDERR), $| = 1)[0]);`
`select((select(STDOUT), $| = 1)[0]);`
`sub t(@);`
`sub d($);`
`sub chug($);`
`my $f = shift;`
`#~ $f = 'blankish.xbm' if not $f;`
`if(not defined $f or not $f =~ /^(.*)\.xbm$/i){`
`    die "Usage: gimme an XBM file dude!\n";`
`}`
`my $name = $1;`
`t "Reading file '$f'...";`
`my $data = chug($f);`
`t "OK";`
`my @lines = split /^/, $data;`
`@lines = grep{chomp; s/^\s+//; s/\s+$//; length;} @lines;`
`#~ t d \@lines;`
`my($width, $height) = (0,0);`
`my @head = @lines[0..5];`
`foreach(@head){`
`    if(/_width\s+(\d+)/){$width = $1;}`
`    if(/_height\s+(\d+)/){$height = $1;}`
`}`
`t "width x height = $width x $height";`
`my @k;`
`foreach(@lines){ push @k, split /,/; }`
`@k = grep { s/^.*(0x[0-9A-Fa-f]{1,2}).*$/$1/o; /(0x[0-9A-Fa-f]{1,2})/o } @k;`
`#~ t d \@k;`
`my $bc = scalar(@k);`
`t "Pulled out $bc hex bytes";`
`if($bc != $width * $height / 8) {`
`    die "byte count $bc does not match that expected for w x h";`
`}`
`t "OK - reorder bytes for MKe bitmap";`
`my $wb = int($width/8) + (($width & 0x07) ? 1: 0);`
`t "width in whole bytes for $width pixels = $wb";`
`my @mke;`
`for(my $col = 0; $col < $wb; $col++){`
`    for(my $row = $height - 1; $row >= 0; $row--){`
`        my $idx = ($row * $wb) + $col;`
`        my $val = $k[$idx];`
`        #~ t "Column $col + Row $row = idx $idx = $val";`
`        push @mke, $val;`
`    }`
`}`
`my $out = "static const uint8_t ".uc($name)."_BM[] = {\n"`
`    ."    $width, // width\n"`
`    ."    $height , // height\n";`
`#~ $out .= join(", ", @mke);`
`while(scalar @mke){`
`    $out .= join(", ", splice(@mke, 0, 16)).",\n";`
`}`
`$out .= "};\n";`
`# meh, just print it out`
`t $out;`
``
`sub t(@) {`
`    foreach (@_) {`
`       print STDOUT "$_\n";`
`    }`
`}`
`sub d($) {`
`    require Data::Dumper;`
`    my $s = $_[0];`
`    my $d = Data::Dumper::Dumper($s);`
`    $d =~ s/^\$VAR1 =\s*//;`
`    $d =~ s/;$//;`
`    chomp $d;`
`    return $d;`
`}`
`sub chug($) {`
`  my $filename = shift;`
`  local *F;`
``   open F, "< $filename" or die "Couldn't open `$filename': $!"; ``
`  local $/ = undef;`
`  return <F>;`
`}  # F automatically closed`
``

</div>

# Firmware Documentation

## Debugging

### Enabling the USB serial debug log messages

To enable the debug logging you must uncomment the following line in
[hardware/emfcamp/sam/libraries/debug/debug.h](https://github.com/emfcamp/Mk2-Firmware/blob/master/hardware/emfcamp/sam/libraries/debug/debug.h#L42)

`// Enable debug task and output`
`// #define DEBUG 1`

### Tilda::log(String text)

This logs “text” to the serial console. To read it connect to it via the
Arduino IDE Serial Monitor. Don’t use “SerialUSB.println” or similar --
it’s not thread-safe and you might end up with utter nonsense.

### Debugging using the JTAG interface

The JTAG interface on the board provides powerful debugging facilities
like breakpoints, backtraces, dumping memory, inspecting variables,
checking task states, catching exeptions etc. To make this to work
requires additional hardware and software, see
<a href="TiLDA_Debugging_using_JTAG" class="wikilink"
title="TiLDA Debugging using JTAG">TiLDA Debugging using JTAG</a>.

## Buttons

The badge has 8 buttons: Up, Down, Left, Right, Center (on the
joystick), A, B and Light. You can use arduino-style
“digitalRead(BUTTON_RIGHT)” to read the current status of any button,
but you can’t define your own interrupt (because we already did that).
This doesn’t mean you can’t wait for a certain button to be pressed, it
just means you have to approach it slightly differently:

Example: A simple app displaying the button code

`void ButtonApp::task() {`
`    ButtonSubscription allButtons = Tilda::createButtonSubscription(LIGHT | A | B | UP | DOWN | LEFT | RIGHT | CENTER);`

`    while(true) {`
`        Button button = allButtons.waitForPress(1000);`
`        if (button == A) {`
`            debug::log(“You pressed button A”);`
`        } else if (button == LEFT) {`
`            debug::log(“You pressed LEFT”);`
`        } else if (button == NONE) {`
`            debug::log(“No button has been pressed in 1000ms”); `
`        }`
`    }`
`}`

### ButtonSubscription Tilda::createButtonSubscription(<buttons>)

Registers a subscriptions for a defined set of buttons and returns a
ButtonSubscription. Multiple Buttons can be combined via “\|” (see
example above). One button can not be subscribed by more than 10
subscriptions (which shouldn’t really happen, but keep it in mind).

Don’t use this function in a constructor, it requires FreeRTOS to be
running. Using it inside the task() function is the only safe place for
it.

### Button ButtonSubscription::waitForPress(TimeInTicks timeout)

This is normally called in a loop. It causes the task to block until one
of the buttons has been pressed. If the timeout occurs before any button
has been pressed “NONE” will be returned.

### ButtonSubscription::waitForPress()

The same as above, but without the timeout.

### ButtonSubscription::clear()

This should be called after an App has been suspended, just before it’s
going to be resumed. It causes the Queue to be cleared which could
otherwise lead to buttons being reported that have been pressed while
other apps were in the foreground. Have a look at the FlashLightApp for
an example.

## LEDs

### Tilda::setLedColor(Led led, Color color);

### Tilde::setLedColor(Color color);

Sets the color of all or one led. Color is an object that takes red,
green and blue as a value between 0 and 255 each. If no led is defined
both leds will be set to the same color.

Example: A simple color-changing task

`void ColorfulTask::task() {`
`    while(true) {`
`        Tilda::setLedColor(LED1, {255, 0, 0}); // Red`
`        Tilda::setLedColor(LED2, {0, 255, 0}); // Green`
`        Tilda::delay(300);`
`        Tilda::setLedColor(LED1, {0, 255, 0}); // Green`
`        Tilda::setLedColor(LED2, {0, 0, 255}); // Blue`
`        Tilda::delay(300);`
`        Tilda::setLedColor(LED1, {0, 0, 255}); // Blue`
`        Tilda::setLedColor(LED2, {255, 0, 0}); // Red`
`        Tilda::delay(300);`
`    }`
`}`

## Display

The Display Library is based on GLCDv3
(http://playground.arduino.cc/Code/GLCDks0108), docs
(http://code.google.com/p/glcd-arduino/source/browse/trunk/glcd/doc/GLCD_Documentation.pdf)
but adapted to support our screen. The Init routine is called in the
setup, and the LCDTask takes care of ensuring the screen is updated
every 40ms if required (Unlike the original GLCD library, screen updates
are decoupled from the graphics routines.)

Also available is M2tklib (https://code.google.com/p/m2tklib/) which is
a nice toolkit library. Further details on using this will come later,
but expect the main loop to be handled for you, and just passing the
menu structure you require for your app.

Right now, you can call the GLCD functions directly with
GLCD.DrawBitmap() for example. This is going to change to be accessed
through the GUITask class in the near future, to ensure only one task at
a time writes to the screen. Expect this to be simply GUITask in place
of GLCD, along with a registering a redraw call back to GUITask. The
bitmap format, by the way, is rather unconventional but there are a
couple of utility scripts to convert popular formats down in the hacking
section below - you can grab the SponsorsApp.h as an example and swap
out the bitmap array with one of your choosing.

Extra features that are included, GLCD.SetRotation() will handle
rotation of the screen for you, and GLCD.CurrentWidth() and
GLCD.CurrentHeight will give you the correct Width and Height for the
current orientation. GLCD.Width and GLCD.Height constants are not
available, and the GLCD predefined Text areas will not be rotated for
you, if you require this define your own text areas.

Note that one function was not ported to the badge version of GLCD,
"Printf", you'll have to cope without it.

## Sound

There's a fully working Piezo on board!

## IMU

### Tilda::getOrientation

returns " ORIENTATION_HELD", "ORIENTATION_RIGHT" (joystick to the right
of the screen), "ORIENTATION_HUNG" or "ORIENTATION_LEFT"

## Flash Storage

We have 2mb of flash storage, but we're not using it in the main
firmware - Please get this working!

## Data: Schedule

### Tilda::getDataStore().getSchedule(day, location)

## Date: Weather Forecast

### Tilda::getDataStore().getWeatherForecast()

## Radio

There's no way of sending messages in the current version of the
firmware, sorry :(

## Time

### Tilda::delay(uint16_t delayInMs)

Works like Arduino’s delay(), but is FreeRTOS-safe. It’s safe to use
this function before FreeRTOS has started.

### tilda::getClock()

Returns an instance of
<https://github.com/MarkusLange/Arduino-Due-RTC-Library/blob/master/rtc_clock.h>

## Settings

### uint16_t tilda::getBadgeId()

## Battery

### float TiLDA::getBatteryVoltage()

Returns the current voltage as a float

### uint8_t TiLDA::getBatteryPercent()

Returns the current voltage as a percentage

### uint8_t TiLDA::getChargeState()

Returns the charge state

0 Charging 1 Not Charging