# SDDM (Simple Desktop Display Manager)

SDDM is my preferred login manager as it works best with KDE and other fun WM's like Hyprland and Sway.

## Synchronize with KDE

When using SDDM with KDE Most configuration can be done through the system settings app from KDE, but to use a fully wayland session, you currently have to explicitly tell SDDM to use wayland.

### Making SDDM use Wayland

Navigate to the configuration directory

    cd /etc/sddm.conf.d

Create a configuration file for SDDM wayland

    touch 10-wayland.conf

Input the following in the file to make SDDM as secure as possible:

    [General]
    DisplayServer=wayland
    GreeterEnvironment=QT_WAYLAND_SHELL_INTEGRATION=layer-shell

    [Wayland]
    CompositorCommand=kwin_wayland --drm --no-lockscreen --no-global-shortcuts --locale1

source: [Arch Wiki](https://wiki.archlinux.org/title/SDDM) 2.12

### Theming SDDM through KDE system settings

1. Either search in settings for SDDM or go to "Colors and Themes" -> "Login Screen (SDDM)".
Here you can choose the theme for SDDM to use.

2. On the same settings page, you also need to "Apply Plasma Settings" For mouse cursor and scaling to corretly sync with KDE