# Changelog

## English

Full release history for **imPRESS Studio**. English section is a clean, public-facing Markdown version based on the supplied HTML changelog. The uploaded HTML ends abruptly at the final `1.1.1` Hot Folder UX section, so that part is marked as incomplete.

### [1.3.6] - June 7, 2026 — Latest, Hotfix
*License activation fix*

#### Fixed
- Fixed a false **“license control file has been modified”** block when switching licenses on the same computer, for example from trial to paid or after re-issuing a license. `LicenseClockGuard` now treats a mismatched control file as belonging to a different license and regenerates it, while the clock-rollback protection remains active.

#### Added
- Added a restart reminder after loading a license key or `.lic` file.
- Added admin-panel actions on the license server to download the `.lic` file and copy the license key to the clipboard.

**Note:** Hotfix for 1.3.5. No changes to the imposition engine. Build: 0 warnings / 0 errors.

### [1.3.5] - June 6, 2026 — Stable
*Machine identifier export to `.lic`, PDF merge*

#### Added
- Added **Export JSON…** in the license activation window. It saves the hardware identifier as a `.lic` file instead of forcing the user to copy a long fingerprint manually.
- The trial-license generator on impressstudio.pl can now accept the exported `.lic` file and fill the fingerprint automatically. Manual paste still works.
- Added a standalone **Merge…** button for selected source PDFs. It merges files in list order and saves the result independently from the imposition project.
- Suggested merged filenames now follow the `merged_<date>_<file-count>_<page-count>.pdf` pattern, for example `merged_2026-06-06_3_48.pdf`.

**Note:** Build: 0 warnings / 0 errors. New PL + EN localization keys. The trial-server admin panel also gained paid-license generation: trial / 6 months / yearly / lifetime / custom.

### [1.3.4] - May 24, 2026 — Stable
*Step & Repeat, mark profiles, localized names*

#### Added
- Added a dedicated `StepAndRepeat` binding for repeating a single PDF page in an NxM grid on a sheet.
- Added per-copy rotation patterns: none, alternating rows, alternating columns and checkerboard.
- Added two spacing modes: gap-between and center-to-center pitch. Center-to-center is useful for label and die-cut workflows.
- Added validation for overlapping copies when pitch is smaller than the item size.
- Added a Step & Repeat section in the template editor and a quick-start preset for 50×50 mm stickers on A4.
- Added Marks v2: named mark profiles stored as JSON under `%APPDATA%\imPRESS Studio\marks-profiles\`.
- Added configurable color bars, registration mark styles, slugs, process/registration black selection and full backward compatibility for legacy templates.
- Added an in-app mark profile editor with save, save as, reset and delete workflows.

#### Changed
- Template preset names/descriptions and built-in mark profile names are now localized live when switching UI language.
- Added 30 localization keys for template presets and mark profile labels, plus 38 keys for the mark profile editor.

**Note:** Build: 0 warnings / 0 errors. Tests: 146/146 green.

### [1.3.2] - May 23, 2026 — Stable
*Full inch support, ICC profile info in File Info, US/ANSI presets*

#### Changed
- Added full mm/inch support across the UI while keeping the internal model in millimeters.
- Extended `DisplayUnitsService` with reverse conversion, inch detection, default decimals and shared formatting helpers.
- Connected all relevant template-editor numeric fields to `MmConverter` and dynamic unit labels.
- Updated preflight reports, File Info, template suggestions and inspector fields to display values in the active unit.

#### Added
- File Info now shows real embedded ICC profile data in the Output Intent section, including profile name and color space where available.
- Added a minimal `IccProfileReader` for ICC v2/v4 profile metadata.
- Added bundled ICC profile plumbing via `Imposition.App/icc/`, with README guidance for ECI profiles and licensing warnings for Adobe profiles.
- Added ANSI A–E paper presets and six new US-format template suggestions.

**Note:** Build: 0 warnings / 0 errors. Smoke test passed.

### [1.3.1] - May 21, 2026 — Stable
*Top bar polish — clearer Plan and Export buttons*

#### Changed
- The **Plan** button now has its own visual style, making it clearer without competing with the main **Export** CTA.
- The **Export** button text is forced to white for better contrast.

**Note:** Cosmetic release only. No imposition/export engine changes. Build: 0 warnings / 0 errors. Tests: 123/123 green.

### [1.3.0] - May 18, 2026 — Stable
*UI redesign, onboarding, floating toolbar, drag-and-drop fix, export progress, OPOS in canvas*

#### Fixed
- Fixed Windows Explorer drag-and-drop by restoring an STA GUI entry path; the previous async `Main` could continue on an MTA thread and silently break OLE drag registration.
- Added extra `AllowDrop` registration and a warning when the app is elevated, because UAC can block Explorer-to-admin drag-and-drop.
- Fixed Summa OPOS marker visibility in the canvas and added halo/core rendering for crop, registration and OPOS marks.
- Fixed several template-suggestion bugs, including invisible cards, invalid N-up values after applying suggestions and wrong binding inheritance.
- Fixed mojibake `Ă—` → `×`, an undersized Settings dialog and obsolete empty-state drawing.

#### Added
- Added a new empty-state onboarding screen with SRA3 mockup, drop zone, checklist and shortcut cheat sheet.
- Added phased export progress: PDF analysis, preflight validation, imposition planning, PDF composition and Ghostscript PDF/X conversion.
- Added Ghostscript auto-detection in standard install paths and `PATH`.

#### Changed
- Reworked the floating canvas toolbar into clear groups: zoom, view toggles, front/back switch and CMYK strip.
- Improved RollFed centering, default margins and duplex suggestions for two-page PDFs.
- Polished the top bar, license badge, step rail, file-list hover actions and status bar clipping.

**Note:** Build: 0 warnings / 0 errors. Tests: 123/123 green.

### [1.2.4] - May 17, 2026 — Stable
*Context-aware suggestions, RollFed duplex, mm/in units, preview ruler*

#### Added
- Added context-aware template suggestions that adapt presets to the current sheet, binding, margins and marks.
- Added RollFed duplex support, including front/back toggle and spread handling for extreme aspect ratios.
- Added display-unit settings for metric/inch mode.
- Added top and left preview rulers with automatic tick spacing and origin marker.

#### Fixed
- Reset All now restores the default template, so loading the same file again produces a preview correctly.

**Note:** Build: 0 warnings / 0 errors. Tests: 123/123 green.

### [1.2.3] - May 17, 2026 — Stable
*Smart preflight, template suggestions, layout optimizer, preview and imposition fixes*

#### Added
- Added content-based bleed detection and a narrative preflight summary panel.
- Added template suggestions from PDF dimensions and auto-registration of the Roll 320 paper preset.
- Added a layout optimizer for automatic N-up calculation.
- Added trim/bleed guide overlay in preview and a manual update check.

#### Fixed
- Fixed duplex pairing for ganging.
- Fixed multiple preview UX issues: 1-based sheet counter, front/back toggle in spread view, white sheet after disabling spread, and related interaction polish.

**Note:** Build: 0 warnings / 0 errors. Tests: 109/109 green.

### [1.2.2] - May 16, 2026 — Stable
*Preview pipeline v3 — no more RAM explosion on A0/A1 rolls*

#### Fixed
- Reduced roll-preview memory use from multi‑GB levels to roughly tens of MB by replacing count-based cache limits with a memory-budget LRU.
- Added bitmap scale clamping and display-pixel-aware rendering, which fixes the root cause of oversized preview bitmaps.
- RollFed now forces single-sheet preview because spread mode does not make sense for roll media.

#### Added
- Added vector overview mode for very low zoom levels, Fiery Impose-style.

#### Changed
- Added roll-aware sheet layout so very long rolls are no longer rendered as tiny 22 px columns.
- Refactored preview probing, cache insertion and bitmap retrieval paths for safer concurrent behavior.

### [1.2.1] - May 16, 2026 — Stable
*Template editor UX after first 1.2 tests*

#### Fixed
- The template editor now preserves state after closing and reopening.
- Validation now blocks closing with an invalid template and shows a red error banner instead of vague failures.
- Fixed architecture around `RollOptions.Enabled`, `OverlayOptions.Enabled`, non-throwing validation and derived roll-split properties.

#### Added
- Added roll-length split/continuous behavior similar to Caldera/Onyx.
- Added per-section enable toggles and a new preset.

#### Changed
- Auto-filled sheet dimensions after selecting a preset and simplified the sheet-size section.
- Updated PL + EN website/help/FAQ/privacy pages with roll-fed and Summa OPOS information.

### [1.2.0] - May 15, 2026 — Stable
*Roll-fed / print & cut — sticker production on rolls with Summa OPOS markers*

#### Added
- Added new `BindingType.RollFed` for step-and-repeat production on roll media.
- Added variable repeat, multi-segment splitting and roll-specific layout behavior.
- Added Summa OPOS / OPOS-XY cutter marks via `CutterMarkType` and `CutterMarkLayout`.
- Added per-sheet logo/overlay support with raster/vector handling.
- Added RollFed, cutter mark and overlay panels to the template editor.

#### Changed
- Extended `Template` with roll, overlay and cutter-mark properties.
- `Template.Validate()` now handles RollFed separately because sheet length is dynamic.
- `PdfComposer` now receives cutter-mark and logo-overlay renderers through DI.

**Note:** Tests: 65/65 green.

### [1.1.1] - May 10, 2026 — Stable
*Run as User — no more writing to Program Files, hot folder UX hardening*

#### Fixed
- Moved dynamic data to `%APPDATA%` / `%LOCALAPPDATA%`, removing `UnauthorizedAccessException` problems for non-admin users and preventing data loss during updates.
- Added central `AppPaths` as the single source of truth for runtime, roaming, local and bundled paths.
- Added one-time idempotent migration from the old 1.0.x locations.
- Unified folder naming from `ImpositionApp` / `imPRESS Studio` inconsistencies to `imPRESS Studio`.
- User-imported ICC profiles now go to `%APPDATA%\imPRESS Studio\icc\`; bundled profiles stay read-only under `{install}/icc/`.

#### Added
- The source HTML ends in the middle of the Hot Folder UX section, so this part is incomplete in the uploaded file.

**Note:** Source file is truncated at the end of this release.

---

## Polski

Pełna historia wydań **imPRESS Studio** z opisem nowych funkcji, ulepszeń i poprawek.

> Uwaga: załączony plik HTML kończy się nagle przy wersji `1.1.1`, w sekcji „Hot folder UX po realnych testach produkcyjnych”. Poniżej znajduje się konwersja tego, co było dostępne w przesłanym pliku.


## [1.3.6] - 7 czerwca 2026 — Najnowsza, Hotfix

*Poprawka aktywacji licencji*


### Naprawione

- **Błędna blokada „Plik kontrolny licencji został zmodyfikowany"** przy zmianie licencji na tym samym komputerze (np. trial → płatna, ponowne wydanie). Strażnik cofania zegara (`LicenseClockGuard`) zapisuje plik kontrolny powiązany z podpisem konkretnej licencji; przy wgrywaniu *innej* licencji stary plik nie pasował i był traktowany jak sabotaż. Teraz niedopasowany plik kontrolny jest rozpoznawany jako „należący do innej licencji" i zapisywany od nowa — sama blokada cofania zegara działa bez zmian. Plik kontrolny jest dodatkowo czyszczony przy każdej aktywacji.


### Dodane

- **Przypomnienie o restarcie** — po wczytaniu klucza lub pliku licencji aplikacja informuje, że należy ją zamknąć i uruchomić ponownie, aby licencja zaczęła obowiązywać.
- **Panel admina (serwer licencji)** — w akcjach przy każdej licencji doszły przyciski *Pobierz plik .lic* oraz *Kopiuj klucz do schowka*, obok wysyłki ponownej i blokady.


**Note:** Hotfix do 1.3.5 — bez zmian w silniku impozycji. Build: 0 warnings / 0 errors.

## [1.3.5] - 6 czerwca 2026 — Stabilna

*Eksport identyfikatora do .lic, łączenie plików PDF*


### Eksport identyfikatora maszyny do pliku .lic

- **Nowy przycisk *Eksportuj JSON…*** w oknie *Aktywacja licencji*, pod przyciskiem *Kopiuj*. Zapisuje identyfikator sprzętowy do pliku `.lic` (natywne okno „Zapisz jako", rozszerzenie `.lic` a nie `.json`) — wygodniejsze niż ręczne kopiowanie długiego ciągu, gdy zamawiasz licencję. Plik zawiera składniki `MAC`/`DISK`/`UUID` oraz gotowy ciąg `fingerprint` (to samo, co kopiuje przycisk Kopiuj) plus nazwę maszyny i znacznik czasu eksportu
- **Strona z generatorem licencji próbnej** (impressstudio.pl) przyjmuje teraz ten plik — zamiast wklejać identyfikator, wgrywasz `.lic` i pole wypełnia się samo. Obsługa po obu stronach: wklejenie ciągu nadal działa


### Łączenie plików PDF (Połącz…)

- **Przycisk *Połącz…*** w panelu plików źródłowych, obok *+ Dodaj*. Aktywny gdy zaznaczysz co najmniej dwa pliki. Scala je w jeden PDF i zapisuje na dysk przez natywne okno „Zapisz jako" — operacja samodzielna, niezależna od impozycji (nie ładuje wyniku do projektu)
- **Sugerowana nazwa** według wzorca `merged_<data>_<liczba_plików>_<liczba_stron>.pdf` (np. `merged_2026-06-06_3_48.pdf`) — liczba stron liczona od razu z analizy, bez wcześniejszego scalania. Pliki łączone w kolejności z listy. Wykorzystuje istniejący silnik scalania, ten sam co dotychczasowe automatyczne łączenie zaznaczonych plików


**Note:** Build: 0 warnings / 0 errors. Nowe klucze lokalizacyjne PL + EN dla obu funkcji. Część serwerowa (panel admina trial-servera) zyskała generator licencji płatnych — trial / 6 mies. / rok / wieczysta / custom — niezależnie od aplikacji desktopowej.

## [1.3.4] - 24 maja 2026 — Stabilna

*Step & Repeat, profile znaczników, lokalizowane nazwy*


### Step & Repeat — dedykowany binding

- **Nowy typ oprawy `StepAndRepeat`** obok istniejących Ganging / RollFed / SaddleStitch / PerfectBound / Wiro / fold-leaflets. Powiela **jedną** stronę źródłowego PDF w siatce NxM na arkuszu — kompletuje zestaw bindingów dla operatorów etykiet, wizytówek, naklejek gdzie produkujesz jeden item w wielu kopiach z konkretnym pitchem maszyny. Odróżnia się od Ganging w dwóch wymiarach: Ganging cykluje przez kolejne strony źródła, S&R zawsze powiela tę samą (page 1 na froncie, page 2 na backu przy duplexie); Ganging zawsze 0°, S&R wspiera wzory rotacji per kopia
- **Cztery wzory rotacji per kopia** (`StepAndRepeatRotation`): *None* (wszystkie 0°), *AlternatingRows* (parzyste rzędy 0°, nieparzyste 180° — nesting dla itemów z osią symetrii), *AlternatingColumns* (analogicznie po kolumnach), *Checkerboard* (szachownica — najlepszy nesting dla asymetrycznych kształtów, np. butelek czy listków). Rotacja per-copy honorowana zarówno w eksporcie PDF, jak i w podglądzie canvasu (PagePlacement.RotationDegrees już istnieje od v1.0)
- **Dwa tryby odstępu** (`StepAndRepeatSpacing`): *GapBetween* (jak Ganging — dzielenie użytecznej powierzchni przez N, gutter + margines), *CenterToCenter* (centra itemów w dokładnie pitch-mm odstępie, siatka centrowana na arkuszu, gutter ignorowany). Pitch C-to-C to standardowa konwencja w przemyśle etykietowym — operator wprowadza wartość z karty maszyny (np. die-cutting roll z pitch 52mm), nie przelicza w głowie gap-from-edge
- **Walidacja** łapie przypadki nakładania kopii — gdy pitch jest mniejszy niż wymiar itemu w danej osi, `Template.Validate` rzuca konkretną wiadomość („Pitch poziomy 45mm jest mniejszy niż szerokość itemu 50mm — kopie zachodzą na siebie") zamiast cichego błędnego wyniku eksportu
- **Nowa sekcja w edytorze szablonów** (osobna karta między Roll a Cutter marks) z dropdownami dla wzoru rotacji i trybu odstępu, plus pola pitch X/Y aktywne tylko przy *CenterToCenter*. Wszystkie wartości honorują aktywną jednostkę mm/cale przez ten sam `MmConverter` i `UnitLabelExtension` co reszta edytora
- **Preset „Naklejki 50×50 — Step & Repeat"** w bibliotece quick-start: A4 portrait, 4×5 siatka itemów 50×50mm z pitch 52mm i tryb *CenterToCenter*. Math zweryfikowana: 3×52+50 = 206 ≤ 210 (W) oraz 4×52+50 = 258 ≤ 297 (H). Pokazuje krok-po-kroku jak skonfigurować typowe zadanie etykietowe
- **Testy (`StepAndRepeatStrategyTests`, 7 nowych)** pokrywają: grid count (single-page → NxM placements), duplex (page 1 front + page 2 back), graceful degradation (duplex z 1-stronicowego źródła → empty back), wzory rotacji per-cell, dokładność matematyki center-to-center (start X/Y + odstępy między sąsiadami)


### Lokalizowane nazwy presetów i profili znaczników

- **12 wbudowanych presetów szablonów** (A4/A5 broszura, perfect bound, ganging 2/4-up, DL Z-fold/harmonijka/3-up, wizytówki 6/8/12-up, naklejki rolka) — labelki i opisy w lewym panelu Menedżera szablonów teraz przeskakują na EN gdy przełączysz język. `TemplatePreset` przepisana z record na klasę z `LabelKey`/`DescriptionKey` + computed `Label`/`Description` (przez `LocalizationService`) + INPC. `TemplateEditorViewModel` subskrybuje `LocalizationService.PropertyChanged` i notyfikuje każdy preset przy zmianie języka, więc lista refreshuje się żywcem bez restartu okna. Template.Name pieczętowany w momencie aplikowania presetu jest w aktywnym języku — już zapisane szablony zachowują swoją nazwę (to user data)
- **5 wbudowanych profili znaczników** (Domyślny / Offset / Druk cyfrowy / Wielkoformat / Bez znaczników) — display layer ignoruje persistent Name pole z seeded JSON i pokazuje localized name przez `MarksProfileDisplay.LocalizedName()` (mapuje stabilne ID na klucze loc). Dotyczy zarówno dropdownu w edytorze szablonu jak i lewej listy w edytorze profili znaczników (przez `MarksProfileNameConverter` w XAML). User-defined profile używają persistent Name jak dotąd
- **30 nowych kluczy lokalizacyjnych** PL + EN (24 dla `TM.Preset.*` × 12 presetów × label+desc, 6 dla `MPL.*` × 5 built-inów + Legacy)


### Marks v2 — profile zamiast pojedynczych checkboxów

- **Profil znaczników to nazwany pakiet ustawień** (crop / pasery / pasek kontrolny / slug / fold / contour-cut) zapisany jako jeden plik JSON w `%APPDATA%\imPRESS Studio\marks-profiles\`. Szablon trzyma tylko referencję (`marksProfileRef`); zmiana profilu propaguje się od razu do każdego szablonu, który z niego korzysta. Pięć wbudowanych profili seedowanych przy pierwszym uruchomieniu: *Domyślny* (zachowanie v1.3.x 1:1), *Offset (powlekany)* z paskiem GATF-11 i slugiem u góry, *Druk cyfrowy* (sam crop + slug, bez paserów ani paska — cyfra sama się kalibruje), *Wielkoformat* (corner registration plus, pasek CMYK przy dolnej krawędzi) i *Bez znaczników*
- **Pasek kontrolny w czterech wariantach**: CMYK process (4 pola), CMYK + tinty (8 pól, legacy default), GATF-11 (paper + CMYK solid + CM/CY/MY overprints + K 25/50/75%) i Brunner-21 (paper + CMYK po cztery stopnie tinta + 3 overprinty + chromatic CMY). Każdy wariant rysowany przez `XColor.FromCmyk` bezpośrednio w PDF — separacje C/M/Y/K są prawdziwe. Wybór krawędzi (góra/dół/lewa/prawa) dla każdego paska. Komentarz w kodzie jasno mówi że GATF-11 i Brunner-21 to rozsądne aproksymacje, nie certyfikowane bary referencyjne (oryginały są pod paywallem System Brunner AG / PIA)
- **Pasery w czterech stylach × trzech pozycjach**: Target (kółko + krzyż, klasyczny offset), Plus (sam krzyż, lepszy na inkjet bo nie rozmazuje się przez ink spread), Diamond (romb + krzyż) i Crosshair (Heidelberg-style — krzyż w małej ramce). Pozycja: środki krawędzi, narożniki, lub oba. Rozmiar i odstęp od krawędzi sterowane z profilu
- **Slug — pasek opisowy na krawędzi arkusza**. Format z placeholderami: `{filename}`, `{date}`, `{date:yyyy-MM-dd}` (lub dowolny .NET format), `{sheet}`, `{sheets}`, `{side}`, `{template}`, `{jobid}`. Domyślnie: `"{filename} · {date} · Arkusz {sheet}/{sheets} · {template}"`. Wybór krawędzi z auto-rotacją tekstu na lewej/prawej. Nieznane tokeny pozostają w formacie (lepiej żeby operator zobaczył literówkę niż żeby ją po cichu wyciąć)
- **Kolor farby per znacznik** — wybór *process black* (K=100) lub *registration black* (C=M=Y=K=100). Registration black drukuje się na każdej separacji w offsecie, więc pasery i crop dla offsetu domyślnie używają registration black, slug w druku cyfrowym — process
- **Pełna kompatybilność wstecz** — szablony v1.3.x bez `marksProfileRef` renderują się dokładnie tak jak wcześniej. `MarksProfileResolver.AdaptLegacy()` buduje syntetyczny profil ze starych boolean flag (`Marks.Crop`, `Marks.Registration`, ...) — żaden plik szablonu nie wymaga migracji. Brakujący profil w referencji (np. user usunął plik) też nie blokuje renderingu, tylko cicho cofa się do legacy lub *Default*


### Edytor profili w aplikacji

- **Nowe okno *Edytor profili znaczników*** otwierane przyciskiem *Zarządzaj…* obok dropdownu profilu w edytorze szablonu. Lista profili po lewej (z odróżnieniem wbudowanych), formularz z pięcioma akordeonami po prawej: Cięcia · Pasery · Pasek kontrolny · Slug · Inne (fold + contour-cut). Wszystkie pola w jednostce mm/cale zgodnie z aktywnym ustawieniem, wszystkie enumy w dropdownach
- **Operacje: Zapisz / Zapisz jako… / Przywróć domyślne / Usuń**. Profile wbudowane można dowolnie edytować i przywracać do bundled defaults — ale nie usuwać. Zapisz jako… prompty o nazwę i tworzy nowy plik z kebab-cased id. Profile użytkownika — pełna swoboda. Po zamknięciu edytora dropdown w edytorze szablonu odświeża się automatycznie
- **Plumbing**: `%APPDATA%\imPRESS Studio\marks-profiles\` zarządzany przez `AppPaths.GetMarksProfilesDir()` + `EnumerateAllDirectories()` (tworzy się przy starcie). `MarksProfileFileStore` tolerancyjny na uszkodzone pliki — jeden malformed nie blokuje listy. Przycisk *Otwórz folder profili* dla użytkowników którzy wolą edytować JSON ręcznie
- **Renderer i podgląd zsynchronizowane** — `PrinterMarksRenderer` (export) i `ImpositionCanvas.DrawMarksOverlay` (preview) konsumują ten sam `MarksProfile` przez `MarksProfileResolver`. Operator widzi w canvasie dokładnie to co wyląduje w PDF (z aproksymacją sRGB → CMYK bo Avalonia nie ma CMYK pipeline). Pole `PreviewViewModel.ActiveMarksProfile` re-rezolwuje się przy każdej zmianie szablonu
- **Loc: 38 nowych kluczy PL + EN** (`MPE.*` + `TM.MarksProfileManage` / `TM.MarksProfileHint` / `TM.MarksProfileOpenFolder` / `TM.Section.MarksProfile`). Komunikaty o błędach zapisu (zła nazwa id, duplikat, błąd I/O) wyświetlane w status banerze edytora


**Note:** Build: 0 warnings / 0 errors. Testy: **146/146 zielone** (+18 od v1.3.2 — `MarksProfileResolverTests`, `SlugFormatterTests`, `StepAndRepeatStrategyTests`). Smoke test: aplikacja startuje czysto, 5 wbudowanych profili znaczników seeduje się do `%APPDATA%`, legacy szablony renderują się identycznie jak przed migracją, przełączenie języka żywcem aktualizuje labelki presetów i nazwy profili.

## [1.3.2] - 23 maja 2026 — Stabilna

*Pełna obsługa cali, profile ICC w File Info i presety US/ANSI*


### Pełna obsługa jednostek mm / cale w całym UI

- **Architektura: model zostaje w mm — konwersja tylko na granicy UI.** Wszystkie `*Mm` properties w modelu (`SheetSize`, `TrimSize`, `LayoutMargins`, `RollOptions`, `MarksOptions`, `OverlayOptions`) zostają w milimetrach — to kanoniczna jednostka przechowywania, szablony JSON nieruszone. Konwersja dzieje się wyłącznie w warstwie XAML przez nowy `MmConverter` (mm ↔ aktywna jednostka) i markup `{loc:UnitLabel}` (label z aktywnym suffiksem). Round-trip bez dryfu: czynnik 25.4 jest dokładny dla każdego `double`, a model nigdy nie jest nadpisywany zaokrąglonym display-value
- **`DisplayUnitsService` rozszerzony** — dodane `ToMm()` (odwrotność `FromMm`), `IsInches`, `DefaultDecimals`, `FormatLengthMm()` (jeden helper dla walidatorów); klasa implementuje teraz `INotifyPropertyChanged`, więc bindowania do `Suffix` w XAML auto-refreshują się przy toggle bez restartu
- **Edytor szablonów (Template Manager)** — wszystkie ~20 pól wymiarowych (sheet W/H, trim, bleed, safe zone, gutter, spine, sheet margin, rolka MaxRollLength/GapX/GapY, custom size, długości i offsety znaczników, cutter mark size/offset, overlay max width/offset X/Y, paper thickness) wpięte do `MmConverter`. Wpisujesz `3.5` w calach — w modelu siedzi `88.9 mm`; przełączasz na mm — pole pokazuje `88.9`. Każdy NumericUpDown działa w jednostce aktywnej
- **Labelki dynamiczne** — ~20 wpisów loc (PL + EN) zmienione z literalnego `"Szerokość (mm)"` na `"Szerokość ({0})"`; `UnitLabelExtension` + `UnitLabelConverter` robią multi-binding (loc + suffiks) tak że label refresuje się i na zmianę języka, i na zmianę jednostki. Settings dropdown z opcjami jednostek (`Metryczne (mm)` / `Cale (in)`) celowo zostawiony z literalami — to label opcji, nie wartości
- **Inspektor zaznaczenia (X/Y/W/H placementu)** w prawym panelu główno-okiennym — 4 NumericUpDown'y konwertują, X i Y dostały nowe loc klucze `Insp.X`/`Insp.Y`
- **Wszystkie display-bindings sheet-size** w toolbarze, statusbar, empty-state mockup, stat-block Bleed/Gutter — twardo zakodowane `Run Text=" mm"` wymienione na `Run` bindowane do `DisplayUnitsService.Instance.Suffix`. Linijka canvasu już respektowała preferencję (jedyne miejsce z wcześniejszej iteracji)
- **Raport walidacji preflight w File Info** — `BleedValidator` (`BLEED_LOW`, `BLEED_CONTENT_UNUSED`, `BLEED_CONTENT_OVERFLOWS_BLEEDBOX`) i `PageSizeValidator` (`TRIM_MISMATCH`) emitują wartości przez `FormatLengthMm()` + suffiks jako kolejny placeholder. Templates loc `HF.PV.Issue.*` przerobione: `"{1} mm"` → `"{1} {3}"` z suffiksem jako dodatkowy arg. Sekcje "PAGE BOXES" i "IMAGES" w dialogu File Info — formaty `FI.Boxes.Row` i `FI.Img.Row` dostały dodatkowy slot `{7}` na jednostkę; `PrepressDisplay.Mm()` teraz konwertuje + precyzja zależna od jednostki (mm: F1, cale: F3 — 0.001 in ≈ 0.025 mm, finer niż realne tolerancje prepress)
- **Dialog sugestii szablonu** — wymiary wejściowego PDF, komunikat "brak dopasowań" i tekst "Różnica X mm" wszystkie w aktywnej jednostce. Nazwy ~30 presetów (PL + EN) przerobione z `"Wizytówka standard (85×55 mm)"` na placeholder `"Wizytówka standard ({0}×{1} {2})"`; `TemplateSuggestion` dostała `PresetWidthMm`/`PresetHeightMm` żeby dialog mógł wstawić wymiary w jednostce użytkownika
- **VM-y re-emitują display values na `DisplayUnitsService.Changed`** — `TemplateEditorViewModel`, `MainWindowViewModel` i `PreviewViewModel` subskrybują event i wołają `OnPropertyChanged` dla ROOT bound properties (`Template`, `CurrentTemplate`, `Preview`, `SelectedXmm/Ymm/Wmm/Hmm`). Reguła jest taka że re-notify musi iść do roota a NIE do leaf `*Mm`, bo leafy siedzą na POCO bez `INotifyPropertyChanged` — Avalonia re-walkuje ścieżki bindingu tylko gdy root fire'uje. To jeden z subtelniejszych haczyków przy podpinaniu konwertera do nested-path bindings na POCO


### Informacje o profilu ICC w File Info

- **Sekcja OUTPUT INTENT pokazuje teraz realne dane osadzonego profilu ICC**, nie tylko "tak/nie". Wcześniejsze 4 pola (Subtype, Condition, Identifier, Embedded) uzupełnione o RegistryName oraz — co najważniejsze — **nazwę profilu** (z tagu `desc` dla ICC v2, `mluc` dla v4) i **przestrzeń barw** (CMYK / RGB / Gray / Lab / N-channel) odczytane z nagłówka profilu
- **Nowy parser `IccProfileReader`** (Imposition.Pdf) — minimalny, tylko to co dialog faktycznie pokazuje: data colour space signature spod offsetu 16 i tag `desc`/`mluc`. Best-effort: uszkodzony lub okrojony profil zwraca nulle, nie rzuca wyjątku. Zweryfikowany na realnym `sRGB Color Space Profile.icm` z Windows — offsety i mapowania zgadzają się 1:1 ze specem ICC
- **`PdfStandardDetector` rozwiązuje pośrednie odwołanie `/DestOutputProfile`** przez `Structure.GetObject(IndirectReference)`, dekoduje filtry strumienia (ICC są standardowo `FlateDecode`-compressed) przez `DefaultFilterProvider.Instance` i wrzuca surowy bufor do parsera. Wcześniejsze pola `EmbeddedProfileName` i `EmbeddedProfileComponents` w modelu były zawsze `null` z komentarzem "expensive and rarely surfaced" — teraz faktycznie się wypełniają. Nowe pole `EmbeddedProfileColorSpace` dla czytelnej etykiety przestrzeni barw. Błędy parsowania (nieobsługiwany filtr, malformed) trafiają do diagnostyki — nie przerywają analizy
- **Nowe klucze loc** `FI.Oi.Registry`, `FI.Oi.ProfileName`, `FI.Oi.ColorSpace` (PL + EN)


### Wbudowane profile ICC (plumbing)

- **Folder źródłowy `Imposition.App/icc/` + reguła kopiowania** w `.csproj` — każdy `.icc` / `.icm` wrzucony do tego folderu trafia automatycznie do `{install}/icc/` przy każdym buildzie i `dotnet publish`. `IccProfileManager` już je czytał z dysku i pokazywał w dropdownie eksportu — brakowało plumbingu do bundlingu
- **README z dokładnymi nazwami plików ECI do pobrania** (zestaw zalecany: PSO Coated v3 / FOGRA51, PSO Uncoated v3 / FOGRA52, eciRGB v2 z [eci.org](https://www.eci.org/en/downloads)) wraz z ostrzeżeniem licencyjnym żeby NIE bundlować profili Adobe (SWOP, GRACoL, Japan Color, Adobe RGB) — są objęte prawami Adobe i nielegalne do redystrybucji w komercyjnym produkcie. Użytkownik może je sam zaimportować przyciskiem "Przeglądaj" (lądują wtedy w `%APPDATA%\imPRESS Studio\icc`)


### Presety i formaty zagraniczne (rynek US)

- **`PaperPresets` rozszerzone o ANSI A–E** — ANSI A (=Letter), ANSI B (=Tabloid, aliasy żeby joby celujące w serię inżynieryjną resolwowały się czysto), ANSI C (431.8×558.8 mm), ANSI D (558.8×863.6 mm), ANSI E (863.6×1117.6 mm). Letter / Legal / Tabloid / Ledger były już wcześniej
- **`TemplateSuggestionEngine` — 6 nowych presetów** dla wejść w wymiarach US:
  - Ulotka US Letter (8.5×11") 1-up na Tabloid (typowa amerykańska prasa)
  - Ulotka US Half-Letter (8.5×5.5") gangowana 2-up na Letter
  - Ulotka US Quarter-Letter (4.25×5.5") gangowana 4-up na Letter
  - Plakat US Tabloid (11×17") 1-up
  - Plakat ANSI C (17×22") 1-up
  - Plakat ANSI D (22×34") 1-upRazem z istniejącymi BusinessCardUS i PostcardUS to pełniejszy zestaw dla operatorów obsługujących klientów spoza UE. Nazwy presetów (PL + EN) używają placeholder `({0}×{1} {2})`, więc wymiary pokazują się w jednostce aktywnej — operator US widzi `"8.5×11 in"`, EU operator widzi `"215.9×279.4 mm"`


**Note:** Build: 0 warnings / 0 errors. Smoke test: aplikacja startuje czysto, logi bez wyjątków. Reguły rekomendacji w narracyjnych "Action" templates (np. `"ustaw bleed ≥ 3 mm"`) celowo zostawione z mm jako branżową konstantą — to porada, nie zmierzona wartość.

## [1.3.1] - 21 maja 2026 — Stabilna

*Dopracowanie topbara — przyciski Planuj i Eksportuj czytelniejsze*


### Przyciski akcji w topbarze

- **Przycisk *Planuj* dostał własny styl** zamiast neutralnego ghost — jasnoniebieskie tło (`#DBF1FF`), granatowy tekst i ikona (`#004AA9`) oraz subtelna, półprzezroczysta obwódka (`#006AC5` z ~25% alfą). Czytelny jako akcja, ale wizualnie nie konkuruje z głównym CTA *Eksportuj*. Nowa klasa stylu `Button.plan` w `App.axaml` — nie rusza pozostałych przycisków ghost
- **Tekst przycisku *Eksportuj* wymuszony na biały** (label + skrót `Ctrl+E`) — pełny kontrast na akcentowym tle niezależnie od motywu


**Note:** Wydanie kosmetyczne — bez zmian w silniku impozycji ani eksportu. Build: 0 warnings / 0 errors. Testy: 123/123 zielone.

## [1.3.0] - 18 maja 2026 — Stabilna

*UI redesign: onboarding, floating toolbar, drag-and-drop fix (STA), pasek postępu eksportu, OPOS w canvasie*


### Drag-and-drop z Eksploratora wreszcie działa

- **Root cause: `[STAThread]` tracony przez `async Task<int> Main`** — atrybut sticky-uje się do entry methodu tylko dopóki nie wykonano żadnego `await`. `hotFolderHost.StartAsync(...)` przerzucał kontynuację na thread pool (MTA), więc `BuildAvaloniaApp().StartWithClassicDesktopLifetime()` startował na wątku MTA. `RegisterDragDrop` (Win32 OLE) wymaga STA — wywoływany z MTA cicho odrzucał rejestrację, kursor pokazywał czerwony zakaz a żaden drag event nie docierał do Avalonia. Fix: `Main` wraca do sync, ścieżka GUI woła `hotFolderHost.StartAsync(...).GetAwaiter().GetResult()`; CLI dispatch wydzielony do prywatnego `RunCliAsync` (CLI nie potrzebuje STA)
- **Window-level + root-Grid + empty-state-Grid `DragDrop.AllowDrop=True`** jako defense-in-depth + `OnWindowOpened` wywołuje `DragDrop.SetAllowDrop(this, true)` po utworzeniu OS-level handle (obchodzi historyczny race w Avalonia 11.0.x)
- **Detekcja elevation przy starcie** — jeśli aplikacja jest uruchomiona z UAC (manifest mówi `asInvoker` ale ktoś odpalił Run as Administrator), `StatusText` ostrzega: "UWAGA: uruchomiono z uprawnieniami administratora — drag & drop z Eksploratora będzie zablokowany przez UAC" (UIPI block z medium IL do high IL)
- **Avalonia 11.0.10 → 11.2.7** + SkiaSharp 2.88.7 → 2.88.9. Razem z STA fixem domyka klasę bugów DnD na Windows. Brak breaking changes w istniejących stylach / bindings / source-genowanych properties


### Empty state — onboarding zamiast martwej przestrzeni

- **Makieta arkusza SRA3 + drop zone + checklist 1·2·3** w centrum canvasu zamiast suchej etykiety "Brak podglądu". Makieta = Viewbox 450:320 z dashed-stroke Rectangle + ItemsControl z UniformGrid placeholderów slotów według aktualnego `NUpX/NUpY` z fallback'iem 2×2. Drop zone = klikalny przycisk (otwiera file picker) z ikoną PDF + "Przeciągnij plik PDF tutaj · lub kliknij, aby wybrać · Ctrl+O". Checklist po prawej: ① Wczytaj PDF (✓ gdy są pliki) ② Wybierz strony (akcent gdy step 1 zrobiony) ③ Eksportuj
- **Drag-hover overlay** — pełnokanwasowy półprzezroczysty Accent-tint z napisem "Upuść, aby dodać do listy" + ikona PDF, widoczny gdy PDF jest przeciągany nad oknem. `IsHitTestVisible=False` żeby nie blokować dropu pod spodem
- **Inspektor empty state z cheat-sheet** — zamiast "Brak danych" widzisz: 3-paskowy skeleton imitujący strukturę bloku Utilization (krótki label, pełnowymiarowy value, średni sub), tekst "Statystyki pojawią się po wczytaniu pliku" i kartę "Skróty klawiszowe" z chipami mono-font: Ctrl+O Otwórz · Ctrl+P Planuj · Ctrl+E Eksportuj


### Floating toolbar canvasu — 4 wyraźne grupy zamiast jednej kolumny

- **Grupa A (top-left)**: zoom controls (−/+/50%/200%/Dopasuj/Rotate). Bez zmian.
- **Grupa B (top-right)**: view toggles — znaczniki cięcia / bleed-trim guides / CMYK / overprint / creep
- **Grupa C (pod B)**: segmented PRZÓD / TYŁ jako dwa ToggleButtony z one-way bindingiem do `Preview.ShowBackSide` (klik handluje w code-behind żeby uniknąć rebound flicker'a). Rozkładówka wydzielona do osobnego Border ftbar — wizualnie ortogonalna do side-switch
- **Grupa D (bottom-center)**: poziomy CMYK strip z 5-tym przyciskiem "Wsz." (włącza wszystkie 4 kanały naraz przez nowy `ShowAllChannelsCommand`). Chipsy CMYK gdy odznaczone tracą kolorowe tło — przezroczyste z neutralną obwódką, łatwiej zauważyć który kanał jest hidden


### Pasek postępu eksportu z fazami

- **Status bar pokazuje aktualną fazę**: Analiza PDF → Walidacja preflight → Planowanie impozycji → Składanie wynikowego PDF → **Konwersja do PDF/X (Ghostscript)**. Jeśli operator nie widzi fazy "Konwersja", od razu wie że PDF/X nie był nawet próbowany (najczęściej PdfXMode = None w opcjach). `ExportPipeline.RunAsync` dostał parametr `IProgress<double>?`, raportuje boundary'a per faza (15/25/30/85/100%); `MainWindowViewModel.ExportAsync` mapuje wartość na fazę-label dla `StatusText`
- **Mini-ProgressBar w prawej części statusbara** (80×4 px, accent foreground) widoczny przez `IsVisible="{Binding IsBusy}"`. Odróżnia się od zielonego paska wykorzystania (utilization)
- **Auto-detekcja Ghostscript** — przy otwarciu Eksport→Opcje skanuje `%ProgramFiles%\gs\gs*\bin\gswin64c.exe`, `%ProgramFiles(x86)%\gs\gs*\bin\gswin32c.exe` i `PATH`, wpisuje najnowszą znalezioną wersję do pola GS path. Eliminuje "Eksport: błąd Ghostscript not found" dla operatorów którzy zainstalowali GS standardowo
- **Success message zawiera nazwy obu plików** gdy PDF/X był włączony: "Eksport zakończony · zapisano X.pdf i X.pdfx1a.pdf" — koniec szukania gdzie poszedł plik PDF/X (sufix `.pdfx1a.pdf` / `.pdfx4.pdf` obok regularnego `.pdf`)


### Znaczniki cięcia + paserki + OPOS w canvasie

- **Canvas renderuje teraz Summa OPOS markery** (czarne kwadraty wokół bounding-boxa wszystkich placementów) — wcześniej `CutterMarksRenderer` żył tylko w pipeline'ie PDF eksportu; canvas o nich nic nie wiedział. `DrawMarksOverlay` woła `CutterMarkLayout.Compute` i rysuje halo-+-core rectangles tak samo jak crop/registration
- **Halo+core trick dla widoczności na każdym tle** — wszystkie marki (Crop, Registration, OPOS) rysowane teraz dwoma penami: gruba biała otoczka (alpha 220, stroke 2.4/2.0) najpierw, czarny rdzeń na wierzchu. Wcześniej czarny stroke 0.5 px na `#0A0E13` (dark theme canvas-bg) był praktycznie niewidoczny — fragmenty marków padających poza papier znikały
- **Min pixel size przy ekstremalnym zoom-out**: `markLen ≥ 6 px`, `markOff ≥ 2 px`, OPOS square ≥ 4 px. Bez clamp'u przy fit-zoom na rolce 320×2000mm (scale ≈ 0.05) krzyżyki cięcia miały sub-pikselową długość. Affekuje tylko podgląd na ekranie — eksport PDF używa surowych mm z `MarkLengthMm`
- **Domyślne marki dla presetu rolkowego**: `Crop=true`, `Registration=true`, `CutterMarks=None`, `ColorBars=false`, `FoldMarks=false`. Wcześniej rolka miała tylko OPOS włączone — operator który chciał klasycznych marków widział "nic". Teraz crop ticks + paserki domyślnie, OPOS opt-in via checkbox
- **Checkbox "Włącz markery Summa OPOS"** w Edytorze szablonów (sekcja MARKERY PLOTERA). Toggle flipuje `CutterMarks` między `None` ↔ `SummaOPOS`; gdy odznaczony, combobox typu + size + offset jest `IsEnabled=false`. Po zamknięciu Edytora `MainWindow` wymusza re-plan jeśli był aktywny job — bez tego zmiana w POCO `MarksOptions` nie propagowała się do canvasu (brak `INotifyPropertyChanged` i `UseTemplate(sameRef)` nie odpalał setter'a)


### RollFed — centrowanie i sugestie

- **Block items centrowany poziomo w canvasie rolki** — `xOriginPts = marginPts + (availWidth - blockWidth) / 2`. Wcześniej pakowane od lewego marginesu, leftover lecił na prawo (block off-center). Y zostaje top-anchored — to kierunek przewijania rolki, centrowanie wpajałoby pusty pasek przed każdym segmentem
- **Sheet margin rolki 0 → 5 mm** — bez tego znaczniki `MarkOffsetMm(3) + MarkLengthMm(5) = 8 mm` wypadały poza stronę i były clipped przez PDF page bounds. Strata: w bardzo wąskich produktach (30 mm) 1 item per row mniej; zysk: marki rzeczywiście istnieją na wyjściu
- **RollFed + 2-stronicowy PDF → duplex sugerowany automatycznie** — `SuggestForContext` dostała parametr `sourcePageCount`, adapter dla RollFed ustawia `Duplex = sourcePageCount >= 2`. Wcześniej rolka zawsze single-sided → user z 2-stronnym PDFem (front+back wizytówki) dostawał tylko front silently


### Sugestie szablonu — naprawione dwa współbieżne bugi

- **Karty w dialogu były niewidzialne** w Avalonia 11.2 — `Application.Current.TryFindResource(key, out _)` bez parametru `ThemeVariant` przestał znajdować zasoby z `ResourceDictionary.ThemeDictionaries`; `Res()` zwracało `Brushes.Transparent` dla każdego pędzla, więc `Background`, `BorderBrush` i `Foreground` kart były przezroczyste — karty były w drzewie wizualnym ale niewidoczne i nieklikalne (Avalonia hit-test pomija Border z przezroczystym tłem). Fix: `TryFindResource(key, Application.Current.ActualThemeVariant, out _)`
- **Plan failed po zastosowaniu sugestii** — adapter `AdaptToCurrentContext` brał `fit.NUpX/Y` z optymalizera który próbuje także rotacji o 90° dla lepszego packing'u. Dla wizytówki 85×55 na SRA3 wybierał wariant rotowany 6×3 (18 slotów zamiast 16), ale `Trim` zostawiał w natywnej orientacji. `Template.Validate()` liczył `NUpX × 91mm + gaps + margins ≈ 590 mm` i odrzucał (450 mm szerokość SRA3). Fix: adapter liczy N-up bezpośrednio formułą walidatora w natywnej orientacji (strata 1-2 sloty w niektórych przypadkach, zysk: zero rozjazdu między NUp a Trim)
- **Wizytówka jako saddle-stitch booklet** — adapter dziedziczył `Binding` z `currentTemplate` (workflow), a powinien ze `source.Template.Binding` (produkt). User w workflow A4 booklet wybierający "wizytówka standard" dostawał SaddleStitch który traktował 2 strony PDF jako rozkładówkę (1 wizytówka pośrodku zamiast 16). Nowa reguła: jeśli `current = RollFed` → keep RollFed (continuous-roll output to workflow override), inaczej → binding ze source preset (wizytówka jest Ganging z definicji)
- **Dialog "Sugerowane szablony" Height 540 → 680** + `CanResize=True`. Wcześniej content (3 matched + expander) nie mieścił się, footer z przyciskiem "Zastosuj" wymagał scrolla


### Topbar, step rail, panele

- **License badge w topbarze po prawej** (przed Plan/Eksport) z warning-tintem gdy `DaysRemaining < 7`. Wcześniej w centrum jako część `StatusText` — gubił się. Nowe pole `LicenseBadgeText` + `LicenseExpiringSoon` w VM, mutually-exclusive `Classes.pill` / `Classes.pill-warning` binding
- **Step rail wyższy (44 → 50 px)**, aktywny krok dostaje `AccentSoft` background (delikatny niebieski tint), step 4 (Eksport) gdy `Preview.Sheets.Count > 0` pokazuje białe ▶ na kółku `Accent` zamiast cyfry "4" — CTA wizualne. Meta-text font-size 10.5 → 11 + `Margin="0,3,0,0"` dla oddechu. `Foreground` nieaktywnych kroków Ink3 → Ink2 (kontrast na granicy WCAG AA → AAA)
- **Lewy panel — hover-actions na liście plików**. Wiersz dostaje `Classes="file-row"`; embedded `StackPanel Classes="hover-actions"` z 24×24 przyciskami ▲▼✕ z `Opacity=0` domyślnie, fade-in na `:pointerover` rodzica. Stary Grid akcji ▲▼✕ pod listą wywalony, Info button stretched. Destrukcyjne "Anuluj operację" + "Resetuj wszystko" wyrzucone z dolnej części panelu (są w menu Plik). Sekcja "Plik wyjściowy" expander zwinięty domyślnie
- **"Wczytaj zaznaczone" dynamic label**: "Wczytaj N zaznaczone" + `IsEnabled` bindowany na `CheckedFileCount`. VM tracking via `SourceFiles.CollectionChanged` + per-entry `PropertyChanged` na `IsIncluded`. File row selection indicator przeniesiony na lewą krawędź, 4 px szerokości, pełna wysokość wiersza, `CornerRadius="2,0,0,2"`
- **Topbar — logo "iP" z subtelnym gradientem** (LinearGradientBrush Accent → ciemny korpus) zamiast jednolitego `BadgeBg`. Nagłówki sekcji w lewym panelu: font-size 10.5 → 11 + `LetterSpacing=0.66` dla professional small-caps look. Dark-mode toggle w menu Widok → Przełącz motyw
- **Statusbar pasek nakładał się na tekst** przy wąskim oknie — Avalonia `StackPanel Orientation=Horizontal` nie respektuje bounds rodzica, jego desired width to suma children. `ClipToBounds=True` na lewej kolumnie statusbara: status text przy wąskim oknie jest czysto przycinany zamiast nakładać się na progress bar


### Drobne polish

- **Mojibake `"Ă—"` → `"×"`** w 4 miejscach renderowanego tekstu (N-Up "2 × 1", statusbar sheet dims "450 × 320 mm", empty-state mockup) + 2 komentarze XAML. Avalonia 11.2 renderowała dosłownie podwójnie zakodowane bajty UTF-8
- **Dialog Settings za małe** (Width 460 Height 440 CanResize=False) — przycisk Zamknij nie mieścił się w pełni przy 3 sekcjach (Język + Jednostki + Motyw). Bump do 480×600 + MinWidth/MinHeight + resize ON
- **Inner ImpositionCanvas `DrawEmptyState` usunięty** — XAML overlay zastępuje go w pełni. Wraz z nim nieużywany `InfoForeground` brush


**Note:** Build: 0 warnings / 0 errors. Testy: 123/123 zielone (zaktualizowany `StickerSize_GetsRollFedBindingWithCropMarksNoOposByDefault` po zmianie defaultu CutterMarks).

## [1.2.4] - 17 maja 2026 — Stabilna

*Inteligentne sugestie z kontekstu, RollFed duplex, jednostki mm/cale, linijka podglądu*


### Sugestie szablonu świadome kontekstu

- **Dwupoziomowy widok sugestii** — gdy masz wczytany szablon, na górze dialogu pokazują się propozycje *dopasowane do Twojego ustawienia*: każdy preset re-anchorowany do aktualnego `Sheet` + `Binding` + marginesów + marks + roll/overlay. Nagłówek "Dopasowane do Twojego ustawienia: {sheet} · {binding}" jasno mówi co i czemu. Oryginalna biblioteka (kanoniczne SRA3 / Roll 320) zwijana w sekcji "Inne formaty" — dla użytkownika który chce wyjść poza swój standard
- **N-up przeliczany przez LayoutOptimizer** dla każdej dopasowanej sugestii — gdy preset "wizytówka 85×55" trafia na Twój arkusz B2, dostajesz sloty obliczone dla B2, nie dla zaszytej SRA3
- **FitWarning (amber bubble)** na kartach gdzie optymalizator wylądował na 1×1 — natychmiastowy sygnał "ten format nie zgang-uje się dobrze na Twoim arkuszu". Nowa metoda `TemplateSuggestionEngine.SuggestForContext(width, height, currentTemplate)` zwraca `TemplateSuggestionSet { Matched, Library, ContextSheetName, ContextBinding }`


### RollFed + duplex — backs wreszcie istnieją

- **Strategia honoruje `Template.Duplex`**: 2-stronicowy PDF (front design + back design) → wszystkie itemy na rolce dostają stronę 1 z przodu i stronę 2 z tyłu. 4-stronicowy PDF → itemy cyklują pary (1+2, 3+4, 1+2…). 1-stronicowy PDF degraduje grzecznie do single-sided (nie ma sensu drukować tej samej grafiki po obu stronach). Backs mają identyczną geometrię jak fronts
- **Spread + toggle przód/tył działają dla RollFed duplex** — wcześniej canvas wymuszał single-side dla każdego RollFed niezależnie od duplex. Teraz blokada zostaje tylko dla rolek bez backs (jednostronnych). Nowy helper `ForceSingleSide(vm)` zastępuje binary `IsRollFed` check
- **Spread dla extreme-aspect (rolki/banery)** — pierwsza wersja wpychała oba arkusze w fit-both `sheetW*2+gap` × `sheetH`, co dla rolki 320×8000mm dawało dwie 16-px strzpy. Teraz `ComputeSpreadLayout` rozpoznaje extreme-aspect i: wysokie rolki — side-by-side z fit do połowy szerokości okna każda, dosunięte do góry; szerokie banery — stacked top-bottom z fit do połowy wysokości każdy, dosunięte do lewej. Bez ręcznego zoomowania


### Jednostki miary mm / cale

- **Settings → "Jednostki miary"** — combo Metryczne (mm) / Cale (in). Nowy enum `DisplayUnits` + property `AppSettings.Units` + parser z fallback'iem. Przechowywanie wewnętrzne pozostaje w mm; to wyłącznie warstwa prezentacji
- **Singleton `DisplayUnitsService`** z obserwowalnym eventem `Changed` — UI odświeża się od razu, bez restartu aplikacji. Helpery `FromMm`, `Suffix` ("mm" / "in"), `BaseTickMm` (10 mm dla metryki, 0.5 in = 12.7 mm dla cali)


### Linijka podglądu

- **Paski na górnej i lewej krawędzi canvas** pokazują współrzędne arkusza w aktywnej jednostce. Origin = lewy-górny róg arkusza (jak we wszystkich design appach), rośnie w prawo i w dół. Etykiety obróconej osi Y czytasz wzdłuż linijki
- **Tick spacing dobierany automatycznie** z mnożników {1, 2, 5, 10, 20, 50, 100, 200, 500, 1000} × base step tak, żeby labelowane major ticki miały ≥60 px miejsca. Na overview labelujesz co 100 mm, na zoom-in co 1 mm — bez ręcznej konfiguracji
- **Origin tick** (niebieski, grubszy) wyraźnie zaznacza krawędź arkusza nawet przy paneę. Chip w narożniku pokazuje aktywny suffix ("mm" / "in") żeby od razu wiedzieć w jakiej jednostce czytasz
- **Rysowana poza scene cache** — zmiana jednostki / zoom / pan odświeża linijkę natychmiast, bez inwalidacji cached bitmap. Wyłączona w spread mode (dwa arkusze = niejednoznaczne origin) i przy rotacji widoku (tick math zakłada osie wyrównane do canvas)


### Reset wszystkiego przywraca domyślny szablon

- **Po Reset → ponowny Load tego samego pliku pojawia się podgląd**. Wcześniej `ResetAll` zerował `CurrentTemplate=null`, przez co kolejny `LoadCheckedFilesAsync` pomijał `PlanCurrentJob` ("if CurrentTemplate is not null") i zostawiał użytkownika z "Wczytano N stron z M plików" oraz pustym canvas. `ResetAll` woła teraz `TryLoadDefaultTemplate()` — wraca do stanu startowego aplikacji


**Note:** Build: 0 warnings / 0 errors. Testy: 123/123 zielone (było 109, +14 nowych — context-aware suggestions, RollFed duplex pairing, display units). Jeden istniejący test `MultipleSourcePages_CycledRoundRobin` zaktualizowany o jawne `Duplex=false` (wcześniej polegał na ignorowanym Duplex w RollFed).

## [1.2.3] - 17 maja 2026 — Stabilna

*Smart preflight, template suggestions, layout optimizer + naprawy podglądu i imposycji*


### Preflight v2 — content-based bleed + narrative summary

- **Content-based bleed detection** — nowy `PdfContentExtentWalker` skanuje per-stronę bounding box wszystkich obrazów + glyphów tekstu (PdfPig), porównuje z TrimBox/BleedBox. Dwa nowe issue codes: `BLEED_CONTENT_UNUSED` ("deklarowany spad bez treści w obszarze bleed — białe paski po cięciu") i `BLEED_CONTENT_OVERFLOWS_BLEEDBOX` ("content wychodzi poza BleedBox — RIP utnie")
- **Smart preflight summary panel** — nad listą issues w FileInfoDialog pojawia się kolorowy banner (4 mood themes: Passing / InfoOnly / WarningsOnly / Critical) z jednolinjowym headline'em i top-5 priorytetowymi akcjami imperatywnymi ("Spad zerowy na 4 stronach — w źródle ustaw bleed ≥ 3 mm"). Pure-function `PreflightSummaryGenerator` + 22 nowe klucze `HF.PV.Action.*` w PL/EN


### Template suggestions z wymiarów PDF

- **Tools → "Sugeruj szablon z PDF…"** — `TemplateSuggestionEngine` z biblioteką 25 presetów (wizytówki, pocztówki, ulotki, plakaty, naklejki rolka z OPOS, hangtagi, CD/DVD, bilety). Euclidean dimensional matching z auto-rotacją, threshold 25mm. Dialog z selectable cards (confidence %, sheet + N-up summary), klik "Zastosuj" pcha gotowy `Template` do MainWindowViewModel
- **Auto-rejestracja "Roll 320" paper preset** — 320×1000mm dla naklejek/etykiet rolkowych. `EnsureAuxiliaryPaperPresets` idempotentne, woła się raz w composition root


### Layout optimizer (auto N-up)

- **Przycisk "Optymalizuj N-up"** w karcie Layout edytora szablonów. `LayoutOptimizer.Optimize` próbuje obu orientacji item (0° i 90°), liczy max sloty przy aktualnych margin/gutter/bleed, tie-break na wyższą sheet utilization. Bez rotacji gdy remis (nie flipuje gratuicznie orientacji)
- **Status banner po kliku**: "Optymalny układ: 6 → 21 sztuk na arkuszu (3×7)" lub "Aktualny układ (12 sztuk) jest już optymalny". Hint o rotacji item jeśli optymalna orientacja wymaga obrotu — nie flipuje Trim w tle, żeby nie zaskoczyć operatora


### Podgląd — Trim/Bleed guides + manual update check

- **Checkbox "Trim/Bleed"** w toolbarze podglądu — per-placement cyjan przerywany = TrimBox (linia cięcia), magenta kropkowany = BleedBox (do gdzie ma sięgać treść). Wizualne potwierdzenie content-bleed preflight checks. Domyślnie off (opt-in dla weryfikacji)
- **Help → "Sprawdź aktualizacje…"** — manual entry point obok cichego startup checka. Nowa `UpdateService.ManualCheckAsync` zwraca tri-state result (`UpdateAvailable` / `AlreadyLatest` / `Failed`) — user zawsze dostaje explicit feedback dialog "Masz najnowszą wersję (1.2.3)" zamiast cichego nulla


### Imposition — duplex pairing dla ganging

- **GangingStrategy**: 2-stronicowy PDF + 12-up + duplex → **1 arkusz** z 12 frontami na stronie A i 12 tyłami na stronie B (było: 2 osobne arkusze jednostronne). 4-stronicowy PDF → 2 arkusze pairowane (1+2), (3+4). Nieparzysta liczba stron: ostatni arkusz ma front + pusty back. `Duplex=false` zachowuje starą semantykę (jeden arkusz per strona). Grid offsets pre-computed raz — front i back dzielą identyczną siatkę


### Podgląd — naprawy UX

- **Licznik arkuszy 1-based** — pokazuje "Arkusz 1/2" zamiast wprowadzającego w błąd "0/2". Nowy `CurrentSheetNumber => CurrentSheetIndex + 1` w PreviewViewModel
- **Toggle przód/tył działa w spread view** — wcześniej w trybie rozkładówki klik na "Tył" nie zmieniał nic wizualnie (oba boki widać równocześnie). Teraz auto-wyłącza spread i pokazuje stronę przeciwną
- **Biały arkusz po odznaczeniu rozkładówki** — gdy `ShowBackSide=true` a `BackPlacements` puste (single-sided ganging), VM auto-wraca do front. Bez tego user wpadał na biały placeholder
- **CMYK bars na środku górnej krawędzi arkusza** — pojedynczy pasek wewnątrz papieru (było: per-placement powyżej arkusza, klipowane na małych sheetach). 40% szerokości arkusza, cap 160px, ~6px wysokości. Niezależny toggle "Paski CMYK" w toolbarze
- **Overview placeholder threshold 0.5 → 0.25** — wcześniej jeden tick scrolla z 50% zoomu (0.5÷1.25 = 0.4) niespodziewanie wpadał w tryb "str. 1" placeholder. Teraz overview kicks in dopiero przy genuinie miniaturowym arkuszu


**Note:** Build: 0 warnings / 0 errors. Testy: 109/109 zielone (było 66 przed sesją, +43 nowe — content-bleed, summary generator, suggestion engine, layout optimizer, ganging duplex).

## [1.2.2] - 16 maja 2026 — Stabilna

*Preview pipeline v3 — koniec eksplozji RAM na rolkach A0/A1, render proporcjonalny do ekranu (Fiery-style)*


### Eksplozja pamięci 3,5 GB → ~70 MB przy rolkach 1000+ slotów

- **Memory-budget LRU zastępuje count-based** — `PdfPreviewRenderer` trzyma się teraz twardego budżetu 512 MB (settable przez `MaxMemoryBytes`) zamiast liczby wpisów (poprzednio 80). Pojedyncze duże bitmapy A0/A1 nie mogą już rozdąć cache do wielu GB. Eviction: snapshot + sort po `LastAccessTick` + drop do limitu
- **Scale-clamping per bitmap (max 64 MB)** — przed alokacją sprawdzamy naturalne wymiary strony (cheap probe Docnet, bez rasteryzacji) i rozwiązujemy *s = √(cap / (w·h·4))* żeby cap'nąć efektywną skalę. A0 portrait nigdy nie zaalokuje 1.16 GB jak przy zoom 6×
- **Display-pixel-aware rendering — root cause fix** dla 3.5 GB. Nowy overload `RequestPageAsync(pdf, page, zoom, displayPixelWidth)` liczy scale z rzeczywistego rozmiaru placementu na ekranie, nie z `vm.Zoom`. Dla 50mm naklejki przy 1600% zoom na 8.7m rolce: zamiast 1446×936 px bitmap (~5 MB) renderujemy 121×78 px (~38 KB) — *~130× redukcja* przy zerowej utracie wizualnej. `SharpnessMargin` 2× zapewnia ostrość przy lekkim zoom in
- **Niskie buckety w `ScaleBuckets`** — dodane 0.5 / 0.75 / 1.0 obok istniejących 1.5 / 2.0 / 3.0 / 4.5 / 6.0. Display-aware caller dostaje skale proporcjonalne do faktycznego demand'u
- **Cancellation tokens per cache key** — `_pendingCts` zastępuje proste `_pending`. Nowy zoom na tej samej stronie cancel-uje wcześniejsze CTS prefix-sweep'em; render-task po `WaitAsync(token)` bail-outuje bez dotykania Docnet jeśli scale stała się stale. Eliminuje "render noise" przy szybkim zoomowaniu


### Vector overview mode (Fiery Impose-style)

- **Przy `vm.Zoom < 0.5`** canvas pomija `TryGetPageBitmap` i rysuje vector placeholder z numerem strony zamiast rasteryzowanego PDF. Mirror tego co robi Fiery Impose / Acrobat na overview zoom — przy fit-to-window użytkownik sprawdza layout, nie content, więc rasteryzacja to czysty koszt bez informacji
- **Zero background renderów** w overview tier — żaden Docnet open, żaden cache slot. Ogromna oszczędność CPU/RAM przy przelatywaniu przez wielostronicowe joby
- Eyedropper wyłączony w overview (nie ma bitmapy do sample), tooltip hit-area zachowany. Centered numer strony zastępuje corner label przy małych rozmiarach


### Roll-aware sheet layout — koniec 22px słupków

- **Aspect ratio > 4:1 → fit po krótszym wymiarze**. Rolka 320×8692mm pod fit-both dawała scale 0.069 → sheet rysował się jako 22px wąski słupek w 600px oknie. Teraz scale 2.06 → sheet wypełnia szerokość okna (~660px), długość przelewa się pionowo, user pan-uje/scroll-uje. Działa też dla horizontal rolek/banerów (fit po wysokości + align-left)
- **Align-to-start zamiast center na długiej osi** — pionowa rolka startuje u góry, pozioma od lewej. User widzi początek rolki, nie tiny chunk po środku
- **Standardowe formaty (A4, A3, B1, kwadrat) niezmienione** — fit-both + center jak dotąd. Threshold 4:1 jest gap'em między A-series (1.41:1) a rolkami (typowo 10:1+)
- Helper `ComputeSheetLayout` zastępuje 3 zduplikowane bloki fit-scale + center calc w canvas


### RollFed wymusza single-sheet (spread nie ma sensu dla rolki)

- **"Rozkładówka" ignorowana dla RollFed binding** — spread view (front+back obok siebie) to booklet concept. Dla ciągłej rolki dawałby już-wąski strip skompresowany jeszcze bardziej. Canvas wymusza `DrawSingleSheet` niezależnie od `vm.ShowSpread`
- **Scene cache działa dla RollFed** nawet gdy spread toggle on — `effectiveSpread` trafia do `SceneSnapshot` i `cacheEligible` check, więc no-op toggle nie inwaliduje cache


### Architektura — refactor i porządki

- `GetOrProbePageBox` — zunifikowany Docnet probe natywnych wymiarów strony z cache w `_pageDimensions`. Używany przez `ClampScaleForMemory` i `ComputeDisplayScale` (DRY)
- `StoreInCache` serializowany przez `_cacheLock` — eliminuje race przy concurrent inserts (TryHitCache reads zostają lock-free). Throughput nie cierpi bo inserts są throttlowane przez `MaxConcurrentRenders` (2-4)
- `PreviewViewModel.TryGetPageBitmap` ma nowy opcjonalny param `displayPixelWidth = 0` — back-compat z istniejącymi callerami, nowa display-aware ścieżka aktywuje się przy > 0
- Build: 0 warnings / 0 errors, testy 66/66 zielone

## [1.2.1] - 16 maja 2026 — Stabilna

*Edytor szablonów — UX po pierwszych testach 1.2: walidacja, persistence, polskie komunikaty*


### Edytor szablonów — koniec utraty stanu i niejasnych błędów

- **Stan edytora przeżywa zamknięcie dialogu** — `TemplateEditorViewModel` przeniesiony z *Transient* do *Singleton* w DI. Wpisane wartości pozostają między kolejnymi otwarciami menedżera szablonów w tej samej sesji aplikacji. Wcześniej każde otwarcie dawało świeży VM z domyślnymi wartościami — wszystko co wpisałeś przepadało jeśli dialog się zamknął
- **Walidacja blokuje zamknięcie dialogu z niepoprawnym szablonem** — kliknięcie *Użyj szablonu* najpierw waliduje. Gdy są błędy, dialog NIE zamyka się, banner z listą błędów pojawia się u góry edytora, użytkownik widzi co poprawić bez utraty żadnych wartości. Wcześniej dialog zamykał się cicho, a błąd wybuchał dopiero w MainWindow podczas planowania
- **Czerwony banner z błędami** nad nagłówkiem szablonu, każdy błąd na osobnej linii. Banner znika automatycznie po naprawieniu wszystkich błędów (kliknięcie OK rewaliduje), również po wczytaniu nowego szablonu z dysku lub presetu
- **Komunikaty walidacji po polsku** — wszystkie błędy w `Template.Validate()` i `RollFedStrategy` przetłumaczone. Zamiast *"Sheet dimensions must be positive"* jest *"Wymiary arkusza muszą być dodatnie"*; *"Roll usable width X cannot fit item Y"* → *"Użyteczna szerokość rolki Xmm nie mieści itemu Ymm"*; itd.
- **Auto-generowanie ID szablonu** — gdy pole ID jest puste, `Validate()` generuje GUID-based zamiast rzucać *"Template Id is required"*. Pole dalej edytowalne — można nadpisać czytelną nazwą
- **Edytor startuje z poprawnymi defaultami** — VM inicjalizuje się `MakeBlankTemplate()` (SRA3 landscape, A4 trim) zamiast pustym `new Template()`. Koniec kaskady błędów *"Sheet/Trim must be positive"* przy pierwszym otwarciu
- **Mniej duplikatów w komunikatach** — chained checki (*"roll usable width -10mm cannot fit item 0mm"*) pomijane gdy bazowe wartości są zerowe. Wcześniej dostawałeś 3 echa tego samego problemu


### Roll length — przełącznik split / continuous (jak Caldera, Onyx)

- **Checkbox "Podziel na segmenty"** + osobne pole długości segmentu zastąpiło anty-wzorzec *"0 = bez limitu"*. UI dychotomia matches branżowy standard — Caldera ma "Continuous" toggle, Onyx ma panel-length mode w device profile, Flexi/Ergosoft to samo. Toggle off = jeden ciągły output PDF; toggle on = aktywny numeryk z domyślnym 1000mm
- **Per-section enable toggles** w sekcji *Rolka* i *Nakładka / Logo* — checkbox *Włącz* przy nagłówku, inputy są wyszarzone gdy wyłączone. Walidacja respektuje flagę: standardowy sheet-fit check pomijany gdy roll active, overlay walidowany tylko gdy enabled. Koniec konfliktów typu "RollFed binding wyklucza saddle stitch validation"
- **Nowy preset:** *"Naklejki 50×50 — rolka 320mm"* — RollFed binding, 1000 sztuk, 6 sztuk/rząd × 167 rzędów, gap 2mm, Summa OPOS markers (3mm, offset 5mm). Klikasz preset → masz gotowy szablon pod produkcję naklejek na rolce


### UI — drobne poprawki

- Auto-fill szerokości i wysokości arkusza po wybraniu presetu (SRA3 → 450×320 itd.) — wcześniej trzeba było ręcznie wpisać. Pod spodem: zamiana referencji `Template.Sheet` na nową instancję `SheetSize` żeby Avalonia odświeżyła nested bindings (mutacja in-place nie wystarczała, bo POCO nie ma `INotifyPropertyChanged`)
- Sekcja "Rozmiar arkusza" uproszczona — usunięto zduplikowany rząd ręcznej edycji W/H/Nazwa. Zostały: dropdown presetu + landscape, plus jedna linia *Dodaj własny rozmiar* z polami szerokości i wysokości (z opisami i normalną wielkością)
- Strony www (PL + EN) — index, help/pomoc, faq, privacy: wpis o *roll-fed + Summa OPOS + overlay loga* w hero/features, nowe pytanie FAQ *"Czy obsługuje produkcję naklejek na rolce z plotterem Summa?"*, footer wersja 1.2.1


### Architektura

- `RollOptions.Enabled` i `OverlayOptions.Enabled` (default false) — bool flagi gate'ujące walidację per-sekcja niezależnie od `Binding`. JSON backward-compatible — stare szablony bez tych pól wczytują się z defaultami false
- `TemplateEditorViewModel.TryValidate()` — niewybuchająca walidacja zwracająca *true/false*, populująca `ValidationErrors` kolekcję bindowaną do bannera
- `TemplateEditorViewModel.RollSplitIntoSegments` — derived property mapująca checkbox split/continuous na `MaxRollLengthMm` (0 vs >0). Bez nowego pola w modelu — UI affordance nad istniejącym semantykiem
- Nowy test: `Validation_AutoGeneratesIdWhenMissing` — pilnuje że pusty Id nie throw'uje. Pokrycie: 66/66 zielone

## [1.2.0] - 15 maja 2026 — Stabilna

*Roll-fed / print&cut — produkcja naklejek na rolce z markerami Summa OPOS*


### Nowa oprawa: RollFed (step-and-repeat na wstędze)

- **Nowy typ oprawy `BindingType.RollFed`** — produkcja naklejek, etykiet i innych aplikacji rolkowych na ploterach lateksowych / solwentowych. Zamiast pojedynczego arkusza generuje segment wstęgi o szerokości rolki (`Sheet.WidthMm`) i wysokości obliczanej dynamicznie z liczby powtórzeń, gapów i wymiarów itemu
- **Variable repeat** — pole `RollOptions.RepeatCount` mówi ile sztuk wyprodukować. Silnik automatycznie liczy ile mieści się w rzędzie (`floor((width + gap) / (item + gap))`), ile rzędów potrzeba i wypełnia ostatni rząd częściowo jeśli liczba nie dzieli się równo
- **Multi-segment split** — gdy `MaxRollLengthMm > 0` (np. ploter ma limit ramki / operator chce krótsze segmenty), wynik dzieli się na N osobnych `SheetLayout`-ów. Każdy segment ma sztywną wysokość = `MaxRollLengthMm`. Bez limitu = jeden segment z tight-fit do faktycznej zawartości
- **Basic nesting (auto-rotation)** — checkbox *Pozwól obrócić* w UI każe silnikowi porównać upakowanie 0° vs 90° i wybrać orientację która zmieści więcej itemów w rzędzie. Nie zaawansowany irregular nesting (to backlog), ale dla prostokątnych naklejek bardzo skuteczne
- **Single-sided (front-only)** — rolki nie duplexuje się; `BackPlacements` zawsze pusty


### Markery plotera Summa OPOS / OPOS-XY

- **Nowy enum `CutterMarkType`** w `MarksOptions`: `None`, `SummaOPOS`, `SummaOPOSXY`. Cztery solid-black kwadraty (default 3mm) w narożnikach bounding-box-u wszystkich non-blank placementów, offset domyślnie 5mm od cut line — Summa wymaga min. 5–8mm clearance dla sensora optycznego
- **Wariant OPOS-XY** dorzuca mały label tekstowy *"Sheet N"* obok markera origin (BottomLeft) — sensor go ignoruje, operator widzi numer segmentu wzrokiem przy ładowaniu rolki na ploter
- **Pure geometry helper `CutterMarkLayout`** w `Imposition.Core/Marks/` — bez zależności od PdfSharp, w pełni testowalny w izolacji (10 testów jednostkowych pokrywających bbox, ignorowanie blank slotów, wybór offsetu, OPOS-XY)
- **Renderer `CutterMarksRenderer`** wpięty w `PdfComposer` obok `PrinterMarksRenderer`. Markery rysowane raz na arkusz/segment (nie per-item) — to zgodne z workflow Summa, gdzie ploter kalibruje się do całej grupy


### Overlay loga / nakładka per arkusz

- **Nowy `OverlayOptions`** — stempluje plik PNG/JPG/PDF na każdym arkuszu wynikowym. 9 anchor pozycji (TopLeft … BottomRight), offset X/Y, max szerokość (aspect ratio zachowany), tryb *Once per sheet* (raz na arkusz) lub *Once per item* (raz na każdy placement)
- **Wsparcie raster + vector** — PNG/JPG przez `XImage`, single-page PDF przez `XPdfForm`. Detekcja po rozszerzeniu pliku
- **Pure geometry helper `LogoOverlayLayout`** — 12 testów jednostkowych dla wszystkich anchor permutacji, aspect preservation, blank ignoring, edge cases
- **Renderer `LogoOverlayRenderer`** wpięty w `PdfComposer`. Brak loga = no-op (puste pole). Brak pliku na dysku = warning w logu, eksport jedzie dalej (defensywnie, żeby nie psuć batch jobów przy nieistniejącej ścieżce)


### Edytor szablonów — nowe panele

- **Sekcja "Rolka (RollFed)"** — pola: liczba powtórzeń, odstęp X/Y, maks. długość segmentu, allow rotation
- **Sekcja "Markery plotera (print&cut)"** — combo typu markera, rozmiar, odsunięcie
- **Sekcja "Nakładka / Logo"** — file picker (PNG/JPG/PDF), combo anchor, max szerokość, offset X/Y, toggle *raz na arkusz*
- Combo box *Typ oprawy* automatycznie pokazuje nową pozycję `RollFed`. Wszystkie nowe panele lokalizowane PL/EN. Pre-existing UI bez zmian


### API i architektura

- `Template` rozszerzony o trzy nowe property: `Roll`, `Overlay` (oraz pola w `Marks`: `CutterMarks`, `CutterMarkSizeMm`, `CutterMarkOffsetMm`). JSON serializacja kompatybilna wstecznie — stare szablony bez tych pól wczytują się z defaultami
- `Template.Validate()` dla `RollFed` pomija standardowy check *required sheet W×H ≥ N-up math* (długość arkusza liczona dynamicznie). Zamiast tego sprawdza, czy item w ogóle mieści się szerokością rolki
- `PdfComposer` przyjmuje teraz dodatkowo `CutterMarksRenderer` i `LogoOverlayRenderer` w konstruktorze (DI). Wywoływane razem z istniejącym `PrinterMarksRenderer` w `RenderSheetSide`. Zarejestrowane jako singletons w `CompositionRoot`
- Pokrycie testami: 65/65 zielone (29 istniejących + 14 RollFed + 10 CutterMarkLayout + 12 LogoOverlayLayout)

## [1.1.1] - 10 maja 2026 — Stabilna

*„Run as User” — koniec zapisu do Program Files, hardening UX hot folderów*


### Krytyczne: aplikacja nie pisze już do Program Files

- **Wszystkie dane dynamiczne przeniesione do `%APPDATA%` / `%LOCALAPPDATA%`** — wcześniej licencje, szablony i niektóre cache zapisywały się do `C:\Program Files\imPRESS Studio\`, co dawało `UnauthorizedAccessException` u użytkowników bez admina, kolizje z UAC przy aktywacji licencji oraz utratę danych przy aktualizacji aplikacji. Aplikacja działa teraz poprawnie pod zwykłym kontem Windows bez podnoszonych uprawnień
- **Nowy centralny helper `AppPaths`** — single source of truth dla wszystkich ścieżek. Roaming (`%APPDATA%\imPRESS Studio\`): `settings.json`, `hotfolders.json`, `templates/`, `presets/`, `licenses/`, `icc/` (user-imported), `export-presets/`, `localization/`. Local-only (`%LOCALAPPDATA%\imPRESS Studio\`): `logs/`, `state/hotfolder.db`, `cache/`, `temp/`, `recovery/`, `runtime/spool/`, `license.stamp`. Bundled assety (read-only ICC, default template) zostają w `{install}/icc/` i `{install}/config/` i nigdy nie są zapisywane podczas runtime'u
- **Migracja 1.0.x → 1.1.1 (idempotentna, jednorazowa)** — przy pierwszym starcie kopiuje stary `Program Files\imPRESS Studio\config\license.json` do `%APPDATA%\imPRESS Studio\licenses\license.json`, kopiuje stare szablony do `%APPDATA%\imPRESS Studio\templates\`, przenosi poprzednie `%LocalAppData%\ImpositionApp\*` (logi, state, cache) do `%LocalAppData%\imPRESS Studio\*`. Operator nie musi ręcznie nic przenosić — wszystko działa po update'cie
- **Ujednolicone nazwy katalogów** — usunięto inconsistency `ImpositionApp` vs `imPRESS Studio` w kilku miejscach (logi, ledger, dialog "Otwórz logi"). Wszędzie teraz używana jest jedna nazwa: `imPRESS Studio`
- **User-imported ICC profiles** trafiają teraz do `%APPDATA%\imPRESS Studio\icc\`. Bundled profile dostarczone z instalatorem dalej w `{install}/icc/` (read-only), oba katalogi merge'owane w combobox eksportu z user-priority na konflikty nazw


### Hot folder UX po realnych testach produkcyjnych

- **Auto-default `.work` / `.archive` / `.error`** — [ucięte w pliku źródłowym]
