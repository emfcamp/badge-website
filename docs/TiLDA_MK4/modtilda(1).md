todo: spec this out

    >>> import tilda
    >>> dir(tilda)
    ['__class__', '__name__', 'Buttons', 'LED', 'Sensors', 'main', 'storage_disable_usb', 'storage_enable_usb']
    >>> dir(tilda.Buttons)
    ['__class__', '__name__', 'BTN_0', 'BTN_1', 'BTN_2', 'BTN_3', 'BTN_4', 'BTN_5', 'BTN_6', 'BTN_7', 'BTN_8', 'BTN_9', 'BTN_A', 'BTN_B', 'BTN_Call', 'BTN_End', 'BTN_Hash', 'BTN_Menu', 'BTN_Star', 'JOY_Center', 'JOY_Down', 'JOY_Left', 'JOY_Right', 'JOY_Up', 'disable_all_interrupt', 'disable_interrupt', 'disable_menu_reset', 'enable_interrupt', 'enable_menu_reset', 'get_all_states', 'has_interrupt', 'is_pressed', 'is_triggered']
    >>> dir(tilda.LED)
    ['__class__', '__name__', 'GREEN', 'RED', 'off', 'on', 'toggle']
    >>> dir(tilda.Sensors)
    ['__class__', '__name__', 'BAT_ADAPTER_24', 'BAT_DONE_CHARGING', 'BAT_FAST_CHARGING', 'BAT_NOT_CHARGING', 'BAT_NO_INPUT', 'BAT_OTG', 'BAT_PRE_CHARGING', 'BAT_USB_HOST', '_raw_bq', 'get_charge_status', 'get_hdc_humidity', 'get_hdc_temperature', 'get_lux', 'get_tmp_temperature', 'get_vbus_connected', 'sample_rate']