<p align="left">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="assets/header-dark.png">
    <img src="assets/header-light.png" width="600" height="160" alt="PDFflow logo and name">
  </picture>
</p>

<p align="center"><b>The PDF workbench; a modern PDF reader and editor built in pure Rust.</b></p>

PDFflow is part of the FlowApps suite. It is a fast, open-source PDF reader and editor: read,
organize, combine, split and secure PDFs in a native app, written in Rust from the ground up. Runs on
macOS and Windows.

## Why PDFflow?

- **Free and open source** — MIT OR Apache-2.0
- **Read, organize, combine, split** — rotate, delete, insert, extract pages; combine multiple PDFs
- **Protect** — password encryption (user/owner), permissions, AES-256
- **Forms, comments, layers** — fill forms, threaded comments, optional content layers
- **Native desktop** — macOS (Apple Silicon and Intel) and Windows x64 builds
- **Private** — works offline; no accounts, no telemetry, no cloud

## Features

- **Read.** Deep zoom, continuous / single-page / two-up views, dark mode, RT/CJK scripts.
- **Organize.** Rotate, delete, insert blank pages, move pages; undo goes the distance.
- **Combine & split.** Merge PDFs, extract pages, split every n pages; links, forms and layers preserved.
- **Protect.** Open and owner passwords, AES-256 encryption, permission handling.
- **Forms & comments.** Fill interactive forms, threaded comment conversations, attachments.

## Download and install

Installers for macOS and Windows are available on the [latest release](https://github.com/reachneoagent-commits/pdfflow/releases/latest). PDFflow is also available through the [FlowApps launcher](https://github.com/reachneoagent-commits/FlowApps-Launcher/releases/latest).

| Platform | Installer |
|---|---|
| macOS, Apple silicon (M1 or newer) | DMG (`PDFflow-<version>-macos-arm64.dmg`) |
| macOS, Intel | DMG (`PDFflow-<version>-macos-x64.dmg`) |
| Windows x64 | Installer (`PDFflow-<version>-windows-x64.msi`) · Portable (`.zip`) |

The app is not code-signed with a paid certificate, so macOS and Windows warn you the first time you open it. Every release lists SHA-256 checksums to check the files against.

- **Mac:** open the DMG and drag PDFflow to Applications. Then right-click PDFflow, choose Open, and click Open again.
- **Windows:** if SmartScreen appears, click More info, then Run anyway.

## Supported platforms

- **Desktop app:** macOS on Apple Silicon and Intel; Windows x64

## Build from source

You need [Rust](https://rustup.rs/) 1.95+. Then run:

```bash
cargo build --release -p pdfcraft
```

The desktop app binary is produced under `target/release/`.

## About this fork

PDFflow is based on [PdfCraft](https://github.com/storytold/pdfcraft) by the ArtCraft team, rebranded as a FlowApps product. The original project is dual-licensed under MIT or Apache-2.0; that licensing is preserved here.

The ArtCraft name, wordmark and logos remain trademarks of the ArtCraft Team and are not used in PDFflow. Per the original NOTICE, forks and modified versions must remove them. (The original `docs/brand/` assets are ArtCraft trademarks and are excluded from this fork.)

## Credits & Acknowledgments

Full third-party credits are in [`NOTICE`](NOTICE) and [`ATTRIBUTION.md`](ATTRIBUTION.md).

- **[PdfCraft](https://github.com/storytold/pdfcraft)** by the ArtCraft team: the original PDF engine, UI and file formats (MIT OR Apache-2.0).
- **FlowApps branding, logo and symbol:** Reach Neo (trademark of Reach Neo).

PDFflow is not affiliated with or endorsed by ArtCraft or the PdfCraft contributors.

## License

PDFflow is dual-licensed under [MIT](LICENSE-MIT) or [Apache-2.0](LICENSE-APACHE), at your option.

**Copyright (c) 2026 Reach Neo and the PDFflow contributors.**
