# System build — state and remaining work

## Verified state

Machine: Framework 13 AMD (Ryzen AI 7 350), Void Linux, runit.

**Displays.** The ultrawide enumerates as **DP-2** under mango, not DP-3 as it
did under dwl. Nothing in `~/system/etc/` or `~/.local/bin/` hardcodes a
connector name, so the clamshell handling was unaffected — but the compositor
config was, and silently: the rule simply never matched while the monitor sat
on its preferred mode and looked correct.

- DP-2 (LG HDR WQHD): 3440x1440, scale 1.0, position 0,0
- eDP-1 (BOE): 2880x1920 @ 120Hz, scale 2.0, position 1000,1440

The x:1000 offset centres the 1440-logical-wide panel beneath the ultrawide.

**Compositor.** `mangowc-0.14.4_1` from XBPS. Binary is `mango`, IPC tool is
`mmsg`; the package kept the old project name. Pulled `scenefx-0.4.1` and
`cJSON`, and links the existing `wlroots0.19-0.19.3` — no new wlroots, no
source build, everything package-managed.

Config at `~/.config/mango/config.conf`, written minimal: only values that
differ from `/etc/mango/config.conf` appear. The default file is the reference;
read it rather than restating it.

- Scroller layout on all nine tags, `scroller_proportion_preset=0.33,0.5,0.8,1.0`
- 0.33 gives three genuine columns on DP-2. Cramped on eDP-1 at 480px logical;
  cycle to 0.8 or 1.0 there. Presets are global at 0.14.4 — per-monitor scroller
  parameters landed upstream later.
- `border_radius=6`, `borderpx=0`, `unfocused_opacity=0.92`, animations and
  blur and shadows off, gaps 5 inner / 3 outer
- Focus is indicated by opacity, not borders

**Escape hatch.** `chvt` is compiled into mango's `default_key_bindings[]`
(`src/config/parse_config.h`) as CHVT(1) through CHVT(12). Unlike dwl, where it
lived in the editable `config.h`, your config cannot remove it. Ctrl+Alt+F2
always works.

**Session chain.** `~/.zprofile`, guarded on `$(tty) == /dev/tty1`:

    exec dbus-run-session /usr/bin/mango -s ~/.local/bin/wayland-session

`wayland-session` (renamed from `dwl-session`) is the single home for session
startup: pipewire, pipewire-pulse, wireplumber, mako, hypridle, the cliphist
watcher, waybar, and ornatus with its log redirect. `exec-once` is deliberately
empty in the compositor config — session composition stays portable across
compositors.

**Bar.** Waybar 0.15.0 from XBPS, using `dwl/tags` and `dwl/window`. Mango
0.14.4 still speaks the dwl IPC protocol, so these bind natively. Config at
`~/.config/waybar/config.jsonc`, static dark stylesheet at
`~/.config/waybar/style.css` — Inter SemiBold with JetBrainsMono Nerd Font
fallback for glyphs, transparent background, Nerd Font escapes written as JSON
`\uXXXX` so the file stays ASCII on disk.

**Theming.** ornatus, unchanged across two compositor migrations. Three
mappings only — kitty, fuzzel, mako — plus the `gsettings` color-scheme call
that reaches Chromium and Electron apps through `xdg-desktop-portal-gtk`. That
chain is confirmed working; the deferred Helium `--force-dark-mode` shim was
never needed. Compositor colours are deliberately unmanaged, which is precisely
why ornatus needed no changes.

Startup race verified clean on a real boot: both outputs attached at
`elapsed_ms=0`, `sweep_unattached` never fired.

**Browser.** Helium switched from a 461MB extracted tree to a single AppImage at
`~/.local/opt/helium.AppImage`, currently 0.15.4.1. The shim at
`~/.local/bin/helium` is a two-line exec; `~/.local/bin/helium-update` checks
GitHub, confirms, downloads to a temp file, verifies the new binary reports the
expected version, and swaps atomically. The desktop entry uses bare `helium`,
so it follows the shim.

**Login.** greetd with tuigreet 0.9.1 on vt1, launching
`/etc/greetd/tuigreet-start`, which runs a login shell — compositor-agnostic,
`.zprofile` does the rest. Both greetd files now mirrored in `~/system/etc/`
and installed by `~/system/install.sh`.

The `--kb-*` overrides were removed: tuigreet's hint bar is hardcoded to
F2 command / F3 sessions / F12 power regardless of the flags, so the labels
contradicted the bindings. Accepting the defaults makes all three agree.

**Power.** Unprivileged power actions go through `loginctl`, which reaches
elogind via polkit. The bare `poweroff` / `reboot` / `halt` binaries are
root-only symlinks to `halt` with no setuid bit, unusable from a session or
from the `_greeter` user. The older note to avoid `loginctl` on runit applies
to session and service management, not power requests.

**System mirror.** `~/system/` holds `etc/{acpi,dhcpcd.exit-hook,elogind,greetd}`
and `usr/local/bin/tz-from-ip`, applied by `~/system/install.sh` — self-
elevating, idempotent, explicit about every file. Note the script is at
`~/system/install.sh`, not `~/system/etc/`. It also enables the acpid runit
service and SIGHUPs `elogind-daemon` by that name, accounting for the orphaned
re-exec.

`tz-from-ip` geolocates on each DHCP lease and repoints `/etc/localtime`.
Worth knowing that ornatus also geolocates by IP — two independent consumers of
the same signal.

**Removed this session.** dwl binary and its `.gitignore` rule, `dwl-status`,
`wlroots0.19-devel`, 206MB of orphaned GIMP dependencies plus `foot-terminfo`,
`~/.config/GIMP`, `~/.config/Ladybird`, and two dangling runit service symlinks
(`polkit`, superseded by `polkitd`; `shhd`, a typo for `sshd`) that had been
failing silently since March.

**Kept deliberately.** `~/projects/dwl` as reference for the pixman/fcft bar
patch. `~/projects/mango` as reference for the shipped default config, though
it duplicates what XBPS provides and could go.

**Not installed, despite earlier notes.** Ollama. That entry was aspirational.

## Remaining

**The 0.16.0 upgrade breaks the bar.** Mango deprecated dwl IPC entirely at
0.16.0. Waybar's `dwl/tags` and `dwl/window` stop working; the replacement is
`mango/window` on a recent Waybar, or a custom module. Void is at 0.14.4, so
this is deferred, not urgent — but it is the thing that breaks on upgrade.

**A bar in Rust.** The stated ambition, and better scoped than it first looks:
layer-shell surface, pixman or a soft renderer, fcft for text, a line-oriented
parser. `mmsg watch all-tags`, `watch focusing-client` and `watch all-monitors`
are persistent event streams, not polls — block on the stream, redraw on
change. Targeting mango's own IPC rather than the deprecated dwl protocol also
means it survives the 0.16.0 upgrade that will break Waybar.

**tuigreet fork.** `github.com/NotAShelf/tuigreet`, rebuilt with Ratatui, adds
TOML configuration files and includes status-bar rendering work that may be
this exact hint-bar bug. Fifteen command-line flags in a wrapper script would
read better as a config file.

**Smaller items.** `~/.local/share/icons/hicolor/*/apps/helium.png` is untracked
— that directory has no `.gitignore` negation. `dump.sh` writes manifests
atomically now, but `xbps-query -m | sed` still reports sed's exit status, so a
failing query with a succeeding sed would write empty; `pipefail` would close it
but isn't POSIX. The eDP-1 wallpaper is a 3440x1440 source cropped hard to
2880x1920 — not a bug, but a narrow vertical slice.

## Operational

- `mango -p -c FILE` validates syntax. It does **not** prove an action name
  resolves; only running does that.
- `SUPER+r` or `mmsg dispatch reload_config` reloads live. No logout needed.
  The dwl-era warnings — ETXTBSY on the running binary, logout per change,
  cascading syntax warnings — no longer apply.
- `mmsg` requires `MANGO_INSTANCE_SIGNATURE`, set by the compositor. It only
  works from inside a mango session.
- The dotfiles `.gitignore` denies `/.config/*` and `/.local/*` and re-includes
  by name. A new area is **invisible** to `status`, not merely untracked, until
  a `!` line exists. `dotfiles check-ignore -v <path>` names the rule and line.
  This is documented in `~/.zshrc` above the alias.
- `dotsave "msg"` does add, commit and push in one step. Single-line messages
  only.
- `git -C ~/` fails on the dotfiles repo — there is no `.git` in `$HOME`. Use
  the alias.
- **Heredocs containing fenced code blocks do not survive copy-paste.** The
  outer block terminates at the first inner fence and the command silently
  never runs. Use a file for anything containing backticks.

## Lessons from this session

The session ran long, and the reason is worth recording plainly.

Roughly seven claims were made from inference rather than verification, and
each was wrong: that MangoWM was still installed; that wlroots and scenefx
needed source builds; that yambar was still maintained; that a scroller resize
fix sat in the 0.14.4-to-0.16.0 gap; that iA Writer was the only font
installed; that ornatus had no remote; that bare `poweroff` would work from the
greeter. Each cost a round trip, and each was one command away from being
settled before it was asserted.

Two verifications also appeared to pass while proving nothing. `git tag -l |
tail` sorts lexicographically, so it showed the *oldest* tags while looking like
the newest. And a README line count and fence count were read as evidence a
heredoc had landed, when both figures described the file it had failed to
replace.

The pattern from the previous session holds without amendment: check what a
verification actually demonstrates, not just that it printed something. The
corollary this session adds is narrower — a summary of prior state is not
evidence. Notes drift. `~/system/etc/install.sh` was at `~/system/install.sh`.
The compositor selector had been deleted. Ollama was never installed. Read the
machine, not the record of it.

One thing that went right and is worth keeping: nothing was removed until its
replacement was proven. dwl stayed installed and bootable through the entire
migration, the Helium tree survived until the AppImage had been launched from
fuzzel, and every irreversible step came last.
