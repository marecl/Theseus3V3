# Hardening

Set up system properly.

## Reasons
**Everything** runs as root on E3V3, including:
1. Kernel 4.4.94
2. Dropbear SSH v2019.78
3. nginx 1.17.7
4. python 3.8.2
5. Moonraker (tbd)
6. Fluidd (tbd)
7. Klipper v0.11.0 (custom)  
For tinkerers: Devices flashed with v0.12.0 should work. Didn't test that in depth yet.

Outdated programs running as root while continuously connected to API server... Not looking good.

## Target structure
Who which user what permissions

## Disclaimer

If you activated the root account then you already know everything, but I'll reiterate.

1. You are liable for what you do with your machine,
2. This guide is provided AS-IS,
3. Works on my end,
4. Information may be incomplete,
5. Back up important files,
5. **DON'T TRUST ME BLINDLY. I'M A STRANGER ON THE INTERNET**
6. **DOUBLE CHECK WHAT I SAY WITH WHAT YOU SEE**

## Separate users
[create user for klipper, moonraker, creality apps]

## Set user/group permissions
[minimal for each, groups, udev]

## Set file permissions
[as above]

## Run as users
[run services as users]

## Reorganize files
Discouraged. Some paths are hard-coded in multiple places and I don't feel like I want to enter this rabbit hole.

Could use some symlinks though.

Common paths are:  
1. `/usr/share/klipper`
2. `/usr/data/printer_data/config`
3. `/usr/data/printer_data/gcodes`
4. `/usr/data/printer_data/log`
5. `/usr/data/creality/userdata/log`
6. `/usr/data/creality/userdata/user_agree_root`