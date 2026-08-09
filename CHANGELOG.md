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

## [1.5.10] - 2026-08-08

Ganging stops being a copier. The N-up grid can now carry the document's consecutive pages instead of the same page over and over — the difference between imposing a 5-page job and imposing page 1 of it five times.

### Added
- **Source pages can be reordered by dragging their thumbnails.** Drag a page in the preview strip to a new position and the loaded document is rewritten in that order, so it is what gets planned, imposed, exported and saved — there is no separate "apply" step to forget. A banner appears once the order has been touched, with a *Restore order* button back to the natural sequence.
  - **The drag says where the page will land.** The tile follows the pointer as a ghost, the tile it came from fades so its origin stays visible, and an insertion caret marks the exact gap the page will drop into — a wrapped grid of look-alike tiles otherwise gives the operator nothing to aim at. The caret overlays the tile rather than occupying columns of its own: reserving space for it widened every tile by 6 px, which at the inspector's width was enough to push a tile onto the next row and break up the strip. An indicator that only appears mid-drag must not change the layout it is pointing at. The caret sits on one side of a tile while the move removes the page before re-inserting it, so the index arithmetic that keeps the caret's promise is pinned by tests: an off-by-one there lands the page one slot from where it was aimed and reads as the feature simply being inaccurate. A page dropped outside the strip cancels the drag instead of being answered with "only PDF files are accepted".
  - **The order survives the page edits that rebuild the document.** Rotating or converting a page re-assembles the loaded document from the source files, in file order, so the arrangement is held as a permutation and re-applied after every assembly rather than by rewriting whatever file happens to be loaded. Otherwise an operator would reorder, rotate one page, and get their arrangement silently reverted — noticed on the printed sheet.
  - **Saving honours the order too.** *Save PDF* reads a source file rather than the loaded document, and *Merge and save* re-merges from the file list — both would have handed back the original sequence. A manual order belongs to the loaded document, so when that document is the file being saved, the save reads it; the redirect is scoped to exactly that case, and a per-file save otherwise stays a per-file save.
  - Page edits address physical pages while the strip names positions, and after a move the two numberings differ. Rotating "the first thumbnail" now rotates the page that is actually first, not the one that used to be. A permutation whose length no longer matches the document is dropped rather than applied: the set of pages changed underneath it, and a stale permutation would scramble the job instead of preserving an intent.

- **The in-app help was rewritten against what the app actually does, in both languages.** It still described a three-binding, session-only-custom-sizes version of the program. Ten sections are new — the step bar and what each numbered step really opens, source files and per-page edits, ganging slot modes, layout optimisation and the sheet advisor, template suggestions, hand-editing slots, marks profiles, preflight (both levels, the five press profiles, and which findings have repairs and which deliberately do not), hot folders, and application settings — and the existing ones were brought up to date: all nine bindings instead of three, the current export dialog, preview tools including separations, eyedropper and the registration overlay, and a glossary that now covers ganging, work-and-turn, TAC, trapping, output intents and the PDF/X flavours. Two stale claims are gone: custom paper sizes persist across restarts, and the shortcut table lists the undo/redo and F1 keys the app has had for a while. A test pins every help key to both dictionaries, because a section translated in one language reaches the operator as a raw key in the middle of the manual.

- **Ganging can now fill its slots with consecutive document pages, not only with copies of one.** Ganging had exactly one behaviour: every slot on a sheet carries the same source page, with duplex pairing pages 1-2 onto the front/back of sheet 1, 3-4 onto sheet 2, and so on. That is right for a business-card run — one design, twelve copies, verso on the back — and wrong for the thing the presets are named after: a 5-page document ganged 2-up came out as three sheets of one page repeated, with half the document on versos and the operator convinced pages 2 and 4 had been dropped. **ConsecutivePages** fills the slots in reading order instead: 1+2, 3+4, 5. With duplex each slot becomes its own leaf (odd page recto, next page verso, X-mirrored so it backs up after a long-edge flip), and work-and-turn pairs each product across the fold rather than repeating one.
  - **Nothing changes for templates that already exist.** The mode defaults to RepeatItem, which is what every saved template and every hot folder has been doing; a template written before this release deserializes into exactly the press sheets it produced yesterday. The multi-page gang presets — A4 2-up, A5 4-up and their Letter / Half-Letter twins — ship with ConsecutivePages, because "2× A4 on the sheet" has always meant two different A4s. The business-card, DL-leaflet and rack-card presets stay on RepeatItem: those are single-design runs.
  - The template manager grows a *Ganging* card explaining, in one sentence per mode, what the current setting does to a multi-page document — the question the old behaviour raised and never answered.

### Changed
- **Switching language or units now changes what is on screen immediately.** The setting was already pushed into the running app; what did not work was the binding almost every caption in the program uses.
  - **`{loc:Loc}` had never followed a language change at all.** The markup extension bound to the localisation service's INDEXER and relied on a WPF-style `PropertyChanged("Item[]")` notification, which Avalonia does not treat as invalidating an indexer path. Every menu, button, label, tab, dialog and tooltip in the application rendered correctly on first paint and then froze in the language the app had started in — for the life of the process. Only the handful of view-model properties that notify by name ever updated, which is exactly why this read as "some of the interface changes" instead of "the mechanism is dead". The extension now binds to a plain property that really does raise PropertyChanged and maps it to text through a converter, so one change fixes every caption in the app; `{loc:UnitLabel}` had the same broken leg and gets the same fix. A headless test drives a real bound control through a language switch, because the defect was invisible at every level above the binding engine.
  - Text that no XAML binding covers had to be pushed by hand, and now is: the sheet canvas repaints and drops its composed scene (the old wording was baked into the bitmap), the preview's front/back labels re-emit, and source-page captions are retranslated in place rather than by rebuilding the strip, which would have thrown away every rendered miniature to change a caption.
  - Readouts that resolve through a **converter** — the "no template" placeholders on the step bar and the template card, the Yes/No duplex line — only re-run when their binding source changes, so they are nudged explicitly. The "Load checked (3)" caption and the licence badge follow too: the badge is now composed from the licence *facts* held in the view-model instead of a sentence built once at startup.
  - The idle "Ready" greeting follows the language; a status message about something that happened does not. "Loaded 12 pages" was true in the language it was written in — retranslating a record would be a small lie, and the settings hint now says so instead of promising a restart nothing needs.
- **Step (4) is now called "Export settings", not "Export".** It opens the options dialog; the green button on the right of the same bar is what writes the file. Two buttons a hand's width apart, both labelled export and only one of them exporting, is a trap the step bar laid every time. The numbered step also loses its ▶ glyph, which read as "run this" right next to the button that actually does.

### Fixed
- **Editing a slot moved the outlines but not the page.** Position, size, rotation and source-page changes in the inspector reached the plan correctly and then appeared to do nothing: the canvas caches its whole composed scene into one bitmap and re-blits it while nothing in its snapshot key changes, and that key carried the sheet NUMBER but nothing about the sheet's content. An edited sheet therefore looked identical to the cached one. The guides and the selection outline are recomputed every frame and did move, which is exactly why this read as "only lines show, physically nothing happens to the image". The cache now compares the sheet instance itself, which every edit path — including undo and redo — replaces rather than mutates.
- **Rotating a page right and then left left it permanently landscape.** The rotation is applied to a working copy by changing the page's `/Rotate`, and writing that through PdfSharp's page wrapper makes the library resolve the page's VISUAL size and bake it into the MediaBox on save. So the round trip came back to `/Rotate 0` on an A4 whose box had become 842×595: the content was upright again, the sheet was not. Measured on a portrait A4 — right then left used to yield `media=842×595`, and now yields `595×842`, identical to the original. The rotation is written straight into the page dictionary instead, which leaves the geometry exactly as the file declared it; the page-tree walk that finds the page also handles nested `/Pages` nodes and inherited `/Rotate`, both of which a flat lookup got wrong on documents assembled from several sources.
- **A freshly loaded document stayed soft until the "Str. N" badge faded.** Two independent defects, and the badge really was implicated in one of them.
  - **The canvas kept blitting a stale composed scene.** The canvas caches its whole composed scene into one bitmap and re-blits it while nothing relevant changes. It flagged that cache as provisional when a page had drawn a *loading placeholder*, but not when the page had drawn a **stand-in bitmap from another scale** — so a scene painted with a 150 px thumbnail blown up to full size counted as final. The completed full-resolution render then had nothing to invalidate, and the soft scene stayed on screen until some unrelated part of the cache key changed. On a freshly loaded document the only thing that changed on its own was the page badge's fade phase, two seconds later — which is exactly when operators saw the preview snap into focus. Stand-ins are now flagged like placeholders, so the real render replaces them the moment it lands.
  - **The thumbnail strip cancelled the canvas's render.** The canvas and the strip share one renderer and ask it for the same page at wildly different sizes — display resolution against 150 px — but it kept a single "newest scale wanted" slot per page. Every thumbnail request marked the canvas's full-resolution render stale; that render bailed out at the concurrency gate. The strip re-requests its visible window on every completed render, so this repeated until the whole strip was cached. Requests now declare which lane they belong to and the two no longer cancel each other. Measured on a 595×842 page: the shared slot left a 446 px bitmap where the canvas had asked for 2677 px.
- **"Optimize N-up" left the N-up fields showing the old grid.** The optimiser wrote the new layout straight into the template, which is a plain object with no change notification, so the status bar announced "4×5" while the spinners next to it still read "4×3" — and the operator had to retype the numbers the button had just computed. The fields now follow the template. Their upper bound moves from 16 to 100 at the same time: a small item on a large sheet genuinely gangs past sixteen rows, and a spinner capped at 16 would have quietly written that cap back over the computed layout the moment it started refreshing.
- **Clicking a source-page thumbnail threw you out of the source-page list.** The canvas jumping to the sheet that page landed on is the point of the click; switching the navigator to the *sheets* list as well meant the operator lost their place after every single click and had to switch tabs back to look at the next page. The canvas still jumps, the sheet tile is still highlighted, and the list you are browsing stays put.

## [1.5.9] - 2026-08-07

The sheet you pick is the sheet you get. Three defects in the template manager conspired to impose jobs on a sheet nobody chose, and the N-up button only ever knew how to make a layout bigger.

### Fixed
- **The sheet picked in the template manager could be ignored, and the job planned on the preset's sheet instead.** Clicking a preset replaced the template — including its sheet — but never told the paper dropdown, so the dropdown kept pointing at the previous pick. Selecting that same entry again raises no change notification, which left the operator no way to get their sheet back: "Business cards 12-up, then B1 landscape" imposed on SRA3. The dropdown and the landscape tick now follow whatever sheet the template actually carries — after a preset click, a template load and *New*.
- **The paper dropdown went blank after applying any landscape preset.** The sync looked the sheet's name up verbatim, and a landscape sheet is named "SRA3 (L)", which is not a paper-preset name — so every ISO preset in the quick-start list (all of which are landscape) left the field empty while describing itself as "SRA3(L) → 12× business card". The name is now read with its orientation suffix, and matched on dimensions when the name says nothing usable. A sheet no preset describes clears the dropdown rather than showing a name that lies about what will print.
- **"Optimize N-up" could not make a layout smaller.** It compared the current N-up against a *denser* packing and stopped there, so a template moved to a smaller sheet — 12-up business cards from SRA3 to A4, 374 mm of layout against 210 mm of paper — was told it was "already optimal", and the mismatch surfaced much later as a validation error at export. The button now fits the layout to the sheet in both directions, using the same arithmetic (and the same tolerance) validation uses, so it can never call a layout correct that export will refuse. When not even one item fits, it says so and leaves the operator's numbers alone instead of writing an N-up of zero.
  - **It no longer applies a grid that assumes a rotated item.** The optimiser considers both item orientations, and the denser one often needs the item turned 90° — but the N-up fields and the trim describe one layout together, so writing the rotated grid while leaving the trim as authored produced a layout that fit *worse* than the one it replaced. Rotation stays what it always claimed to be in the status bar: a suggestion.
- **The sheet card now shows the sheet it holds** — name, width × height in the active unit. The dropdown was the only feedback in that card, so a sheet it could not name showed as nothing at all.

## [1.5.8] - 2026-08-03

Template suggestions stop being a distance ranking and start being a recommendation: six weighted signals, an explanation for every entry, and a memory of what this particular shop actually prints.

### Added
- **Template suggestions are ranked on six weighted factors instead of dimensional distance alone.** Distance still dominates by design — an operator picked the product their PDF *is*, not whatever packs densest — but proportion agreement, page count, sheet fit, workflow fit and how product-specific a preset is now all contribute. Every weight lives in one place as a named constant, and the six sum to exactly 1.0, which a test enforces so a future edit cannot quietly move every quality band.
  - **Sheet efficiency is deliberately barred from reordering products.** A 90×50 business card and a 90×50 roll label are the same size and gang completely differently; letting yield decide would swap the product under the operator. It contributes to the score shown on the card and to nothing else. Yield comparisons are only meaningful across sheets for *one* product — which is what the alternative-sheet panel is for.
- **Every suggestion now explains itself.** Each card carries a short checklist — "dimensions match to the millimetre", "page count suits this product", "fits your current sheet and binding", "matches your measurement system" — built only from reasons that actually hold. A checklist padded with weak entries teaches operators to stop reading it, so a preset whose proportions merely happen to agree does not claim that as a reason when the dimensions already matched exactly.
- **Alternative press sheets are suggested when they would yield materially more items.** A card whose layout leaves half the sheet empty now says which other sheet would gang it denser, with the item count and the improvement factor. **It never changes the sheet the suggestion applies** — which sheet a shop runs depends on its press, its stock and its finishing, none of which the engine can see. It makes the trade-off visible and leaves the decision where it belongs. Roll-fed products are excluded: segment length is derived, not chosen, so "a bigger sheet" is not something the operator can act on.
- **Products are categorised and the library list is grouped by category.** Business cards, leaflets, booklets, posters, labels, stickers, tickets, hang tags and the rest each get a heading, ordered by their best-ranked member so the most likely product still comes first and nothing is reordered within a group. "Show me the other labels" is a real question and it used to require reading a flat list of forty entries.
- **Four match-quality grades instead of two.** Exact / Close is still what the engine reports to any existing caller — that contract is unchanged — but the card now shows Perfect, Excellent, Good or Worth checking, so a near-tie and a stretch no longer look identical. A dimensional snap can never be graded below Excellent: being the same size as the product is the strongest evidence available, and no combination of weaker signals should talk us out of it.
- **The engine learns which preset this shop picks for a given size.** Open an 85×55 file, choose "Business card", and the next 85×55 file ranks that preset higher. Three deliberate picks for the same size is already a habit rather than a coincidence, so the bonus reaches full strength quickly — and it is capped low enough that learned habit can promote a near-tie and never override a clear dimensional mismatch. A shop that has printed a hundred business cards must still be told when the file in front of them is an A4 leaflet. Sizes are bucketed, so an 85.04×54.98 export of the same card feeds the same history entry rather than building a second half-strength one, and a rotated PDF matches the same entry.
  - **It persists.** The history is written to `suggestion-preferences.json` in the roaming profile and survives closing the app — a preference that had to be relearned every morning would be the same as no preference at all. Nothing leaves the machine, and *Settings → Suggestion learning* shows how much has been remembered with a button to forget all of it, for the shop that changes press or product mix.
  - **It can change its mind.** Counts are capped at the saturation point and every rival preset for that size loses a point when another is chosen, so switching from cards to labels at 90×50 takes the same three deliberate picks that established the old habit — rather than having to outvote a tally that has been growing for a year. A hand-edited history file cannot buy an unbeatable preference either: counts are clamped on load.

- **Creep compensation accepts grammage instead of caliper.** The paper can now be described the way it is actually quoted — "90 gsm" off the ream wrapper — rather than by a thickness nobody has measured. One or the other is authoritative, never both: picking a mode disables the other input and the imposition reads a single resolved figure, so two half-remembered numbers can never disagree.
  - **Grammage needs the paper family, and that is not a formality.** Mass per square metre says nothing about thickness on its own: 90 gsm gloss art is about 0.072 mm while 90 gsm bulky book paper is about 0.162 mm — more than twice as thick. Creep is a shift applied to page content, so a 2× error moves artwork further from correct than switching compensation off would. Five families ship with their mid-range bulk (gloss art, silk, uncoated offset, recycled, bulky), and *Custom* takes the figure from the mill's data sheet.
  - The dialog shows what the description resolves to — "0.117 mm (117 µm)" — in both modes, so the conversion is visible rather than implied, and a setup that resolves to nothing is refused at validation instead of silently imposing without compensation.
  - Existing templates are untouched: they carry only a thickness, the mode defaults to thickness, and they impose exactly as before.
- **Preflight now checks the safe zone.** Text that crosses the trim line is reported as an error; text that clears the trim but sits closer to it than the safe zone is a warning. A guillotine works to a few tenths of a millimetre and a clamped stack shifts, so text 1 mm from the edge survives the proof and loses its last character somewhere in the middle of the run — the single most common reason a job comes back. Only **text** is measured, never the combined content box: a background running past the trim is the entire point of bleed, and testing against it would raise a finding on essentially every correctly-prepared file. The template's own safe zone wins when it declares one, because a hang tag punched near the edge needs more room than a business card.
- **Preflight now checks total area coverage against the press limit.** Too much ink in one place does not dry: it sets off onto the back of the next sheet, blocks in the delivery, and on a web can pick the paper apart. It is one of the few defects that ruins the whole run rather than one copy, and nothing in preflight looked for it. A small heavy element is a warning; the same coverage over a postcard-sized area, or anything a fifth past the limit, is an error.
  - **The numbers are read from the file, not measured from a render.** The check uses the CMYK values the content stream actually declares, so a panel authored at 370% reports 370%. Recovering that from a rasterised page is not possible even in principle — rendering flattens to RGB, and converting back to CMYK rebuilds near-black as mostly K. Measured against this release's own test file, the same 370% panel comes back at about 105% through the raster route. An ink check built that way would quietly clear files that flood the press, which is worse than having no check at all.
  - **It says what it did not look at.** Placed images carry their own pixels and are not inspected, so when a flagged page also contains images the report says so rather than implying the page was cleared. The on-demand coverage scan in the file-info dialog keeps its place as a relative "which page is heaviest" signal and now states plainly that its averages under-report.
- **Preflight now checks how black is built** — two opposite mistakes, both worth a rerun. Small text set in a four-colour black shows every registration error as coloured fringing and belongs in K alone; a large solid in flat 100% K with nothing underneath dries to a washed-out, mottled grey and wants support from the other separations. Reported, never repaired: the right support values depend on the press, the stock and the ink limit, and a designer may have chosen flat K deliberately to keep a fold or a barcode clean.
- **Preflight now reports trapping — strictly what the file says about itself.** Three facts, each of which changes what happens next: whether the document declares a `/Trapped` state at all (PDF/X-1a and PDF/X-3 require an explicit True or False, and "Unknown" is reported separately from "nobody set it"); whether trap networks are already baked into the pages, which are tied to the separations and resolution they were built for and go stale when colour is converted or the job is re-imposed — and which PDF/X-4 does not permit at all; and whether a file that declares itself untrapped is carrying spot colours, since spot-to-process boundaries are exactly where a registration error shows as a white line.
  - **It does not decide where a trap belongs, and does not pretend to.** That means comparing every pair of adjacent objects for shared separations across the whole page — a trapping engine, not a preflight rule. A check that guessed at trap placement would be wrong in the direction that costs a reprint, so this one stays inside what it can actually know.
  - Single-separation work is exempt: one ink cannot mis-register against itself, and flagging greyscale jobs would be noise on exactly the jobs that can never benefit.
  - The spot-work warning is a per-profile switch, on for the three offset profiles and off for digital and large format. Shops that trap at the RIP would otherwise be told about every spot job they ever open, which is how a rule earns a reputation for crying wolf.
- **Fixed: the annotation repair could strip a file's trapping.** It cleared the entire `/Annots` array, and trap networks and printer marks live there alongside comments and form fields — so asking it to clean up mark-up could silently remove the trapping the file was prepared with, or the registration marks and colour bars placed as annotations. It now removes only interactive and mark-up annotations and keeps production ones, including any annotation it cannot identify: the cost of keeping a stray comment is cosmetic, the cost of dropping unrecognised trapping is a reprint.
- **Preflight now catches the same spot colour spelled several ways.** `PANTONE 185 C`, `PANTONE 185C` and `Pantone 185 C` in one document are three separations to a RIP — three plates and three wash-ups for an ink the shop ordered once. A file assembled from a client logo, a finisher's die line and an older panel gets there without anyone doing anything unusual, and it is normally discovered at make-ready. The stock suffix is deliberately **kept**: 185 C and 185 U are genuinely different inks, and folding them would turn a typo-catcher into a source of wrong advice.
- **Preflight now reports layers, and specifically when they disagree with themselves.** A layer switched off in the viewer can still be marked to print — which is how a "client comments" or die-line layer nobody can see on the proof ends up on the plate — and equally a visible layer can carry `PrintState /OFF`, so what was approved on screen never reaches the press. Both are reported with the layer names, the first as an error. The basic analysis has always known *whether* a file has layers; knowing *which* one and whether it prints is what an operator can act on.
- **Preflight now flags image encodings a RIP may refuse.** JPEG 2000 and JBIG2 are legal PDF and render perfectly in every viewer, which is exactly why they reach platesetting unnoticed; plenty of production RIPs still refuse them, PDF/X-1a forbids JPEG 2000 outright, and they are also the formats this app's own RGB repair cannot convert. 16-bit images are noted separately — valid, occasionally deliberate, double the data for a precision no press can hold.
- **The suggestion library takes your own products.** *Settings → Your own products* adds a signature folded card, a customer's fixed label size or the die-cut half the town orders, with its own name, size, category and press sheet. They appear alongside the built-ins, group under the same headings and take part in the same learning.
  - This closes a gap this release opened. 1.5.8 taught the ranking to learn which preset a shop prefers — but only among the forty built-ins, so a 100×210 folded card still came back as "Ticket" or "Bookmark", the operator picked the least-wrong entry, and the engine dutifully learned the wrong name and saved it onto the template. Learning preferences without being able to add products was half a mechanism.
  - Your products default to belonging to no measurement system, so they stay visible whether you work in millimetres or inches — hiding an operator's own product from them would be indefensible. Adding a product with an existing name updates it rather than creating a rival for the same size, and the name is the product's identity: renaming starts its learned history over.
- **Prepress profiles.** The thresholds preflight judges files against are now a named, editable set instead of constants in the code — ink limit, safe zone, the three resolution bands, and the colour and black policies. Five ship with the app, covering coated and uncoated sheet-fed offset, newsprint, digital and large format, with the ink limits and resolution floors each process actually calls for. A banner at 120 dpi is correct work, and judging it by the floor that suits a business card was rejecting every properly-prepared large-format job.
  - A profile describes the **press**; the template and export options still supply what belongs to the **job** — expected trim, bleed depth, PDF/X target. Keeping the two apart is what lets one profile serve every job on that press instead of being welded to the dimensions it was created with.
  - Editing a built-in and saving produces a profile of your own; the shipped ones cannot be overwritten, so a future release can still correct a threshold everywhere. Deleting the active profile falls back to the default rather than leaving preflight without one.
- **Hot folders can run the full prepress inspection**, with their own profile — the folder that accepts customer artwork can be strict while the folder fed by a known-good system stays fast. Off by default on purpose: the deep inspection re-parses every file, and 1.5.7 spent real effort removing exactly that kind of duplicate parse from the pipeline. Findings go to the log; they stop a job only when *Strict preflight* is also on, so switching the inspection on never changes what passes — only what you are told.

### Changed
- The settings dialog scrolls instead of clipping. It grew a prepress-profile card this release and no longer fits on a short screen; it still shrinks to its content when there is room.
- Suggestion cards show the match grade next to the confidence percentage, coloured from the grade rather than the raw number so the word and the colour can never disagree.
- **The suggestion engine still does no prepress validation.** Bleed, safe zones, crop marks, page errors and printability remain entirely Preflight's job — a second opinion from a size-matching engine would be a worse one, and two components disagreeing about whether a file is printable is how operators learn to trust neither.

## [1.5.7] - 2026-07-31

One place to browse a job: the sheet strip moves into the inspector, and a thumbnail click stops destroying the plan.

### Fixed
- **Preflight now reports RGB in vector artwork, not only in images.** The rule inspected image objects exclusively, so a file whose RGB lived in a logo, a tinted background or a chart — the common case — passed preflight completely clean and surprised the operator at export time. It is reported as a distinct finding, and the RGB repair handles it.
- **Applying a suggested template left the header reading "No template".** The suggestion engine built its templates with an empty name and a comment stating the UI would fill in the localised one — nothing ever did. An operator who had just picked "Leaflet A4" was told there was no template, and the imposed job still claimed none after planning. The name is now set when the suggestion is selected. It deliberately omits the dimension suffix the card shows: the template gets saved to disk, and a name baked with "in" reads wrong once the operator switches to millimetres — the dimensions live in the template itself and render in whatever unit is active.
- **Template suggestions now follow the unit preference.** A shop configured in inches was offered A4, DL and SRA3 products, even though the template editor's quick-start presets already switched to the US set on the same setting — the two halves of the app disagreed. Suggestions are now filtered to the matching family. Two deliberate exceptions keep the list useful: presets belonging to no national standard (roll labels, stickers, hang tags, tickets) always appear, and an **exact** dimensional match survives regardless of preference — a US shop opening a genuinely A4 file must still be told it is A4.
- **Tooltips and slot selection could land a few dozen pixels off for one frame.** On a frame that rebuilt the preview's scene cache, hit rectangles from the cache-drawing pass (in cache coordinates, offset by the overscan margin) were left in place alongside the correctly recomputed ones, so whichever matched first won. The stale set is now dropped before the rebuild.
- **An in-app update no longer kills the app mid-shutdown.** Accepting an update started the installer and immediately called `Environment.Exit`, which unwinds nothing: the application's exit path never ran, so pending diagnostics were dropped, the last log lines stayed in the buffer, and hot-folder jobs in flight were abandoned with their files stranded in `.work` and their ledger rows stuck at `reserved` until the next start recovered them. On top of that the installer was already running against the live process, racing its own teardown. The app now closes through the normal shutdown path — hot folders drain, telemetry flushes, logs close — and the installer is started only afterwards, against an idle installation. Its SHA-256 is re-checked immediately before launch, so a file swapped after download is refused instead of executed.
- **Feature-usage diagnostics were never actually sent.** The counters behind *Settings → Diagnostics* (export, template manager, hot folders, page edit, Save PDF, preflight) were incremented correctly and then died with the process: the only flush ran at startup — before the operator had opened anything — so every batch left the app with an empty `features` map. There was no periodic flush and no flush on exit, even though the code documented both. Counters are now pushed every 5 minutes and once more on shutdown, and a failed send hands them back to the queue so an offline spell no longer discards a whole session's usage. Empty batches are never sent.
- **Clicking a page thumbnail no longer throws away the imposition.** With a planned job on screen, a left click on a source-page thumbnail rebuilt the canvas in plain page-preview mode: the imposition disappeared, the sheet strip at the bottom of the canvas disappeared with it, and — unannounced — every manual slot edit (move / rotate / blank) was discarded along with its undo history. The click now jumps to the **sheet that page landed on**, flipping to the verso when the page only appears there, and leaves the plan and the edits untouched. A page that is not part of the plan (blanked slot, page range) reports that in the status bar instead of changing the view.
- Leaving the imposition for the plain page preview is now an explicit action ("Show source pages") that says up front when it is about to drop manual slot edits.

### Added
- **Preflight can now repair, not just report.** The file-info dialog's validation tab gained a Repairs panel offering only the fixes that apply to the file in front of you: **missing TrimBox / BleedBox**, **RGB → CMYK conversion** through an ICC profile, and **removal of annotations and form fields**. Each runs on a copy — the original is never opened for writing — and the result is written to a path you choose.
  - **The repaired file is re-checked and the outcome reports what actually changed**, listing the finding codes that are genuinely gone from the written file. A repair tool that says "3 fixes applied" is asking to be believed; this one shows the evidence, and says so plainly when a repair changed nothing.
  - Colour conversion reuses the very engine the PDF/X-4 export uses, so a repaired file behaves identically to an exported one. Without a CMYK ICC profile the repair is not offered at all rather than falling back to an approximation, and an image in an encoding the converter cannot read (JPEG 2000, 16-bit) is reported as unconverted instead of being passed off as CMYK.
  - RGB repair is offered even when the validator reports nothing: the RGB rule inspects images only, so vector artwork — a logo, a tinted background, a chart — never raised a finding. The repair scans the file itself.
  - Findings that cannot be repaired correctly in place — missing embedded fonts, low-resolution images, live transparency — deliberately have no fix. Guessing at them would put a plausible-looking wrong file on the press.
- **Hover descriptions for every production mark in the preview.** Pointing at a mark on the sheet now explains what it is and why it is there — crop marks, contour-cutter (OPOS) markers, registration marks, the control strip, fold marks, the slug line, the sheet barcode/QR, and the trim and bleed guides. Page slots already had a tooltip; the marks around them, which are the least self-explanatory things on the sheet, had none. Available in Polish and English like the rest of the interface. Marks take priority over the page slot underneath, so a hairline sitting on top of artwork is still hoverable.

### Changed
- **Sheet navigation moved from the canvas into the inspector's "Preview" tab.** The floating strip at the bottom-left of the canvas and the thumbnail grid in the inspector were two navigators that looked alike and behaved differently — one moved around inside the plan, the other tore the plan down. They are now one list behind an **Sheets / Source pages** switch, which selects Sheets automatically after planning.
- Sheet tiles carry more than a number: the sheet's filled-slot count, a `2×` badge for double-sided sheets, and an accent frame on the sheet currently displayed. The old strip had no current-sheet indicator at all, so on a long run there was no way to tell where you were without reading the toolbar counter.

- **Hot folders: the same content could be exported twice.** The ledger's "has this already been processed?" check and the reservation insert were two independent statements, so with `maxConcurrentJobs` above 1 two consumers could both pass the check for a file whose content had already been exported. The pair is now a single immediate transaction, and a unique index enforces it at the database level as well. Verified against a real database: eight concurrent attempts on an already-completed fingerprint now yield zero reservations, while thirty distinct fingerprints still proceed in parallel.
- **Hot folders: a data race could silently freeze the polling watcher.** The directory snapshot was a plain dictionary written concurrently by the polling loop and the start-up rescan for the whole life of a folder start. Concurrent writes to that type are undefined behaviour — in practice an exception, or an endless spin that pins a thread at 100% CPU with nothing in the log, surfacing as "the hot folder stopped seeing files".
- **Hot folders: a watcher could come back to life after being stopped.** The recovery path that rebuilds a `FileSystemWatcher` after an OS error did not check whether the folder had been disposed meanwhile, leaving an orphaned watcher holding a directory handle open — which also blocks a network share from being unmounted — and firing events into a dead pipeline.
- **Hot folders: one bad folder no longer breaks every reload, permanently.** A runner that failed to start aborted the whole configuration reload before the new configuration was published, leaving the internal state describing one generation and the runner list another. Every subsequent reload then failed outright until the application was restarted. Failures are now isolated per folder, as they always were on the initial start.
- **A locked `hotfolders.json` no longer stops the application from starting.** Only malformed JSON was handled; a file momentarily held open by a backup agent, antivirus or cloud sync propagated all the way out and exited the process with code 2 — before any window appeared, so the operator saw nothing happen at all.
- **PDF/X output intent could claim CMYK for an RGB or grayscale profile.** The ICC header was read with a call that is allowed to return fewer bytes than requested; the short read fell through to the CMYK default, producing a non-conformant file that passed our export and failed in the customer's RIP.
- **Grayscale conversion silently skipped images with a malformed `/Indexed` palette**, leaving colour artwork in a file that was supposed to be monochrome. The guard that the RGB converter already had is now applied here too.
- **A failed ICC profile load leaked a colour-system handle.** The constructor opened the sRGB profile first, so if the output profile was rejected the exception left the first handle open forever — no object existed to dispose it and there is no finalizer. A hot folder pointed at a bad profile leaked one handle per job, in a loop.

### Security
- **The sheet- and page-range fields could exhaust memory from a typo.** A range was expanded element by element with no limit, before any check against the document's page count could run: `1-20000000` — ten characters — allocated about 726 MB, and a slightly larger number ended the process. Ranges are now capped at 100 000 numbers with a clear message. Measured before and after.
- **Closed a path-traversal hole in marks profiles.** A template's `marksProfileRef` reached `Path.Combine` unchecked, so an imported template could point the read outside the profiles directory. Only saving validated the identifier; reading and deleting now do too, with the path builder itself as a last line of defence.
- **Updated dependencies with published advisories** — `System.Text.Json` to 8.0.5 (two high-severity) and `SQLitePCLRaw` to 3.0.4 / 3.50.3 (one high). Dependency auditing is now on for every build, in transitive mode, and the two advisories that have no available fix are listed explicitly so that anything **new** still fails loudly instead of being lost in a wall of known warnings.
- **Ghostscript is invoked through an argument list instead of a hand-quoted command line**, removing a class of bug around paths with spaces, UNC paths and trailing separators.
- **An environment variable can no longer downgrade the licence or telemetry endpoint to plain HTTP.** Those requests carry the machine fingerprint; only HTTPS, or loopback for development, is accepted.
- Both network services now share one HTTP client per process instead of creating one per request, which left sockets in `TIME_WAIT` and forced a fresh TLS handshake every time.

### Performance
- **Hot folder throughput: two WMI queries removed from in front of every job.** The per-job licence check re-read the disk serial and motherboard UUID each time — `Win32_DiskDrive` alone routinely costs hundreds of milliseconds, seconds on a machine with parked disks. A 500-file batch paid over a minute of pure overhead, and application start-up paid it again before the window appeared. Hardware cannot change while the process runs, so the fingerprint is cached; the licence signature and expiry are still verified for every job.
- **The same PDF is no longer parsed two or three times per export.** Analysis — the most expensive operation in the application — ran when the file was added, again for preflight and again inside the export pipeline. Results are now reused for as long as the file on disk is unchanged.
- **Colour conversion caches per-colour results.** Every `rg` operator in a content stream previously cost a separate call into the operating system's colour engine plus four array allocations; a real document uses a few dozen distinct colours.
- **Image conversion reads pixels by row rather than one at a time**, and decodes without the alpha channel it immediately discarded — 12 million redundant bounds checks and a 25% larger buffer on a single 4000×3000 photograph.
- **Diagnostics no longer read the settings file on every event.** The opt-out flag was loaded and deserialised from disk on each check, including from the UI thread.

### Removed
- The floating sheet strip and the `ShowSheetStrip` flag that gated it.
- Seven unused public members, among them the only renderer entry point that rasterised synchronously — it bypassed the concurrency gate, so any future use would have frozen the interface on a large page.
- Four directories that were created at every start and never written to, one of which implied support for custom language packs that does not exist.

## [1.5.6] - 2026-07-25

DeviceRGB image conversion in the imPRESS engine — PDF/X-4 from RGB files without Ghostscript.

### Added
- **RGB images no longer force the Ghostscript path.** The imPRESS engine now converts DeviceRGB images to DeviceCMYK itself, through the very ICC profile that becomes the OutputIntent (Windows colour engine, relative colorimetric). Handled: **JPEG (DCTDecode)**, **8-bit Flate/raw rasters** and **Indexed palettes** with an RGB base — converted pixel-by-pixel to 8-bit DeviceCMYK with Flate compression, in bounded chunks so peak memory stays flat.
- **Same fail-safe philosophy:** exotic flavours (JPEG 2000, 16-bit) are never guessed — if any RGB image would be left unconverted the engine aborts with a clear message instead of shipping a file with a false conformance claim. Exotic `/DeviceRGB` usage (cs/CS operators, shadings) still routes to Ghostscript.
- Verified end-to-end: a 4-page file of RGB photos → native PDF/X-4 with every image in DeviceCMYK, colours and printer marks intact (control render), PDF 1.6 header + GTS_PDFX + OutputIntent.

### Fixed
- **The app no longer stays closed after an in-app update.** The updater runs the installer silently, but the installer's "launch the app" step was flagged to be skipped in silent mode — so an update replaced the files and left the user with nothing running. The installer now relaunches the app after a silent install (at the original user's privilege level), while the interactive wizard keeps its usual final-page checkbox. Verified by installing an isolated probe package with `/SILENT` and confirming the payload was executed.
- **PDF/X-4 export failed with "select an ICC profile" — after the sheets had already been composed.** The job was imposed and written, then packaging aborted if no profile was selected in the options. The engine now falls back to a bundled production profile (PSO Coated v3 → ISO Coated v2 → any CMYK profile in the profile directories), logs the choice and completes the export. A hard error remains only when the machine has no CMYK profile at all.

## [1.5.5] - 2026-07-19

Anonymous, opt-out diagnostics + small UI fixes.

### Added
- **Lightweight anonymous diagnostics** to help improve the app. Sent only: a *random install id* (a locally-generated GUID — not the hardware fingerprint and not linkable to it), app and OS version, .NET version, license type, locale, startup time and coarse module-usage counters (export, template manager, hot folders, page edit, Save PDF, preflight — counts only).
- **Nothing can leak by construction:** the app only ever sends a fixed, predefined set of fields — never file names, paths, project names or content, typed data, per-click history or crash data (crash reporting is deliberately omitted).
- **Full control:** on by default, with a one-time first-run notice and a toggle under *Settings → Diagnostics*. Sending is backgrounded with a short timeout and never blocks the app; when offline it is simply skipped. Privacy policy updated (PL/EN). New trial-server endpoint `POST /api/telemetry` with dedicated tables and a storage smoke test.

### Fixed
- **Empty startup preview** no longer shows a misleading "SRA3 · 450 × 320 mm" (that was only a fallback shown when no template is loaded) — replaced with a neutral "Load a file and pick a template" hint; the real sheet size appears once a template is active.

## [1.5.4] - 2026-07-19

Zero-click 30-day trial on first launch.

### Added
- **The 30-day trial starts automatically on first launch — no form, no clicking.** With no valid license the app sends *only its hardware fingerprint* (MAC/disk/UUID hashes — no personal data, no e-mail, no consent form) to the license server and receives a signed 30-day trial that is saved automatically.
- **One trial per machine — a Windows reinstall doesn't help.** The "one trial per machine" record is server-authoritative and keyed on the hardware fingerprint, so reinstalling Windows or formatting the disk cannot reset the 30 days. A component-level dedup also catches a single-part swap (e.g. a new NIC).
- **Only the first launch needs internet;** the saved license then works offline (with the existing clock-rollback guard). No connection on first run → a clear message and manual activation; trial already used → an info note and a path to activate a paid license. The web form and manual activation remain available as a fallback.
- New trial-server endpoint `POST /api/auto-trial` (anonymous, fingerprint-only) plus a component-level dedup query; covered by a standalone dedup smoke test.

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
