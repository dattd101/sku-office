# Rust Office Modern v0.4.1 — Windows desktop MVP

**Architecture:** Next.js 15 (static export) + React 19 + Tailwind CSS + Tauri 2 + Rust.

This is a functional **MVP**, not a feature-complete Microsoft Office 2019 replacement. It contains three workspaces with modern Ribbon-inspired UI:

- **Word:** editable plain text on an A4-like canvas, basic DOCX text import/export.
- **Excel:** editable grid, multiple sheets, XLS/XLSX import and XLSX export. Formula strings are exported but **not calculated** inside the app.
- **PowerPoint:** slide list, title/body editor, basic PPTX text import/export.
- Native Windows file dialogs, New/Open/Save/Save As, basic in-memory undo/redo.
- Optional JSON project files preserve this MVP's own data model.

**IMPORTANT:** OOXML conversion is deliberately basic and **not round-trip safe** for complex documents. Opening and re-saving existing DOCX/PPTX/XLSX can discard unsupported formatting, media, embedded objects, charts, relationships, notes, macros and more. Work on copies of valuable files. Word formatting toolbar buttons are visual placeholders; native rich-text editing is not implemented.

## Build via GitHub Actions (no local Rust required)

1. Upload all project files **including `.github/workflows/windows-build.yml`** to a GitHub repository.
2. Open **Actions → Build Rust Office Windows x64 → Run workflow**.
3. After success, open the run → **Artifacts** → download `RustOfficeModern-Windows-x64-portable`.
4. Extract the artifact and the contained `RustOfficeModern-Windows-x64-portable.zip` to obtain `RustOfficeModern.exe`.

The GitHub workflow runs `npm install`, TypeScript check, Next static export, Cargo check, Tauri `--no-bundle`, and archives the Windows x64 `.exe`. It does **not** create an installer. The executable requires Microsoft Edge WebView2 Runtime, normally present on Windows 10/11. On machines without it, install WebView2 Runtime separately.

## Local development

Prerequisites: Node.js 22, Rust stable MSVC, Visual Studio C++ Build Tools and Windows WebView2 runtime.

```powershell
npm install
npm run desktop
```

To build a portable executable locally:

```powershell
npm run desktop:build -- --no-bundle
```

Expected binary: `src-tauri/target/release/rust-office-modern.exe`.

## Technical notes

- `next.config.ts` uses `output: 'export'`; **no Node server is required at runtime**.
- `src-tauri/src/lib.rs` contains the Tauri IPC commands and JSON ↔ native Rust model conversion.
- `src-tauri/src/formats.rs` contains limited OOXML readers/writers adapted from the earlier Rust MVP.
- `components/office.tsx` contains the desktop UI. UI state is client-side; saving/opening calls native Rust.
- No external CDN, fonts or API is needed at runtime.
- The `.exe` has not been validated on a Windows runner in this environment. If the workflow reports an error, use the exact compiler log to refine the code.

## Known limitations

- Formatting controls beyond editing and navigation are not implemented.
- No charts, pivot tables, formula evaluation, spellcheck engine, rich-text layout, animations, slide shape editor, print pipeline, or full Office compatibility.
- No simultaneous multiple-document tabs.
- For robust production Office compatibility, replace the simplistic OOXML layer with a tested document model and lossless package preservation strategy.

## Fix for GitHub Actions Cargo workspace error (v0.4.1)

The Tauri manifest now contains a standalone `[workspace]` table with `resolver = "2"`.
This avoids accidental workspace inheritance when a repository also contains an
older root `Cargo.toml` (for example from the earlier egui prototype). The
workflow checks `cargo metadata` before `cargo check`.

**Upload the extracted contents as the repository root**, not as a nested
folder. Remove obsolete files from the previous egui project where possible.
Do not add `src-tauri` to an unrelated root workspace while keeping the nested
`[workspace]` declaration. Commit the new files, then start a NEW workflow run
from the new commit (re-running an old failed run checks the old revision).

This fix addresses the workspace configuration error shown in the supplied log.
Compilation and runtime still need to be verified by the Windows runner.
