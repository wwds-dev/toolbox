# Toolbox

Small utilities kept apart from the lab's standalone apps — none of these is a product
in its own right, launched separately with its own window and lifecycle. Each is its own
git repo, included here as a submodule.

- `unblock_tracker/` — PySide6 "Unblock Tracker" app; watches whether an Instagram profile
  has unblocked you.
- `convert_epub/` — "Ebook Converter.app"; any-format ebook/doc conversion via Calibre.
- `image_tools/` — small image-prep utilities (`dpi/`, `rename/`, `move/`).

A proper GUI front end for Toolbox (its own small launcher, in the spirit of Lab Hub) is
planned but not built yet.

## Layout

Each member's own README is the full reference; this is the map, as of the submodule
commits recorded here (Nov 2026).

### `unblock_tracker/` — Unblock Tracker

PySide6 app, versioned `v<MAJOR>.<BUILD>` (shown in the window title). Besides
reachability it diffs follower/following/post counts, follower membership and
relationship flags, and can watch several handles in one run.

| Path | |
|---|---|
| `main.py` | Entry point; `--selftest` checks a build (icon, config outside the bundle, Keychain round-trip) |
| `unblock_tracker/` | Engine, no Qt dependency: `checker.py` (logged-in Selenium and anonymous checks, pure `classify`), `parsing.py`, `signals.py` (snapshot diffs), `monitor.py` (run loop and event store), `notifiers.py` (Telegram / Pushbullet), `secrets.py` (macOS Keychain), `proxies.py`, `config.py`, `version.py` |
| `ui/` | Settings, Monitor and Events tabs, plus `help_dialog.py`, which renders `docs/GUIDE.md` |
| `docs/GUIDE.md` | End-user guide, opened in-app from **How to use** (⌘?) |
| `ROADMAP.md` | Open work and the reasoning behind it |
| `config.example.json` | Committed settings template; the real `config.json` is git-ignored and secrets live in the Keychain |
| `build_app.sh` | PyInstaller build into `dist.noindex/`; `--install` copies to `/Applications` |
| `VERSION`, `scripts/stamp_version.py` | Lab-wide version scheme: hand-edited MAJOR, BUILD from `git rev-list --count HEAD`, stamped into `_build_info.json` for frozen builds |
| `tests/` | pytest suite; touches no network, browser or real Keychain |

### `convert_epub/` — Ebook Converter

Drag-and-drop queue for files or whole folders; any format Calibre reads to any
format it writes, by shelling out to Calibre's `ebook-convert` (not bundled —
`brew install --cask calibre`). Same `v<MAJOR>.<BUILD>` scheme, shown in the title.

| Path | |
|---|---|
| `main.py` | Entry point; `--selftest` converts a real file end to end |
| `ebook_converter/` | `formats.py` (pinned read/write lists), `calibre.py` (find and run `ebook-convert`), `jobs.py` (what converts, where to, what is skipped), `runner.py`, `config.py` (settings in `~/Library/Application Support/Ebook Converter/`), `version.py` |
| `ui/` | `main_window.py` (queue, options, Retry), `worker.py` (background conversion thread) |
| `build_app.sh` | PyInstaller build into `dist.noindex/`, self-tests the packaged binary; `--install` copies to `/Applications` |
| `VERSION`, `scripts/stamp_version.py` | Version scheme, as above |
| `tests/` | pytest; `-m "not slow"` skips the real-Calibre integration tests |

### `image_tools/` — print-prep scripts

Plain Pillow scripts with no command-line arguments: every input and output path is a
constant at the top of the script, edited before each run.

| Path | What it does | Paths currently in the code |
|---|---|---|
| `dpi/dpi_resize_exact.py` | Every `.png` → RGBA, resized (stretched, aspect ratio not kept) to exactly 3600×4800 px — 12"×16" at 300 DPI — and saved as PNG with 300-DPI metadata. A failing file is reported and skipped | in `~/Documents/lab/websites/altmerch.store/RAW`; out `/Users/as/Documents/lab/websites/altmerch.store/changed_DPI` |
| `dpi/dpi_fit_canvas.py` | `.png`/`.jpg`/`.jpeg` scaled proportionally and centred on a 3600×4800 px, 300-DPI canvas (black by default, `background_color`) — no distortion | in `~/Documents/lab/websites/altmerch.store/Sweet Poison`; out intended as `…/Sweet Poison/output_ready_for_print` (see `image_tools/TODO.md`: the join is broken) |
| `rename/rename_for_print.py` | Renames the images in the source folder **in place** to `sweet_poison_001`, `_002`, …, numbering on from the highest index already in the target folder; both folders must exist; stops at a name collision | source `/Users/as/Documents/lab/websites/altmerch.store/Sweet Poison`; target `…/Sweet Poison/ready_for_print` |
| `move/move_small_images.py` | Moves `.jpg`/`.jpeg`/`.png`/`.tiff`/`.bmp` images ≤200×200 px into a `Delete` subfolder, then posts a macOS notification | `~/Downloads/Pictures/Cyberpunk` → `…/Cyberpunk/Delete` |
| `move/move_small_images.workflow` | Automator Quick Action — it runs `~/Documents/Tech/scripts/move_images.py`, not the script in this repo | |
| `requirements.txt`, `*/requirements.txt` | Pillow (unpinned at the root, `pillow==12.2.0` in each subfolder) | |

> The `dpi/` and `rename/` paths point at `~/Documents/lab/websites/altmerch.store/`, where the
> store folder lives since 2026-11-17 (`~/Documents/Business/Merch/altmerch.store` is a symlink to it).

## Cloning

Submodules aren't fetched by a plain `git clone`. Use:

    git clone --recurse-submodules <this repo's url>

or, on an existing checkout:

    git submodule update --init --recursive
