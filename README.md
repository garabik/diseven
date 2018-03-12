## Rationale

Touchpad registering events while typing on a keyboard can be really annoying.
Fortunately, many input drivers (mot prominently synaptics, but also libinput)
offer the possibility to disable the touchpad while typing on the keyboard.

Unfortunately, not *all* the devices offer this possibility.
If `xinput list-props <device>` does not display the property, you are out of luck.

This little software helps with the situation - providing a process that
monitors /dev/input/ and stops specified input device if it registered a
keyboard event, and releases it if a specified time elapsed since the last
keystroke (this does not apply to modifier keys such as Ctrl or Alt).

This is more or less just a stopgap measure, until the drivers are
updated/fixed, but it works reasonably well.

## Installation

Make /dev/input/ readable by you - either by changing the access rights to the devices, or, in modern distributions, make yourself a member of `input` group.

Put `diseven.conf` into /etc, edit it according to your needs.

Put the path to the keyboard device into the `devices` configuration. Either a specific path, or a glob pattern. `/dev/input/by-id/*kbd` is mostly good generic choice.

`enable_command` and `disable_command` are crucial, these are commands that
enable or disable the device (touchpad). For libinput,  
`'xinput set-prop "pointer:deviceid" "Device Enabled" X'` where X=0 or X=1 should be sufficient.

You get the `deviceid` from the `xinput list` command. Do not use the number, it might change upon reboot.

Run the diseven program from the terminal first to see eventual error messages.
 

