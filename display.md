# Display Settings

## OLED Display Brightness script

TODO: Autorun script on login/boot (currently having issues with permissions, Udev not able to access displays)

```sh
#!/bin/sh

echo $DISPLAY

# modify this path to the location of your backlight class
path=/sys/class/backlight/intel_backlight

luminance() {
    read -r level < "$path"/actual_brightness
    factor=$((max))
    new_brightness="$(bc -l <<< "scale = 2; $level / $factor")"
    printf '%f\n' $new_brightness
}

read -r max < "$path"/max_brightness

xrandr --output eDP1 --brightness "$(luminance)"

inotifywait -me modify --format '' "$path"/actual_brightness | while read; do
    xrandr --output eDP1 --brightness "$(luminance)"
done
```

#### Turn it into a service (BROKEN: Display detection issues)
Add a `.service` file in `/etc/systemd/system`.

Example service
```
[Unit]
Description=Background service for OLED Brightness
ConditionFileIsExecutable=/usr/local/bin/xbacklightmon

[Service]
ExecStart=/usr/local/bin/xbacklightmon
TimeoutSec=0
StandardOutput=tty

[Install]
WantedBy=multi-user.target
```

## Set custom scaling factor for an app

In the `/usr/share/applications` directory. Open a `x.desktop` file and add `--force-device-scale-factor=2` to the Exec line. (This doesn't always work)

Example spotify

```
[Desktop Entry]
Type=Application
Name=Spotify
GenericName=Music Player
Icon=spotify-client
TryExec=spotify
Exec=spotify --force-device-scale-factor=2 %U
Terminal=false
MimeType=x-scheme-handler/spotify;
Categories=Audio;Music;Player;AudioVideo;
StartupWMClass=spotify
```