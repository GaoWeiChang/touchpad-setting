# Synaptics Touchpad Setting/Fix for Ubuntu 24.04 (Xorg)
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
Configure documentation : https://wiki.archlinux.org/title/Touchpad_Synaptics
```
Section "InputClass"
    Identifier "Synaptics Touchpad"
    MatchIsTouchpad "on"
    Driver "synaptics"

    Option "TapButton1" "1"
    Option "TapButton2" "0"
    Option "TapButton3" "2"

    Option "FingerLow" "20"
    Option "FingerHigh" "40"
    Option "MaxTapTime" "180"
    Option "VertTwoFingerScroll" "on"
    Option "HorizTwoFingerScroll" "on"
    Option "VertScrollDelta" "-90"
    Option "HorizScrollDelta" "-100"

    Option "MinSpeed" "1.5"
    Option "MaxSpeed" "1.7"
    Option "AccelFactor" "0.05"
    
EndSection
```
Install synaptics driver
```
sudo apt install xserver-xorg-input-synaptics
```

Reboot system
```
sudo reboot
```

After reboot, the touchpad now can fully works without lagging.
