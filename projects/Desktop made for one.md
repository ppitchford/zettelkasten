---
id: "20260814121839"
date: 2026-08-14
tags:
  - project
---
# Desktop made for one

Single-user desktop environment on the Framework 13 / Void Linux machine — compositor, session, theme, bar, lock. The proof-of-concept the idea drives, and the standing record of how the machine is actually configured. Sessions should read this before changing anything on the desktop.

**Status:** active
**Next action:** reconcile `system-build-state.md` — it is untracked in the vault and overlaps this record; fold it in or promote it

State below was current on 2026-08-14, revised 2026-08-17. Claims about *what is installed or enabled* decay; verify before relying on one. Claims about *method* — how a thing behaves, how to diagnose it — do not.

## Compositor

MangoWM 0.14.4 (`/usr/bin/mango`, packaged), config at `~/.config/mango/config.conf`. Unfocused opacity removed; focus is signalled by a 3px border in Catppuccin Mocha mauve `#cba6f7`, with unfocused borders fully transparent `0x00000000` so focus changes don't shift geometry. `rootcolor` set to `#373d49` to match the wallpaper during startup. `urgentcolor` `#f38ba8`.

`scroller_structs=5` reduces the scroller's edge inset; combined with `gappoh=3` that leaves 8px at each screen edge. `mouse_natural_scrolling` removed, which reverts to the default and inverts the scroll direction. The empty Autostart section and an untested per-monitor tag rule were deleted.

**Scroller centring: resolved, and the answer is that it doesn't exist.** Read from source at `~/projects/mango/src/layout/scroll.h`:

- `scroller_prefer_center` centres only when the previous and focused windows' proportions *sum to more than* the available width — the opposite of an under-full strip. Two windows at 0.33 never trigger it.
- `scroller_focus_center` centres the focused window unconditionally, but with three columns filling the output that pushes a neighbour off-screen. Actively harmful in normal use.
- The centring branch is gated behind `need_scroller`, which is set by the *overspread* block — so disabling overspread makes centring unreachable rather than enabling it. `need_apply_center` is computed independently and then made unreachable, which reads as a bug.
- `git log 0.14.4..0.16.0 -- src/layout/scroll.h` shows six commits, none a centring fix. Upgrading would not help.

Both settings removed. The layout is back to defaults: overspread on, no centring. Worth reporting upstream.

## Session and login

**greetd and tuigreet are gone from the machine** (removed 2026-08-14). `agetty-tty1` is enabled in their place. agetty execs `login`, zsh reads `.zprofile`, and the tty1 guard execs `dbus-run-session /usr/bin/mango -s ~/.local/bin/wayland-session`. greetd's `tuigreet-start` had only ever been getting to a login shell, so agetty reaches the same place by a shorter route.

Reasoning: tuigreet cost a hardcoded hint bar that contradicted the `--kb-*` flags, a power menu that failed silently, and a polkit rule that would have needed writing, testing and mirroring — against 4.2MB of disk. Password at the console is the trade.

Removal took `xbps-remove tuigreet greetd`, then `rm -rf /etc/greetd`. xbps removed `/etc/sv/greetd` and `/etc/pam.d/greetd` itself. `/etc/greetd` had to go by hand because three of its four files were hand-written and therefore unowned. `/run/runit/supervise.greetd*` are tmpfs and clear at reboot.

**The tty1 session cannot be stacked, and this is structural.** `.zprofile` uses `exec`, so mango *replaces* the tty1 login shell rather than forking from it. The process runit supervises as `agetty-tty1` therefore *is* the session — `sv status agetty-tty1` reports the pid that parents `dbus-run-session` → mango. tty1 stays occupied for the session's life and agetty cannot respawn a prompt there until logout. Verified 2026-08-14.

**Do not `sv down greetd` from inside a live session.** greetd *is* the parent of the login shell that exec'd mango; stopping it kills the session outright. Cost a hard reboot to learn.

**And the converse, which is the subtler half:** unlinking a display-manager service *without* stopping it leaves the session it already spawned running as an orphan. Switching greetd → agetty mid-session produced two live compositors on one seat — see the Helium diagnosis below. Whichever way you go, the session started by the outgoing manager has to be ended deliberately; neither disabling nor removing does it for you.

**Fingerprint does not reach the console.** `pam_fprintd.so` appears nowhere in `login` → `system-local-login` → `system-login` → `system-auth`. hyprlock talks to fprintd directly over D-Bus (`fprint: claimed device` in its debug output), in parallel with its PAM password path. Console login is password-only unless `pam_fprintd.so` is added to a shared include — which would affect sudo and su as well, in the file that locks out every tty if it goes wrong.

**Power actions.** `loginctl poweroff` / `reboot` / `suspend`, which reach elogind through polkit. The bare `poweroff`, `reboot`, `halt` and `zzz` binaries are root-only with no setuid and fail silently for an unprivileged user. `varia system` was calling all of them and had been broken since installation; its logout case was also still `pkill -x dwl`. All four fixed.

**elogind reports `down` under runit and is fine.** The wrapper re-execs and orphans the daemon to PID 1, so runit loses the pid. `pgrep -x elogind` is the real check. Three rounds were spent chasing this as a crash loop — and then a fourth, because this note and `install.sh` both matched on `elogind-daemon`, which is not the process's name and matches nothing: the reload branch in `install.sh` silently took its else path on every run. Corrected 2026-08-17.

## Wallpaper

`~/Pictures/wallpapers/wallpaper-1-3840.jpg`, downscaled from the 7952×4473 master via ffmpeg Lanczos at `yuvj444p -q:v 2`. Measured: dominant `#373d49` (h 220, s 14, l 25), lilac highlights h 274–296, dusty rose h 345, nothing above 17% saturation. Consumed by both ornatus and hyprlock.

## Theme

Catppuccin — Mocha dark, Latte light — after Rosé Pine → Iceberg → Catppuccin in a single day. Bundle files under `~/.config/theme/{light,dark}/`: `kitty.conf`, `fuzzel.ini`, `mako.conf`. Fuzzel carries a 3px border at radius 6, `#cba6f7` in both variants (Latte's `#8839ef` was too harsh), matching mango so the focus signal is one colour throughout.

## Waybar

Static, outside ornatus's `MAPPINGS`. Opaque `#373d49` — the measured modal colour beneath the bar on both outputs. Because it's pinned to the wallpaper rather than the theme, it never needs to follow the sun, so the planned fourth `Mapping` and `SIGUSR2` reload in `theme.rs` were dropped. Mocha glyphs; focused tag uses the same mauve as the window border.

Uses `dwl/tags` and `dwl/window`; mango 0.14.4 still advertises `zdwl_ipc_manager_v2`. Mango deprecates dwl IPC at 0.16.0, at which point these modules stop working — the replacement is `mango/window` on a recent Waybar, or a custom module. That is the thing that breaks on upgrade.

## Hyprlock

Background repointed to the current wallpaper — the configured `junji-ito-dark.png` had not existed for some time and hyprlock fails to black silently. Mocha colours, 3px mauve outline on the input field. Fingerprint and password auth confirmed working in parallel, including through a suspend and resume cycle.

## Apps XBPS doesn't own

Helium and Obsidian are the two applications installed outside the package manager, so `xbps-install -Su` never touches them and nothing else records how they got here. Each has an updater on PATH that is now the sole install record: `helium-update` and `obsidian-update`, both of which install from nothing when their prefix is absent. A rebuild is therefore clone dotfiles → `~/system/install.sh` → run both. Neither tree is tracked; both are far too large, and `~/.gitignore` says so where the rule would otherwise look like an oversight.

**1Password decides which browsers may exist, and its reasons are invisible from the outside.** Resolved 2026-08-17, from the extension in Helium never using the fingerprint reader. The whole chain is extension → `1Password-BrowserSupport` → desktop app → polkit → `pam_fprintd` → reader, and if the first hop fails the extension silently falls back to asking for the account password. Nothing in the UI says why.

BrowserSupport verifies the browser binary before it will connect, and Helium failed that check two different ways:

- **As an AppImage: `BrowserFileDidNotExist`.** The binary lives in a user FUSE mount, and `fuse_allow_current_process()` requires euid, suid, uid *and* egid, sgid, gid all to match the mounting user. `1Password-BrowserSupport` is setgid `onepassword`, so it fails the gid half unconditionally and cannot stat the binary at all. No AppImage browser can ever pass this. `sg <group> -c 'ls <mountpoint>'` reproduces the denial in one line, which is the cheap way to confirm it.
- **Extracted under `$HOME`: `BrowserProcessVerification(BinaryPermissions)`.** 1Password rejects a binary on a user-writable path. Root-owning the file alone would not help; the parent directories are writable, so the binary could simply be swapped.

`/opt/helium`, root-owned, passes both — which is why the install lives there and not in `~/.local/opt` beside Obsidian, which has no such constraint. `/etc/1password/custom_allowed_browsers` must also name the binary (`helium`), and is now mirrored in `~/system`; without it the extension cannot reach the app no matter where the tree sits.

`~/.config/1Password/logs/BrowserSupport/1Password_rCURRENT.log` names the exact gate that failed on every connection attempt. It is the only thing that made this diagnosable, and it is worth reading first the next time the extension "just asks for a password".

**This is also why the YubiKey swap stays a one-line change.** The extension's unlock runs through `/etc/pam.d/polkit-1`, the same stack as sudo and the desktop app — so replacing `pam_fprintd.so` with a U2F module in the two mirrored PAM files covers the browser extension too, with nothing browser-specific to redo.

Obsidian is the same shape and a different lesson: it self-updates its asar into `~/.config/obsidian`, so the *app* stays current while the Electron shell around it — and the Chromium inside that shell — never moves. It sat on an April 2025 shell running a current 1.13.7 asar. `obsidian-update` updates the half that carries browser CVEs. Upstream publishes no signatures for those assets, unlike Helium's tarballs, which are verified against a pinned key; TLS to github.com is the whole trust boundary there.

## Diagnosing a client with no window

Resolved 2026-08-14, from Helium reporting "Opening in existing browser" with no window after the agetty switch. The method generalises to any Wayland client that appears to start and then isn't there.

Helium's profile is `~/.config/net.imput.helium` — the shim passes no `--user-data-dir`, and the directory is named for the app id, not the binary, which is why `~/.config/helium` globs found nothing. (Helium was an AppImage when this was written; it is an extracted tree in `/opt` now, and the profile path is unchanged.)

`SingletonLock` in a Chromium profile is a **symlink whose target is `hostname-pid`** — here `framework13-2305`. That makes stale-versus-live a one-step check: read the link, test the pid. It was live, so the singleton hand-off was working correctly and the lock was never the problem.

The actual cause was two compositors. Both the greetd-era mango (started 10:47:58) and the agetty-era one (10:57:28) had bound `/run/user/1000/wayland-0`; the later one re-bound the path, so new clients reached it while the earlier kept every client it already had. Helium started seven seconds after the first and stayed attached to it — alive, rendering, on a compositor with no output anyone could see. The fix was killing the orphaned tree.

Two techniques worth keeping:

- **Attribute a client to a compositor by socket inode.** `ss -x -p` gives each compositor's peer inodes; `/proc/<pid>/fd` gives the client's. Intersect them. This is the only way to tell which of two same-named compositors a process is talking to, since the socket *path* is identical for both.
- **`/proc/<pid>/environ` is useless for Chromium.** It overwrites that region setting its process title, so `WAYLAND_DISPLAY` and friends read back as nulls. Don't conclude the variable was unset.

Orphaned Wayland clients mostly exit when their compositor dies, but not all of them — hypridle survived and needed killing by hand.

## Learnings worth keeping

- **`/etc/mango/config.conf` is a sample, not the compiled defaults.** Deleting `border_radius=6` on the grounds that it matched the shipped file removed the rounded corners — the compiled default is 0. The minimal-config principle therefore needs a caveat: delete only what has been confirmed to behave identically when absent.
- **"reload_config merges, doesn't reset" is contradicted.** Removing `border_radius` and reloading took effect immediately. Whatever the earlier observation was, it does not generalise.
- `mmsg dispatch reload_config` returns `{"success":true}` and is a more reliable check than pressing `SUPER+r`, since it distinguishes a failed reload from a setting that did nothing.
- **xbps disables system users, it doesn't delete them.** Removing greetd printed "Disabled `_greeter` system user" — the account survives with its GECOS rewritten to name the uninstalled package and its shell set to `/bin/false`. Deliberate: deleting it would free UID 992 for reuse by the next system package, which would silently re-own any file still carrying it. Leave these alone.
- Iceberg Light inverts the ANSI convention. `color0` at 92% lightness, every bright darker than its normal. Tooling emitting bright colours for emphasis gets the opposite. This was the actual cause of the "colours don't stand out" problem — not saturation, and not accent separation, on which Iceberg Light scored better than Rosé Pine Dawn.
- Mocha's `color15` is darker than `color7` — same inversion class, confined to the white slot. Left stock.
- Simultaneous contrast, not a rendering bug. Identical `#cba6f7` reads darker on fuzzel (light surround) than on mango (dark surround). Sampled and confirmed identical.

## Retired

`/etc/greetd/inquisitor` — a Warhammer 40K themed greetd greeter, deleted with the rest of `/etc/greetd` on 2026-08-14. Recorded because it is worth *not* reviving: it never worked. Its `send_recv` opened a fresh connection to `/run/greetd.sock` per message and wrote bare JSON with no length prefix, while greetd's IPC requires a 4-byte little-endian header on one persistent connection. `create_session` and the password response therefore landed on separate connections with no shared session state, and authentication could not have succeeded. The two helpers that *did* build the header, `greetd_msg` and `greetd_recv`, were never called.

## Open

- **Wallpaper derivation not captured.** The ffmpeg downscale exists nowhere in the dotfiles; a rebuild wouldn't reproduce it.
- **`system-build-state.md` is untracked in the vault** and covers ground this note also covers. Two records of the same machine is one too many.
- **Waybar breaks at mango 0.16.0.** The config uses `dwl/tags` and `dwl/window`; mango deprecates dwl IPC there. Verified 2026-08-17 that the repo still ships `mangowc-0.14.4_1`, which is what's installed, so `xbps-install -Su` won't spring it yet. The replacement is `mango/window` on a recent Waybar, or a custom module.
- Stale assets in `~/Pictures/wallpapers/`: `watchtower-wide.jpg`, `junji-ito-light.png`, `bw_dunes.jpg`, `solar-gradients/`. Deliberately left.

## Closed since 2026-08-14

- greetd is gone from the mirror, the install script and `xbps-manual.txt`. Verified 2026-08-17.
- `50-fprintd.rules` is mirrored at `~/system/etc/polkit-1/rules.d/` with an install block. It grants *enrolment*, which is a separate permission from the verification sudo and polkit perform at the prompt — so a rebuild missing it looks fine until a finger has to be re-registered.
- `~/.claude/CLAUDE.md` describes mango, not dwl.
- The launcher icons under `~/.local/share/icons/hicolor/*/apps/` are deliberately *not* tracked. Both artwork files ship inside the upstream tarballs and are reinstalled by `helium-update` and `obsidian-update` on every run, so tracking them would version a generated file. `~/.gitignore` records the reasoning.
- `~/.local/bin/obsidian` was a *directory* on PATH holding an AppImage, shadowed by a `.zshrc` alias that pointed into it. Both are gone; the shim is a file like every other launcher.

## Links
- [[Desktop made for one — the idea]] — the design question this build is the proof-of-concept for.
