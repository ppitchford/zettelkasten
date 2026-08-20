---
id: "20260814121839"
date: 2026-08-14
---
# Desktop made for one

Single-user desktop environment on the Framework 13 / Void Linux machine — compositor, session, theme, bar, lock. The proof-of-concept the idea drives, and the standing record of how the machine is actually configured. Sessions should read this before changing anything on the desktop.

State below was current on 2026-08-14, revised 2026-08-20. Claims about *what is installed or enabled* decay; verify before relying on one. Claims about *method* — how a thing behaves, how to diagnose it — do not.

## Compositor

MangoWM 0.14.4 (`/usr/bin/mango`, packaged), config at `~/.config/mango/config.conf`. Unfocused opacity removed; focus is signalled by a 3px border in Sanctum's dark accent green `#669961`, with unfocused borders fully transparent `0x00000000` so focus changes don't shift geometry. `rootcolor` is `#000000` to match the desktop, but ornatus's layer-shell surface covers it — it only shows in the window before ornatus attaches, so it is a startup backstop rather than the thing that makes the desktop black. `urgentcolor` `#c54128`.

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

**Flat black since 2026-08-19.** `~/.config/theme/black.jpg`, generated with `ffmpeg -f lavfi -i color=c=black:s=64x64 -frames:v 1 -q:v 1`. 245 bytes, and 64×64 rather than full-resolution because ornatus scales to cover — a uniform source upscales to a uniform result at near-zero decode cost, against ~300ms per output for the 4K JPEG it replaced. JPEG is lossy, so the decode was verified before use: `ffmpeg -i black.jpg -f rawvideo -pix_fmt rgb24 - | od -An -tu1 -v | sort -u` returns only `0`.

ornatus takes a single `wallpaper` path, not one per variant, so the desktop is black at all hours — the light bundle dims the applications while the ground stays `#000000`. Intended.

hyprlock no longer consumes a wallpaper at all; it draws `color = rgb(000000)` directly, which decouples the lock screen from the image file that previously had to exist for it not to fail silently to black.

It lives under `~/.config/theme/` rather than `~/Pictures/` because it is part of the theme rather than a photograph, and because that directory is already tracked. Safe to put there: ornatus never enumerates `theme_dir` — `theme.rs` only joins the fixed names `current` and `<variant>/<source_name>` — so a stray file at its root is ignored.

`wallpaper-1-3840.jpg` and its ffmpeg derivation are retired. The measured `#373d49` that the bar was pinned to is now historical.

## Theme

**Sanctum** since 2026-08-19 — the lineage is now Rosé Pine → Iceberg → Catppuccin → Tokyo Night → Sanctum. Palettes lifted from [github.com/jdanielmourao/obsidian-sanctum](https://github.com/jdanielmourao/obsidian-sanctum): hues and the grey ramp in `src/scss/variables/colors.scss`, role assignments in `theme.scss`. Obsidian itself was removed on 2026-08-19, so the palette now outlives the application it was written for.

Dark is the theme's own `sanctum-black` variant — `--background: rgb(var(--black))`, a literal `#000000`, so pure black is native to Sanctum rather than an override forced onto it. Light is `sanctum-default-light`: `#f4f4f0` ground, `#161616` text.

The greys are a warm-neutral ramp on a Le Corbusier Architectural Polychromy basis, built so each step greyscales to a matching value — a different character from Tokyo Night's cool navy, which is what it briefly replaced. Tokyo Night lasted a few hours and fell to one property: its Day variant sets body text to `#3760bf`, a blue rather than a black, which reads wrong on a light ground.

Two deliberate departures from a literal transcription:

- Mako's dark background is `#161616`, not `#000000`. Sanctum maps notices to `--background-modifier-message: var(--layer-1)`; a notification is a raised layer in this theme, not base background.
- Sanctum's `--text-on-accent` is white uniformly. On the green accent that measures 3.30:1 against black's 6.29:1, so the focused-tag text is black and only the red urgent state takes white (5.00:1, against black's 4.16:1).

Sanctum defines no terminal ANSI set — it is an Obsidian theme. The sixteen colours in `kitty.conf` are derived from its ten-step hue ramps and are therefore local authorship, not upstream.

Bundle files under `~/.config/theme/{light,dark}/`: `kitty.conf`, `fuzzel.ini`, `mako.conf`. Fuzzel's border matches mango's accent in each variant so the focus signal stays one colour throughout.

## Waybar

Static, outside ornatus's `MAPPINGS`. Opaque `#000000`, Sanctum glyphs, focused tag on the same green as the window border.

**It does not follow the sun, and that is the intent.** The bar was previously pinned to the wallpaper rather than the theme, which made the fourth `Mapping` and `SIGUSR2` reload in `theme.rs` unnecessary by accident. It is now black by choice: confirmed 2026-08-19 that the bar and the lock screen should stay black at all hours, including against the light desktop. The fourth `Mapping` is therefore not wanted — don't add it, and don't record this as drift.

Uses `dwl/tags` and `dwl/window`; mango 0.14.4 still advertises `zdwl_ipc_manager_v2`. Mango deprecates dwl IPC at 0.16.0, at which point these modules stop working — the replacement is `mango/window` on a recent Waybar, or a custom module. That is the thing that breaks on upgrade.

## Hyprlock

Black at all hours by design, like the bar — confirmed 2026-08-19, not an oversight of the solar switch. Solid `color = rgb(000000)` — no `path` at all, which removes the failure mode that bit before, where a configured image (`junji-ito-dark.png`) had stopped existing and hyprlock failed to black silently. Sanctum colours, green outline on the input field. Verified visually 2026-08-19. Fingerprint and password auth confirmed working in parallel, including through a suspend and resume cycle.

## Neovim

Tracks the theme marker like everything else — and **had never actually done so**. `init.lua` required `options` and called `require("lazy").setup("plugins")`, and nothing else: `lua/theme.lua` was never required, so its file watcher and light/dark switching had been dead code since they were written. Rosé Pine was configured solely by its lazy `opts`, which is why the editor sat on dark Main whatever `~/.config/theme/current` said, and why nobody noticed — a dark editor under a dark theme looks correct half the time. Fixed 2026-08-19; the `require("theme")` goes *before* `lazy.setup` so the `User LazyDone` autocmd exists before lazy fires that event.

Sanctum ships no Neovim port. `folke/tokyonight.nvim` is installed and kept purely as the highlight engine, with its colour slots overwritten by Sanctum's through `on_colors` — every group tokyonight defines then resolves to a Sanctum colour. lualine runs `theme = "auto"` so it derives from the active colorscheme instead of contradicting it. Rosé Pine removed.

Verified across a live marker flip: light gives `bg=#f4f4f0 fg=#161616`, dark gives `bg=#000000 fg=#c7c5c2`.

## Application theming through the portal

GTK, Chromium and Electron applications follow the light/dark switch, and the whole path is `xdg-desktop-portal`. ornatus sets `org.gnome.desktop.interface color-scheme` to `prefer-dark` / `prefer-light` (`theme.rs:155`); `xdg-desktop-portal-gtk` reads that GSetting and re-exports it as the `org.freedesktop.appearance` `color-scheme` key, which is what applications actually subscribe to.

**Verified end to end on 2026-08-19**, by flipping the key by hand and watching Helium and Obsidian — removed later the same day — both go dark against a still-light desktop:

```sh
gsettings set org.gnome.desktop.interface color-scheme prefer-dark
gdbus call --session --dest org.freedesktop.portal.Desktop \
  --object-path /org/freedesktop/portal/desktop \
  --method org.freedesktop.portal.Settings.ReadOne \
  org.freedesktop.appearance color-scheme
```

The reply is `uint32`: **0 no preference, 1 prefer dark, 2 prefer light** — note this is *not* the same encoding as the GSetting's strings, and reading the GSetting alone doesn't prove the portal is re-exporting it. Restore with `prefer-light` afterwards; nothing does it for you (see Open).

## Apps XBPS doesn't own

Helium is the only application installed outside the package manager, so `xbps-install -Su` never touches it and nothing else records how it got here. `helium-update` on PATH is the sole install record, and it installs from nothing when `/opt/helium` is absent. A rebuild is therefore clone dotfiles → `~/system/install.sh` → run it. The tree is not tracked; it is far too large, and `~/.gitignore` says so where the rule would otherwise look like an oversight.

Obsidian was the second such application until 2026-08-19 (see Retired).

**1Password decides which browsers may exist, and its reasons are invisible from the outside.** Resolved 2026-08-17, from the extension in Helium never using the fingerprint reader. The whole chain is extension → `1Password-BrowserSupport` → desktop app → polkit → `pam_fprintd` → reader, and if the first hop fails the extension silently falls back to asking for the account password. Nothing in the UI says why.

BrowserSupport verifies the browser binary before it will connect, and Helium failed that check two different ways:

- **As an AppImage: `BrowserFileDidNotExist`.** The binary lives in a user FUSE mount, and `fuse_allow_current_process()` requires euid, suid, uid *and* egid, sgid, gid all to match the mounting user. `1Password-BrowserSupport` is setgid `onepassword`, so it fails the gid half unconditionally and cannot stat the binary at all. No AppImage browser can ever pass this. `sg <group> -c 'ls <mountpoint>'` reproduces the denial in one line, which is the cheap way to confirm it.
- **Extracted under `$HOME`: `BrowserProcessVerification(BinaryPermissions)`.** 1Password rejects a binary on a user-writable path. Root-owning the file alone would not help; the parent directories are writable, so the binary could simply be swapped.

`/opt/helium`, root-owned, passes both — which is why the install lives under `/opt` rather than anywhere in `$HOME`. `/etc/1password/custom_allowed_browsers` must also name the binary (`helium`), and is now mirrored in `~/system`; without it the extension cannot reach the app no matter where the tree sits.

`~/.config/1Password/logs/BrowserSupport/1Password_rCURRENT.log` names the exact gate that failed on every connection attempt. It is the only thing that made this diagnosable, and it is worth reading first the next time the extension "just asks for a password".

**This is also why the YubiKey swap stays a one-line change.** The extension's unlock runs through `/etc/pam.d/polkit-1`, the same stack as sudo and the desktop app — so replacing `pam_fprintd.so` with a U2F module in the two mirrored PAM files covers the browser extension too, with nothing browser-specific to redo.

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

- **ornatus reads `config.toml` once at startup and `--refresh` does not reload it.** `Config::load_or_create` is called at `main.rs:98`; SIGUSR1 runs `app.refresh()`, which is the sun-and-theme path only. A wallpaper change therefore needs the daemon restarted, not signalled — and the failure is quiet, because the refresh succeeds and logs normally while rendering nothing new. The wallpaper is also only redrawn when a surface is first configured or resized, never on the refresh tick.
- **A file in `~/.config` is not evidence a package is installed.** xbps leaves config behind on removal. `~/.config/okularrc` and `okularpartrc` are still present for an okular that was uninstalled; reading them as an installed app produced a wrong claim about which Qt applications run here. Check `xbps-query`.
- **"reload_config merges, doesn't reset" is contradicted.** Removing `border_radius` and reloading took effect immediately. Whatever the earlier observation was, it does not generalise.
- **xbps disables system users, it doesn't delete them.** Removing greetd printed "Disabled `_greeter` system user" — the account survives with its GECOS rewritten to name the uninstalled package and its shell set to `/bin/false`. Deliberate: deleting it would free UID 992 for reuse by the next system package, which would silently re-own any file still carrying it. Leave these alone.
- Iceberg Light inverts the ANSI convention. `color0` at 92% lightness, every bright darker than its normal. Tooling emitting bright colours for emphasis gets the opposite. This was the actual cause of the "colours don't stand out" problem — not saturation, and not accent separation, on which Iceberg Light scored better than Rosé Pine Dawn.
- Mocha's `color15` is darker than `color7` — same inversion class, confined to the white slot. Left stock.
- Simultaneous contrast, not a rendering bug. Identical `#cba6f7` reads darker on fuzzel (light surround) than on mango (dark surround). Sampled and confirmed identical.

## Retired

`/etc/greetd/inquisitor` — a Warhammer 40K themed greetd greeter, deleted with the rest of `/etc/greetd` on 2026-08-14. Recorded because it is worth *not* reviving: it never worked. Its `send_recv` opened a fresh connection to `/run/greetd.sock` per message and wrote bare JSON with no length prefix, while greetd's IPC requires a 4-byte little-endian header on one persistent connection. `create_session` and the password response therefore landed on separate connections with no shared session state, and authentication could not have succeeded. The two helpers that *did* build the header, `greetd_msg` and `greetd_recv`, were never called.

**Obsidian**, removed 2026-08-19 along with its Web Clipper. The clipper was the last real argument for keeping the application, and the clipper turned out to be the problem rather than a capability worth protecting — 175 clips in seven weeks against one source note. 626MB of tree and profile, neither ever tracked; the shim, the updater, the desktop entry and the `obsidian://` scheme handler were tracked, and are gone. `obsidian-update` is recoverable from the dotfiles history if this is ever reversed.

The install-shape lesson outlives the application. Obsidian self-updated its asar into `~/.config/obsidian`, so the *app* stayed current while the Electron shell around it — and the Chromium inside that shell — never moved: an April 2025 shell running a current 1.13.7 asar. Any self-updating Electron application has that split, and an updater is only doing its job if it covers the half carrying browser CVEs. Upstream published no signatures for those assets, unlike Helium's tarballs, which are verified against a pinned key; TLS to github.com was the whole trust boundary there.

## Open

- **Qt applications ignore the theme.** `QT_QPA_PLATFORMTHEME` is unset, so Qt clients don't follow the portal colour-scheme that GTK, Chromium and Electron all honour. No install is needed — `libqxdgdesktopportal.so` already ships in `/usr/lib/qt6/plugins/platformthemes/` — but the variable has to be exported in `.zprofile` ahead of the compositor `exec`, since apps spawned from mango keybinds inherit the compositor's environment rather than `wayland-session`'s. Note that the Qt client this actually affects is unidentified: `qt6-base` is installed and something wrote `QtProject.conf` on 2026-08-17, but okular is *not* installed and was named in error.
- **`system-build-state.md` is untracked in the vault** and covers ground this note also covers. Two records of the same machine is one too many.
- **Waybar breaks at mango 0.16.0.** The config uses `dwl/tags` and `dwl/window`; mango deprecates dwl IPC there. Verified 2026-08-17 that the repo still ships `mangowc-0.14.4_1`, which is what's installed, so `xbps-install -Su` won't spring it yet. The replacement is `mango/window` on a recent Waybar, or a custom module.
- Stale assets in `~/Pictures/wallpapers/`: `watchtower-wide.jpg`, `junji-ito-light.png`, `bw_dunes.jpg`, `solar-gradients/`. Deliberately left.

## Closed since 2026-08-14

- **The black desktop survives a rebuild.** `~/.config/ornatus/config.toml` and `~/.config/theme/black.jpg` are both tracked as of 2026-08-19; the config needed a `!` line, the image needed only to be moved into the already-tracked theme directory. Before that a fresh clone would have got Sanctum everywhere sitting on ornatus's default wallpaper — the one part of the change that would not have reproduced.
- greetd is gone from the mirror, the install script and `xbps-manual.txt`. Verified 2026-08-17.
- `50-fprintd.rules` is mirrored at `~/system/etc/polkit-1/rules.d/` with an install block. It grants *enrolment*, which is a separate permission from the verification sudo and polkit perform at the prompt — so a rebuild missing it looks fine until a finger has to be re-registered.
- `~/.claude/CLAUDE.md` describes mango, not dwl.
- The Helium launcher icon under `~/.local/share/icons/hicolor/*/apps/` is deliberately *not* tracked. The artwork ships inside the upstream tarball and is reinstalled by `helium-update` on every run, so tracking it would version a generated file. `~/.gitignore` records the reasoning. The Obsidian icon beside it went with the application on 2026-08-19.

## Links
- [[Desktop made for one — the idea]] — the design question this build is the proof-of-concept for.
