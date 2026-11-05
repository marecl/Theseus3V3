# Hardening Theseus 3V3

Well, it's Ender 3 V3. But from this point on we're swimming with Orcas.  

Creality did a great job at being sloppy with their system.  
The "root account" is the **only** account on the printer. It runs **everything**.  
And the printer is constantly connected to API servers _somewhere_. With hard-coded default password and multiple services listening on the web. Probably with default ssh keys too. Totally not sus.  

Initially I just wanted to trim Creality services, but with how system is organized it will need a bit more work.

## Disclaimer

If you activated the root account then you already know everything, but I'll reiterate.

1. You are liable for what you do with your machine,
2. This guide is provided AS-IS,
3. Works on my end,
4. Information may be incomplete,
5. Back up important files,
5. **DON'T TRUST ME BLINDLY. I'M A STRANGER ON THE INTERNET**
6. **DOUBLE CHECK WHAT I SAY WITH WHAT YOU SEE**

A word of comfort: if you brick your printer, you can easily unbrick it with [this guide](https://wiki.creality.com/en/ender-series/3v3-3v3plus-unbrick-firmware-flashing).

This *should* work on firmware `1.2.3.28`.

## (WIP) Performance
This guide will lead to disabling **a lot of** services. While the CPU isn't the worst for the usecase, freeing up some extra resources will sure help.  

The rest is to be measured correctly.

Preliminary results after 30min of idle:

| what  | before |     after    |
|-------|--------|--------------|
|tasks  |        |           26 |
|CPU avg|        | 0.9 0.14 0.9 |
|memory |     76 |           59 |

## Cut away the bloat

### Servers

There are some unused services that don't affect printing.  

`vi /etc/init.d/S99start_app`

Leave on: display-server, master-server, wifi-server  
Disable: app-server, audio-server, Monitor, upgrade-server, web-server  

You can also just move those binaries:
```
mv /usr/bin/app-server /usr/bin/app-server.old
mv /usr/bin/audio-server /usr/bin/audio-server.old
mv /usr/bin/Monitor /usr/bin/Monitor.old
mv /usr/bin/upgrade-server /usr/bin/upgrade-server.old
mv /usr/bin/web-server /usr/bin/web-server.old
/etc/init.d/S99start_app restart
```

Leftover services are not needed for printing, only for the builtin display. If you wish not to use it, you can disable them without consequences (worst case scenario it will bite you).

If you're annoyed with polluted shell, just `reboot`.

#### master-server
Transfers data from Moonraker to the display.  

#### display-server
UI.  
Logs: XYZ positioning, temperatures, user interaction. 5 second intervals.

#### wifi-server
Provides list of WiFi access points on display.  
If turned off, WiFi tab on display stops working.  
Existing connections are unaffected.  
Logs: Wifi connection status. Nothing exciting.

----

#### audio-server
Plays UI sounds. Through nonexistent speaker. Kernel driver is always loaded tho, I wonder if I can blacklist it.

#### Remaining servers
No idea. Prints just fine without them.  

### Logger
`log_main` process runs all the time and calls **many** reverse IP lookup domains.  
I think this compresses and uploads logs to Creality servers, but I didn't investigate for long.  
Well, uploading logs requires connecting to Creality account and I don't want to do that. Besides, who knows what else it collects. Or when it uploads. Good riddance.  
Startup is triggered by any server via `/usr/bin/report.sh`.  

I don't know if moving `report.sh` will break the system, but renaming the binary sure won't.  

```
# mv /usr/bin/report.sh /usr/bin/report.sh.old
mv /usr/bin/log_main /usr/bin/log_main.old
killall log_main
```

### AI

Creality says this is "in optimization", which in their terms really means "half-assed enough to ship". Latest firmware update is dated at April 2025 so... yeah. Dream on.  
I may use a camera in the future, but I sure as hell won't use this "feature".
Probably placebo, but I feel like disabling this made the display more responsive.  

```
/etc/init.d/S70cx_ai_middleware stop
mv /etc/init.d/S70cx_ai_middleware /etc/init.d/disabled.S70cx_ai_middleware
```

### WebRTC
Web Real-time Communication service. Not date/time service, NTP has a separate startup script.
Log files indicate it collects system version, MAC, S/N and model number.  
Config in `init.d` has multiple "environments" which are then passed to the process. This is read from `/usr/data/creality/userdata/config/system_config.json`, key: `deploy_setting`.  
Didn't verify beforehand, but most likely this one was making multiple attempts at reversing IP.

```
/etc/init.d/S97webrtc stop
mv /etc/init.d/S97webrtc /etc/init.d/disabled.S97webrtc
```

### Network activity

There is quite a lot of activity going on, even if not logged in to the cloud.  

`app-server` contains hard-coded URLs and IPs pointing to Creality servers and Alibaba Cloud.
`wifi-server` contains `8.8.8.8`, which is Google's public DNS.

Recommended blocked domains are in `hosts` file. Copy its contents to `/etc/hosts`.

There are other servers being pinged/accessed by the device which I don't feel comfortable with.
The list is not exhaustive and there's a great possibility that some URLs can be accessed by other programs.
Device is also very persistent in setting up time from NTP servers, pinging every regional NTP pool, google and Cloudflare.

Those are good enough to start looking for URLs:
```
strings device_manager | grep -Eo ".{0,50}www\.{0,50}"
strings device_manager | grep -Eo ".{0,50}\.com.{0,50}"
strings device_manager | grep -Eo ".{0,50}\.cn.{0,50}"
strings device_manager | grep -Eo ".{0,50}\.io.{0,50}"
strings device_manager | grep -Eo ".{0,50}\.org.{0,50}"
strings device_manager | grep -Eo ".{0,50}\.net.{0,50}"
strings device_manager | grep -Eo "http\:\/\/.{0,50}"
strings device_manager | grep -Eo "https\:\/\/.{0,50}"
strings device_manager | grep -Eo ".{0,10}([0-9]{1,3}[\.]){3}[0-9]{1,3}.{0,50}"
```

### Kernel modules
Theoretically unnecessary, but I don't like the idea of having a kernel module not related to the printer.

```
mv libalibabacloud-oss-cpp-sdk.so libalibabacloud-oss-cpp-sdk.so.disabled
mv libai_capture.so libai_capture.so.disabled
mv libai_engine.so libai_engine.so.disabled
mv libalibabacloud-oss-cpp-sdk.so libalibabacloud-oss-cpp-sdk.so.disabled
```

## Tweaks

### QoL

### (WIP?) Persistent dropbear

It would be suboptimal to accidentally remove `/usr/data/creality/userdata/user_agree_root` which will cut you off SSH.

### File browsing

`vi /etc/profile`  
Add: 
```
alias ll='ls -l'
alias la='ls -la'
```

### Hostname

`vi /etc/hostname`  
Change to preferred hostname.

### Log level
Logs are... huge. Once in a while they are compressed to `gz` archive and kept on the device.

`vi /usr/data/creality/userdata/log/log_config.json`  
Change `log_level` to desired value:
* `LOG_DEFAULT` = 0
* `LOG_DEBUG` = 1 (_set by default_)
* `LOG_INFO` = 2
* `LOG_WARNING` = 3
* `LOG_ERROR` = 4 (_recommended_)

Not that those have any value to the user.  
Run `/etc/init.d/S99start_app restart` to apply changes.  

## Updates

Some packages can be updated with [helper script](https://github.com/Guilouz/Creality-Helper-Script).

I recommend the following:  
1. [Install] -> Updated Moonraker
2. [Install] -> Updated Fluidd
3. [Customize] -> Remove Creality Web Interface

## (WIP) Users and permissions

**Everything** runs as root on E3V3, including:
1. Kernel 4.4.94
2. Dropbear SSH v2019.78
3. <span style="color:red">nginx</span> 1.17.7
4. <span style="color:red">python</span> 3.8.2
5. <span style="color:red">Moonraker</span> (tbd)
6. <span style="color:red">Fluidd</span> (tbd)
7. <span style="color:red">Klipper</span> v0.11.0  
For tinkerers: Devices flashed with v0.12.0 should work. Didn't test that in depth yet.

Outdated programs running as root while continuously connected to API server... Not looking good.

### Separate users
[create user for klipper, moonraker, creality apps]

### Set user/group permissions
[minimal for each, groups, udev]

### Set file permissions
[as above]

### Run as users
[run services as users]

### Reorganize files
Discouraged. Some paths are hard-coded in multiple places and I don't feel like I want to enter this rabbit hole.

Common paths are:  
1. `/usr/share/klipper`
2. `/usr/data/printer_data/config`
3. `/usr/data/printer_data/gcodes`
4. `/usr/data/printer_data/log`
5. `/usr/data/creality/userdata/log`
6. `/usr/data/creality/userdata/user_agree_root`

## Easter eggs

`HUMBNAIL` - a typo which is _everywhere_. Not just init scripts. I mean directory names, macros, gcodes, _EVERYWHERE_.

Leftover bootlogo of Piocreat IPX2 in system version [1.2.3.21](https://github.com/Guilouz/Creality-Ender3-V3-Extracted-Firmwares/blob/main/Firmware/etc/boot-display/pic_00.jpg)
