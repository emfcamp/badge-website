## Through the REPL

- Todo: Please write how to do that on different operating systems
- todo: how to use a REPL

**Important**: once connected to the serial port, you'll only see output
the badge sends (usually none). Hit Ctrl+C to exit from the current
process and get back the REPL.

### Windows/WSL

#### Identify serial device

The serial device should be a COM port that only shows when you plug the
badge in, e.g. COM4:

1.  Open Computer Management (compmgmt.msc)
2.  Navigate to Device Manager
3.  Expand Ports (COM & LPT)

Alternatively in PowerShell:

`Get-WMIObject Win32_SerialPort | select DeviceID,Description`

#### Connect to identified device

Open PuTTY, enter the COM port found above, e.g. COM4, select Serial,
and click Open.

Alternatively in WSL:

`sudo screen /dev/ttyS{n}`

where {n} is the numeric part of the COM port (e.g. /dev/ttyS4 for
COM4). Use Ctrl+A, D to disconnect.

## Use the "mass storage" app

- Only works on OSX at present (2nd Sept 2018)
- easy to do
- make sure to safely eject every time
- has a tendency to corrupt the filesystem - link to reset instructions
- hard to debug errors
- todo: add more information

## Use Mk4-Apps tilda_tools

- Probably most convenient, but a bit more involved:
- You can use "./tilda_tools run \<file.py\>" to run code without
  copying anything, but if you want to make sure you have all your
  libraries updated use "./tilda_tools app my_app" instead.
- <https://badge.emfcamp.org/wiki/TiLDA_MK4/tilda-tools>