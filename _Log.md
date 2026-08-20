---
id: "20260727000000"
date: 2026-07-27
---
# Log

The running record and capture surface — the one place to jot, newest day on top. Under today's `## YYYY-MM-DD`, drop whatever lands: a task, a fleeting thought, what happened, a note you made. It is not a task ledger — in the Morning Review the captures get sorted out: tasks to a project or the calendar, an idea worth developing to a `#seedling` note, the rest kept as record or deleted.
### 2026-08-19
* 11:41 — Desktop is flat `#000000`. ornatus draws a 64×64 solid black JPEG scaled to cover rather than a 4K photograph: a uniform source upscales to a uniform result, so the decode drops from ~300ms per output to under 100ms, and the file is 245 bytes. JPEG is lossy, so the decode was verified before use — every RGB byte reads zero. The wallpaper had been load-bearing in an unobvious way: waybar's background was `#373d49`, the measured modal colour of the image beneath the bar at two specific geometries, which pinned the bar to that photograph at those resolutions. That constraint is gone with the photograph.
* 12:49 — ornatus reads `config.toml` once at startup and `--refresh` does not reload it. `Config::load_or_create` is called at `main.rs:98`; SIGUSR1 runs the sun-and-theme path only, and the wallpaper is redrawn just when a surface is configured or resized. So the refresh succeeded, logged normally, and rendered nothing new. A wallpaper change needs the daemon restarted. Worth closing in `theme.rs` — the daemon sets state but never reconciles it, which is the same reason a colour-scheme key set by hand stays wrong until the next sunrise.
* 13:05 — Whole system moved to Sanctum, by way of Tokyo Night, which lasted about an hour. Tokyo Night Day sets body text to `#3760bf` — a blue, not a black — which reads wrong on a light ground. Sanctum ships a `sanctum-black` variant whose background is literally `#000000`, so pure black is native to it rather than forced, and its light side puts text at `#161616`. Two deliberate departures from a literal transcription: mako's dark background is `#161616` because Sanctum maps notices to a raised layer rather than the base; and the urgent tag keeps white text while the focused tag takes black, because white measures 5.00:1 on the red where black gives 4.16:1, and the inverse holds on the green accent, 6.29:1 against 3.30:1.
* 13:20 — Neovim's `lua/theme.lua` had never been loaded. `init.lua` required `options` and called `lazy.setup`, and nothing else, so the theme-marker watcher and the light/dark switching in it were dead code since the day they were written. Rosé Pine was configured solely by its lazy `opts`, which is why the editor sat on dark Main whatever the marker said — and why nobody noticed, since a dark editor under a dark theme looks correct half the time. The `require` goes before `lazy.setup` so the `User LazyDone` autocmd exists before lazy fires it.
* 13:51 — Obsidian had crashed seven times between 12:31 and 13:03 and left no usable trace. Crashpad has captured nothing since April, memory was never near a limit, and `dmesg` is restricted. The reason there was nothing to read is the same one that hid waybar's death yesterday: anything mango spawns inherits the session's stdout, so Obsidian's output was going to `/dev/tty1`, the console behind the compositor. Never reproduced, and overtaken by events.
* 15:19 — Helix and IWE installed — `hx` from xbps, `iwe`/`iwes`/`iwec` 0.19.1 built from source into `~/.local/bin`. `iwe init` detected 66 bare-name wiki links and chose `wiki_link_path = "short"`, which rewrites link paths as documents move; set to `"preserve"`, since renaming a note as its wording sharpens is routine here. Auto-format is off against the upstream snippet's default: `normalize` would rewrite 223 of 226 files, so format-on-save would have churned the vault on the first save. The Helix config is project-local so `iwes` does not attach to every README on the machine.
* 16:10 — Deleted the reading inbox. 175 clips in seven weeks — 80 in July, 92 in the first nineteen days of August — against one source note in the vault's entire history, holding seven lines of extracts. The clipper made capture frictionless, and what it left behind was a full-text artifact that read as progress without being any; [[Insecurity work is dangerous because it is frictionless]] names the mechanism, written in April about something else. The URLs are worth keeping and the full text is not, so an index of all 175 sits outside the vault to be scanned once and shed. Three of the clips were 0-byte failures with no URL at all.
* 16:25 — Removed Obsidian. Its remaining draw was the Web Clipper, and the clipper turned out to be the problem rather than a capability worth protecting, so the last argument for keeping the application went with it. 626MB of tree and profile, neither ever tracked; the shim, the updater, the desktop entry and the `obsidian://` scheme handler were, and are gone. `obsidian-update` is recoverable from the dotfiles history if this is ever reversed.
* 17:05 — Vault reduced to what is actually in the graph, and renamed `zettelkasten` → `notes`. Out: four root documents measured at zero links in either direction, both `writing/` drafts, `templates/` (orphaned when Obsidian went), `attachments/` (empty, held open by a `.gitkeep` — the folder existed to exist), and `.obsidian/`. What remains is 32 root zettels, `projects/` and `sources/`. The method name was generating scaffolding: every question it prompted — should essays live here, should attachments — resolved to *it depends what you actually do*, and the standards live in `_Style guide.md`, which does not weaken because the directory is called something plainer. No path anywhere referenced the old name.
* 18:35 — The numbers that prompted all of it: 28 zettels, of which 20 were written in April and none since July. The four meta files come to 44KB and the project notes to 39KB, against 20KB of zettels — the apparatus is four times the size of the thinking it exists to support, and output went to zero as the process got better specified. Twenty of the 28 form a single argument across three clusters — performed versus genuine emotion, the conditions for real work, and writing as the act — which is an essay sequence nobody has named. Reading Ahrens against Doto: claims survive projectlessness, material does not. 21 of 28 zettels already carry their own `## References`, so the April practice was already right — read, form a claim, write the zettel, cite the source in it, and skip the source note entirely.

### 2026-08-18
* 10:07 — The status bar was gone and nothing recorded why: waybar was not running at all, and `wayland-session` had started it as a bare `waybar &` — no supervision, no log — so a mid-session death left no trace to read. It now respawns, records every exit to `~/.cache/waybar.log`, and gives up after ten immediate failures so a broken config fails loudly instead of spinning silently. The battery module chatters its charge state to stdout on every poll, which would bury the useful lines, so that is filtered out. Verified by killing it and by swapping the binary for `false` to force the bail.
* 13:05 — Occasional localized fracture on the ultrawide, still undiagnosed. Only one thing is settled: mango is not doing it. `allow_tearing` defaults to `TEARING_DISABLED` at 0.14.4, the config never sets it, and `check_tearing_frame_allow` — which has exactly one caller — returns false immediately, so the compositor never requests an async flip. Every other explanation was proposed and withdrawn: a bandwidth deficit that had double-counted the 8b/10b encoding, a link "fallback" from HBR2 to HBR that is simply the lowest tier carrying the mode and therefore what a power-optimising driver picks, and a missing FEC that is normal for a sink supporting neither FEC nor DSC. A screenshot caught nothing because the artifact had already passed. What remains untested is whether it appears on a genuinely static screen: if nothing is redrawing and it still fractures, it cannot be a compositor repaint bug and the fault is downstream in the link or the panel.
* 13:10 — The ultrawide moved USB-C ports and came back as DP-3, so the `monitorrule` keyed on `^DP-2$` stopped matching and mango auto-placed it at 2440,1440 rather than the configured origin. Keyed the rule on the panel's serial instead, matched by exact `strcmp` and independent of the port. This corrects 2026-08-12, which recorded DP-2-under-mango versus DP-3-under-dwl as a compositor difference — the connector name follows the port, not the compositor.
* 13:38 — Mirrored `/etc/iwd/main.conf`. The iwd package ships no such file, so it was entirely hand-written and backed up nowhere; a rebuild would have lost it and taken DNS through resolvconf with it. Dropped NetworkManager, installed but never enabled and with a stale wireless lease still under `/var/lib`, and amdvlk, which registered a second Vulkan ICD beside Mesa's RADV for the same GPU. 99MB. Left the iwd/dhcpcd overlap alone: dhcpcd holds the current lease and `dhcpcd.exit-hook` depends on it binding, so the tidier-looking fix would have silently killed timezone-following.
* 16:06 — Handed PDFs to Helium, which already declared `application/pdf` and has PDFium built in, and removed okular. The orphan sweep took 89 packages and 493MB — the whole kf6 stack, the Qt modules only okular used, and the signon and accounts plumbing beneath them. epub, mobi, xps, dvi, tiff and md now have no handler; nothing but okular opened them. Started tracking `mimeapps.list`, which carries the obsidian and claude-cli scheme handlers and would have vanished on a rebuild.
* 16:44 — Pared Neovim from twenty-two plugins to ten and its data directory from 675MB to 43MB. `lsp.lua` had been enabling native `vim.lsp.completion` on every `LspAttach` while nvim-cmp was also loaded — two completion engines at once, and 0.11 had made the plugin redundant without anyone noticing. mason kept a private 576MB tree and, because nvim-lspconfig names jsonls, html and cssls separately, installed `vscode-langservers-extracted` three times over: 288MB for one 96MB package. Servers now resolve off PATH from xbps and one npm global install. JavaScript goes through tsgo — the typescript 7.x package ships a native Go binary that serves LSP directly, so `tsc --lsp --stdio` replaces both typescript-language-server and the tsserver.js only typescript 5.x still carried, with no version pin to maintain. The zettelkasten role left with zk-nvim and render-markdown; the journal auto-open went too, having thrown `E484` on every argument-less launch for some time, reading a `templates/daily.md` and a `journal/` that no longer exist.
* 16:50 — Recorded npm globals in the manifests, without which a rebuild would leave the editor with no language servers and no clue why. Dropped claudecode.nvim and the snacks.nvim it pulled in: Claude Code has only ever been driven from the terminal here, where it reads files off disk and runs commands, so editor integration adds nothing to review or to project work.
* 16:54 — Fixed the `dump.sh` guard, which had never worked. 2026-08-12 recorded it as writing each manifest to a temp file and moving it into place only on success — but `xbps-query -m | sed > .tmp` is a pipeline, so `if` tested sed, and sed succeeds on empty input. A failed query therefore looked like a success and the empty file was moved into place: precisely the silent truncation the pattern was written to prevent, and it had been live for six days. Confirmed against a stub `xbps-query` exiting 1 — the old script took the manifest from 121 lines to 0, the new one leaves it at 121 and says so. The flatpak block was never affected, being a single command with a redirect rather than a pipeline.

### 2026-08-17
* 13:20 — Made 1Password unlock with the fingerprint reader. Three separate blockers, none of them the sensor: polkit had no PAM stack and so fell through to `/etc/pam.d/other`, which is password-only; 1Password refuses polkit outright unless the path to the system bus socket is root-owned, and Void's dbus service creates `/run/dbus` as `dbus:dbus`, so a runit core-service now wins the race by creating it first; and mango ships no polkit authentication agent, so the challenge failed in 7ms without ever reaching the sensor — `hyprpolkitagent` starts with the session now. Corrected the elogind process name in the same pass: `install.sh` and CLAUDE.md both matched `elogind-daemon`, which matches nothing, so the reload branch had been silently taking its else path on every run.
* 13:29 — Extended the reader to sudo, mirroring a package-owned file for the first time. Cut the PAM timeout from the 30 second default to 10 — authentication is serialised, so the reader blocks password entry entirely while it waits.
* 13:57 — Removed the greetd user config the agetty switch left behind, tracked and on disk for three months, pointing at a path that went with the packages. Recorded `hyprpolkitagent` in the package manifest.
* 14:14 — Rebuilt the artspace backup schedule around a laptop that is usually off. The fixed Sunday 10:00 slot had last succeeded 2026-05-17 and quietly missed roughly thirteen weeks; the log looked healthy because nothing ran to write to it. Now `@reboot` and hourly, with a stamp file deciding what is due, flock against overlapping triggers, and a retention floor of the four newest — pruning by age alone deletes every copy at once after a long gap, which is exactly when the backups matter most.
* 16:07 — Found why the 1Password extension in Helium still never used the reader, and it was not the auth stack at all. `1Password-BrowserSupport` verifies the browser binary before it will open a connection, and an AppImage cannot pass: the binary lives in a user FUSE mount, the kernel requires gid as well as uid to match the mounting user, and BrowserSupport is setgid `onepassword`, so it fails that check unconditionally and cannot stat the binary. Extracting under `$HOME` cleared it and hit the next gate — 1Password rejects a binary on a user-writable path, since the parent directories make it swappable. Helium installs to `/opt/helium` now, root-owned, from the signed upstream tarball. That reverses 2026-08-12's move to an AppImage, which was right on the information available then.
* 16:51 — Rewrote `helium-update` for tarballs: it verifies the release signature against a pinned key before unpacking, refuses to swap the tree under a running instance, and installs from nothing when `/opt/helium` is absent — making it the only record of how Helium reaches the machine. Mirrored `/etc/1password/custom_allowed_browsers`, without which a rebuild reproduces the whole bug in silence.
* 17:30 — Updated [[Desktop made for one]]: recorded the browser-verification chain as method, corrected its claim that Helium runs from an AppImage, and moved four resolved items to a Closed section. greetd is gone from the mirror, `50-fprintd.rules` is mirrored, CLAUDE.md describes mango, and the launcher icons are deliberately untracked rather than merely forgotten.
* 17:31 — Gave Obsidian a real install path. It was an AppImage inside `~/.local/bin/obsidian` — a *directory* on PATH, reachable only through a `.zshrc` alias pointing into it, recorded nowhere and updated by nothing. It self-updates its asar into `~/.config/obsidian`, so the app was current at 1.13.7 while the Electron shell around it, and the Chromium inside that, dated from April 2025. Now an extracted tarball at `~/.local/opt/obsidian` with `obsidian-update` beside `helium-update`. Mirrored `50-fprintd.rules` in the same pass: it grants fingerprint *enrolment*, a different permission from the verification sudo and polkit perform, so a rebuild without it looks correct until a finger has to be re-registered.

### 2026-08-14
* 11:15 — Traced Helium's "Opening in existing browser" with no window to two live compositors rather than a stale lock. Yesterday's greetd→agetty switch left the greetd-era mango running; the agetty-era one re-bound `wayland-0`, so Helium — started seven seconds after the first — stayed attached to a compositor with no visible output. Killed the orphaned tree and the duplicate daemon set it carried.
* 11:57 — Removed greetd and tuigreet, then `/etc/greetd` by hand, three of whose four files were hand-written and so unowned by either package. xbps disabled the `_greeter` user rather than deleting it, which is correct — deleting would free UID 992 for reuse.
* 12:10 — Confirmed the tty1 session can't be stacked: `.zprofile` execs, so mango replaces the login shell and the process runit supervises as `agetty-tty1` *is* the session. The duplicate was a one-time artifact of switching managers mid-session, not a standing fault.
* 12:20 — Opened [[Desktop made for one]] as an active project note, carrying the machine's desktop configuration and its open threads. Promoted from Planned; resolves the dangling link in [[Desktop made for one — the idea]].

### 2026-08-13
* 10:00 — Updated color scheme to match new wallpaper.
* 11:19 — Dropped the unfocused window opacity in favour of a 3px lavender focus border.
* 11:21 — Made the Waybar background opaque and wallpaper-toned, with Iceberg Dark glyph colours.
* 11:29
	* Replaced the Rosé Pine theme bundles with Iceberg.
	* Pointed hyprlock's background at the current wallpaper.
* 11:31 — Moved hyprlock to Iceberg Dark alongside the wallpaper change.
* 12:14 — Replaced Iceberg with Catppuccin across all themed surfaces.
* 13:49 — Reduced the scroller's outer padding via scroller_structs, inverted the mouse scroll direction, and removed the dead Autostart section and an untested per-monitor tag rule.
### 2026-08-12
- 08:31 — Committed to dwl as the permanent compositor by hardcoding it into the `.zprofile` session chain and deleting the `~/.config/compositor` selector, making git checkout the recovery path rather than a runtime switch.
- 08:46 — Regenerated the package manifests to capture the state of the system after the dwl migration.
- 12:57
	- Recorded the dwl migration in the Claude Code guidance files and tightened the instructions around asserting system state without verifying it.
	- Replaced the old `launcher` script with native fuzzel for application  launching, and repurposed it as `varia`, a dispatcher covering the six modes fuzzel cannot handle: calc, files, emoji, symbols, clipboard, and system.
	- Fixed the fuzzel theme by moving `border-width` and `border-radius` into the `[border]` section where they are actually read, switched the icon theme to Papirus for its full 88,300-icon set, and raised the file-search line count.
	- Regenerated the manifests after installing Papirus and removing GIMP and foot.
- 14:29 — Replaced dwl with MangoWM 0.14.4 from the Void repositories, installed alongside scenefx and cJSON against the existing wlroots 0.19.3. Wrote a minimal configuration stating only the values that differ from the shipped default: scroller layout on all nine tags with a 0.33 proportion preset for three columns on the ultrawide, 6px corner radius, 0.92 unfocused opacity, no borders, and no animations. Discovered in the process that the ultrawide enumerates as DP-2 under mango rather than DP-3 as it did under dwl.
- 15:00 — Added Waybar using its dwl IPC modules, which mango 0.14.4 still speaks, with a static dark stylesheet in Inter SemiBold and JetBrainsMono Nerd Font. Renamed `dwl-session` to `wayland-session` and made it the single home for session startup, removing the duplicated `exec-once` list from the compositor config. Pointed `.zprofile` at mango, making it the login desktop.
- 15:06 — Removed the dwl binary, the orphaned `dwl-status` script, and the `.gitignore` rule that excluded the binary, keeping the source tree as reference for the eventual Rust bar.
- 15:08 — Dropped `wlroots0.19-devel`, orphaned now that nothing is built
  against it, and swept 206MB of unrelated orphaned dependencies left behind by GIMP and foot.
- 15:22 — Corrected the ornatus README, which still described the retired sixteen-frame solar gradient, a `wallpaper_dir` key that no longer exists, and a `mango.conf` theme mapping that was never needed. Documented the reload chain accurately, including the `gsettings` color-scheme call that reaches Chromium and Electron applications through the GTK portal.
- 15:27 — Fixed the remaining file headers in `theme.rs`, `location.rs`,
  `wayland/mod.rs` and the systemd unit, all of which still called ornatus a
  solar-gradient daemon.
- 15:30 — Rewrote `dump.sh` to write each package manifest to a temporary file and move it into place only on success, so a failed query can no longer truncate the record to nothing — a failure mode indistinguishable from a legitimate removal, and one nearly mistaken for it earlier in the day.
- 15:35 — Noted `check-ignore -v` in the `.zshrc` dotfiles comment as the way to confirm which `.gitignore` rule is hiding a file, a trap the existing comment warned about without saying how to diagnose.
- 15:45 — Switched Helium from a 461MB extracted tree to a single AppImage, reducing the install to 153MB and making future updates an atomic file replacement rather than a directory swap
- 15:52 — Wrote `helium-update`, which checks GitHub for a newer release, confirms before acting, downloads to a temporary file, verifies the new binary reports the expected version, and only then swaps it into place. Its first run found an update waiting and moved from `0.15.3.1` to `0.15.4.1`.
- 16:42 — Brought the greetd configuration into the system mirror and its install script, and removed the tuigreet keybinding overrides after establishing that the hint bar is hardcoded to F2, F3 and F12 regardless of the flags, so the displayed labels had been contradicting the actual bindings. Confirmed that unprivileged power actions must go through `loginctl`, since the bare binaries are root-only and unusable from the greeter.
- Removed the dead `~/.config/GIMP` and `~/.config/Ladybird` directories.
- Deleted two dangling runit service symlinks, `polkit` and `shhd`, that had been failing silently since March.
- Deleted two zettelkasten notes superseded by the migration.
- Verified the ornatus startup race is genuinely fixed: both outputs attached at zero milliseconds on a real boot.
### 2026-08-11
* 09:00 — Took dogs to PawCommons for a ½ day of play and grooming.
* 09:35 — Evaluated stock dwl on tty2.
* 12:00 — Fixed `PATH`/`PKG_CONFIG_PATH` duplication, then added tty1 guard and compositor selector to `.zprofile`.
* 12:55 — Built dwl v0.8 with bar, movestack, and gaps patches; wrote `config.h` carrying MangoWM keybinds.
* 13:40 — Cut over to dwl as login compositor, and removed Quickshell and MangoWM from the boot path.
	* Configuration is now compile-time which means every change costs a rebuild and a session.
* 13:25 — Added bar patch to dwl with a transparent background.
* 13:54 — Updated bar font to `iMWritingQuat Nerd Font` and a transparent selected scheme.
* 14:11 — Implemented code to hide empty tags.
* 14:24 — Replaced numbered tags with dots (bullets), changed the selected scheme color to Rosé Pine gold (0xf6c177), and hid the layout symbol.
* 15:16 — Pay the California Franchise Tax Board penalty.
* 16:54 — Installed `xdg-desktop-portal-gtk`; Helium and Obsidian now follow the theme live via Ornatus.
	* `sweep_unattached`'s warning in `/tmp/ornatus.log` is still the only instrument for the unproven startup race fix.
* 7:17 — Repaired `launcher` system menu — four of five actions were silently broken (swaylock, `loginctl`, `mmsg`); replaced with hyprlock and runit commands.
* 18:26 — Replaced wifi name and volume text with Nerd Font glyphs in the status bar.
* 18:48 — Added signal-driven bar refresh for instant volume feedback.
* 18:56 — Replaced `pkill -f` with a PID file after the pattern matched the session pipeline and forced two logouts.
* 19:05 — Uninstalled MangoWM and Quickshell; 1,746 lines removed from version control.

- [x] Migrate from MangoWM to DWL with Bar patch.
## 2026-08-10
- 10:00 — Attend the anatomy scan appointment with Rachel.
- 11:30 — Brunch with Rachel.
- 12:30 — Remove shower doors in the second bathroom.
- 13:30 — Update my PMI community profile.
- 18:00 — Interview with PMI-SD representative for mentorship program.

- [x] Look up distance learning law programs.

## 2026-08-09
- 11:37 — Spent thirty minutes trying to resync Obsidian mobile with my repository, but I seem to have an issue with my fine-grained access token. The quickest solution is to make a classic token with repository scope instead; it has no per-repository selection step to get wrong, so if the clone still give a 404 error message you'll know the problem is elsewhere.
- 13:00 — Hands On Safety Test (HOST) at the San Diego Fine Woodworkers Association.
- 19:00 — Dinner for Clendon’s birthday at Vigilucci’s in Carlsbad.

- [x] Add reminder to calendar to sign up for the SDFWA hand tool joinery class on September 1.
## 2026-08-07
- [x] Find the following articles and add to the reading inbox:
	- [x]  Separate Writing and Formatting
	- [x] How to Record and Retrieve Anything You've Ever Had to Look Up Twice
	- [x] How to effectively write quality code with AI
	- [x] Thin Desires Are Eating Your Life
	- [x] Amazon's Ring and Google's Nest — U.S. Surveillance State
	- [x] A mini data center
	- [x] How I Learned to Read Way, Way More
	- [x] Cognitive Debt: When Velocity Exceeds Comprehension
	- [x] uv is the best thing to happen to the Python ecosystem in a decade
	- [x] Rob Pike's 5 Rules of Programming
	- [x] The peril of laziness lost
	- [x] Meetings are forcing functions
	- [x] The Angine de Poitrine Argument for UBI
	- [x] The Git Commands I Run Before Reading Any Code
	- [x] June thoughts — newsletter
	- [x] How to help someone use a computer
	- [x] You Can't Tell People Anything — Habitat Chronicles
	- [x] agents need control flow, not more prompts
	- [x] Nobody Gets Promoted for Simplicity
	- [x] Good software knows when to stop
	- [x] Media vita in morte sumus
	- [x] The Creative Power of Walking
	- [x] Using LLMs at Oxide — also relevant to the Personal Website / job-search toolkit
	- [x] Bullshit Jobs — Graeber's theory
	- [x] How to Set up a Local Coding Agent on macOS — Kyle Howells

## 2026-08-06
- 13:10 — Submitted an application for the Director of Product role with Flux Power. Separately I message Michele
- [x] Schedule an appointment with a Primary Care doctor to establish care, have an annual physical, and have blood/lab work completed.
- [x] Determine what systems to use for class management, payments, etc. #yoga-studio
- [x] Organize your bathroom cabinets. #waiting 
- [x] Review [Cog](https://lab.puga.com.br/cog/) and determine whether to use or not.
- [x] Schedule a regular garage cleaning.
- [x] Pay the IRS penalty.
- [x] Watch [“How To Remember Everything You Read”](https://www.youtube.com/watch?v=UBdlprawTkA)
- [x] Watch [“How To Journal Like A Stoic Philosopher”](https://www.youtube.com/watch?v=6211qMWYz90)
- [x] Watch [“The Notebook System That Saved My Brain”](https://youtu.be/BDde5vwj83E?si=ZUdutti6PGZki8M0)
- [x] Watch [“9 Boring Habits That Will Put You Ahead of 99% of People”](https://youtu.be/LXaFHI_2Hus?si=VvabE5zhzaZ4lTrd)
## 2026-07-31
- 12:03 — Applied to be a mentor for the PMI San Diego chapter this fall.

## 2026-07-28
- 09:30
	- I time-stamped yesterday's entries, taking the times from the git history so that each bullet carries the time its thread of work began.
	- I settled that work is recorded under the date its session began, so a sitting that runs past midnight stays with the evening it started, and I moved the Morning Review rewrite back under 07-27 on that basis.
	- I decided against recording a time frame for each entry, because a range claims a continuity the work rarely has and it forces a rule for splitting a session at midnight.
	- I switched the log to 24-hour times so that they match the `YYYY-MM-DD` dates and the `YYYYMMDDHHmmss` id.
	- I settled the entry format as a bullet, the time, an em dash, and then the sentence, keeping the bullet because it is what carries the nesting.
- 13:00 — Begin drive from Park City to San Marcos.

## 2026-07-27
- 12:57
	- I overhauled the vault by moving to a single running log that replaces the weekly-note stream, renaming every zettel to a title-based filename, and pinning the system notes so they sort above the zettels. 
	- I formalized the task system around three surfaces, the log, project notes, and the calendar, and I defined a daily Morning Review that runs them.
- 13:39 — I fixed the Obsidian Web Clipper, which had been wrapping author names in dead wikilinks, and I settled how clipped articles flow into the reading inbox.
- 18:22 — I adopted project-based learning, in which each skill is learned by building a real project and the learning lives in that project's note, and I created [[_Learning]] as the counterweight for the deliberate study that building alone skips.
- 20:31 — I scoped ArtSpacesSoCal as an active, learn-by-building Go project, established that Claude should tutor rather than write the code, and set a Go refresher as the first step.
- 21:03 — I set up a writing folder as the home for my original writing and moved my first two essays into it.
- 21:18
	- I rewrote the Morning Review as five steps against Tiago Forte's model, extending the mnemonic to Every Commitment Needs Tracking Today and pulling the choice of what to do today out of Tasks into a step of its own.
	- I gave both reviews their own headings so their bodies read standalone, which lets the same text serve as the description of the recurring calendar events.
	- I replaced "a thought with legs" with "an idea worth developing" across the style guide and this log so the vocabulary stays single.
- 00:38 — I settled that a phrase or idea extracted from a book becomes a zettel directly, because the reading inbox holds only clipped articles that have not been judged yet.

**To route (carried from the last weekly plan):**
- [x] Make hotel reservation for our drive from UT to CA.
- [x] Request Shane Peters forward his emails with the City of San Marcos.
- [x] Audit your task manager (Things3) and migrate tasks to Obsidian (Upcoming; Areas & Projects).
- [x] Add tasks with dates and recurring tasks to your Google Calendar.

## 2026-04-23
- 08:30 — Appointment with Dr. Hirsch at *San Deigo Ear, Nose, and Throat*.
	- My sleep test score was '5' which is borderline for sleep apnea.
	- The medication has helped with my breathing, but I have a definitive deviated septum on my right side.
	- The recommendation is to lose ~10-20 lbs of weight, sleep on my side, use nasal strips, and continue to address my allergy issues.
	- If I continue to have issues in the next 3-6 months inquire to schedule a septoplasty.
- Distilled Sari Azout's [Letter to a friend who is thinking of starting something new](https://www.sariazout.com/p/letter-to-a-friend) into six zettels and created a seventh synthesis zettel ([[Insecurity work is dangerous because it is frictionless]]).
- Downloaded Obsidian and migrated note system. This required very little effort as the Neovim system was already using Markdown.

**Notes created:**
- [[Insecurity work produces no progress but repeats without notice]]
- [[Insecurity work is dangerous because it is frictionless]]
- [[Ideas require emotional capital as much as financial capital]]
- [[The passion economy places a double burden on work]]
- [[Pre-commit your stopping conditions before the pressure arrives]]
- [[Industry norms pull a business toward conformity]]
- [[The antidote to outcome-driven anxiety is treating the process as the destination]]

## 2026-04-22
**Notes created:**
- [[Solving the right problem unlocks creative immersion]]
- [[The moment to begin writing is a threshold, not a schedule]]
- [[Emotional restraint is most powerful at the moment it breaks]]

## 2026-04-21
- 07:00 — Oil change appointment at the Chevrolet dealership in Carlsbad.
- 08:00 — Rescheduled my dentist and orthodontia appointments.

**Notes created:**
- [[A novel begins with a vital, surprising, unanswered question]]
- [[A novel requires months or years of incubation before serious writing begins]]
- [[The first challenge in writing prose is finding the voice and tone]]
- [[Overcoming the confidence struggle requires giving yourself permission to write privately]]

## 2026-04-02
- Completed the overhaul of my LinkedIn profile, working on the experience, education, and skills sections.
- Continued to make progress on my Apple to Linux migration, setting up screenshot utilities and 1password.
- Created zettels on the article about [[Surface acting]].

**Notes created:**
- [[Surface acting]]
- [[Social expectations drive performative emotions]]
- [[Genuine emotions build psychological resources]]
- [[Anhedonic flatness]]
- [[Contentment performance creates a vicious cycle]]

## 2026-04-01
- Purchased my first orbital sander.
- Worked with Claude on defining target industries and roles for my job search.
- Spent more time than I should have working through my resume.

## 2026-03-31
- Most of today was spent focused on migrating from Things3 to Todoist.
	- I used Claude to assist in establishing a new system to managing tasks.
	- This system keeps projects at a surface level, relies more heavily on a daily review, and allocates the weekly review to focus on auditing work in progress.
- In discussing with Claude what roles I should seek out, I'm able to position myself as a data/technical product manager.
	- The personal blog/website will be a component of my job search.
- Paid the annual CAM difference for the studio space.
- I need to schedule more time for reading, note-taking/thinking, and writing.

## 2026-03-29
- Read several more chapters of the second Eisenhorn book 'Malleus'.
- Set up my zettelkasten, opting to use the `zk` plugin instead of cobbling together two or three others.
- I had shell scripts added to my `.zshrc` in case I ever run into an issue with `zk` and need to deactivate it.
- Migrated two notes from my git repository into the new local directory.

## 2026-03-26
- Created a git repository for notes on GitHub.
- Implemented a spell check plugin for Neovim.
- Implemented a feature to automatically save zettels using pure Neovim Lua and built-in commands.

## 2026-03-25
- Completed the 'Premium Audit' that was requested by Employers.
- Switched my $SHELL on my Framework 13 from `bash` to `zsh`.
	- I ported over my existing `.zshrc` from my MacBook Airmo and modified it to remove references to MacOS specific software.
- I set up this note taking system using the Zettelkasten (German for "slip-box") knowledge management method popularized by Niklas Luhmann.
- Scripts were added to the `.zshrc` to generate each note type using templates.

**Notes created:**
- [[A zettel is atomic and self-contained]]
- [[A zettel gains meaning through its connections]]
