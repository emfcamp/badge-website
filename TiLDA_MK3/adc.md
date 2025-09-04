See here
[1](https://docs.micropython.org/en/latest/pyboard/library/pyb.ADC.html)
for the main micropython documentation.

All ADC readings are referenced to the supply rail, which is about 3.3V.
To increase the ADC accuracy, the internal voltage reference can be
read, and used to offset variations in the supply rail.

    adcin = pyb.ADC(channel_to_read).read()
    ref_reading = pyb.ADC(0).read()
    supply_voltage = 4095/ref_reading*1.21  # or change 1.21 with the calibrated value - see below
    adc_voltage =  adcin / 4095 * supply_voltage

Note, the factory reads the internal reference with a supply voltage of
3.0V, and stores this reading. This can then be used to get the actual
voltage reference. For example

    import stm
    factory_reading = stm.mem16[0x1FFF75AA]
    reference_voltage = factory_reading/4095*3   # will be approximately 1.21V

For convenience, the library \`onboard' provides the following functions
for reading the battery voltage, unregulated voltage, or light level

    import onboard
    onboard.get_battery_voltage()
    onboard.get_unreg_voltage()
    onboard.get_light()