# wifi.json

A simple json document in the root directory of the badge:

    {"ssid":"your-wifi-name","pw":"your-wifi-password"}

If you're connecting to an open wifi you can leave out the "pw" :

    {"ssid":"your-unsecured-wifi-name"}

Most badge apps that use the wifi chip will expect a wifi.json file to
exist. Please make sure it's valid json and that both name and password
are in the correct case.

Please make sure to "eject" the usb storage properly before restarting
the badge.