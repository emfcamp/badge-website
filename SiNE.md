<figure>
<img src="800px-SiNE_Front_Angle.JPG"
title="800px-SiNE_Front_Angle.JPG" />
<figcaption>800px-SiNE_Front_Angle.JPG</figcaption>
</figure>

## About SiNE

SiNE: Investigating the Neighbourhood of EMW
Each attendee of Electromagnetic Wave receives a SiNE badge which has
two purposes; firstly it allows you to take part in a treasure hunt
based around the boat. By solving the clues you will be directed to an
object or location either on or near the Stubnitz. The clue letter is
also the first letter of the answer - when you arrive at the correct
location you will find a large matching letter. Hold your badge in front
of the letter, and the corresponding light on your badge will
illuminate. The first person to solve all the clues and bring their
fully illuminated badge to the ticket desk wins two tickets to
Electromagnetic Field 2014. Anyone else solving the puzzle will be
allowed to choose from a selection of prizes for as long as they last!

The second purpose of the badge is as a locator beacon - as people walk
around the boat they will create a trail of the places they visit and
the talks they attend. We hope to use this information to better
schedule our future events. The data will be made available publicly
after the event, and we'll provide a way of finding their badge
identifier for those who want to get their own trail.

### Scavenger hunt

As you locate the beacons hidden around EMWave more of your LED's will
light up. To save power the LED do not stay on all the time and the
beacons you have seen are flash in groups every few seconds.
The beacons you have seen is store in the EEPROM and so the data is
preserved even if you remove the power

#### Clue List

Now EMWave has past here is the clue list: (Will add answers soon
--<a href="User:Dpslwk" class="wikilink" title="Dpslwk">Dpslwk</a>
(<a href="User_talk:Dpslwk" class="wikilink" title="talk">talk</a>)
14:21, 6 May 2013 (UTC))

- A: I can't believe it's not butter!
- B: Bond would be comfortable here and he has a licence.
- C: One of your EMW badge designers.
- D: One of your EMW badge designers.
- E: Powered by Orange.
- F: ____ and aft.
- G: False patch of nature.
- H: All _____ on deck!
- I: Initial illumination, informing incomers.
- J: Java's main export.
- K: A nautical speed, mooring the Stubnitz.
- L: Budding safecrackers.
- M: An essential orienteering tool.
- N: A German Venus? Not exactly Botticelli!
- O: Fun times in the Emergency Room!
- P: A most unusual Landrover.
- Q: Encoded in a massive square.
- R: A lifesaver in Germany.
- S: Quite possibly the most unusual holiday home.
- T: One of your EMW badge designers.

### Locator ID

![](800px-SiNE_Front_Buttons.jpeg "800px-SiNE_Front_Buttons.jpeg") Each
badge is programmed with a unique ID, that is transmitted about 5 times
a second. There will be Raspberry Pi's doted around the ship, collecting
data.
EMF will not know your badge ID, as there are handed out at random.
A badge's ID is shown on the LED using binary encoding. This is done at
power up, or by pressing and holding the "ID" button
If you **do not** wish to have your badge trasnmitting an ID you can
clear it by holding the "Erase" button for about 5 seconds.

The badge ID is 9 bit's long, these are displayed across the A-I LED's,
a lit LED is 1 and unlit is 0, LSB first.
Example if LED's A D E and H are lit then we have 010011001 in binary or
0x099 in HEX or 153 in decimal.

Unfortunately due to a lack of time the Raspberry Pi's were never setup,
as so no data was collected.

<div style="clear: both">

</div>

## Sponsors

![](800px-SiNE_Back_Angle.JPG "800px-SiNE_Back_Angle.JPG") SiNE was only
possible thanks to our sponsors:

- [Twilio](http://www.twilio.com) A Cloud communications company
  sponsored the parts need to make badges.
- [Ciseco](http://ciseco.co.uk) A Nottingham based electronics company
  that make low power wireless radios, donated the time to build the
  badges on there SMT assemble line.

<div style="clear: both">

</div>

### DevBoards

![](DevBoards.JPG "DevBoards.JPG") Before designing the final SiNE badge
we built three development boards, not bothered about the looks, they
were built to test the hardware and wiring between the parts.
Once it was confirmed that the parts worked together they were passed
onto the software developer to start work on the code need for the
scavenger hunt and location tracking.
Using these development boards meant the software was ready to go around
the same time as the final badges were produced.

<div style="clear: both">

</div>

## Badge Hacking

![](800px-SiNE_Schematic.png "800px-SiNE_Schematic.png") We actively
encourage users to hack there badge better and hope the information
provided below will help

### Flashing and Fuses

We use avrdude and an ISP programer to flash the ATTiny44A Example
command lines can be found in the Makefile in the SiNE-Firmware github

avrdude part flag (-p) is t44

Fuses:

- Low: 0xC2
- High 0xD7
- Extended: 0xFF

### Components

The following parts were used: (data-sheet links to come)

- ATTiny44A - MCU running all the code
- 74HCT164AD - Shift register connecting the matrix of LED's
- LED's - 20 0805 LED's for each of the hidden locations
- IR Receiver - listening for the unique ID of each of the hidden
  beacons
- IR Sender - sending out the badges ID for Location tracking
- 2032 Coin Cell - Power for the day and possible more
- ISP header - Used for programming of the ATTiny44A

### Resources

- [PCB](https://github.com/EMF-TiLDA/SiNE-PCB)
- [Firmware](https://github.com/EMF-TiLDA/SiNE-Firmware)

<a href="Category:Badges" class="wikilink"
title="Category:Badges">Category:Badges</a>