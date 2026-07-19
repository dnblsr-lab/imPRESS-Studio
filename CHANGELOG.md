# Changelog

All notable changes to **imPRESS Studio** are documented in this file.

The format is based on [Keep a Changelog](https://keepachangelog.com/en/1.1.0/)
and the project adheres to [Semantic Versioning](https://semver.org/).
Installers for released versions are published on the project's GitHub Releases page.

## [Unreleased]

### Planned
- Windows Service mode for hot folders (daemon outside the logged-in user session, autostart at boot).
- REST API + webhooks for hot folders (submit jobs via `POST /jobs`, completion callbacks, MIS integration).
- JDF/JMF integration (job-ticket parsing for hot folders).
- macOS and Linux builds (Avalonia is cross-platform).
- OpenGL preview engine for faster large-sheet rendering.
- Additional UI languages (German, Czech), more gang-run presets, per-hot-folder multi-template routing by filename pattern.

## [1.5.3] - 2026-07-14

UX fix pack: clean start with no template, an honest status bar, tabbed file info.

### Fixed
- **No template forced at startup.** The app no longer auto-loads the bundled "A4 saddle-stitched booklet" sample — the user picks a template (manager, suggestions or a file). Auto-load still honours a deliberately saved default `template.json` in the user profile.
- **Honest status bar.** Removed the always-green "Validation OK" pill (nothing had been checked yet) and the static "3.0 mm · CMYK · 300dpi" decoration shown before anything was loaded. The bleed pill now appears only when a template is active and shows its real bleed.
- **"Duplex: true" in the Polish UI** — booleans in the inspector now render as localized "Tak / Nie" (Yes / No) via a shared converter.
- **"Avalonia.Data.Binding" rendered instead of an empty state.** Four spots (the Template step tile, the inspector, the template-manager header) used `FallbackValue={loc:Loc …}` — a markup extension there yields a Binding OBJECT, and when the fallback fired the UI rendered its ToString(). A new `LocFallbackConverter` shows the localized placeholder ("no template" / "not saved") instead.

### Added
- **File → "Save PDF…" with export options.** Saves the selected source file (with page edits — rotation, grayscale) without imposition. A new options dialog offers the same choices as the imposition export: **format** (plain PDF / PDF/X-1a / X-3 / X-4), **engine** (Auto / imPRESS Export Engine / Ghostscript), **PDF version** (1.4–1.7) and **ICC profile**. Plain save is a copy with edits (optionally just re-stamping the version header); PDF/X produces a print-standard file — X-4 natively via the imPRESS engine, X-1a/X-3 via Ghostscript. Settings persist separately from the main export; the original on disk stays untouched.
- **Source-file pill.** After a file is analyzed the top bar shows its real parameters: PDF version, colour spaces (CMYK / RGB / CMYK+RGB) and — when the file contains images — the lowest effective resolution ("PDF 1.7 · RGB · min 287 dpi"). DPI is a free by-product of the standard analysis pass (pixel samples ÷ placed size), no extra scan.
- **"Preview" inspector tab — page thumbnails with edits.** A new first tab shows thumbnails of every page of the loaded file — **no page cap**. Bitmaps exist only for thumbnails inside a sliding visibility window (viewport + margin, ~90 items): scrolling moves the window and thumbnails outside it release their references, so a 1000-page book costs the same memory as a leaflet, and revisited pages come straight back from the renderer cache. Superseded edit working copies are cleaned from the temp directory. Right-click opens the edit menu: **rotate 90° CW / CCW / 180°** (lossless /Rotate flip), **grayscale this page** and **grayscale the whole file**. Non-destructive: every edit produces a temp working copy — the original PDF is never touched; analysis, preview, merge, imposition and export automatically use the edited version; a banner above the thumbnails flags active edits and "Revert file edits" restores the original. Edits chain (two 90° turns = 180°).
- **Thumbnails tab appears only after a file is loaded; clicking a thumbnail shows that page on the main canvas** (with an active imposition the canvas switches to the page preview — the plan survives, "Plan" brings the sheets back). Since thumbnails now navigate pages, the bottom page strip on the canvas shows **only for a planned imposition** (sheet numbers) and disappears in the page preview.
- **Tolerant stream decoder.** Some design-tool PDFs carry Flate content streams terminated without a final block — PdfSharp's strict inflater threw "Unexpected EOF" on them and the grayscale conversion crashed. The engine now avoids the eager bulk decode and falls back to a tolerant .NET inflate that keeps the decoded prefix — exactly what RIPs do. Regression-tested.
- **Fading page-number badge.** The "n / N" numbering stamp on the preview canvas now shows for 2 seconds after every page/sheet change and fades out smoothly — it no longer permanently covers page content. The export itself still prints the stamp at full ink.
- **Grayscale runs entirely on the imPRESS engine — no Ghostscript.** Vector colour operators (RGB and CMYK) in page and form content streams are rewritten to luminance gray; images (JPEG/DCT, 8-bit Flate RGB/CMYK rasters, Indexed palettes) are converted pixel-by-pixel to 8-bit DeviceGray with Flate compression. Single-page edits convert the extracted page in isolation and splice it back, so images shared with other pages stay untouched. Exotic image flavours (JPX, 16-bit) are left in colour rather than risking a corrupt file. Verified end-to-end with renders.

### Changed
- **File info window redesigned.** Tabs instead of expanders — Overview (file, metadata, properties) / Validation / Colour & PDF/X (standard, OutputIntent, spot colours, transparency) / Page content (boxes, images, fonts) / Ink coverage — the same tab pattern as the main-window inspector. All hardcoded light-theme colours replaced with theme-dictionary brushes, fixing the dialog in dark mode.

## [1.5.2] - 2026-07-14

Hot folders 2.0: imPRESS engine support, template picker, a simpler dialog; complete page-box set on exported sheets.

### Added
- **imPRESS Export Engine in hot folders.** Per-folder PDF/X engine selection (Auto / native / Ghostscript) and output PDF version for plain exports (1.4–1.7) — the same options as the export dialog, persisted in `hotfolders.json`.
- **Template picker.** Instead of typing a path, pick a template from the saved-templates list (the same library the template manager uses); a "From file…" button and an editable path field remain for out-of-library templates.
- **Simpler edit dialog.** The basic flow is name → input folder → output folder → template → export; work/archive/error folders (auto-created), file filters, success/failure behaviour, the naming pattern and the Ghostscript path moved into an "Advanced" expander.
- **Zero-config Ghostscript.** A path is no longer required for PDF/X — the pipeline resolves the bundled copy itself (settings → bundled → Program Files); applies to the GUI and CLI too.

### Fixed
- **Exported sheets now declare the full page-box set** (CropBox = BleedBox = TrimBox = MediaBox, including the cutting-guide page) — re-importing our own output used to trigger "BleedBox missing" warnings and a "document declares no bleed" error. Deep validation additionally recognises imPRESS-produced sheets (via the Producer field) and shows a single info note — the sheet is the final format; item bleed lives inside it — instead of false bleed errors.

## [1.5.1] - 2026-07-13

Fix pack after 1.5.0: localized preflight, no more silent manager ↔ job template divergence, About window sizing.

### Changed
- **Quick export preflight messages moved to the localization system** (Polish/English following the app language): `BLEED_LOW`, `RGB_ONLY`, `TRIM_MISMATCH`/`TRIM_ROTATED`, `SIGNATURE_PADDING`, `SADDLE_TOO_THICK`, `DUPLEX_ODD_PAGES`, `ENCRYPTED`, `NO_ICC`, the PDF/X rules and the rest. The deep prepress validation already had localized categories and recommended actions.

### Fixed
- **3 mm bleed silently became 0 on every "Use template" (the root cause of the recurring `BLEED_LOW` reports).** The main window's status pills (bleed, gutter, sheet size) bound `Run.Text` to template fields — and Avalonia's `Run.Text` binds two-way by default (WPF heritage). On a `CurrentTemplate` swap the stale displayed value was written *into the freshly applied template*: the preset carried 3 mm, the job received 0. Reproduced with a headless-UI test using the exact pill markup; fixed by forcing `Mode=OneWay` on every display-only field, with a regression test guarding the pattern. New template-chain diagnostics (preset → commit → adopt → plan) made the bisection possible and stay in the app.
- **"Template in the manager ≠ template of the job" clarity.** Closing the manager without "Use template" now prints which template stays active in the status bar, and the `BLEED_LOW` message names the template being judged and points to where to fix it.
- **Shared template reference.** The template editor (a singleton) handed the main window its live `Template` reference — a later manager session (even one abandoned with Cancel) could silently mutate the active job's template. "Use template" now hands over a deep copy.
- **`RGB_ONLY` message clarified:** it now states that PDF/X exports convert colours automatically (imPRESS engine / Ghostscript) while a plain export keeps RGB values unchanged.
- **About window:** a fixed height clipped the footer with the Close button; the window now sizes to its content.

## [1.5.0] - 2026-07-13

The imPRESS Export Engine release: native PDF/X-4 with ICC colour conversion — no Ghostscript required — a native ink-coverage scanner, and a redesigned template manager.

### Added
- **imPRESS Export Engine — native PDF/X-4 without Ghostscript.** A new in-house packaging engine embeds the OutputIntent with the selected ICC profile, full XMP metadata (`pdfxid:GTS_PDFXVersion`, IDs, dates, title), `/Trapped`, a TrimBox on every page and the PDF 1.6 version stamp — entirely natively. Honest scope: the engine packages print-ready files (CMYK/ICC) and converts vector RGB (below); transparency flattening, RGB images, and PDF/X-1a / X-3 remain the Ghostscript engine's job.
- **ICC colour conversion with no external libraries.** Vector RGB fills and strokes in the composed file are rewritten to DeviceCMYK through the selected ICC profile using the Windows colour engine (WCS/mscms, relative colorimetric intent). Vector-only RGB artwork no longer forces the Ghostscript path — the native engine converts it itself and reports the conversion in the export log.
- **Two-stage conformance gate.** Source analysis (RGB text/images, encryption, raster logo overlay) **plus** a deep scan of the composed file's decoded content streams and form XObjects for DeviceRGB operators. Vector RGB → native conversion; RGB images or an unreadable stream → Ghostscript. Fails safe: doubt never ships a non-conformant file with a false conformance claim.
- **Engine selection in the export dialog and CLI** (`--engine auto|native|gs`): Auto picks the native engine whenever the file qualifies; forcing native on a file with RGB images produces a clear message instead of a silent conformance failure.
- **Output PDF version selection (1.4–1.7)** for plain exports (`--pdf-version` in the CLI, a combo in the dialog). PDF/X modes pin the version their spec requires (X-1a/X-3 → 1.4, X-4 → 1.6).
- **All composer-drawn ink switched to DeviceCMYK** — backgrounds, sheet numbers, cutting guides, barcodes — fixing a PdfSharpCore trap where the default colour mode rewrote every colour as RGB. Verified end-to-end: both a CMYK job and a vector-RGB job package natively and render correctly (reds converted per SWOP); conformance markers (GTS_PDFX, XMP, OutputIntents, TrimBox) confirmed in the outputs.
- **Native ink-coverage scanner.** "Scan ink coverage" in the file-info window now runs on the imPRESS engine: pages are rendered by the internal rasterizer and pixels converted to CMYK through the export ICC profile (SWOP fallback). Ghostscript is no longer needed for the scan; the results (average C/M/Y/K + Σ, 250% warning threshold) are honestly labelled a composite page average, not separation densitometry.

### Changed
- **Template manager redesigned** to match the main window's visual language: a header bar with the template name, an active-binding pill and save actions; the editor split into "General" / "Marks" / "Binding & special" / "Extras" tabs (the same pattern as the main-window inspector) instead of one long scrolling form. Binding-specific cards (saddle, gatefold, fold panels, roll, Step & Repeat, creep) appear only when the chosen binding actually uses them; an enabled roll section never disappears, and a binding with no special options gets an explanatory empty state. The validation banner sits above the tabs so errors are visible from any tab. Every existing function is preserved.

### Fixed
- **Export metadata:** output files now sign themselves "imPRESS Studio" (Creator) and "imPRESS Export Engine" (Producer) — previously the internal module name "Imposition.App" leaked into the Creator field.
- **Confusing `X4_VERSION` preflight note** when exporting with PDF 1.7 selected: the report now states clearly that PDF 1.6 applies only to the PDF/X-4 copy (`.pdfx4.pdf`, an ISO 15930-7 requirement) and the main export keeps its own version setting.

### Tests
- 282/282 passing, including packaging read-back (version stamp, TrimBox, conformance markers via PdfPig), composed-RGB scan classification, ICC transform sanity on SWOP primaries, and a decoded re-scan proving all `rg`/`RG` operators are rewritten after conversion.

## [1.4.8] - 2026-07-06

Major release: ticket numbering, work-and-turn, front/back registration preview, sheet advisor, ink-coverage scan, PDF/X-3, Preflight 2.0, CLI parity and a batch of fixes.

### Added
- **Ticket numbering (variable data lite).** A consecutive number on every item — tickets, coupons, raffle stubs: start number, step (negative allowed), .NET format with the `{0}` token (e.g. `"No {0:0000}"`), 9 anchor positions, offsets and font size. The sequence is continuous across sheets (sheet order, then slot order), stamped on fronts; the preview shows exactly the numbers the export writes. New "Ticket numbering" section in the template editor, format/step validation, verified end-to-end with a rendered 15-up sheet.
- **Work-and-turn duplex mode (Ganging).** One form carries the fronts on the left half of the grid and the backs on the right half at exactly mirrored positions; the sheet is printed, turned left-to-right and printed again with the same form — **half the CTP plates**, and every cell yields a complete duplex product after cutting. Mode selector (Sheetwise / WorkAndTurn) next to the Duplex checkbox; validation enforces an even N-up X. Mirror geometry pinned by tests and verified visually.
- **Front/back registration preview.** A new canvas toolbar toggle overlays the back side **mirrored** at 50% opacity on the front — like holding the sheet against the light. Front/back layout misregistration is visible instantly; the overlay registers no hit areas (clicks still select front slots) and cooperates with the scene cache.
- **Sheet advisor.** "Compare sheets…" in the template editor ranks every paper size by utilisation for the current item (bleed/gutter/margin honoured, 90° item rotation considered): "SRA3: 15 pcs · 86% · 3×5". Pure math on the layout optimizer (new `SheetAdvisor` in Core, with tests).
- **Ink-coverage scan (inkcov).** An on-demand Ghostscript scan in the file-info window: average C/M/Y/K coverage per page plus the Σ total, pages above 250% flagged ⚠. Honestly labelled as a page average (not a per-pixel maximum) — no fake TAC guarantees.
- **PDF/X-3:2003 export** — the third target next to X-1a and X-4 (PDF 1.4, ICC-based colour, no live transparency); full chain: export dialog, PDFX_def, GS arguments, `.pdfx3.pdf` suffix, preflight rules (`X3_TRANSPARENCY`, `X3_VERSION`), hot folders and CLI.
- **CLI parity with the GUI:** new `impose` options `--pdfx none|x1a|x3|x4`, `--icc`, `--sheet-range`, `--page-range`, `--numbering`, `--strict-preflight` (legacy `--pdfx4` still works).
- **Hot folders:** per-folder sheet numbering and cutting-guide options (two new checkboxes in the edit dialog + `hotfolders.json` fields).
- **Ctrl+Z / Ctrl+Y for slot edits** — the previously unused `UndoRedoStack` is now wired to the inspector: every slot edit (move/rotate/blank) is undoable; the stack clears on a new plan.
- **Custom paper sizes survive restarts** — persisted in settings.json and re-registered at startup (they used to live only in process memory).
- **License activation dialog:** a "Renewal & transfer" section — a pre-filled renewal e-mail with the machine fingerprint, and local deactivation (with confirmation) as the first step of a license transfer.

### Fixed
- **Preview bitmap eviction race** — evicted bitmaps are now disposed on the UI thread (between frames); no more `ObjectDisposedException` risk during aggressive zooming of large jobs.
- **Eyedropper UI hitch** — sampling now reads only cached bitmaps (no synchronous render on the UI thread).
- **Step & Repeat duplex:** backs are mirrored across the sheet width with rotation pairing (AlternatingColumns/Checkerboard with even column counts now register front-to-back) and carry the back-side flag.
- **Working merge moved to the temp directory** — no more `merged_imposition.pdf` silently overwritten next to the user's source files; the default output path still lands next to the sources, and the previous working merge is cleaned up.
- **"Page range × slot edits" warning** — exporting with a source page range now states explicitly (status + history) that manual slot edits will not be part of that export.
- **Event-subscription leaks** — the main and preview view-models implement `IDisposable` and detach from long-lived services (renderer, units, localization) when the window closes.

### Also in this release (preflight & suggestions)
- **Page-count-driven template suggestions.** The suggestion library previously knew only flat products and rolls — a 16-page A5 PDF was suggested a "flyer". The engine now honours the source page count: **≥ 5 pages** surfaces **saddle-stitched A4/A5/A6 booklets** (2-up spread on SRA3, 8-page signature, duplex), **49+ pages** adds **perfect-bound A4/A5 catalogues**, and a document of **exactly 5–6 DL pages** proposes a **Z-fold DL leaflet** with correct fold geometry (panels meeting at the fold lines: zero bleed/margin on a landscape A4 sheet). Each carries a dedicated rationale explaining why it appeared. On a dimensional tie the page-count-justified product outranks the flat one, and fixed-capacity products (Z-fold = 6 pages) hide for longer documents.
- **New flat presets:** square flyers 148×148 and 210×210 mm, a 50×200 mm bookmark, and a B2 poster (1-up on a B2 sheet).
- **Deep preflight — new "Page content & interactive elements" category:**
  - `TEXT_TOO_SMALL` — glyphs below 4 pt (sizes read from the transformed text matrix, so content-stream scaling cannot fool it); reports the smallest size found.
  - `TEXT_REGISTRATION_BLACK` — text filled with C=M=Y=K=100%: 400% ink and coloured fringes on misregistration; body text should be K-100.
  - `HAIRLINE_STROKES` — paths stroked with an explicit width of 0 ("0 w") that vanish on the platesetter. Only the literal zero is flagged — the one value that survives any content-stream transform (PdfPig exposes raw, untransformed widths), so no false positives.
  - `BLANK_PAGES` — aggregated info listing pages with no text or images.
  - Annotation census (`ANNOTATIONS_MARKUP` warning for comments/stamps, `FORM_FIELDS` warning for AcroForm widgets — also when declared only in the catalog, `ANNOTATIONS_LINKS` info for harmless hyperlinks) via a new per-page annotation walker.
- **Quick preflight — binding and orientation diagnostics:** `TRIM_ROTATED` (pages matching the template trim only after a 90° turn get one aggregated hint instead of per-page mismatch warnings), `SIGNATURE_PADDING` (how many blank pages the signature will add), `SADDLE_TOO_THICK` (saddle stitch above 64 pages — consider perfect binding), `DUPLEX_ODD_PAGES` (odd page count in duplex ganging → empty last back).

### Changed
- Stricter template validation: negative spine width rejected; a safe zone consuming the whole item (2×safe ≥ shorter trim side) reports a clear error; legacy inline marks get numeric sanity checks (zero crop length, negative offsets, zero OPOS marker size while enabled).

### Tests
- 275/275 passing (40 new in this release), including an end-to-end fixture: a real PDF authored with 3 pt text, registration-black text, a zero-width stroke and a blank page runs through the deep analyzer and every new check fires. Ticket numbering, work-and-turn geometry and the new CLI options verified visually via CLI + Ghostscript renders.

## [1.4.7] - 2026-07-04

Full-code review release: correct scale for 90° roll rotation, versos survive Duplex=off, export matches the preview 1:1, one coordinate frame for `/Rotate` pages.

### Fixed
- **Roll-fed auto-rotation rendered content at the wrong scale.** The composer and the preview fitted the *unrotated* page into the rotated slot before spinning it — a non-square item (e.g. a 100×50 mm label) landed at `min(w/h, h/w)` scale, i.e. half size for 2:1, with white gaps inside every slot. Fitting now targets the slot with its dimensions swapped back (same rotation pivot), so the trim fills the slot exactly. Fixed in both `PdfComposer` and the preview canvas; verified visually on a rendered 6-up roll.
- **Unticking "Duplex" on saddle-stitch/perfect-bound/folded work silently dropped every verso from the export.** Double-sided binding strategies always plan back sides, but the composer skipped them when `Duplex=false` — the preview showed the full booklet while the exported PDF contained fronts only. Single-sidedness is now decided solely by the strategies (empty back placements); the composer renders every side the plan carries.
- **Manual slot edits (move / rotate / blank in the inspector) were silently discarded at export.** The export pipeline unconditionally re-planned the job, so edits lived only in the preview. Edited sheets are now written back to the job before export, and the pipeline plans only jobs without an existing plan (CLI, hot folders and the source-page-range path still plan as before). Note: after changing *template* parameters, click "Plan imposition" again before exporting.
- **Pages with `/Rotate` — one coordinate frame for geometry and preflight.** `PdfBoxParser` mixed two coordinate systems: MediaBox/CropBox arrived from PdfPig already rotated (visual frame) while TrimBox/BleedBox/ArtBox were read raw from the page dictionary. Consequences for `/Rotate 90/270` files: false `TRIM_MISMATCH` warnings, wrong bleed-fit geometry in composition and preview (double width/height swaps), and skewed bleed-validator comparisons. The parser now maps every box into the visual frame (90°/180°/270° transforms verified empirically against PdfPig glyph coordinates) and all consumers use the dimensions directly.
- **Memory leak in the preview with an active CMYK channel filter / overprint simulation:** the filtered-bitmap cache grew without bound while zooming (each zoom bucket added a multi-megabyte copy). The cache is now bounded (FIFO, 8 entries, evicted bitmaps disposed) and cleared on every job change; a dead guard in the disposal path was removed.
- **`PdfPreviewRenderer.Dispose()` destroyed the process-wide PDFium singleton** (`DocLib.Instance`), tearing the native library down under in-flight renders and any other consumer. The renderer no longer disposes the singleton.

### Changed
- Verso placements in Step & Repeat and Ganging finally carry `IsBackSide=true`, consistent with the remaining strategies (Wiro, Roll-fed, saddle stitch).
- Saddle-stitch validation now accounts for bleed on the outer sheet edges (2×bleed per dimension; the spine and inter-copy cut lines are unchanged). Templates sized with zero bleed allowance will now fail the sheet-fit check.
- Documentation: the marks-profile resolution order (`MarksProfileResolver`) and the accordion scheme in `FoldingStrategy` now match the actual, test-pinned behaviour.

## [1.4.6] - 2026-06-27

Resilience fixes: backup restore after a failed export, protection against malformed base64 activation keys, narrowed catch blocks in ICC and TrimBox readers.

### Fixed
- **Output file loss on a failed rename.** `PdfExporter` backed up the existing target (`.bak`) before the atomic rename; if the rename failed (disk full, antivirus lock), the catch block deleted only the temp file and never restored the backup. The backup is now restored automatically and any restore failure is logged separately.
- `LicenseValidator.SaveLicense` threw an unhandled `FormatException` on activation keys with invalid base64 characters (clipboard corruption, truncation). The decode is now guarded and raises a descriptive error instead.
- `ExportPipeline.ReadIccChannelCount` and `SourceTrimReader.Read` had bare `catch {}` blocks that swallowed even `OutOfMemoryException`/`StackOverflowException`; both now exclude critical exceptions from the fallback path.

## [1.4.5] - 2026-06-15

### Added
- **US template presets when units are set to inches.** The "Ready presets" list follows the active unit: ISO set (A4/A5/SRA3/DL) for millimetres, US set after switching to inches — Letter/Half-Letter saddle booklets, Letter perfect bound, Letter/Half-Letter ganging, Letter tri-fold (11×17), 4-panel accordion, 3-up rack cards (3.5×8.5″), 6/8/12-up US business cards (3.5×2″), 2×2″ roll stickers and Step & Repeat. US geometry throughout: 0.125″ bleed, 0.25″ safe zone and sheet margin, 13×19″ working sheet. Paper library gains `Half Letter`, `12×18` and `13×19`.

### Changed
- **The source page range resets whenever a new source is loaded.** Previously a range set for one file could silently carry over to the next job and truncate or overrun it. Persisted export settings (Ghostscript, ICC, PDF/X) are untouched.

## [1.4.4] - 2026-06-14

### Added
- **Grayscale (K-only) step wedge** on the second half of the marks edge: 8 patches in 12.5% steps from solid black to white, for tonal-reproduction and dot-gain control on the black separation. Preview draws both bars exactly as the exported PDF does.

### Changed
- **The CMYK control bar no longer overlaps the centre registration mark** — it is now centred on the first half of the edge (¼ of its length) instead of the full edge.

## [1.4.3] - 2026-06-13

### Added
- **Warning when the tracking barcode / QR overlaps printed content.** Overlap geometry is computed by the shared `BarcodeMarkLayout` used by both export and preview; an overlapping code is outlined with a red dashed frame on the canvas and flagged in the status line after planning.

### Changed
- **Per-type mark toggles in the preview, driven by the template.** The single "Marks" button was split into separate view filters (crop, registration, fold, slug, barcode/QR); each toggle is bounded by the template's marks profile — a mark type disabled in the template disappears from the preview automatically, and the profile re-resolves live after template edits.

## [1.4.2] - 2026-06-13

Major production release: page/sheet ranges, cutting guide, tracking codes, new binding layouts — plus hard imposition and hardening fixes.

### Added
- **Two independent export ranges:** a *source page range* (which source pages get imposed — applied before planning via a temp subset) and an *output sheet range* (which finished sheets get written — reprint a single damaged sheet without exporting the whole run). Shared `RangeExpression` parser validates expressions up front.
- **Cutting guide for the guillotine:** an optional page appended to the PDF with a sheet schematic and a numbered knife-position table in millimetres (vertical cuts from the left edge, horizontal from the top).
- **Barcode / QR tracking code** in the marks profile — Code 128 or QR with job metadata (same placeholders as the slug: `{jobid}`, `{sheet}`, `{sheets}`, …).
- **Saddle-stitch sheet layouts:** classic TwoUp, StackedCopies (N spread copies in rows — cut into strips then fold; digital workflow) and the 8-page signature (2×2 form, top row rotated 180°, folded twice).
- **Gatefold styles:** 3-panel (6 pages) and 4-panel (8 pages) with a configurable fold reduction so the inward-folding wings do not collide at the centre.
- 231 unit tests passing (range parser, page subset extraction, PDFX_def generator, mark layouts, binding fixes, and more).

### Fixed
- **PDF/X export produces a conformant OutputIntent and actually uses the selected ICC profile** (previously a dead option — the Ghostscript arguments were incomplete). Also fixed: a Ghostscript stdout/stderr pipe deadlock (streams now drained concurrently), `--permit-file-read` for GS 10.x SAFER mode, orphaned GS processes on cancel, and the PDF/X output file naming.
- **Creep compensation direction and sidedness** — the maximum shift previously landed on the *outer* sheet (inverted gradient); both leaves of each folded sheet now shift toward the spine correctly on front and back.
- **Z-fold back-side page order** is now [5, 6, 1] (was [6, 1, 5], a physically impossible leaf pairing).
- **Roll-fed duplex backs are mirrored across the web width** with the correct side flag, so multi-design runs register back-to-front.
- **Wiro supports duplex** (previously always single-sided); adjacent pages share a leaf with a mirrored verso.
- License-key validation runs fully in memory (no more temp-file writes to `%TEMP%`); downloaded update installers are verified against the GitHub asset SHA-256 digest before launch; sheet numbering `{1}` shows the true planned sheet total; cancellation-token lifecycle fixed in the main window; the composer no longer re-parses the whole source PDF just to read its page count; marks profiles store enums as readable names.

## [1.4.1] - 2026-06-10

### Fixed
- **Bleed now extends past the cut line, as prepress requires.** The composer used to scale the whole page (bleed included) *into* the trim cell, shrinking the trim and leaving white gaps between items instead of overlapping bleed. A single geometry helper (`BleedFit`, trim→cell mapping) is now shared by the export composer and the preview canvas, so what you see is what prints. Preview clips excess bleed at the sheet edge like the output does.
- **False `TRIM_MISMATCH` preflight warnings** — the analyzer reported the CropBox as "trim"; for correctly-made files with bleed, CropBox = trim + bleed, so every good file warned. The real TrimBox is now read (with the spec-mandated Trim→Crop→Media fallback).
- The preview ruler now respects the light theme.

### Added
- First regression coverage of the PDF composition layer: `BleedFit` unit tests plus a differential composer test (compose → rasterise → assert against the source raster).

## [1.4.0] - 2026-06-07

### Added
- **Operation history (the "History" tab)** in the right inspector: session journal of loads, plans, exports, edits — each entry stores a restorable workspace snapshot. Hovering an entry reveals **↺ restore**, which brings back the entire project state from that point (files, template, layout, export options). Entries are stored as localisation keys, so descriptions switch language live; the list is capped at 200 entries.
- Collapsible inspector panel — a slim chevron handle gives the preview the full window width.

### Changed
- Activation window polish: semantic status colouring (green active / red invalid), theme-aware cards, and the `.lic` file loader moved above the key field as the primary path.

## [1.3.6] - 2026-06-07

### Fixed
- Wrong "license control file was modified" lockout when switching licenses on the same machine (trial → paid, renewal). The clock-rollback stamp is HMAC-keyed per license, so a stamp from a *different* license is now treated as first-run instead of tampering.

### Added
- Restart reminder after loading a key or license file; license-server admin panel gained "Download .lic" and "Copy key" actions.

## [1.3.5] - 2026-06-06

### Added
- **Machine-ID export to a `.lic` file** from the activation window — upload it to the trial-license generator instead of copy-pasting the fingerprint.
- **PDF merge ("Merge…")** in the source panel: combines two or more checked files into a single PDF on disk, with a suggested `merged_<date>_<files>_<pages>.pdf` name.

## [1.3.4] - 2026-05-24

### Added
- **Step & Repeat as a dedicated binding:** replicates one page across an N×M grid with four per-copy rotation patterns (none / alternating rows / alternating columns / checkerboard at 180°) and two spacing modes (gap-based like ganging, or centre-to-centre pitch for machine-defined constants). Validation rejects pitches smaller than the item (overlapping copies). Ships with a quick-start preset and 7 new tests.
- **Marks v2 — named marks profiles** instead of loose checkboxes: a profile bundles crop / registration / control bar / fold / slug / contour-cut settings as a single JSON file. Control bar in four variants (CMYK process, CMYK+tints, GATF-11, Brunner-21); registration marks in four styles × three positions; slug line with `{filename}`, `{date[:format]}`, `{sheet}`, `{sheets}`, `{side}`, `{jobid}`, `{template}` placeholders; per-mark ink choice (process black vs. registration black). Fully backward compatible — v1.3.x templates render identically via a legacy adapter.
- **In-app marks profile editor** (list, edit, save-as, reset built-ins, delete) with export and preview consuming the same resolved profile.

### Changed
- Built-in template presets and marks profiles now have localised display names (PL + EN).

## [1.3.2] - 2026-05-23

### Changed
- **Full mm / inch support across the UI.** The model stays metric; conversion happens only at the UI boundary. All ~20 dimension fields in the template editor, the selection inspector, toolbar/status readouts and dialogs display and accept the active unit, with dynamic unit labels and live refresh on unit change.

### Added
- **ICC profile details in File Info:** the OUTPUT INTENT section now shows real embedded-profile data (colour space, profile name, registry) via a new minimal ICC parser; indirect `/DestOutputProfile` references are resolved and decoded.
- Bundled-ICC plumbing: every `.icc`/`.icm` dropped into the app's `icc/` folder ships with the installer and appears in the export dialog.
- US/ANSI paper presets (ANSI A–E, aliases for Letter/Tabloid) and six US-format template suggestions (Letter/Half-Letter/Quarter-Letter flyers, Tabloid/ANSI C/ANSI D posters).

## [1.3.1] - 2026-05-21

### Changed
- Topbar action buttons: "Plan" got a distinct light-blue style; "Export" text forced to white for full contrast on the accent background.

## [1.3.0] - 2026-05-18

### Fixed
- **Drag-and-drop from Explorer finally works.** Root cause: `[STAThread]` is lost after the first `await` in an async `Main` — Win32 `RegisterDragDrop` requires an STA thread, so the GUI now starts synchronously. Defence-in-depth `AllowDrop` registration, elevation detection at startup (UAC breaks drag-and-drop between integrity levels), and an Avalonia 11.0.10 → 11.2.7 upgrade close out the whole DnD bug class.
- Crop marks, registration marks and Summa OPOS markers are now rendered on the preview canvas (previously export-only), with a white-halo/black-core technique for visibility on any background and minimum pixel sizes at extreme zoom-out.
- Template suggestions: invisible suggestion cards under Avalonia 11.2 (theme-variant resource lookup), "Plan failed" after applying a suggestion (the adapter adopted rotated-fit N-up values inconsistent with the model), and bindings wrongly inherited from the workflow template (a business-card preset no longer becomes a saddle-stitch booklet).

### Added
- Empty-state onboarding: an SRA3 sheet mockup with a drop zone and a 1·2·3 checklist instead of a bare "no preview" label; full-canvas drag-hover overlay; inspector skeleton with a cheat sheet.
- Export progress with named phases in the status bar (analysis → preflight → planning → composing → PDF/X) plus a mini progress bar; Ghostscript auto-detection; the success message names both output files when PDF/X is enabled.

### Changed
- Floating canvas toolbar reorganised into four clear groups (zoom / view toggles / front–back / CMYK strip with an "All" button).
- Roll-fed: the item block is centred across the web, the default roll sheet margin is 5 mm so edge marks are not clipped, and a 2-page PDF suggests duplex automatically.
- Topbar, step rail and panels polish: license badge with expiry warning tint, hover actions on the file list, dynamic "Load N checked" label, fixed status-bar overlap on narrow windows, mojibake fixes.

## [1.2.4] - 2026-05-17

### Added
- **Context-aware template suggestions:** a two-tier dialog — presets adapted to *your current sheet, margins and workflow* on top (N-up recomputed by the layout optimizer per suggestion, with an amber warning when an item only fits 1×1), original library presets below.
- **Roll-fed duplex:** a 2-page PDF puts design A on every front and design B on every back; spread view and the front/back toggle now work for roll jobs, with an extreme-aspect-aware spread layout.
- **Measurement units (mm / inches)** in Settings, applied live via an observable units service.
- **Preview ruler** along the top and left canvas edges in the active unit, with automatic tick spacing, an origin marker and a unit chip.

### Fixed
- "Reset all" restores the default template, so the next load can plan and preview again.

## [1.2.3] - 2026-05-17

### Added
- **Preflight v2:** content-based bleed detection (a per-page content bounding box detects declared-but-unused bleed → white strips after trimming) and a narrative summary banner with four mood states above the issue list.
- **Template suggestions from PDF dimensions:** a 25-preset library (business cards, postcards, flyers, posters, labels…) matched by trim size with rotation awareness; auto-registers the "Roll 320" paper preset.
- **Layout optimizer (auto N-up):** tries both item orientations and reports e.g. "Optimal layout: 6 → 21 items per sheet (3×7)".
- Preview trim/bleed guides (cyan dashed trim, magenta dotted bleed) and a manual "Check for updates…" entry.

### Fixed
- Ganging duplex pairing: a 2-page PDF at 12-up now yields one sheet with 12 fronts and 12 backs (previously two single-sided sheets).
- Preview UX: 1-based sheet counter, front/back toggle in spread view, no more blank sheet when leaving spread view on single-sided jobs, CMYK bars drawn inside the sheet, overview-mode threshold tuned.

## [1.2.2] - 2026-05-16

### Fixed
- **Preview memory explosion on 1000+-slot rolls: 3.5 GB → ~70 MB.** Memory-budget LRU cache (hard 512 MB budget), per-bitmap scale clamping (64 MB cap), display-pixel-aware rendering (bitmaps sized to actual on-screen demand, Fiery-style), lower scale buckets, and per-cache-key cancellation of stale renders.
- Roll-fed forces single-sheet view (spread is a booklet concept; it made no sense for a continuous web).

### Added
- **Vector overview mode:** below 50% zoom the canvas draws vector placeholders with page numbers instead of rasterising PDFs — zero background renders while flying over sheets.

### Changed
- Roll-aware sheet layout: extreme-aspect sheets (>4:1) fit by the shorter dimension and align to the start of the web — no more 22-pixel sliver previews; standard formats unchanged.

## [1.2.1] - 2026-05-16

### Fixed
- **Template editor keeps its state** across dialog close/reopen (view-model lifetime change) and **validation blocks closing** with an invalid template: a red error banner lists every problem, all messages localised, template IDs auto-generate when blank, and the editor starts from sane defaults (SRA3 landscape, A4 trim).

### Added
- Roll length as a split/continuous toggle with a segment-length field (replacing the "0 = unlimited" anti-pattern), per-section enable toggles for Roll and Overlay sections, and a "50×50 stickers on a 320 mm roll" preset.

### Changed
- Sheet width/height auto-fill after choosing a paper preset; simplified sheet-size section; website updates (roll-fed, OPOS, logo overlay).

## [1.2.0] - 2026-05-15

### Added
- **Roll-fed binding (step-and-repeat on a web)** for label and sticker production: variable repeat count, automatic per-row packing, multi-segment splitting with a maximum segment length, and basic nesting (auto 0°/90° orientation choice).
- **Summa OPOS / OPOS-XY cutter markers:** four solid black squares (default 3 mm) at the corners of the placement bounding box for print-and-cut plotters; OPOS-XY adds a human-readable sheet label.
- **Per-sheet logo overlay:** stamps a PNG/JPG/single-page PDF on every output sheet with nine anchor positions, offsets and max-width scaling.
- Template editor panels for Roll, plotter markers and overlay; 65 unit tests passing.

## [1.1.1] - 2026-05-10

### Fixed
- **Critical: the application no longer writes to Program Files.** All dynamic data moved to `%APPDATA%` / `%LOCALAPPDATA%` behind a central `AppPaths` helper, with an idempotent one-time migration of 1.0.x licenses and templates, unified directory naming, and user-imported ICC profiles in `%APPDATA%\imPRESS Studio\icc`.
- Backend hardening: hard guard clauses on every I/O boundary, atomic settings writes with corrupted-config rotation, a startup summary log of all key paths, and less log noise.

### Added
- Hot-folder UX after real production testing: auto-defaulted `.work`/`.archive`/`.error` folders, real-time per-field validation in the edit dialog, a license-status banner, an orphan-recovery banner after a crash, and Start/Restart disabled while the license is invalid.

## [1.1.0] - 2026-05-09

### Added
- **Hot folders — flagship automation feature.** Watched input folders impose every dropped PDF with an assigned template and write the result to an output folder. Multiple concurrent folders with isolated runners; multi-layer copy-completion detection; polling fallback for unreliable SMB/DFS watchers; crash recovery via a WAL-mode SQLite ledger; fingerprint-based duplicate suppression (SHA-256 of the first 1 MB + size + mtime); Polly retry policies with exponential backoff and jitter; a global Ghostscript process throttle; per-job license validation.
- Hot folder manager UI (add/edit/delete/start/stop/restart, status list, last-error banner, full PL/EN localisation) and CLI verbs `hotfolder run|list|start|stop|reload`.
- Config in `%APPDATA%\imPRESS Studio\hotfolders.json` (atomic writes, strict validation, long-path support).
- Production resilience: an atomic file state machine (input → `.work` → output/archive/error), bounded job queue, watcher-overflow recovery with full rescans, and top-level exception guards on every loop.

## [1.0.8] - 2026-04-26

### Added
- Full ICC profile management in the export window (dropdown with auto-scan of the `icc/` folders, refresh, "open folder" helper).
- **Clock-rollback protection for licenses:** every successful validation stores an HMAC-signed high-water timestamp; turning the system clock back to dodge expiry is detected and refused.

## [1.0.7] - 2026-04-26

### Fixed
- **Export without a valid license — patched.** Clicking Export re-validates the license on disk every time; the previous cached check could be bypassed by deleting the license file mid-session.

### Changed
- **New hardware fingerprint:** hostname dropped (trivially changeable); three independent SHA-256 components — physical NIC MAC (virtual adapters filtered), system-disk serial, motherboard UUID — with a **2-of-3 tolerance**, so replacing one component does not invalidate the license. Pre-1.0.7 single-hash licenses still validate via a fallback.

### Added
- Publisher tooling: fingerprint string in the activation window, `tools/Issue-License.ps1`, extended `license fingerprint` / `license issue` CLI.

## [1.0.6] - 2026-04-26

### Changed
- **Progressive zoom:** while a sharper scale renders in the background, the canvas instantly shows the best already-cached bitmap (Google Maps-style); coarser scale buckets (5 levels instead of 19) and cancel-on-zoom-spam keep rendering costs flat; the CMYK/overprint filter cache is keyed by source-bitmap reference.

## [1.0.5] - 2026-04-26

### Added
- **CMYK channel toggles** in the preview toolbar — hide any subset of inks (on-screen separation simulation).
- **Colour eyedropper** — a floating readout of RGB and CMYK values under the cursor.

## [1.0.4] - 2026-04-26

### Added
- Parallel page rasterisation with a bounded thread pool, prefetching of adjacent sheets, zoom quantisation into cache buckets, and an LRU bitmap cache.

### Fixed
- Strict preflight no longer throws when an interactive report dialog is available; a race condition in the preflight report dialog (UI-thread marshalling); installer script guard with a readable error when `publish/` is missing.

## [1.0.3] - 2026-04-26

### Added
- **PDF/X-1a:2001 export** alongside PDF/X-4 (PDF 1.4, CMYK+spot, flattened transparency) with Ghostscript arguments selected per standard.
- **Preflight report dialog before export** — warnings (RGB-only, missing ICC, encrypted source, oversized pages…) are listed for the operator to confirm or cancel.
- Sheet-number stamp visible in the preview (previously export-only).

## [1.0.2] - 2026-04-26

### Added
- **Sheet numbering** (position, format `{0}/{1}`, font size, margin) stamped on every output sheet.
- Ghostscript file picker; Inno Setup installer (install location, Start menu folder, desktop shortcut); auto-update via GitHub Releases.

### Fixed
- Export options (Ghostscript path, ICC profile, preflight, sheet range) persist in `%APPDATA%` between sessions.

### Changed
- Versioning normalised to SemVer (1.01 → 1.0.2 line); Velopack replaced with Inno Setup.

## [1.0.1] - 2026-04-19

### Added
- Application menu bar (File, Edit, View, Tools, Settings, Help) with visible shortcuts; a Settings window with UI language choice (Polish/English); the i18n infrastructure (PL/EN dictionaries, XAML `{loc:Loc}` markup, live language switching); gang-run presets (DL 3-up, business cards 6/8/12-up).

## [1.0.0] - 2026-04-18

First official production release ("First Impress").

### Added
- Licensing system: activation by Base64 key or `.json` file, hardware binding, Trial/Standard/Pro editions; activation window with a copyable machine ID; license CLI (`keygen`, `fingerprint`, `issue`, `activate`, `info`).
- File-info panel with full PDF metadata; application icon and logo; rebranding from "Imposition Studio" to **imPRESS Studio**.

### Security
- RSA-2048 signature verification of licenses (the private key never ships with the application); SHA-256 hardware hash.

## [0.9] - 2026-04-10

### Added
- Multi-file loader with checkboxes, source-file merging before imposition, list reordering, per-file info window, "Reset all".

### Fixed
- Hang when loading an encrypted PDF; template name display in the title bar.

## [0.8] - 2026-03-28

### Added
- Statistics panel (paper area, utilisation, waste, cost estimate, weight) with real-time inputs for sheet price and grammage; binding info card; sheet thumbnail strip.

## [0.7] - 2026-03-15

### Added
- Interactive sheet preview on the imposition canvas: cursor-centred zoom, panning, 90° view rotation, spread view, creep visualisation, overprint simulation, keyboard shortcuts (Ctrl+P/E/O, Ctrl+0, F, R, ←/→).

## [0.6] - 2026-02-25

### Added
- Printer marks (crop, registration, CMYK bars, fold marks); creep compensation with configurable paper thickness; Z-fold and accordion options; custom paper sizes.

### Changed
- Extended margin model: bleed, safe zone, gutter, spine, sheet margin.

## [0.5] - 2026-02-10

### Added
- PDF/X-4 conversion via Ghostscript (ISO 15930-7) with ICC profiles; strict preflight; export sheet ranges; the `impose` CLI command.

## [0.4] - 2026-01-20

### Added
- Template manager with full parameter editing, JSON import/export, ready-made templates, and pre-run validation.

## [0.3] - 2026-01-08

### Added
- Perfect binding with spine support; initial Z-fold; configurable signatures (4/8/16 pages).

## [0.2] - 2025-12-18

### Added
- First working imposition: saddle stitch 2×1; PDF export via PdfSharpCore; drag & drop; status bar.

## [0.1] - 2025-12-01

### Added
- Project skeleton: Avalonia 11 + .NET 8 + MVVM; module split (Core / Pdf / Export / Ui / Utils / App); PDF loading and analysis via PdfPig; DI container; Serilog logging.
