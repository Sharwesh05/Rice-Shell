# rice

A Material 3 desktop shell for Hyprland, built on [Quickshell](https://quickshell.org).
Modelled on the Clavis shell ([StatIndet/quickshell](https://github.com/StatIndet/quickshell), built for niri),
with original code for Hyprland's Lua config.

Installed as a plain folder at `~/Modules/Rice-Shell`; run as `rice` (`qs -c rice`).

## Features

| Area | What it does |
|---|---|
| **Bar** | Workspaces with a stretching indicator, the active window, clock, weather, media, CPU/RAM/temp rings, tray, recording indicator, status chip. It can sit at the top or bottom, floating or docked, and each module can be hidden. |
| **Dynamic island** | Pill under the clock. It morphs for media (album-art palette, cava spectrum, synced LRCLIB lyrics), volume/brightness, and notification previews, and expands into a hub with Media / Focus (pomodoro, calendar) / Tools tabs. |
| **Launcher** | Apps as a list or grid, ranked by usage, with desktop actions. Prefixes: `/` files, `;` clipboard (text and image preview), `.` emoji, `:` wallpapers, `>` commands, `=` calculator, units and currency, `?` web, `!` keys & help (every keybind, searchable). |
| **Sidebar (right)** | Quick tiles, a quick-actions row (screenshot, record, colour picker, clipboard, emoji), sliders, media, calendar, notifications, and Wi-Fi / Bluetooth pages. |
| **Dashboard (left)** | Open-Meteo weather: animated sky, hourly chart, 7-day forecast, AQI/UV/wind/sun/moon cards. Also an Info tab. |
| **Dock** | Pinned and running apps, magnify on hover, window previews, drag to reorder, a Downloads stack, autohide. |
| **Desktop widgets** | Cookie clock, weather, liquid CPU/RAM, sparklines, network, storage, battery, calendar, to-do and cava visualizer cards on a snapping grid, with an edit mode. |
| **Capture** | Region/window/screen screenshots, colour picker, screen and audio recording. |
| **Settings app** | 15 pages: General, Bar, Appearance, Wallpaper, Network, Bluetooth, Audio mixer, Displays, Night light, Idle, Dock, Default apps, Autostart, Shortcuts, About. |
| **System** | Notifications, OSD, lock screen (PAM `rice-lock`), power menu, polkit agent, idle lock/DPMS/suspend, night light (hyprsunset), caps/num-lock OSD. |
| **Theming** | matugen generates the shell's colours from the wallpaper, plus kitty, GTK 3/4, Qt (qt5ct/qt6ct), btop, cava, foot, fuzzel and Hyprland borders (`matugen/`). Each target can be turned off: `rice ipc call ecosystem disable <app>`. |

## Docs

- [Keybinds](docs/keybinds.md): the rice binds, your Hyprland binds and the keys inside each panel
- [IPC](docs/ipc.md): every `rice ipc call` target and function
- [Files, Dotfiles and theming](docs/files-and-theming.md): what is written where, the symlink script, and matugen app theming
- [Dotfiles/README.md](Dotfiles/README.md): the Dotfiles repo layout

Most-used keys: **Super+/** all keybinds (searchable) · **Super+R** launcher · **Super+N** sidebar · **Super+A** dashboard · **Super+I** island ·
**Super+,** settings · **Super+W** wallpapers · **Super+C** clipboard · **Print** screenshot · **Super+L** lock ·
**Super+Esc** power menu.

## Dependencies

| Area | Packages |
|---|---|
| Shell | [quickshell](https://quickshell.org) (`qs`), Hyprland (Lua config), xdg-desktop-portal-hyprland |
| Fonts / icons | Inter, JetBrainsMono Nerd Font, Material Symbols Rounded, a Noto emoji font |
| Theming | matugen (generates shell + kitty/GTK/Qt/btop/cava/foot/fuzzel/Hyprland colours) |
| Audio | pipewire + wireplumber, cava (spectrum visualiser), any of kitty/foot/alacritty for theming |
| Capture | grim, satty or swappy (edit), wf-recorder / gpu-screen-recorder / pw-record (record), notify-send (libnotify) |
| Clipboard / launcher | cliphist, wl-clipboard (`wl-copy`), fuzzel (optional launcher), fd or plocate (file search) |
| System services | brightnessctl, playerctl (MPRIS), NetworkManager (`nmcli`), BlueZ (bluetooth), upower, power-profiles-daemon, hyprsunset (night light), pipewire/wireplumber |
| Lock / polkit | PAM service `rice-lock` (ext-session-lock lock screen), a polkit authentication agent |
>
| **Lock screen crashes when submitting a password?** Create the missing PAM service:
| ```sh
| printf '#%%PAM-1.0\nauth      include   system-auth\naccount   include   system-auth\n' | sudo tee /etc/pam.d/rice-lock
| ```
>
| Terminal / apps | kitty or foot (themed by rice), btop, starship |
| Network extras | xdg-open (open links/files) |

Run-time checks: `rice` degrades gracefully — e.g. without `cava` the spectrum stays off, without
`grim` capture is unavailable — but Quickshell, Hyprland, Pipewire,
NetworkManager, BlueZ and matugen are required for the full experience.

## Usage

```sh
rice                                  # start (autostarted by rice.lua)
qs -p ~/Modules/Rice-Shell/bin/rice            # run from the repo with live reload
rice ipc show                         # list every IPC target and function
rice ipc call wallpaper set ~/Pictures/Wallpapers/foo.jpg
```

Wallpapers are read from `~/Pictures/Wallpapers` (you can change the folder in Settings → Wallpaper).
State is stored in `~/.local/state/rice/`.

`Dotfiles/` holds the Hyprland Lua config, kitty, starship and the generated app themes. It is linked into
`~/.config` by `Dotfiles/symlink`, including `~/.config/quickshell/rice`, so rice runs live from this repo.

## Layout

```
config/      Theme (M3 roles), Tokens, Motion, Settings, Paths
services/    Audio, Brightness, Net, Media, Notifs, SysStats, Metrics, Wallpapers, Apps, Panels (IPC),
             Hypr, Weather, Lyrics, MediaPalette, Spectrum, Timers, Todo, Dock, Capture, Recording,
             Clipboard, FileSearch, Ecosystem, Idle, NightLight, Polkit
components/  StyledText, Icon, Surface, IconButton, Chip, Tile, Slider, Ring, AppIcon, JsonStore
modules/     bar, island, launcher, sidebar, dashboard, dock, cards, capture, notifications, osd,
             power, lock, polkit, keyboard, background, settings
matugen/     templates + apply.sh for the app theming (written into Dotfiles/)
docs/        keybinds, IPC, files and theming
```
