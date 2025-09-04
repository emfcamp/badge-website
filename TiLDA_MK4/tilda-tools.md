*tilda_tools* is a toolchain for working with the micropython
environment on the badge.

## Dependencies

- Python 3
- pyserial
- dfu-util (only needed for reflashing firmware)

## Installation

- Clone the Mk4-Apps repo from <https://github.com/emfcamp/Mk4-Apps/>
  (see [Cloning a
  repository](https://help.github.com/articles/cloning-a-repository/)
  for help). Alternatively you can download a ZIP file from the
  repository page.

<!-- -->

- Open a terminal in the Mk4-Apps/ directory. *tilda_tools* is in the
  root of this directory.

<!-- -->

- Windows Users - you will need to install Pyserial to get some commands
  working - Instructions
  [here](https://badge.emfcamp.org/wiki/TiLDA_MK4/Get_Started)

## tilda-tools

    tilda_tools <options>

Windows users, create/update a tilda_tools.bat windows batch file,
containing the following:

    @echo off
    python %CD%/.development\tilda_tools.py %*

This batch file massively simplifies previous versions, but some things
may still not work, depending upon whether you can get the driver to
work.

    Parameters
    -----------------

    -d --device  : serial interface (default: auto)
    -s --storage : path to flash storage

    Usage
    ------------------------------------

    Reboot badge
    $ tilda_tools reset

    Soft reboot badge and start specific app
    $ tilda_tools reset --boot my_app

    Update files on the badge to match the current local version, restarts afterwards
    $ tilda_tools sync

    Notes for tilda_tools sync:

    1. You will probably want to delete the apps you don't want before doing this.  There are a lot of apps now and they will take up valuable file space and may well use it all up, crashing the badge.
    2. You may want to delete bootstrap.py otherwise, when you reset the badge, it will ask you to connect to wifi and then it will download from the server. i.e. it's like doing a factory reset.
    3. Consider syncing a single app

    Update files in folder(s) to match current local version
    $ tilda_tools sync my_game shared
    $ tilda_tools sync <pattern1> <pattern2> ...

    Sync (as above), but execute my_app after reboot
    $ tilda_tools sync --boot my_app [<other sync parameter>]

    Sync (as above), but execute a single file afterwards without copying it to the badge
    $ tilda_tools sync --run some_other_file.py

    Sync a given app and execute it
    $ tilda_tools app home_default

    Executes a single file on the badge without copying anything (Using pyboard.py)
    $ tilda_tools run my_app/main.py

    Runs local validation (doesn't require a badge, but doesn't run unit tests)
    $ tilda_tools validate

    Runs local validation and badge-side tests
    $ tilda_tools test

    Update firmware on badge (warning, this will delete all settings etc. stored on the badge!)
    $ tilda_tools firmware-update

    Setup wifi.json to be copied to the badge on every sync
    $ tilda_tools wifi