<!-- PDFflow (rebrand/apply.py) -->
# PDFflow

This repository is **PDFflow**, a fork of PdfCraft. Read `docs/UPSTREAM.md` first. In short:

- Never hand-edit a file that came from upstream to change branding, ids, links or icons: add the
  change to `rebrand/brand.json` (or a patch in `rebrand/patches/`) and run `python3 rebrand/apply.py`,
  so it survives the next upstream merge.
- PDFflow's own files are `rebrand/`, `brand/`, `docs/UPSTREAM.md`, `README.md`,
  `assets/header-*.png` and `.github/workflows/release.yml` / `windows-release.yml`.
- Crate, binary and environment-variable names stay `pdfcraft*` / `PDFCRAFT_*`.

@AGENTS.md
