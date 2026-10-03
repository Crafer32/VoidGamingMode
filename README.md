# VoidGamingMode

SteamOS-like Gaming Mode for Void Linux: switch between your desktop and a
gamescope + Steam Big Picture session, including Steam's "Switch to Desktop".
Works with runit, SDDM, elogind and doas. No systemd.

## Requirements

SDDM, gamescope, Steam, opendoas, elogind (cgroup v2) and an audio server that
starts on demand in the gaming session (PulseAudio does, with autospawn).

## Install

As root, from this folder:

```sh
install -D -o root -g root -m 555 usr/bin/steamos-session-select /usr/bin/steamos-session-select
install -D -o root -g root -m 644 usr/share/wayland-sessions/gamescope-steam.desktop /usr/share/wayland-sessions/gamescope-steam.desktop
install -D -o root -g root -m 644 usr/share/applications/gaming-mode.desktop /usr/share/applications/gaming-mode.desktop
mkdir -p /etc/sddm.conf.d
echo "/usr/bin/steamos-session-select boot || true" > /etc/sv/sddm/conf
echo "permit nopass :wheel cmd /usr/bin/steamos-session-select" >> /etc/doas.conf
sv restart sddm
```

Append (`>>`) to doas.conf, never overwrite it. Keep the script `root:root 555`:
doas runs it as root without a password.

## Usage

```
steamos-session-select gaming|desktop
```

Steam calls it from "Switch to Desktop" (needs `-steamos3`, already set).

## Configure

- **User**: `deck` by default. Change it in the script.
- **Boot session**: `boot` starts Plasma; `boot gamescope-steam` in
  `/etc/sv/sddm/conf` boots into Gaming Mode.
- **GPU / monitor**: add `--prefer-vk-device vendor:device -O DP-1` to the `Exec`
  line of `gamescope-steam.desktop`.

## License

GPL-3.0
