## Software

- Interpreter for high level functionality
- microPython / eLUA - run 'out of the box' on several microcontrollers.
  Would just require HAL c code for access to peripherals
  - Would allow users to run their own code on the badge without any
    additional software on their PCs. micropython appears as mass
    storage or via a terminal on USB
- have SD card support for additional user storage

## User Interface (Screen Options)

- 'low res' black & white LCD (as 2014)
  - can be seen during the day without a backlight
  - backlight needed at night (~10mA)
  - good outside contrast
- 'high res' colour TFT LCD
  - higher resolution and colour adds to experience
  - backlight needed all the time (can be dimmed in low light)
  - contrast is poor in daylight
  - cost about the same as the mono LCD
  - 'transflective' modules with improved daylight contrast available,
    but likely increased cost

## Wireless Network

- Downloading camp info
- Can be used to obtain badge location based on RSSI
- send messages to friends
- ask for friends location
- sensor network
  - combine with location to map temperatures and so on over the weekend
- potential parts - MRF89XA (3mA rx current, 868MHz, 10mW)

## Other peripherals

- IR for sharing between badges
  - can share python/LUA scripts, contact details
  - used to 'friend people' to share messages via the radio
- accelerometer to go into low power mode when put down
  - eg MMA8652 (40p)
- buzzer/speaker for notifications (and general use)

## Power supply

- ideally would last the weekend (60 hours)
- single AA
  - standard alkaline - 2.5Whr (~15mA average at 3.3V)
  - lithium AA - 4.5Whr (~25mA average at 3.3V)
- lipo
  - 2014 lipo ~4Whr
  - requires charging circuitry
  - ideally wants a hard back to prevent damage
  - increased cost

## Hackability

- conductive thread pads next to lanyard
  - supply little bags of LEDs and thread for say £2
- extra GPIO broken out
  - ardiuno footprint?
- python/LUA would allow for beginners to hack the badge