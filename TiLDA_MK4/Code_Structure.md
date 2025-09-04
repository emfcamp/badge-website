### Headers

main.py headers:

    """ <your description>
    """
    ___title___         = "<your_app_name>"
    ___license___      = "MIT"
    ___dependencies___ = ["wifi", "http", "ugfx_helper", "sleep"]
    ___categories___   = ["<see below>"]
    ___bootstrapped___ = True # Whether or not apps get downloaded on first install. Defaults to "False", mostly likely you won't have to use this at all.

Please use one of these categories:

- System
- Homescreens
- Games
- Sound
- EMF
- Villages
- Phone
- LEDs
- Sensors
- Demo
- Other

### Exiting main.py

Ensure your apps calls app.restart_to_default() when finished

    from app import restart_to_default # import at the beginning of your code


    restart_to_default() # call on exit of main.py