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
