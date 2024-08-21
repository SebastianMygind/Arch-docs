# Flatpak

For Arch Linux installing flatpak requries only the flatpak package, but because many applications require xdg-desktop-portals to work correctly with scaling fonts and more, these are the recommended packages for most environments, including KDE and GNOME:

    sudo pacman -S flatpak xdg-desktop-portal xdg-desktop-portal-gtk

KDE and Gnome installs their own portal which is the best implementation for their dekstop environment, but for wl-roots based compositors other desktop portals are recommended see [the wiki](https://wiki.archlinux.org/title/XDG_Desktop_Portal).

## Cursor themes

Some applications do not correctly display cursors and can fallback to undefined cursor themes, this breaks the experience when browsing different applications. A crude way to fix this is to give all flatpaks access to the directories where cursor themes are stored. These directories are:
- ~/.icons
- ~/.local/share/icons

An easy way to give this access is through the [flatseal](https://flathub.org/apps/com.github.tchx84.Flatseal) flatpak.

1. Go to the "All Applications" section.
2. Locate the "Filesystem" category.
3. Add the cursor directories as "Other files"

    ~/.icons/:ro  
    ~/.local/share/icons/:ro

":ro" means that the flatpaks only have read only permissions when accessing the directories, which limits what they can do in the directory.

## Application specific

This section will show some more specific tutorials for individual flatpaks.

### Spotify

Scaling is somewhat broken when using HiDPI displays. To fix this for a specific monitor you can force Spotify to use some specified scale: X. For me X=1.6 works best.

The command to give the flatpak is:

    flatpak run com.spotify.Client --force-device-scale-factor=X

This only works for the current Spotify session and will need to be run again after every restart. To make the scaling persistent you need to make the above command run every app-startup. This can be done through the .desktop file for the Spotify flatpak.

First locate the .desktop file for Spotify:

    cd /var/lib/flatpak/exports/share/applications

The .desktop file should be called  com.spotify.Client.desktop

Edit the file and change the Exec command:

    [Desktop Entry]
    Type=Application
    Name=Spotify
    GenericName=Online music streaming service
    Comment=Access all of your favorite music
    Icon=com.spotify.Client
    Exec=/usr/bin/flatpak run --branch=stable --arch=x86_64 --command=spotify --file-forwarding com.spotify.Client --force-device-scale-factor=1.6 @@u %U @@
    Terminal=false
    MimeType=x-scheme-handler/spotify;
    Categories=Audio;Music;AudioVideo;
    Keywords=Music;Player;Streaming;Online;
    StartupWMClass=Spotify
    X-GNOME-UsesNotifications=true
    X-Flatpak-Tags=proprietary;
    X-Flatpak=com.spotify.Client

Here I added the scaling command directly after com.spotify.Client, seperated by only a space.

**Alternative solution**

You can also force spotify to use wayland.

You will stil need to navigate to the .desktop file for spotify, but you must then replace the Exec command with

    Exec=/usr/bin/flatpak run --branch=stable --socket=wayland --arch=x86_64 --command=/app/extra/bin/spotify --file-forwarding com.spotify.Client --enable-gpu-rasterization --enable-zero-copy --enable-gpu-compositing --enable-native-gpu-memory-buffers --enable-oop-rasterization --enable-features=UseSkiaRenderer --ozone-platform=wayland --disable-gpu-sandbox '--enable-features=UseSkiaRenderer,WaylandWindowDecorations' @@u %U @@

You can remove --disable-gpu-sandbox if you don't have an Nvidia gpu. I.E. you have AMD og Intel.
