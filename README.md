# pi-kiosk-setup
Single shell script that turns a fresh install of Raspberry Pi OS into a fullscreen kiosk.

## Author's life story

Fed up with spending loads of time setting up my Pi kiosk from scratch every time an SD card died or I messed up the OS, I spent loads of time making this script to automate it.

I'm using [MagicMirror](https://github.com/MichMich/MagicMirror) in ["Server Only" mode](https://docs.magicmirror.builders/getting-started/installation.html#server-only) so my config is safe elsewhere, all I need to provide to the script is the project name and URL.

## Scant details

- This script should allow anyone with basic Raspberry Pi and ssh knowledge to go from a fresh install of Raspberry Pi OS to a simple fullscreen webpage ("kiosk") within a few minutes.
- You can ignore the `ssh-copy-id` prompt, but if you get sick of typing your password when you ssh in - go learn about ssh keys.
- It uses Chromium, so any webpage should work.
- It's designed to be safe to run multiple times - in case it's interrupted/cancelled for some reason, if you want to change the settings or if you want to run a newer version (if I ever update it).
- If using the project name to ssh doesn't work on your network, continue using your IP.

## Pseudo-legal disclaimer

I make no guarantees this will work on your device, but worst-case scenario you have to wipe your SD card *(not accounting for external factors, including but not limited to: shark attack, bear attack, heart attack or religious cold callers)*

You shouldn't blindly execute a stranger’s code, ideally you should understand the code here so you don't need to trust me. But good luck because, despite my attempt to add helpful comments, it's trash.

## The only bit you care about

Run this:

```
/bin/bash -c "$(curl -fsSL https://github.com/luke255/pi-kiosk-setup/raw/main/pi.sh)"
```

Good luck!

## B O N U S - R O U N D

You can sleep/wake the connected monitor using simple HTTP calls. The following example uses `kiosk` for the hostname, substitute this for your use.

Turn off the screen: `http://kiosk:8080/0`

Turn on the screen: `http://kiosk:8080/1`

Get screen status: `http://kiosk:8080/` (0 = off, 1 = on)

If, like me, you use [Homebridge](https://github.com/homebridge/homebridge) and want to be able to say "Hey Siri! Kiosk off" or set time/location-based automations - install the [homebridge-http-rgb-push](https://github.com/QuickSander/homebridge-http-rgb-push) plugin and add the following to the `"accessories"` section of your config:

```
{
    "accessory": "HttpPushRgb",
    "name": "Kiosk",
    "service": "Switch",
    "switch": {
        "status": "http://kiosk:8080/",
        "powerOn": "http://kiosk:8080/1",
        "powerOff": "http://kiosk:8080/0"
    }
},
```

You can set the `"name"` to something different.

