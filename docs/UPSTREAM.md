# Keeping up with upstream

PDFflow is a fork of [PdfCraft](https://github.com/storytold/pdfcraft) (MIT or Apache-2.0).
This is how we take their updates without losing our name, icon and links.

## The one rule

**Every change PDFflow makes to a file that came from upstream is made by `rebrand/apply.py`,
never by hand.** The script reads `rebrand/brand.json` and can be run again at any time, so after a
merge we can take upstream's version of any conflicted file and re-apply our changes in one step.
`apply.py` is the same in every FlowApps fork; only `brand.json` differs.

## What lives where

| Ours (upstream has no such file, or we replace theirs whole) | Upstream's, changed only by `rebrand/apply.py` |
|---|---|
| `rebrand/` (`apply.py`, `brand.json`, `app-icon/`, `patches/`) | everything under `crates/`, `apps/`, `packaging/` |
| `brand/` (logos, `.ai` source) | `assets/app-icon/` (file names theirs, artwork ours, from `rebrand/app-icon/`) |
| `docs/UPSTREAM.md`, `README.md`, `assets/header-*.png` | `CLAUDE.md`, `NOTICE`, `.gitattributes`, `.gitignore` |
| `.github/workflows/release.yml`, `windows-release.yml` | |

`brand.json` holds:

- **edits**: find and replace in one file (window title, bundle id `com.pdfflow.app`, version
  and log lines, Mac `Info.plist.in` and `package.sh`, Windows installer names and links). Every
  occurrence is replaced; `"regex": true` uses a regular expression.
- **prepend / append**: text added once (the agent note in `CLAUDE.md`, our `NOTICE` paragraph,
  merge rules in `.gitattributes`, release-build lines in `.gitignore`).
- Patches in `rebrand/patches/` for changes too big for find and replace, applied with a
  three-way merge: no Discord button, community card or ArtCraft links anywhere, and the About box says PDFflow.
- Icons: `rebrand/app-icon/` copied over upstream's files of the same names.

Left alone on purpose: crate and binary names (`pdfcraft*`), environment variables, log-file names,
licences and credits, upstream's docs, Linux packaging (we don't ship Linux), and the Windows
installer ids and `pdfcraft.exe` (changing them would stop installed copies from upgrading). The
settings folder stays `PDFflow`, because existing users' settings live there.

## Which upstream version we follow

PdfCraft cuts releases on side branches, so their release tags are not on `main`. PDFflow follows
their `main`.

## Merging upstream

Set the clone up once, so upstream's tags arrive as `upstream/v…` and never collide with ours:

```bash
git remote add upstream https://github.com/storytold/pdfcraft.git
git config remote.upstream.tagOpt --no-tags
git config --add remote.upstream.fetch '+refs/tags/*:refs/tags/upstream/*'
git config merge.ours.driver true
```

Never push the `upstream/*` tags. Then:

```bash
git fetch upstream
git checkout -b upstream-sync
git merge upstream/main
```

If there are conflicts:

1. For a file in the right-hand column above, take theirs: `git checkout --theirs -- <file>`.
2. For a file we deleted that they changed ("deleted by us"): keep it deleted, `git rm <file>`.
3. For `README.md` or our workflows, keep ours: `git checkout --ours -- <file>`.

Then re-apply the rebrand and check it:

```bash
python3 rebrand/apply.py
python3 rebrand/apply.py --check     # prints nothing and exits 0 when everything is rebranded
```

If the script stops with `could not find … to edit`, upstream changed that line: find where the
text went and update that entry in `rebrand/brand.json`. If a patch leaves conflict markers,
resolve them, then refresh the patch with
`git diff <upstream commit> -- <its files> > rebrand/patches/01-no-discord.patch`.

Build and test, commit and open a pull request:

```bash
cargo build --locked -p pdfcraft
cargo test --locked --workspace --no-fail-fast
```

Run the app once and look at it before releasing: tests don't see icons or the title bar.

### Known work for the next merge

Tried on 2026-11-26 against upstream `main` (e862b7756, 1042 commits ahead): `brand.json` stops at
`apps/pdfcraft/src/main.rs`, because upstream changed where the name is used:

- The `legacy` / `or_else` `storage_dir("PdfCraft")` lines are gone; `main.rs` now has
  `eframe::storage_dir("PdfCraft")` in the `data_dir` line.
- New places with the name: `single_instance.rs` (`join("PdfCraft")`), `startup.rs`
  (`AppInfo { id: "pdfcraft", name: "PdfCraft", … }`) and `crates/ocr/src/lib.rs` (model folder).
- `crates/ui-egui/src/platform/macos_association.rs` already has `brand.json` edits for its bundle
  id and its test's captured binding; check they still find their text.

## Releasing

Actions → Release builds the macOS DMG and the Windows portable zip and publishes a GitHub
release. The FlowApps launcher installs and updates PDFflow from those releases.

## Automatic check and prepare

`rebrand/upstream.py` (the same file in every FlowApps fork) and two workflows do the routine part:

- **Upstream check** runs every Monday and on demand. It keeps one `upstream-update` issue current:
  the new upstream commits, a trial merge, and whether the rebrand still applies. It closes the issue
  once we're up to date.
- **Upstream prepare** runs only by hand. It merges upstream on an `upstream-sync/<date>` branch,
  re-applies the branding, checks it, updates `rebrand/upstream.json` and opens a pull request for
  review. It needs the `UPSTREAM_PUSH_TOKEN` secret (see the workflow). If a conflict or the rebrand
  needs a person, it stops without pushing anything; then merge by hand as above.

`"conflicts"` in `rebrand/upstream.json` is `"theirs"`: a conflicted file takes upstream's version and the rebrand brands it again. Upstream's own workflow changes are
never brought in automatically; the pull request lists them. Locally: `python3 rebrand/upstream.py
status`, `report --out FILE` or `prepare --body FILE`.
