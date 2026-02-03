# Synaptics Touchpad Fix for ThinkPad on Ubuntu 24.04
A configuration guide to fix Synaptics touchpad lag/stuttering issues on laptops running Ubuntu 24.04 LTS.

## Edit GRUB kernel parameters
```
sudo vim /etc/default/grub
```
Replace `GRUB_CMDLINE_LINUX_DEFAULT` line with:
```
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash i8042.nomux=1 i8042.reset=1 i8042.nopnp=1 i8042.noloop=1 psmouse.synaptics_intertouch=1"
```
Update GRUB
```
sudo update-grub
```

## Create and edit synaptics config for X11
Create file
```
sudo nano /etc/X11/xorg.conf.d/70-synaptics.conf
```
Paste this
```
Section "InputClass"
    Identifier "touchpad"
    Driver "synaptics"
    MatchIsTouchpad "on"
    MatchDevicePath "/dev/input/event*"
    Option "TapButton1" "1"
    Option "TapButton2" "2"
    Option "VertEdgeScroll" "1"
    Option "VertTwoFingerScroll" "1"
    Option "HorizTwoFingerScroll" "1"
    Option "SHMConfig" "on"
EndSection
```
Install synaptics driver
```
sudo apt install xserver-xorg-input-synaptics
```

## Edit blacklist config
Open blacklist conf file
```
sudo vim /etc/modprobe.d/blacklist.conf
```
Comment out (or if doesn't exists add into it)
```
blacklist i2c_hid
blacklist hid_multitouch
```

## Check touchpad is detected properly
```
libinput list-devices
```
Make sure it has this in output
```
Device:           Synaptics TM3471-020
Scroll methods:   *two-finger edge
Click methods:    *button-areas clickfinger
```
Reboot system
```
sudo reboot
```

After reboot, the touchpad now can fully works without lagging.
