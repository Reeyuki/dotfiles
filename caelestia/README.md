# ✨ Caelestia branch

Caelestia-specific configuration for Reeyuki's dotfiles.

The `main` branch keeps the legacy Hyprland setup (`hyprland.conf` + `keyBinds.conf`).
This branch keeps the same shared configs (kitty, fastfetch, fish, waybar, swaync, nvim, scripts, ...)
but replaces the Hyprland config with [Caelestia](https://github.com/caelestia-dots/caelestia)'s
Lua config, with the old dotfiles keybinds merged in.

> [!NOTE]
> `caelestia/hypr` is a snapshot of Caelestia's Hyprland component. It is not
> automatically updated when Caelestia changes upstream, and the Caelestia CLI
> does not know about this branch. Re-apply the copy below after updating Caelestia.

## Layout

```text
caelestia/
├── hypr/                 # Caelestia Hyprland config (Lua) with ported keybinds
│   ├── hyprland.lua      # entry point
│   ├── variables.lua     # Caelestia defaults
│   ├── hyprland/         # env, general, input, rules, keybinds, ...
│   ├── scheme/           # colours
│   └── utils/            # helper functions
└── user/                 # machine-specific overrides (go to ~/.config/caelestia)
    ├── hypr-vars.lua     # variable overrides
    ├── hypr-user.lua     # monitor / custom Hyprland config
    └── user-config.fish  # Caelestia shell config
```

Everything outside `caelestia/` is shared with `main`.

## Requirements

Caelestia itself, installed from the AUR:

```bash
paru -S --needed caelestia-cli caelestia-shell
```

Packages used by the ported keybinds:

```bash
paru -S --needed hyprshot scrot nautilus spotube-bin vscodium-bin \
    nwg-drawer playerctl swaync waybar
```

Also needed by the included configs and scripts:

- `firefox`, `kitty`, `hyprpicker`, `wireplumber` (`wpctl`), `cliphist`, `ydotool`
- `mpv`, `rofi`, `swww` for `hyprland/scripts/lofi.sh` and the wallpaper scripts
- Caelestia's own dependencies (installed with `caelestia install`)

## Install

Clone the branch to `~/dotfiles` (the keybinds use `$HOME/dotfiles`):

```bash
git clone -b caelestia https://github.com/Reeyuki/dotfiles ~/dotfiles
```

Some legacy scripts still reference `~/.config/dotfiles`, so keep that path working:

```bash
ln -sfn ~/dotfiles ~/.config/dotfiles
```

Install Caelestia first so it generates `~/.config/hypr`, then copy this branch's
config over it:

```bash
caelestia install

cp -r ~/dotfiles/caelestia/hypr/. ~/.config/hypr/

mkdir -p ~/.config/caelestia
cp ~/dotfiles/caelestia/user/hypr-vars.lua ~/.config/caelestia/
cp ~/dotfiles/caelestia/user/hypr-user.lua ~/.config/caelestia/
cp ~/dotfiles/caelestia/user/user-config.fish ~/.config/caelestia/

hyprctl reload
```

Adjust `caelestia/user/hypr-user.lua` for your monitor before copying.

## Keybinds

The full list is in `caelestia/hypr/hyprland/keybinds.lua`. Notable mappings:

| Key | Action |
| --- | --- |
| `SUPER + A` | Caelestia launcher (same as `SUPER + SUPER_L`) |
| `SUPER + F` | Firefox |
| `SUPER + W` | Toggle floating |
| `SUPER + E` | Nautilus |
| `SUPER + S` | Spotube |
| `SUPER + Z` | nwg-drawer (move window with `SUPER + LMB`) |
| `SUPER + X` | swaync notifications (resize with `SUPER + RMB`) |
| `SUPER + Y` | YouTube in Firefox |
| `SUPER + G` | VSCodium |
| `SUPER + H` | Colour picker |
| `SUPER + SPACE` | Play/pause media |
| `SUPER + TAB` | Next wallpaper |
| `SUPER + SHIFT + A` | Lofi radio |
| `SUPER + ALT + W` | Restart waybar and swaync |
| `SUPER + ALT + Q` | Floating fastfetch |
| `ALT + RETURN` | Fullscreen |
| `Print` | Region screenshot (`hyprshot`) |
| `SUPER + Print` | Screenshot with cursor |
| `SHIFT + Print` | Full-screen screenshot (`scrot`) |
| `SUPER + F2/F3/F4` | Volume down / up / mute |
| `SUPER + grave` | Toggle special workspace `overveiw` |
| `SUPER + SHIFT + grave` | Move window to `overveiw` |

Caelestia defaults kept (selected over the old dotfiles binds):

| Key | Action |
| --- | --- |
| `SUPER + T` | Terminal |
| `SUPER + V` | Clipboard |
| `SUPER + P` | Pin window |
| `SUPER + R` | Todo workspace |
| `SUPER + L` | Lock |
| `SUPER + N` | Sidebar |
| `SUPER + SHIFT + C` | Colour picker |
| `CTRL + ALT + DELETE` | Session menu |
| `ALT + TAB` | Cycle windows |
| `SUPER + Comma` / `SUPER + SHIFT + Comma` | Group window / lock group |
| `SUPER + Arrows` | Focus / move window |
| `SUPER + 0-9` / `SUPER + SHIFT + 0-9` | Go to / move to workspace |

Dropped from the old keybinds because Caelestia keeps those keys:

- `SUPER + V` nvim, `SUPER + P` VLC, `SUPER + T` fastfetch, `SUPER + R` kill window
- `SUPER + L` wlogout, `SUPER + N` hyprpaper, `CTRL + ALT + DELETE` wlogout
- `SUPER + SHIFT + C` centre window, `SUPER + Page_Down` previous wallpaper
- `SUPER + Comma` / `SUPER + SHIFT + Comma` special workspace
- `SUPER + Arrows` resize, `ALT + TAB` bring-to-top
- `SUPER + B` Chromium (removed entirely, no Chromium bind in this branch)

## Updating

After `caelestia install` or a Caelestia update, re-copy the config and the user
overrides:

```bash
cp -r ~/dotfiles/caelestia/hypr/. ~/.config/hypr/
cp ~/dotfiles/caelestia/user/hypr-vars.lua ~/.config/caelestia/
cp ~/dotfiles/caelestia/user/hypr-user.lua ~/.config/caelestia/
hyprctl reload
```

If upstream Caelestia changed its Hyprland config, refresh `caelestia/hypr` from
the upstream repo and re-apply the ported keybinds before copying.
