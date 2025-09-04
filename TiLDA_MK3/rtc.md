The following code snippets demonstrate the RTC. See the micropython
docs
[1](https://docs.micropython.org/en/latest/pyboard/library/pyb.RTC.html)
for more information.

    # Create a RTC object
    rtc = pyb.RTC()

    # If the RTC isn't running, call
    rtc.init()

    # Set the time
    rtc.datetime((2016, 5, 1, 4, 13, 0, 0, 0))

    # Get the time
    print(rtc.datetime())