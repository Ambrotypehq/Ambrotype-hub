# Ambrotype hub

Shared layouts, themes, presets, collage templates, size profiles, borders, plugins and custom interfaces for [Ambrotype](https://github.com/shardpelt/Ambrotype). Layouts and colour themes are called "looks" here (they live in `looks/`). Ambrotype's **Browse** button (also **Appearance → Browse hub…**) lists the looks and plugins here and installs them with one click. You can also skip the hub: keep a look in your own GitHub repo (`theme.json` or `layout.json` at its top) and people paste the repo's link into the Browse dialog.

Like [Omarchy](https://learn.omacom.io/2/the-omarchy-manual/90/extra-themes)'s themes, there is no server: the hub is this repo. Each look is one small JSON file, and `index.json` lists them with a checksum. Looks contain no code, so they're safe to install. The app checks each file's checksum and validates it before saving it.

**New to this?** [Make it yours](https://github.com/shardpelt/Ambrotype/blob/main/docs/make-it-yours.md) walks through picking a look, changing a layout, making a theme from three colours, sharing it, and running a hub of your own. The file format is in [ui-layouts.md](https://github.com/shardpelt/Ambrotype/blob/main/docs/ai/ui-layouts.md).

## The app's built-in looks and presets

Ambrotype's own layouts (Classic, Compact, …), themes (Dark, Light, …) and starter presets live here too. The app ships a copy of them, so it works offline: its `crates/amb-gui/data/hub/bundle.txt` lists which items, and `tools/sync-hub.py` in the app repo copies them from a checkout of this repo. To change one, change it here first, then sync the app. A built-in layout must only use toolbar buttons and panels the app already has.

## A good layout

- **One task, done well.** Show what the task needs, at the size it needs. Anything else is a Ctrl (⌘)+K search away, and every command is in the menu bar.
- **Organising stays in the Library.** The built-in editing layouts leave out Rating & keywords (the keys 1–5, 6–9, P, X and U still work while editing); the Library shows them for the selection. Film lab and Collage prep keep them, as sorting is part of those tasks.
- **Give it a clear name and a one-line description** that says what it's for ("Colour grading: the wheels large, the rest folded").

## Add your look

**From the app (easiest):** in **Appearance**, pick your own layout or theme and press **Submit to hub…**. GitHub opens with the file already filled in. Press "Propose changes" and then "Create pull request". If you don't have write access, GitHub makes the fork for you.

**By hand:**
1. Fork this repo.
2. Add your file as `looks/layouts/<name>.json` or `looks/themes/<name>.json`. Use lower case and dashes, e.g. `warm-dark.json`. The format is described in [ui-layouts.md](https://github.com/shardpelt/Ambrotype/blob/main/docs/ai/ui-layouts.md).
3. Check it with `ambrotype --check-look looks/themes/warm-dark.json`.
4. Run `python3 tools/build_index.py` to update `index.json`.
5. Open a pull request.

If you skip step 4, a maintainer can run it before merging.

**Rules:**
- One look per file, with a clear `name` and a one-line `description`. Add `"author"` to be credited.
- Looks are shared under CC0 (public domain) unless the file says otherwise in `"license"`.
- Nothing that copies another product's branding or name.

## Presets

Develop presets go in `presets/<name>.xmp` (lower case and dashes), in Lightroom's XMP preset format: export one from Lightroom, or use Ambrotype's **Presets → Save…** and take the file from the presets folder. The app lists them under **Browse hub… → Presets**, shows what each changes, and installs it into the presets list (group "Hub"). They also make up the **Preset library** (Presets → Preset library…), grouped by their `crs:Group` (give each a `Name`, a `Group` and a one-line `Description`).

- Share only presets you made. Bought or downloaded packs belong to their makers.
- Add `crs:Author`, `crs:Description` and a `crs:License` if you like; presets are CC0 otherwise.
- Run `python3 tools/build_index.py` to list it in `index.json`.

## Collage templates, size profiles and borders

- **Collage templates** go in `collages/<name>.json`: the file the collage maker's **Share…** saves (`"format": "ambrotype-collage/1"`). Installed ones join the templates in the collage maker.
- **Borders** go in `borders/<name>.json`: the file the export dialog's border **Share…** saves (`"format": "ambrotype-border/1"`). Installed ones join the border presets.
- **Size profiles** go in `sizes/<name>.json`: a named set of sizes, installed as size presets for the crop tool and the export dialog:

  ```json
  { "format": "ambrotype-sizes/1", "name": "Etsy", "description": "Listing photos and the shop banner.",
    "sizes": [ { "name": "Listing", "width": 2000, "height": 1600 }, { "name": "Banner", "width": 3360, "height": 840 } ] }
  ```

## Plugins

Plugins are small programs that make edits (Ambrotype's **Plugins** button). They run sandboxed: no files, no network, only settings the user can see and undo. How to write one: [plugins.md](https://github.com/shardpelt/Ambrotype/blob/main/docs/ai/plugins.md).

- **Send one in:** in the Plugins dialog, pick your plugin and press **Share as pull request…**. It shows the files first, then sends them with the GitHub CLI (your own login) or opens GitHub's upload page in your browser.
- **Where they go:** `plugins/<id>/`, with `plugin.json`, the built `plugin.wasm`, and the Rust source (`Cargo.toml`, `src/`). No source, no merge: reviewers read the code and rebuild the module. Say the plugin's licence in its `Cargo.toml` (`license = "..."`), any open-source licence works.
- **Reviewing:** read the source and the permissions it asks for, then `amb plugin build plugins/<id>` (the module should come out the same, or replace it with yours) and `amb plugin check plugins/<id>`.
- **Installing:** **Browse** in the toolbar (or the Plugins dialog's **Browse hub…**) lists them, with what each asks to see, and installs one with a click. The app checks the manifest and the module against `index.json` (run `python3 tools/build_index.py` after merging a plugin), and the plugin asks for its permissions the first time it runs.

## Interfaces

A custom interface replaces Ambrotype's whole window with someone's own HTML, CSS and JavaScript: a one-slider app, a big-button touch editor, a culling screen. How to make one (by hand or with an AI agent): [custom-interface.md](https://github.com/shardpelt/Ambrotype/blob/main/docs/ai/custom-interface.md).

- **Send one in:** in **Settings → General → Interface**, choose yours and press **Share…**. It lists the files first, then sends them with the GitHub CLI (your own login) or opens GitHub's upload page in your browser. By hand: add the folder as `interfaces/<id>/` and open a pull request.
- **What it holds:** `index.html`, an `interface.json` with a `"name"` (and `"description"`, `"author"`, `"license"`), and the files it loads. Only web files (pages, scripts, styles, pictures, fonts), at most 200 files and 20 MB. Nothing loaded from the internet. It needs a way back to the built-in interface.
- **Reviewing:** an interface is code with the app's full reach (unlike a plugin, it isn't sandboxed). Read every script before merging. Refuse anything that loads code or data from the internet, or touches files it has no reason to.
- **Installing:** **Browse → Interfaces** lists them with their files, warns that it's code, and installs one into the app's `interfaces/` folder. Every file is checked against `index.json`. **Use now**, or Settings → General → Interface, switches to it.

## For maintainers

- CI (`.github/workflows/check.yml`) fails when a file isn't valid JSON, is in the wrong folder, or `index.json` is out of date, when a plugin folder lacks its manifest, module or source, and when an interface lacks its `index.html` or `interface.json`, or holds a file that isn't a web file.
- Before merging, run `ambrotype --check-look` on new files, and try them in the app.
- The app reads `index.json` from `https://raw.githubusercontent.com/<owner>/<repo>/main/index.json`. Ambrotype's default points at `Ambrotypehq/ambrotype-hub`. Anyone can point their app at another hub in **Settings → General**.
