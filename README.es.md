# VoidGamingMode

[English](README.md) | **Español**

Modo Gaming al estilo SteamOS para Void Linux. Cambia entre tu escritorio y Steam
Big Picture (gamescope), con el "Cambiar a escritorio" de Steam incluido. Sin systemd.

Da por hecho un escritorio funcionando con SDDM. Usuario por defecto `deck` y
sesión `plasma`: se cambian en el script.

## XBPS

```sh
# xbps-install -Syu void-repo-multilib
# xbps-install -S steam gamescope pulseaudio opendoas mesa-32bit git -y
```

## Descarga

```sh
$ cd /tmp
$ git clone https://github.com/Crafer32/VoidGamingMode.git
$ cd VoidGamingMode
```

## Instalación

```sh
# install -D -o root -g root -m 555 usr/bin/steamos-session-select /usr/bin/steamos-session-select
# install -D -o root -g root -m 755 usr/local/bin/gaming-session /usr/local/bin/gaming-session
# install -D -o root -g root -m 644 usr/share/wayland-sessions/gamescope-steam.desktop /usr/share/wayland-sessions/gamescope-steam.desktop
# install -D -o root -g root -m 644 usr/share/applications/gaming-mode.desktop /usr/share/applications/gaming-mode.desktop
```

## doas

```sh
# echo "permit nopass :wheel cmd /usr/bin/steamos-session-select" >> /etc/doas.conf
# doas -C /etc/doas.conf && echo ok
# useradd -m -G wheel deck    # o, si ya existe: usermod -aG wheel deck
```

## SDDM

```sh
# mkdir -p /etc/sddm.conf.d
# echo "/usr/bin/steamos-session-select boot || true" > /etc/sv/sddm/conf
# sv restart sddm
```

## Uso

```sh
$ steamos-session-select gaming          # cambiar a Modo Gaming (también: gamescope)
$ steamos-session-select desktop         # cambiar al escritorio (también: plasma)
# steamos-session-select boot [SESSION]  # sesión que abre SDDM al arrancar (por defecto: plasma)
```

`boot` lo ejecuta `/etc/sv/sddm/conf` una vez por arranque; los otros dos no
necesitan root (se elevan solos con doas).

## Licencia

GPL-3.0
