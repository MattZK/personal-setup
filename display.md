# Display Settings

## OLED Display Brightness
Install `linux-oled` kernel.

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
