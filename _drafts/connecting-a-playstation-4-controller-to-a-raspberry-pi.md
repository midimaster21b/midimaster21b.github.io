---
layout: post
author: joshua
categories: [tutorials]
tags: [raspberry pi, python, PS4 controller, ds4drv, linux]
excerpt_separator: <!--more-->
comments: true
---

In the last few weeks I needed to connect a PS4 controller to a Raspberry Pi for a project. In this specific case I was connecting a PS4 controller to a Raspberry Pi 3 running [Raspbian Stretch Lite](http://downloads.raspberrypi.org/raspbian_lite/images/raspbian_lite-2018-10-11/2018-10-09-raspbian-stretch-lite.zip). While I have not tested other systems, the below steps should work for any Debian-based flavors of linux(Raspbian, Ubuntu, etc.) installed on any version of the Raspberry Pi with bluetooth. <!--more-->Please let me know if this tutorial works for you or if you have any issues or questions regarding this tutorial in the comments below!

## Setup the Raspberry Pi

1. Connect to your Raspberry Pi using ssh or open up a terminal window before proceeding.

1. Make sure the package manager is up to date on your Raspberry Pi.

    ```bash
    sudo apt-get update
    sudo apt-get upgrade
    ```

1. Install pip, a python package management system.

    ```bash
    sudo apt-get install python-pip
    ```

1. Install the dualshock 4 driver (ds4drv) using pip.

    ```bash
    sudo pip install ds4drv
    ```

1. Create the ds4drv configuration file using your favorite editor.

    ```bash
    sudo vi /etc/ds4drv.conf
    ```

    ```
    # Many of the settings used here are directly connected to their command line
    # counterparts, see "ds4drv --help" for more information about available options.

    ##
    # Global options
    ##
    [ds4drv]
    # Run ds4drv in background as a daemon
    daemon = true

    # Location of the log file in daemon mode
    daemon-log = ~/.cache/ds4drv.log

    # Location of the PID file in daemon mode
    daemon-pid = /tmp/ds4drv.pid

    # Enable hidraw mode
    #hidraw = true


    ##
    # Controller settings
    #
    # This is the default profile for each controller.
    # Multiple controllers slots are defined by increasing the number.
    #
    # Controller sections contain:
    #  Key: A option, these are the same options that can used on the command line
    #       but without the "--" prefix.
    #  Value: The option's value, should be "true" if no value is needed.
    #
    # See "ds4drv --help" for a complete list of available options.
    ##
    [controller:1]
    # Enables LED flash on low battery
    #battery-flash = true

    # Sets LED color
    #led = 0000ff

    # Enables profile switching
    #profile-toggle = PS

    # Profiles to cycle through
    #profiles = xpad,kbmouse


    ##
    # Profiles
    #
    # Profiles allows switching controller settings during runtime.
    #
    # Profile sections always require a name and are then enabled on a controller
    # with "profiles = <profile1>[,<profile2>]".
    #
    # The same settings available for controllers are used here.
    ##
    [profile:xpad]
    led = ff0000
    # Emulate the same button mapping as wired Xbox 360 controllers
    emulate-xpad = true

    [profile:kbmouse]
    led = 00ff00
    # Enable trackpad mouse
    trackpad-mouse = true
    # Custom button mapping
    mapping = keyboard
    # Custom action bindings
    bindings = exec_stuff


    ##
    # Mappings
    #
    # Mappings let you map buttons and sticks to mouse, key and joystick events.
    #
    # Mapping sections always require a name and are then enabled in a profile
    # with "mapping = <name>".
    #
    # Mapping sections contain:
    #  Key: A Linux input event, see /usr/include/linux/input-event-codes.h for a complete list
    #  Value: Button on the DS4, use --dump-reports to see all the available buttons
    ##

    [mapping:keyboard]
    # General button to key mapping
    KEY_UP = dpad_up
    KEY_LEFT = dpad_left
    KEY_DOWN = dpad_down
    KEY_RIGHT = dpad_right
    KEY_Z = button_cross
    KEY_X = button_circle

    # Turn analog stick directions into buttons
    KEY_W = -left_analog_y
    KEY_A = -left_analog_x
    KEY_S = +left_analog_y
    KEY_D = +left_analog_x

    # Map relative mouse movement to a analog stick
    REL_X = right_analog_x
    REL_Y = right_analog_y

    # Map mouse buttons
    BTN_LEFT = button_r2
    BTN_RIGHT = button_l2

    # Emulate mouse wheel on r1 and l1
    REL_WHEELUP = button_l1
    REL_WHEELDOWN = button_r1

    # Mouse settings
    #mouse_sensitivity = 0.6
    #mouse_deadzone = 5

    # Scroll wheel emulation settings (values are in seconds)
    #mouse_scroll_repeat_delay = 0.25 # How long to wait before continual scrolling
    #mouse_scroll_delay = 0.05 # Lower this to scroll faster; raise to scroll slower


    ##
    # Bindings
    #
    # Bindings let you bind button combos to special built-in actions.
    #
    # Binding sections can be defined with a name and are then enabled in a profile
    # with "bindings = <name>".
    #
    # It's also possible to define a global bindings section that is enabled
    # on all profiles.
    #
    # Sections contains:
    #  Key: A button combo
    #  Value: An action, see next section for valid actions.
    #
    #
    # Valid actions:
    #  next-profile                                  Loads the next profile
    #  prev-profile                                  Loads the previous profile
    #  load-profile <profile>                        Loads the specified profile
    #  exec <command> [arg1] [arg2] ...              Executes the command with
    #                                                specified arguments
    #  exec-background <command> [arg1] [arg2] ...   Same as exec but launches in
    #                                                the background
    #
    #
    # Actions will be pre-processed and replace variables with real values.
    #
    # Valid variables:
    #  $profile                The current profile
    #  $name                   Pretty name of the current device
    #  $device_addr            Bluetooth address of the device
    #  $report.<attribute>     Replace <attribute> with a valid attribute,
    #                          use --dump-reports to see which are available
    ##

    [bindings]
    # Cycle profiles
    #PS+Right = next-profile
    #PS+Left = prev-profile

    # Go directly to specified profile
    #PS+Up = load-profile kbmouse
    #PS+Down = load-profile default


    [bindings:exec_stuff]
    # Execute a command in the foreground, blocking until it has finished
    PS+Cross = exec echo '$name'

    # Execute a command in the background
    PS+Triangle = exec-background sh -c 'echo "disconnect $device_addr" | bluetoothctl'
    ```

## Connecting the PS4 Controller

1. Make sure the ds4drv driver program is running.

    ```bash
    sudo ds4drv
    ```

1. Hold the Playstation button and the share button simultaneously on the PS4 controller until the top of the PS4 controller starts blinking white (usually around 5 seconds).

## Prove that the PS4 controller is connected

1. Run the python program below to print the values that the contoller is sending

- TODO: INCLUDE SAMPLE PROGRAM!!!
