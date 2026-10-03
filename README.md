# VoidGamingMode

**English** | [Español](README.es.md)

SteamOS-like Gaming Mode for Void Linux. Switch between your desktop and Steam
Big Picture (gamescope), Steam's "Switch to Desktop" included. No systemd.

Assumes a working desktop with SDDM. Default user `deck` and session `plasma`:
change them in the script.

## XBPS

```sh
# xbps-install -Syu void-repo-multilib
# xbps-install -S steam gamescope pulseaudio opendoas mesa-32bit -y
```

## Install

```sh
# install -D -o root -g root -m 555 usr/bin/steamos-session-select /usr/bin/steamos-session-select
# install -D -o root -g root -m 644 usr/share/wayland-sessions/gamescope-steam.desktop /usr/share/wayland-sessions/gamescope-steam.desktop
# install -D -o root -g root -m 644 usr/share/applications/gaming-mode.desktop /usr/share/applications/gaming-mode.desktop
```

## doas

```sh
# echo "permit nopass :wheel cmd /usr/bin/steamos-session-select" >> /etc/doas.conf
# doas -C /etc/doas.conf && echo ok
# useradd -m -G wheel deck    # or, if it exists: usermod -aG wheel deck
```

## SDDM

```sh
# mkdir -p /etc/sddm.conf.d
# echo "/usr/bin/steamos-session-select boot || true" > /etc/sv/sddm/conf
# sv restart sddm
```

## Use

```sh
$ steamos-session-select gaming          # switch to Gaming Mode (also: gamescope, gamescope-steam)
$ steamos-session-select desktop         # switch to the desktop (also: plasma)
# steamos-session-select boot [SESSION]  # pick the session SDDM opens at boot (default: plasma)
```

`boot` is run by `/etc/sv/sddm/conf` once per boot; the other two need no root
(they elevate through doas).

## License

GPL-3.0
