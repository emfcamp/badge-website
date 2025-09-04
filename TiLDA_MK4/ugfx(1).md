The badge makes use of [uGFX](http://ugfx.io) for providing drawing
functions on the LCD. Most of this functionality is available through
the micropython interface, and you may wish to browse the [uGFX
documentation](https://wiki.ugfx.io/index.php/Main_Page) for more
details.

To discover exactly which ugfx calls are available in micropython, see
<https://github.com/emfcamp/micropython/blob/tilda-master/stmhal/modugfx.c>

## Basic usage

uGFX is comprised of 'widgets,' such as buttons and labels, and
'containers' which are used to group widgets.

To create a button on the screen, use
`ugfx.Button(x,y,width,height,text)`, and a button will be drawn on the
screen.

As well as widgets, there are 'primitives' such as drawing circles and
lines, which can be drawn anywhere on the screen or in a container. For
example `ugfx.circle(50,50,20,ugfx.RED)` will draw a circle.

## Detailed documentation

Note. all co-ordinates are from the top left (battery symbol) corner.
All widgets are created from the "ugfx" package, eg: ugfx.Contailer().

### Colour format

Internally, ugfx uses 565 format (5 bits for red and blue, 6 for green).
Preset colours are available, for example **ugfx.RED**, **ugfx.ORANGE**,
etc. To convert from 24 bit RGB format, use
**ugfx.html_color(0xRRGGBB)** to return the 16 bit 565 format.

### Styles and Fonts

Use `ugfx.set_default_font(...)` to change the font. The font options
are:

- *ugfx.FONT_SMALL*
- *ugfx.FONT_MEDIUM*
- *ugfx.FONT_MEDIUM_BOLD*
- *ugfx.FONT_TITLE*
- *ugfx.FONT_NAME*.

Note that widgets use the font which was default when they were created,
while the *container.text()* primitive uses the font that was default
when the container was created

### Containers

`   .Container(x, y, width, height {,style})  # the style is optional`

Containers can be used to group widgets together. They can also perform
primitive drawing functions. When drawing widgets or primitives, the
coordinates are relative to the top left corner of the container.

Containers can be shown or hidden, and all the widgets will be redrawn
(primitives are not widgets and are not redrawn). Containers can also be
placed on top of other widgets. When the top container is hidden, the
widgets below will be redrawn.

Upon creation a style can be passed to a container, which will then be
used by default by widgets created as part of that container. If no
style is specified at creation, the current default style will be used.

The following example shows how to create a container, add an object and
show it.

    c = ugfx.Container(100,100,200,100)
    b = ugfx.Button(10, 10, 40, 30, "OK", parent=c)
    c.show()            # the container will not be shown until this point
    c.fill_circle(5,5,5,ugfx.RED) #draws a small red circle. Note this has to be done after .show()

### Primitives

All primitives can be drawn anywhere on the screen with, for example
`ugfx.circle(..)`, or anywhere within a container, with
`c=ugfx.Container(30,30,100,100); c.circle(..)`

Note: The container must be shown *before* you can draw primitives in
it. When hiding the container, all drawings are lost!

#### Clear

`.clear(colour = ugfx.WHITE)`

clears the screen to the specified colour. Warning, this is slow!

#### Lines

`.line(`<x1>`, `<y1>`, `<x2>`, `<y2>`, `<colour>`)`

`.thickline(`<x1>`, `<y1>`, `<x2>`, `<y2>`, `<colour>`, `<width>`, `<round>`)`

Draws a line from *x1,y1* to *x2,y2* using *colour*. Thickline will draw
a line or arbitrary width, with the option of rounded corners

eg. *ugfx.thickline(0,0,100,170,ugfx.YELLOW,7,0)*

#### Circle

`.circle(x, y, radius, colour)`

`.fill_circle(x, y, radius, colour)`

Draws a circle at *x,y* of <radius> using *colour*, either with a 1
pixel border or filling the area.

eg. *ugfx.circle(180,150,40,ugfx.RED)*

#### Arc

`.arc(x, y, r, angle1, angle2, colour)`

`.fill_arc(x, y, r, angle1, angle2, colour)`

Similar to the circle functions, however two angle parameters specify
between which two angles drawing occurs

#### Ellipse

`.ellipse(x, y, a, b, colour)`

`.fill_ellipse(x, y, a, b, colour)`

Draws an ellipse at *x,y* of *a* width and *b* height using *colour*,
either with a 1 pixel border or filling the area.

#### Rectangles

`.box(1, y, a, b, colour)`

`.area(x, y, a, b, colour)`

Draws a rectangle or filled rectangle at *x,y* of *a* width and *b*
height using *colour*.

#### Polygon

`.polygon(x, y, array, colour)`

`.fill_polygon(x, y, array, colour)`

Draws or fills a polygon starting at *x,y* using *colour*. *Array* is an
array of coordinates that specifies the corners.

eg. *ugfx.polygon(0,0, \[ \[0,20\],\[20,20\],\[20,0\]\], ugfx.RED)*

#### Text

`.text(x, y, text, colour)`

Draws a text string *text* at *x,y* in *colour*. Note that a
ugfx.text(..) call will take the default font, while container.text(..)
will take the containers font.

eg. *ugfx.text(40,40,"My name is...",ugfx.BLUE)*

#### Other

`.width()`

`.height()`

Gets the height or width of the screen *ugfx.width()* or container
object *c.width()*

### Widgets

Widgets can be drawn anywhere on the screen, or within a container. The
widgets take the optional parameter *parent=* to set the parent
container. Widgets can have their style set on creation, otherwise will
inherit

Widgets also accept input from the buttons. For example, the 'A' button
can be 'attached' to an on-screen button, such that pressing the button
on the badge causes the on-screen button to be redrawn in a depressed
state.

#### Common Widget methods

`.text([text])`

Gets or sets the text displayed by the widget.

`.visible([show])`

Gets or sets the visibility of the badge. *b.visible(0)* will hide, and
*b.visible(1)* will show.

`.attach_input(button, function)`

Attaches a physical button to a widget, so that the user can cause the
widget to redraw, for example to scroll or become depressed.

The input *button* specifies which button, with the options ugfx.BTN_A,
ugfx.BTN_B, ugfx.BTN_MENU, ugfx.JOY_UP, ugfx.JOY_DOWN, ugfx.JOY_LEFT,
ugfx.JOY_RIGHT.

The input *function* specifies what the button actually does. For
example, the list has three different functions: scroll up, scroll down,
and select. Note that some widgets by default attach the joystick to the
relevant functions.

`.detach_input(function)`

Detaches an input. See above for more details.

`.destroy()`

Frees up all the resources assoicated with the object. While the
micropython garbage collector will clear any old objects, the graphics
library also has its own memory area, which can become full if objects
are not destroyed after they are needed.

`.set_focus()`

Gives focus to the widget instance. Normally this will draw a box around
the widget, the colour is specified by the style.

#### Button

`b=ugfx.Button(x, y, w, h, text, *, parent=None, trigger=None, shape=ugfx.Button.RECT, style=None)`
(note: parameters after '\*' are optional)

Draws a button at *x,y* having width *a* and height *b*. The option
'trigger' specifics which physical switch (if any) causes the display to
be redrawn. The shape options are *ugfx.Button.RECT*,
*ugfx.Button.ROUNDED*, *ugfx.Button.ELLIPSE*, *ugfx.Button.ARROW_UP*,
*ugfx.Button.ARROW_DOWN*,
*ugfx.Button.ARROW_LEFT',*ugfx.Button.ARROW_RIGHT''.

#### Textbox

`ugfx.Textbox(x, y, w, h, *, text=None, parent=None, maxlen=255})`

Draws a text edit-box which can take input from the on-screen keyboard.
Will automatically accept key-presses from the keyboard, which will edit
the text. The textbox needs to have focus using .set_focus() for it to
receive the key-presses.

#### Checkbox

`.Checkbox((x, y, w, h, text=None, parent=None, trigger=None, style=None)`

`.checked()`

Get or set the checked state

#### Label

`ugfx.Label(x, y, w, h, text, *, parent=None, style=None, justification=None)`

A label displays text. Unlike the primitive text, this Label supports
different justifications, wordwrap and changing text. The different
justification options are *ugfx.Label.LEFT*, *ugfx.Label.RIGHT*,
*ugfx.Label.CENTER*, *ugfx.Label.LEFTTOP*, *ugfx.Label.RIGHTTOP* and
*ugfx.Label.CENTERTOP*.

#### List

`ugfx.List(x, y, w, h, *, parent=None, up=ugfx.JOY_UP, down=ugfx.JOY_DOWN, style=None)`

`.enable_draw()`

`.disable_draw()`

`.add_item(text)`

`.assign_image(index, image)`

`.remove_item(index)`

`.selected_text()`

`.selected_index()`

`.count()`

#### Graph

`ugfx.Graph(x, y, w, h, origin_x, origin_y)`

`.show()`

`.hide()`

`.set_arrows(ugfx.Graph.ARROWS_X_POS | ugfx.Graph.ARROWS_Y_POS)`

Set which axes have arrows and in which direction. In this example
arrows are set for the x and y positive directions, with the following
options available: ARROWS_X_POS, ARROWS_Y_POS, ARROWS_X_NEG,
ARROWS_Y_NEG

`.plot( point(s)_x, point(s)_y, {new_series} )`

Plot either an array, or a point. Optional second parameter specifies
whether to start a new series or join onto previous.

`.appearance( thing_to_change, shape, size, colour, {spacing} )`

Changes the style of either points, lines, axis or grids. Each parameter
is used as follows:

'thing_to_change' - select what aspect to change, the options are
{STYLE_POINT, STYLE_LINE, STYLE_XAXIS, STYLE_YAXIS, STYLE_XGRID,
STYLE_YGRID}

'shape' - selects the shape of the thing to change, where the options
depend on what you are changing. The options for points is: {POINT_NONE,
POINT_DOT, POINT_SQUARE, POINT_CIRCLE} The options for line/axis/grid
are: {LINE_NONE, LINE_SOLID, LINE_DOT, LINE_DASH}

'size' - number of pixels wide of the thing to change

'colour' - use UGFX.Red, UGFX.Orange,... or UGFX.html_color(0xRRGGBB) to
ensure the right format

'spacing' - only valid for grid

See the [BARMS logger
app](https://github.com/emfcamp/Mk3-Firmware/blob/master/apps/logger/main.py)
for an example

#### Imagebox

`.Imagebox(x, y, w, h, filename, *, cache=0, parent=None, style=None)`

This displays an image box. This will automatically animate a passed gif

#### Keyboard

`.Keyboard(x, y, w, h, parent=None)`

### Styles

    s=ugfx.Style()  # create a style based on the current default style

    s.set_enabled([text_colour, edge_colour, fill_colour, progress_colour]) # sets the style for when something is enabled
    s.set_pressed([text_colour, edge_colour, fill_colour, progress_colour]) # sets the style for when something is pressed
    s.set_disabled([text_colour, edge_colour, fill_colour, progress_colour]) # sets the style for when something is disabled

    s.set_focus(colour)  # sets the colour used for focus
    s.set_background(colour) # sets the background colour for the style

    ugfx.set_default_style(s) # use this style from now on

    b=ugfx.Button(0,0,40,30,"OK", style=s) # use the style for this button

## Tips and tricks

### Tearing

When writing large areas of the screen, a 'tearing'
[1](https://en.wikipedia.org/wiki/Screen_tearing) effect may be
observed.

The screen module is comprised of a large memory, with one memory
location to store the RGB data for each pixel. The LCD driver
continuously updates the LCD pixels, by reading the memory in a
sequential, line-by-line manner, and updating the LCD with the data from
the memory. This 'read line-pointer' moves from the top to the bottom of
the screen (when viewed in portrait), at about 70Hz (the refresh rate of
the screen)

This large memory as part of the screen means it can be driven by a
microcontroller which may have a considerably smaller memory. The
microcontroller therefore only needs to update the memory when it whats
the content to change.

Consider the scenario where the microcontroller wants to set the screen
from one colour to another. The microcontroller needs to update the
entire memory (320x240x2 = 153kB) with the new colour. At the same time
the 'read line-pointer' is reading the same memory to update the LCD. In
this case, tearing occurs if the 'read line-pointer' reads the top half
of the memory containing the new colour, but then catches up with
microcontroller writing to the memory, then the 'read line-pointer'
starts reading the old colour in the bottom half of the memory.

To avoid tearing the 'read line-pointer' should not cross the region the
microcontroller is updating. Since the microcontroller writes to the
screen slightly slower than the LCD reads it, providing the
microntroller starts writes to the top of the memory just after the LCD
starts reading from the top, the read and write pointers will not
overlap, and tearing will not occur. To sync the microcontroller with
the LCD 'read line-pointer,' there is a vsync/tear output (connected to
pin named 'TEAR') which is pulled high when the 'read line-pointer'
reaches a given line (default is line 0). This can be turned on and off
with **ugfx.enable_tear()** and **ugfx.disable_tear()**. To change the
line at which the tear output is generated, use
**ugfx.set_tear_line(0..319)**.

Example code:

`   ugfx.enable_tear()`
`   tear = pyb.Pin("TEAR", pyb.Pin.IN)`
`   `
`   def vsync():`
`       while tear.value() == 0:`
`           pass`
`       while tear.value():`
`           pass`

### Reducing power consumption

Use the following to dim the backlight, which uses about 80mA at full
brightness

    ugfx.backlight(b)     # sets the backlight. Range is 0-100
    b = ugfx.backlight()   # reads the current backlight

## Top-Level calls

`.power_mode(`<mode>`)`

*mode* can be any of: ugfx.POWER_ON, ugfx.POWER_OFF,
ugfx.POWER_DEEP_SLEEP, ugfx.POWER_SLEEP

`.orientation(deg)`

Rotate the display by number of degrees, eg 180 for upside-down.

Calling with no args returns the current value

`.display_image(x, y, '/path/to/image', orientation)`
`.display_image(x, y, img_obj, orientation)`

Display an image from a file of a supported type, or a previously opened
image object, on screen at the x,y co-ordinates. Optionally change its
orientation.

`.write_command(0x35,0)`

Low-level command to write to a data register directly.

`.poll()`

Yield to ensure the screen is redrawn when waiting on user input

`.ball_demo()`

Ball Demo!

`.get_pixel(x, y)`

Returns the colour of the pixel at x,y

`.Image(filename, cacheme)`

Creates and returns (or loads from cache) a ugfx image object from the
image in the filename.