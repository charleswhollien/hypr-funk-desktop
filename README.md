# Hyprland Desktop

A complete Hyprland desktop configuration for an Ubuntu 26.04 GNOME machine. It includes Hyprland, Waybar, Mako notifications, idle handling, screenshots, clipboard history, themed wallpapers, and an Omarchy-style terminal screensaver.

## Install On Ubuntu 26.04 GNOME

Install every package used by this setup first:

```bash
sudo apt update
sudo apt install hyprland xdg-desktop-portal-hyprland hypridle waybar ghostty nautilus fuzzel grim slurp wl-clipboard cliphist swaybg mako-notifier wireplumber brightnessctl playerctl pavucontrol python3-terminaltexteffects jq procps fontconfig fonts-dejavu-core fonts-font-awesome libnotify-bin
```

Place this directory at:

```text
~/.config/hypr
```

Restore script permissions if needed:

```bash
chmod +x ~/.config/hypr/scripts/hypr-background
chmod +x ~/.config/hypr/scripts/hypr-theme
chmod +x ~/.config/hypr/scripts/omarchy-launch-screensaver
chmod +x ~/.config/hypr/scripts/omarchy-screensaver
```

Create the Ghostty profile used by the keybindings and screensaver:

```bash
mkdir -p ~/.config/ghostty
cat > ~/.config/ghostty/config <<'EOF'
# Match Kitty's built-in default font and color palette.

font-family = DejaVu Sans Mono
background = #000000
foreground = #dddddd
background-opacity = 1
term = xterm-256color
cursor-color = #cccccc
cursor-text = #111111
selection-foreground = #000000
selection-background = #fffacd

palette = 0=#000000
palette = 1=#cc0403
palette = 2=#19cb00
palette = 3=#cecb00
palette = 4=#0d73cc
palette = 5=#cb1ed1
palette = 6=#0dcdcd
palette = 7=#dddddd
palette = 8=#767676
palette = 9=#f2201f
palette = 10=#23fd00
palette = 11=#fffd00
palette = 12=#1a8fff
palette = 13=#fd28ff
palette = 14=#14ffff
palette = 15=#ffffff
EOF
```

Log out of GNOME, choose Hyprland from the session picker, and sign in. Hyprland starts `swaybg`, `mako`, `waybar`, `hypridle`, and the clipboard watchers through `exec-once`.

For an already-running Hyprland session, apply changes without logging out:

```bash
hyprctl reload
hypridle --config ~/.config/hypr/hypridle.conf &
```

Verify the core commands:

```bash
command -v hyprctl hypridle waybar ghostty fuzzel grim slurp wl-copy cliphist swaybg mako wpctl brightnessctl playerctl pavucontrol tte jq pgrep pkill notify-send
test -x /usr/libexec/xdg-desktop-portal-hyprland
fc-match "Font Awesome 5 Free"
fc-match "DejaVu Sans Mono"
```

Smoke-test the desktop:

- `Super+Q` opens Ghostty.
- `Super+Space` opens fuzzel.
- `Print` saves a region screenshot and copies it to the clipboard.
- `Shift+Print` saves a full-screen screenshot and copies it to the clipboard.
- `Super+H` opens clipboard history.
- `Super+Shift+L` launches the Omarchy-style screensaver.
- Leaving the session idle for 5 minutes launches the screensaver through `hypridle`.
- Leaving the session idle for 30 minutes turns the displays off; keyboard or mouse input wakes them.

## Packages

Package purposes:

| Package | Provides | Used for |
| --- | --- | --- |
| `hyprland` | `hyprland`, `hyprctl` | Window manager and reload/control command |
| `xdg-desktop-portal-hyprland` | Hyprland portal service | Portal integration for Hyprland sessions |
| `hypridle` | `hypridle` | Launch screensaver after 5 minutes idle and turn displays off after 30 minutes |
| `waybar` | `waybar` | Top status bar |
| `ghostty` | `ghostty` | Terminal opened by `Super+Q` |
| `nautilus` | `nautilus` | GNOME Files opened by `Super+E` and `Super+F` |
| `fuzzel` | `fuzzel` | Launcher opened by `Super+Space` and `Super+R` |
| `grim` | `grim` | Screenshot capture |
| `slurp` | `slurp` | Region selection for `Print` screenshots |
| `wl-clipboard` | `wl-copy` | Copy screenshots to the clipboard |
| `cliphist` | `cliphist` | Wayland clipboard history |
| `swaybg` | `swaybg` | Wallpaper/background display |
| `mako-notifier` | `mako` | Notification popups |
| `wireplumber` | `wpctl` | Volume and microphone controls |
| `brightnessctl` | `brightnessctl` | Brightness keys |
| `playerctl` | `playerctl` | Media play/pause/next/previous keys |
| `pavucontrol` | `pavucontrol` | Audio mixer opened by clicking Waybar audio |
| `python3-terminaltexteffects` | `tte` | Omarchy-style screensaver effects |
| `jq` | `jq` | JSON parsing for the screensaver focus check |
| `procps` | `pgrep`, `pkill` | Screensaver process checks and wallpaper restarts |
| `fontconfig` | `fc-match` | Font verification |
| `fonts-dejavu-core` | DejaVu Sans Mono | Ghostty font |
| `fonts-font-awesome` | Font Awesome glyphs | Waybar icons |
| `libnotify-bin` | `notify-send` | Low-urgency alert when the screensaver text effects package is missing |

## Files

- `hyprland.conf` - active Hyprland config.
- `hyprland.conf-base` - base/reference Hyprland config kept alongside the active file.
- `hypridle.conf` - idle timeout config that launches the screensaver after 5 minutes.
- `branding/screensaver.txt` - ASCII text animated by the Omarchy-style screensaver.
- `mako/config` - notification popup styling and behavior.
- `scripts/hypr-background` - local wallpaper switcher.
- `scripts/omarchy-launch-screensaver` - launches the Omarchy-style terminal screensaver.
- `scripts/omarchy-screensaver` - runs `tte` effects for the screensaver window.
- `scripts/hypr-theme` - local theme switcher for imported Omarchy palettes.
- `themes/current.conf` - Hyprland color palette used by the active config.
- `themes/omarchy/` - imported Omarchy theme folders and original palette files.
- `wallpapers/` - local SVG backgrounds, including `current.svg`.
- `waybar/config.jsonc` - Waybar module layout and behavior.
- `waybar/style.css` - Waybar visual styling.
- `waybar/theme.css` - Waybar color palette imported by `style.css`.

## Programs

Hyprland defines these helper variables:

| Variable | Program | Purpose |
| --- | --- | --- |
| `$terminal` | `ghostty` | Terminal emulator |
| `$fileManager` | `nautilus` | GNOME Files file manager |
| `$menu` | `fuzzel` | App launcher/menu |

## Terminal

`Super+Q` and `Super+T` open Ghostty.

Ghostty's config lives outside this directory at:

```text
~/.config/ghostty/config
```

Current Ghostty appearance:

| Setting | Value |
| --- | --- |
| Font family | `DejaVu Sans Mono` |
| Background | Kitty default black, `#000000` |
| Foreground | Kitty default white-gray, `#dddddd` |
| Background opacity | `1.0` |
| Terminal type | `xterm-256color`, for better compatibility on SSH/remote hosts |
| Palette | Kitty built-in default 16-color palette |

Current Ghostty config:

```conf
# Match Kitty's built-in default font and color palette.

font-family = DejaVu Sans Mono
background = #000000
foreground = #dddddd
background-opacity = 1
term = xterm-256color
cursor-color = #cccccc
cursor-text = #111111
selection-foreground = #000000
selection-background = #fffacd

palette = 0=#000000
palette = 1=#cc0403
palette = 2=#19cb00
palette = 3=#cecb00
palette = 4=#0d73cc
palette = 5=#cb1ed1
palette = 6=#0dcdcd
palette = 7=#dddddd
palette = 8=#767676
palette = 9=#f2201f
palette = 10=#23fd00
palette = 11=#fffd00
palette = 12=#1a8fff
palette = 13=#fd28ff
palette = 14=#14ffff
palette = 15=#ffffff
```

## Startup

Hyprland starts these helper services once at login when the commands exist:

```conf
exec-once = command -v swaybg >/dev/null 2>&1 && swaybg -i ~/.config/hypr/wallpapers/current.svg -m fill
exec-once = command -v mako >/dev/null 2>&1 && mako --config ~/.config/hypr/mako/config
exec-once = command -v waybar >/dev/null 2>&1 && waybar -c ~/.config/hypr/waybar/config.jsonc -s ~/.config/hypr/waybar/style.css
exec-once = command -v hypridle >/dev/null 2>&1 && hypridle --config ~/.config/hypr/hypridle.conf
exec-once = command -v wl-paste >/dev/null 2>&1 && command -v cliphist >/dev/null 2>&1 && wl-paste --type text --watch cliphist store
exec-once = command -v wl-paste >/dev/null 2>&1 && command -v cliphist >/dev/null 2>&1 && wl-paste --type image --watch cliphist store
```

This starts:

- `swaybg` for the wallpaper
- `mako` for notification popups
- `waybar` for the top bar
- `hypridle` for idle-triggered screensaver launch and display sleep
- `wl-paste` watchers for `cliphist` clipboard history

The Waybar command uses:

```conf
exec-once = command -v waybar >/dev/null 2>&1 && waybar -c ~/.config/hypr/waybar/config.jsonc -s ~/.config/hypr/waybar/style.css
```

`exec-once` commands run when Hyprland starts. After adding `hypridle` during an existing session, start it manually once:

```bash
hypridle --config ~/.config/hypr/hypridle.conf &
```

## Monitor

The monitor configuration uses the preferred mode, automatic position, and scale `1`:

```conf
monitor=,preferred,auto,1
```

## Environment

Cursor sizes are set to `24`:

```conf
env = XCURSOR_SIZE,24
env = HYPRCURSOR_SIZE,24
```

## Look And Feel

General layout settings:

| Setting | Value |
| --- | --- |
| Inner gaps | `5` |
| Outer gaps | `10` |
| Border size | `2` |
| Active border | theme accent gradient from `themes/current.conf` |
| Inactive border | theme inactive border from `themes/current.conf` |
| Resize on border | disabled |
| Tearing | disabled |
| Layout | `dwindle` |

Decoration settings:

| Setting | Value |
| --- | --- |
| Window rounding | `0` |
| Active opacity | `1.0` |
| Inactive opacity | `1.0` |
| Shadows | enabled |
| Shadow range | `2` |
| Shadow render power | `3` |
| Blur | enabled |
| Blur size | `2` |
| Blur passes | `2` |
| Special workspace blur | enabled |
| Blur brightness | `0.60` |
| Blur contrast | `0.75` |

Group tabs use monospace text, theme-driven colors, and a 22px groupbar.

## Animations

Animations are enabled. Workspace animations are disabled.

Defined curves:

- `easeOutQuint`
- `easeInOutCubic`
- `linear`
- `almostLinear`
- `quick`

Enabled animations include borders, windows, fades, layers, special workspaces, and zoom factor.

## Layout Behavior

Dwindle settings:

```conf
dwindle {
    pseudotile = true
    preserve_split = true
    force_split = 2
}
```

Master layout setting:

```conf
master {
    new_status = master
}
```

Misc behavior:

| Setting | Value |
| --- | --- |
| Default Hyprland wallpaper | disabled; custom wallpaper is handled by `swaybg` |
| Hyprland logo | disabled |
| Splash rendering | disabled |
| Scale notification | disabled |
| Focus on activate | enabled |
| Missed ping threshold | `3` |
| Focus under fullscreen | enabled |

Cursor behavior:

| Setting | Value |
| --- | --- |
| Hide cursor on key press | enabled |
| Warp cursor on workspace change | enabled |

Special workspaces are hidden when switching regular workspaces.

## Input

Keyboard and pointer settings:

| Setting | Value |
| --- | --- |
| Keyboard layout | `us` |
| Follow mouse | enabled |
| Pointer sensitivity | `0` |
| Touchpad tap-to-click | enabled |
| Touchpad natural scroll | enabled |
| Gesture | 3-finger horizontal workspace gesture |

There is also an example per-device mouse rule:

```conf
device {
    name = epic-mouse-v1
    sensitivity = -0.5
}
```

## Keybindings

The main modifier is:

```conf
$mainMod = SUPER
```

### Screenshots

| Keys | Action |
| --- | --- |
| `Print` | Select a region with `slurp`, capture it with `grim`, save it to `~/Pictures/Screenshots/`, and copy it to the clipboard |
| `Shift+Print` | Capture the full screen with `grim`, save it to `~/Pictures/Screenshots/`, and copy it to the clipboard |

Screenshot filenames use this format:

```text
screenshot-YYYY-MM-DD_HH-MM-SS.png
```

### Clipboard History

Clipboard history uses `cliphist` with two `wl-paste` watchers:

```conf
exec-once = command -v wl-paste >/dev/null 2>&1 && command -v cliphist >/dev/null 2>&1 && wl-paste --type text --watch cliphist store
exec-once = command -v wl-paste >/dev/null 2>&1 && command -v cliphist >/dev/null 2>&1 && wl-paste --type image --watch cliphist store
```

History picker:

```conf
bind = $mainMod, H, exec, cliphist list | fuzzel --dmenu | cliphist decode | wl-copy
```

Clear history:

```conf
bind = $mainMod SHIFT, H, exec, cliphist wipe
```

### Apps And Session

| Keys | Action |
| --- | --- |
| `Super+Q` | Open terminal |
| `Super+T` | Open terminal |
| `Super+E` | Open GNOME Files |
| `Super+F` | Open GNOME Files |
| `Super+Space` | Open fuzzel |
| `Super+R` | Open fuzzel |
| `Super+H` | Open clipboard history with `cliphist` and `fuzzel` |
| `Super+Shift+H` | Clear clipboard history |
| `Super+Shift+L` | Launch the Omarchy-style screensaver; press any key to exit |
| `Super+M` | Run `hyprshutdown` if installed, otherwise exit Hyprland |
| `Super+C` | Close active window |
| `Super+W` | Close active window |

## Screensaver

The Omarchy-style screensaver is launched manually with `Super+Shift+L` and automatically after 5 minutes of idle time when `hypridle` is running. After 30 minutes idle, Hyprland turns the displays off with DPMS and turns them back on when activity resumes.

Idle config:

```conf
listener {
    timeout = 300
    on-timeout = sh -c "$HOME/.config/hypr/scripts/omarchy-launch-screensaver"
}

listener {
    timeout = 1800
    on-timeout = hyprctl dispatch dpms off
    on-resume = hyprctl dispatch dpms on
}
```

The launcher opens Ghostty with class `org.omarchy.screensaver`. Hyprland matches that class and fullscreenes the window:

```conf
windowrule {
    name = omarchy-screensaver
    match:class = org.omarchy.screensaver

    fullscreen = true
}
```

The animated text comes from:

```text
branding/screensaver.txt
```

The screensaver scripts log launch/startup issues to:

```text
/tmp/hypr-omarchy-screensaver.log
```

### Window Mode

| Keys | Action |
| --- | --- |
| `Super+V` | Toggle floating |
| `Super+P` | Toggle pseudotile |
| `Super+J` | Toggle split direction |

### Focus

| Keys | Action |
| --- | --- |
| `Super+Left` | Focus left |
| `Super+Right` | Focus right |
| `Super+Up` | Focus up |
| `Super+Down` | Focus down |

### Move Windows

| Keys | Action |
| --- | --- |
| `Super+Shift+Left` | Move active window left |
| `Super+Shift+Right` | Move active window right |
| `Super+Shift+Up` | Move active window up |
| `Super+Shift+Down` | Move active window down |
| `Super+Left Mouse Drag` | Move floating/tiled window |
| `Super+Right Mouse Drag` | Resize window or adjust split size |

### Workspaces

| Keys | Action |
| --- | --- |
| `Super+1` through `Super+9` | Switch to workspace 1-9 |
| `Super+0` | Switch to workspace 10 |
| `Super+Shift+1` through `Super+Shift+9` | Move active window to workspace 1-9 |
| `Super+Shift+0` | Move active window to workspace 10 |
| `Super+Mouse Wheel Down` | Switch to next existing workspace |
| `Super+Mouse Wheel Up` | Switch to previous existing workspace |
| `Alt+Tab` | Switch to next existing workspace |
| `Alt+Shift+Tab` | Switch to previous existing workspace |

### Special Workspace

| Keys | Action |
| --- | --- |
| `Super+S` | Toggle special workspace named `magic` |
| `Super+Shift+S` | Move active window to special workspace `magic` |

### Media, Volume, And Brightness

| Key | Action |
| --- | --- |
| `XF86AudioRaiseVolume` | Raise volume by 5%, max 100% |
| `XF86AudioLowerVolume` | Lower volume by 5% |
| `XF86AudioMute` | Toggle output mute |
| `XF86AudioMicMute` | Toggle microphone mute |
| `XF86MonBrightnessUp` | Raise brightness by 5% |
| `XF86MonBrightnessDown` | Lower brightness by 5% |
| `XF86AudioNext` | Next media track |
| `XF86AudioPause` | Play/pause media |
| `XF86AudioPlay` | Play/pause media |
| `XF86AudioPrev` | Previous media track |

Media controls require `playerctl`. Volume controls use `wpctl`. Brightness controls use `brightnessctl`.

## Window Rules

### Suppress Maximize Events

All maximize requests are ignored:

```conf
windowrule {
    name = suppress-maximize-events
    match:class = .*
    suppress_event = maximize
}
```

### XWayland Drag Fix

Floating XWayland windows with empty class/title are prevented from taking focus in specific drag cases.

### Hyprland Run

Windows with class `hyprland-run` are floated and moved near the lower-left area of the monitor:

```conf
move = 20 monitor_h-120
float = yes
```

## Waybar

Waybar runs at the top of the screen with height `30`.

### Layout

Left modules:

- `hyprland/workspaces`

Center modules:

- `hyprland/window`

Right modules:

- `tray`
- `network`
- `pulseaudio`
- `battery`
- `clock`

### Workspaces

Workspace buttons show the workspace name and can be clicked to activate. Waybar keeps 5 persistent workspaces on every monitor.

### Window Title

The active window title is centered, limited to 80 characters, and separated per output.

### Tray

Tray icons are 14px with 8px spacing.

### Network

| State | Display |
| --- | --- |
| Wi-Fi | Wi-Fi icon and SSID |
| Ethernet | wired indicator |
| Disconnected | `offline` |

Tooltip shows interface name and IP/CIDR.

### Audio

Audio shows the current volume percentage. Muted audio shows `muted`. Clicking the audio module opens `pavucontrol`.

### Battery

Battery states:

| State | Threshold |
| --- | --- |
| Warning | 30% |
| Critical | 15% |

Charging and plugged-in states use separate icons.

### Clock

Clock format:

```text
Sun Jul 12  03:30 PM
```

Tooltip format:

```text
YYYY-MM-DD
```

## Theme

This config has a small local theme layer using imported Omarchy palettes, without importing Omarchy's full Lua/template theme engine.

The current applied theme is:

```text
retro-82
```

Hyprland reads its colors from:

```text
themes/current.conf
```

That file controls:

- Active border gradient
- Inactive border color
- Shadow color
- Group tab colors
- Group tab text colors

Waybar reads its colors from:

```text
waybar/theme.css
```

That file controls:

- Bar background
- Foreground text
- Muted text
- Tooltip border
- Active workspace accent
- Warning and critical battery colors

To change the theme, use `scripts/hypr-theme apply <theme>`. The switcher regenerates theme files, reloads Hyprland, and restarts Waybar when it runs inside the active session.

### Omarchy Themes

The original Omarchy theme folders are stored under:

```text
themes/omarchy/
```

Available imported themes:

- `catppuccin`
- `catppuccin-latte`
- `ethereal`
- `everforest`
- `flexoki-light`
- `gruvbox`
- `hackerman`
- `kanagawa`
- `last-horizon`
- `lumon`
- `matte-black`
- `miasma`
- `nord`
- `osaka-jade`
- `retro-82`
- `ristretto`
- `rose-pine`
- `solitude`
- `tokyo-night`
- `vantablack`
- `white`

List themes with:

```bash
scripts/hypr-theme list
```

Apply a theme with:

```bash
scripts/hypr-theme apply gruvbox
```

The switcher reads `themes/omarchy/<theme>/colors.toml` and regenerates:

- `themes/current.conf`
- `waybar/theme.css`

## Alerts

Notifications are handled by `mako`.

Hyprland starts Mako with:

```conf
exec-once = command -v mako >/dev/null 2>&1 && mako --config ~/.config/hypr/mako/config
```

Mako config lives at:

```text
mako/config
```

Current notification behavior:

- Top-right popups
- Up to 5 visible notifications
- Normal notifications time out after 7 seconds
- Low urgency notifications time out after 4 seconds
- High urgency notifications stay until dismissed
- Themed background, text, and border colors

Start it immediately from inside Hyprland with:

```bash
mako --config ~/.config/hypr/mako/config &
```

## Backgrounds

Wallpapers are local SVG files stored under:

```text
wallpapers/
```

The active wallpaper is:

```text
wallpapers/current.svg
```

Hyprland starts the wallpaper with `swaybg` when it is installed:

```conf
exec-once = command -v swaybg >/dev/null 2>&1 && swaybg -i ~/.config/hypr/wallpapers/current.svg -m fill
```

Available backgrounds:

- funk-desktop
- `catppuccin`
- `gruvbox`
- `rose-pine`
- `tokyo-night`

List backgrounds with:

```bash
scripts/hypr-background list
```

Apply a background with:

```bash
scripts/hypr-background apply gruvbox
```

The switcher restarts the wallpaper immediately when run from a terminal inside Hyprland. It still updates `wallpapers/current.svg` when run from a shell that is not connected to Wayland, but it cannot start the visible wallpaper from there.

## Waybar Style

Global Waybar style:

| Setting | Value |
| --- | --- |
| Border radius | `0` |
| Font | `monospace`, `Font Awesome 5 Free` |
| Font size | `12px` |
| Background | theme background from `waybar/theme.css` |
| Text color | theme foreground from `waybar/theme.css` |

Workspace styling:

- Inactive workspaces use the theme muted foreground.
- Active workspace uses the theme accent background.
- Urgent workspace uses the theme urgent background.

Module styling:

- Right-side modules use 10px horizontal padding.
- Muted audio and disconnected network are dimmed.
- Battery warning uses the theme warning color.
- Battery critical uses the theme critical color and background.

## Reloading

After editing this config from inside the Hyprland session, reload with:

```bash
hyprctl reload
```

If `hyprctl reload` cannot reach the Hyprland socket from another shell, reload from a terminal running inside the active Hyprland session or log out and back in.
# hypr-funk-desktop
