See here
[1](https://docs.micropython.org/en/latest/pyboard/library/pyb.SPI.html)
for the main micropython documentation.

SPI1 is used for the board's onboard wifi.

SPI2 is exposed on the 'X' header, under the white silkscreen section
for your name.

    from pyb import SPI

    spi = SPI(2) #create SPI on port 2
    spi.init(SPI.MASTER) #set the mode. Other things like baudrate=80000, polarity=1, phase=0, crc=None can be set here

    spi.send(170) #send int byte
    spi.send(b'\xAA') #send hex byte