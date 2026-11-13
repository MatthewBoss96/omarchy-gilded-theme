# Gilded

A dark Omarchy theme in black and gold. When you select it, an intro plays: a meteor burns a viola meadow to ash, a distant tower sends down a line of light, and a black viola with gold edges grows back from the ash.

![Gilded on an Omarchy desktop](.github/screenshot.webp)

<p align="center"><img src=".github/bloom.webp" alt="The intro: the meadow burns to ash and a gilded viola grows back" width="360"></p>

The terminal colours are metals: vermilion, champagne, bronze, brass, verdigris, steel and rose-gold, with gold as the accent. That keeps btop, the editor and everything else in the theme's palette.

## Install

```bash
omarchy theme install https://github.com/MatthewBoss96/omarchy-gilded-theme.git
```

The theme installs as `gilded`. To switch to it later, run `omarchy theme set gilded`.

## Replay the intro with a hotkey

Omarchy doesn't let a theme installed from git add keybindings, so this is a manual step.

1. Save this script as `~/.local/bin/omarchy-replay-intro` and make it executable (`chmod +x ~/.local/bin/omarchy-replay-intro`):

   ```bash
   #!/bin/bash
   # Replay the current background's intro (e.g. the Gilded intro).
   # Alternate intros named <wallpaper>.<variant>.mp4 take turns on each press.

   intro=$(omarchy-theme-bg-boot-intro --resolve-only)
   [[ -n $intro ]] || exit 0

   shopt -s nullglob
   intros=("$intro" "${intro%.*}".*."${intro##*.}")
   turn_file=$HOME/.local/state/omarchy-replay-intro.turn
   turn=$(( ($(cat "$turn_file" 2>/dev/null || echo 0) + 1) % ${#intros[@]} ))
   echo "$turn" >"$turn_file"
   intro=${intros[turn]}

   # Decode offscreen first, as a theme switch does, so playback starts without a stutter.
   owe intro-prepare "$intro" >/dev/null 2>&1
   exec owe intro --start still --refresh "$intro"
   ```

2. Add this line to `~/.config/hypr/bindings.lua`, replacing `<you>` with your username. The full path is there because Hyprland may not have `~/.local/bin` on its `PATH`.

   ```lua
   o.bind("SUPER + CTRL + ALT + I", "Replay background intro", "/home/<you>/.local/bin/omarchy-replay-intro")
   ```

3. Press `SUPER + CTRL + ALT + I`. The intro replays and settles back on the wallpaper. The script works with any background that has an intro, not only Gilded. On a background without one, the key does nothing.

## Contents

- `colors.toml`: the colour palette. Omarchy builds the terminal, editor and app themes from it.
- `backgrounds/1-gilded-golden-viola.jpg`: the default 3840x1600 wallpaper, a black viola with glowing gold edges in an ashen field under a golden sunbeam.
- `backgrounds/intros/1-gilded-golden-viola.mp4`: its intro, 26 s at 60 fps, that plays when the theme is selected. A meteor strikes a sunny viola meadow and burns it to ash, a tower appears on the horizon and sends down a line of light, and where it struck a black viola grows, opens and turns gold, ending on the wallpaper.
- `backgrounds/2-gilded-viola-meadow.jpg`, `3-gilded-meteor-blast.jpg` and `4-gilded-tower-beam.jpg`: three more 3840x1600 wallpapers, moments from the intro: the meadow before the meteor, the blast of fire and debris flying at you, and the line of light from the tower. Pick them with the background switcher.
- `icons.theme`: sets the Yaru-yellow icon theme.
- `unlock.png` and `preview-unlock.png`: the gold Omarchy logo for the boot screen where you unlock your disk, and its preview. Select it with `omarchy plymouth switcher`.
- `preview.png`: the screenshot the Omarchy theme picker shows.
