# Rust Office Desktop — working MVP source

Native Windows desktop application written in Rust. One executable with Word, Excel and PowerPoint editing modes and a Ribbon-inspired toolbar.

## Prerequisites

- Windows 10/11, Rust stable (`rustup`), MSVC C++ Build Tools (Desktop development with C++).
- Internet access for initial Cargo dependency downloads.

## Run

```powershell
cargo check
cargo test
cargo run --release
```

## Implemented

- Word: edit plain text; import text from DOCX; export basic DOCX.
- Excel: import XLS/XLSX/XLSB/ODS using calamine; edit a paged 10-column x 35-row grid; multiple sheets; export XLSX with strings, numbers and formulas.
- PowerPoint: import text from PPTX slides; edit title/body; add/delete slides; export simple PPTX slides.
- Native desktop file dialogs; New, Open, Save, Save As; mode tabs; Ribbon-inspired controls.

## Important limitations

This is an MVP, **not a complete Microsoft Office 2019 replacement**. It does not preserve advanced OOXML content. DOCX formatting, tables, images, headers, tracked changes are not preserved. Excel styles, charts, pivot tables, images, merged cells, macros and advanced formula evaluation are not preserved. PPTX shapes, slide masters, themes, images, animations and precise positioning are not preserved. Formula strings are saved but are not calculated in the editor. Slide order is inferred from slide filenames, which may differ from actual presentation order. Editing and re-saving complex documents can lose content. Work on copies, not original files.

The ZIP was assembled in an environment without Cargo/Rust; compilation and runtime behavior have not been verified here. `cargo check` on Windows is the required first verification step.

## Next engineering milestones

1. OOXML package model with relationship handling, round-trip tests, preservation of unknown parts.
2. Command model, undo/redo, autosave and crash recovery.
3. Virtualized spreadsheet grid and formula dependency engine.
4. Rich text model, shaping, pagination and layout.
5. Slide scene graph, drawing engine, master/theme support.
6. Test corpus and interoperability testing against Microsoft Office/LibreOffice.

## GitHub Actions: build Windows .exe (no local Windows build required)

This repository includes `.github/workflows/windows-build.yml`.

1. Create a new GitHub repository, then upload **the contents of this folder** to the repository root (including the hidden `.github` folder).
2. Commit and push to `main`, or open **Actions → Build Windows EXE → Run workflow**.
3. When the job succeeds, open its workflow run and download the `rust-office-windows-x64` artifact. Extract the artifact ZIP, then extract `rust-office-windows-x64.zip` to get `rust-office-desktop.exe`.
4. For an optional GitHub Release, push a version tag: `git tag v0.2.0` then `git push origin v0.2.0`. GitHub Actions must have permission to create releases (`contents: write` is configured at the job level; repository policies may restrict this).

The workflow uses the GitHub-hosted Windows runner, stable Rust MSVC, `cargo check`, `cargo test`, and a release build for `x86_64-pc-windows-msvc`. Build failures will be visible in Actions logs. `Cargo.lock` is not included in this starter archive, so the workflow resolves dependencies at build time; commit the generated lockfile for reproducible subsequent builds.

**Status:** This workflow was authored and checked for file structure, but no GitHub Actions run has been executed here. A downloadable executable is produced only after a successful workflow run.
