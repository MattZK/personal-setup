# Display Settings

## OLED Display Brightness

## New Fix
Install `linux-oled` kernel.

### Old Fix
Requires `inotify-tools` package!

Save as `/usr/local/bin/xbacklightmon`

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

Add autostart script. Save as `control-brightness.desktop` in `~/.config/autostart`

```
[Desktop Entry]
Name=XBrightness
GenericName=Brightness Control
Comment=Background task to control xrandr brightness instead of backlight brightness
Exec=/usr/local/bin/xbacklightmon
Terminal=false
Type=Application
X-GNOME-Autostart-enabled=true
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
