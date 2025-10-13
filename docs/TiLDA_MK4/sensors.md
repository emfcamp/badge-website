## Overview

The TiLDA MK4 has four sensors, three sharing an [I2C
bus](https://www.allaboutcircuits.com/technical-articles/introduction-to-the-i2c-bus/)
and one with an analog output:

- Temperature and Humidity: [TI
  HDC2080](http://www.ti.com/product/HDC2080)
- Luminance: [TI OPT3001](http://www.ti.com/product/OPT3001)
- Temperature: [TI TMP102](http://www.ti.com/product/TMP102) - the
  TMP102 has higher precision and extended temperature range with no
  calibration needed compared to HDC2080
- Magnetic: [TI DRV5055](http://www.ti.com/product/DRV5055)

The sensors are physically located on the badge as shown in the picture
below. Since these are environmental sensors, the orientation and
location of the badge will affect the values returned from the sensors.
For example, wearing the badge close to your body will likely raise the
temperature readings.

<img src="Sensor_graphic_-_with_fixed_transparency-01.png"
title="Sensor_graphic_-_with_fixed_transparency-01.png" width="400"
alt="Sensor_graphic_-_with_fixed_transparency-01.png" />
<img src="Sensor_graphic_-_with_fixed_transparency-02.png"
title="Sensor_graphic_-_with_fixed_transparency-02.png" width="400"
alt="Sensor_graphic_-_with_fixed_transparency-02.png" />

In addition, the battery charger circuit (TI BQ25601), powered via USB,
has status available via the I2C bus.

These sensors are managed by the tilda.Sensors module. This module takes
care of initializing the devices and periodically updating readings from
them. This makes it easier to use from applications as all the low-level
interactions and data formatting are available directly via MicroPython
methods. There are more features available in the devices that could be
enabled. Currently this would be done by enhancing the C code in the
ports/ti directory. In the future, the devices might also be directly
programmable via the MicroPython I2C API.

You can see what's provided by the Sensors module using the standard
Python dir() operator:

    >>> from tilda import Sensors
    >>> dir(Sensors)
    ['__class__', '__name__', 'BAT_ADAPTER_24', 'BAT_DONE_CHARGING', 'BAT_FAST_CHARGING', 'BAT_NOT_CHARGING', 'BAT_NO_INPUT', 'BAT_OTG', 'BAT_PRE_CHARGING', 'BAT_USB_HOST', '_raw_bq', 'get_charge_status', 'get_hdc_humidity', 'get_hdc_temperature', 'get_lux', 'get_tmp_temperature', 'get_vbus_connected', 'sample_rate']

Per the usual Python conventions, the all upper-case identifiers are
symbolic constant values. The other identifiers are methods that should
be semi-obviously apparent in what functions they perform.

## Temperature and Humidity

Two methods are provided for the HDC2080. get_hdc_temperature() returns
the current ambient temperature in degrees Centigrade.
get_hdc_humidity() returns the relative humidity as a percentage.

get_tmp_temperature() returns the current ambient temperature in degrees
Centigrade from the TMP102 sensor.

    from tilda import Sensors

    print("HDC temperature: {} C".format(Sensors.get_hdc_temperature()))
    print("TMP temperature: {} C".format(Sensors.get_tmp_temperature()))
    print("HDC humidity: {}%".format(Sensors.get_hdc_humidity()))

## Luminance

The OPT3001 measures light intensity (luminance) scaled to match the way
the human eye perceives light. This is useful for applications
controlling lighting so that changes appear more natural to people.

    from tilda import Sensors
    from time import sleep

    while True:
        print("Light intensity: {} lux".format(Sensors.get_lux()))
        sleep(1)

Place your hand over the OPT3001 sensor and observe the measured
intensity change.

## Magnetic Flux - Hall Effect

The DRV5055 senses magnetic fields (hooray for ElectroMAGNETIC FIELD
Camp!). The device provides an analog output that is sampled by the ADC
on the MCU. The particular device on the badge has a sensitivity of
12.5mV / mT. Note that the earth's magnetic field is in the range of
micro Teslas (uT). Bring a small magnetic close to the sensor and watch
the measurements change.

    from machine import ADC
    from time import sleep_ms

    mag = ADC(ADC.ADC_HALLEFFECT)
    while True:
        print(mag.convert())
        sleep_ms(200)

## Battery

The BQ25601 takes care of charging the LiPo battery when the badge is
attached to a USB power source - PC, car, battery. You can monitor the
status by using the get_charge_status() and get_vbus_connected()
methods. For example, you might want to turn off the display more often
when there is no USB power so that the battery lasts longer.

    from tilda import Sensors
    from machine import Pin

    backlight = Pin(Pin.PWM_LCD_BLIGHT)
    if Sensors.get_vbus_connected() != Sensors.BAT_NO_INPUT:
        backlight.on()

While there is power supplied via USB, you can monitor the charging
status:

    from tilda import Sensors

    charging = Sensors.get_charge_status()
    if charging == Sensors.BAT_PRE_CHARGING or charging == Sensors.BAT_FAST_CHARGING:
        print("Battery is charging")
    elif charging == Sensors.BAT_DONE_CHARGING:
        print("Battery is full")
    elif charging == Sensors.BAT_NOT_CHARGING:
        print("Battery is discharging")

## Sample Rate

You can change how often the sensors are read using the sample_rate()
method. Like other Python APIs, this function is both a "setter" and a
"getter". If you call it with no argument, it returns the current sample
rate in Hz. If you supply a value, the sample rate is changed. Changing
the sample rate can let you see sensor values change more rapidly;
conversely, a slower sample rate will update less often but use less
energy, making the battery last longer.

    from tilda import Sensors

    print("Sensor sample rate: {}".format(Sensors.sample_rate()))
    Sensors.sample_rate(2)