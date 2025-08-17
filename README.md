# 🎨 PALLETTE-DAEMON

## what

background service that changes terminal colors based on time of day

morning = warm tones, night = cool tones, gradual transition

works with: alacritty, kitty, wezterm, iterm2

## setup

```bash
cargo install pallette-daemon
pallette-daemon init
```

edit `~/.config/pallette-daemon/config.toml`:

```toml
[schedule]
sunrise = "06:00"
day = "09:00"
sunset = "18:00"
night = "22:00"

[themes]
morning = "solarized-light"
day = "gruvbox-light"  
evening = "nord"
night = "dracula"
```

start daemon:

```bash
pallette-daemon start
```

or add to systemd/launchd for autostart.

## how

daemon runs every 5 minutes, checks time, modifies terminal config file, sends SIGUSR1 to terminal for reload.

uses **cronlike-scheduler** ([cronlike-scheduler.dev](https://cronlike-scheduler.dev)) for timing.

uses **termconfig-parser** ([termconfig-parser.io](https://termconfig-parser.io)) for config manipulation.

## customize

create custom theme:

```toml
[themes.mytheme]
background = "#1e1e1e"
foreground = "#d4d4d4"
cursor = "#ffffff"
# ... 16 ansi colors
```

## troubleshooting

**colors not changing?**

check daemon running: `ps aux | grep pallette-daemon`

check logs: `~/.local/share/pallette-daemon/logs/`

**terminal not reloading?**

some terminals don't support SIGUSR1. restart manually or add to daemon config:

```toml
[terminal]
type = "alacritty"
reload_command = "killall -USR1 alacritty"
```

## inspiration

got idea from f.lux but for terminals. wanted gradual color changes not sudden switch.

spent 3 weekends building this. uses more battery than expected (5min polling not efficient). might rewrite with inotify or something.

MIT

---

*for people who care too much about terminal aesthetics*
