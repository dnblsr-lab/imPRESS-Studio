# imPRESS Studio Changelog

## English

This section is a clean public English overview generated from the Polish source changelog. The full original Polish changelog follows below.

### Release overview

| Version | Date | Type | Highlights |
|---|---:|---|---|
| 1.4.0 | June 7, 2026 | Latest, Stable | Operation History — a session log in the right panel with restore-to-checkpoint |
| 1.3.6 | June 7, 2026 | Hotfix | License activation hotfix |
| 1.3.5 | June 6, 2026 | Stable | Machine identifier export to .lic and PDF merge |
| 1.3.4 | May 24, 2026 | Stable | Step & Repeat, mark profiles and localized names |
| 1.3.2 | May 23, 2026 | Stable | Full inch support, ICC profile details in File Info and US/ANSI presets |
| 1.3.1 | May 21, 2026 | Stable | Topbar polish — clearer Plan and Export buttons |
| 1.3.0 | May 18, 2026 | Stable | UI redesign: onboarding, floating toolbar, STA drag-and-drop fix, export progress and OPOS in preview |
| 1.2.4 | May 17, 2026 | Stable | Context-aware template suggestions, RollFed duplex, mm/in units and preview ruler |
| 1.2.3 | May 17, 2026 | Stable | Smart preflight, template suggestions, layout optimizer and preview/imposition fixes |
| 1.2.2 | May 16, 2026 | Stable | Preview pipeline v3 — no more RAM explosions on A0/A1 rolls, screen-proportional rendering |
| 1.2.1 | May 16, 2026 | Stable | Template editor UX after first 1.2 tests: validation, persistence and localized messages |
| 1.2.0 | May 15, 2026 | Stable | Roll-fed / print & cut — sticker production on roll media with Summa OPOS marks |
| 1.1.1 | May 10, 2026 | Stable | Run as User — no more writes to Program Files and stronger hot-folder UX |
| 1.1.0 | May 9, 2026 | Stable | Hot Folders — automated production imposition |
| 1.0.8 | April 26, 2026 | Stable | Color & Clock — ICC management and clock rollback protection |
| 1.0.7 | April 26, 2026 | Stable | License Hardening — strict export gate, new fingerprint and hardware tolerance |
| 1.0.6 | April 26, 2026 | Stable | Progressive Zoom — instant preview while zooming |
| 1.0.5 | April 26, 2026 | Stable | Ink Inspector — CMYK channel separation and color sampler |
| 1.0.4 | April 26, 2026 | Stable | Adaptive Preview — faster preview for large PDFs |
| 1.0.3 | April 26, 2026 | Stable | Print-Ready — PDF/X-1a, preflight report and sheet numbering in preview |
| 1.0.2 | April 26, 2026 | Stable | Numbering & Persistence — sheet numbering and saved export options |
| 1.01 | April 19, 2026 | Stable | Polish Pass — menus, settings and gang-run |
| 1.00 | April 18, 2026 | Stable | First Impress — first official production release |
| 0.9 | April 10, 2026 | Beta | Release Candidate — final testing before 1.0 |
| 0.8 | March 28, 2026 | Beta | Polish & Stats — UI polish and statistics panel |
| 0.7 | March 15, 2026 | Beta | Preview Master — interactive sheet preview |
| 0.6 | February 25, 2026 | Beta | Print Marks & Creep |
| 0.5 | February 10, 2026 | Beta | PDF/X-4 & Ghostscript |
| 0.4 | January 20, 2026 | Beta | Template Manager |
| 0.3 | January 8, 2026 | Beta | Binding Types |
| 0.2 | December 18, 2025 | Beta | Saddle Stitch MVP |
| 0.1 | December 1, 2025 | Alpha | Genesis — application skeleton |

## 1.4.0 — June 7, 2026 (Latest, Stable)

*Operation History — a session log in the right panel with restore-to-checkpoint*

### Added — Operation History tab

- **The “History” tab in the right inspector is now active.** It now shows a chronological session log for all important operations. The newest entry appears at the top, with time (`HH:mm:ss`), a category icon and a short one-line description.
- **Logged operations include:** loading a source file, adding files to the list, removing files, loading selected files, planning an imposition, successful/cancelled/failed export, PDF merging, template changes, export option changes, drag-and-drop slot assignment and project reset.
- **Entries are localized live.** Each history item stores a localization key and arguments, not a baked string. Switching PL ↔ EN refreshes the whole history immediately without restarting the window.
- **Clear button.** The session log can be cleared. It is capped at the latest 200 entries, so long sessions do not grow the list forever.
- **Restore to a selected operation.** Hovering an entry reveals a restore button (`↺`). It restores the full in-memory project state from that point: source file, template, export options, file list and planned sheet layout. Already exported PDF files on disk are not deleted.

### Added — Collapsible inspector panel

- A slim handle between the preview and the right inspector now lets the user collapse or restore the side panel. The preview can use the full available width, while the toggle remains visible.

### Changed — Activation window

- License status now uses semantic colors in both themes: green for active licenses, red for inactive ones.
- Cards now respect the active theme instead of using hard-coded bright backgrounds.
- Loading a `.lic` file is now placed above the activation-key field, because it is the faster primary flow.
- Labels were corrected from “JSON” to `.lic`, matching the actual user-facing file extension.
- Activation logic is unchanged; this release only improves UI clarity.

## 1.3.6 — June 7, 2026 (Hotfix)

*License activation hotfix*

### Fixed

- Fixed a false “license control file was modified” block when changing licenses on the same machine, for example trial → paid or reissued license. `LicenseClockGuard` now recognizes a control file that belongs to a different license and recreates it instead of treating it as tampering.
- The anti-clock-rollback guard still works as before.
- The control file is additionally cleared during each activation.

### Added

- The app now reminds the user to restart after loading a license key or license file.
- The license-server admin panel now includes buttons to download the `.lic` file and copy the key to the clipboard.

## 1.3.5 — June 6, 2026 (Stable)

*Machine identifier export to `.lic` and PDF merging*

### Added — Machine identifier export

- Added an **Export `.lic`** flow in the activation window. It saves the machine fingerprint to a `.lic` file, avoiding manual copy/paste of long hardware identifiers.
- The trial-license generator on the website can now accept that `.lic` file and fill the fingerprint field automatically. Manual paste still works.

### Added — PDF merge

- Added a **Merge…** button in the source files panel. It becomes active when at least two files are selected.
- The merge operation writes a new PDF via the native Save As dialog and does not automatically load the result into the project.
- Suggested filenames follow the pattern `merged_<date>_<file_count>_<page_count>.pdf`.

## Older release summary

The detailed historical notes are preserved in the Polish section below. Main older milestones:

- **1.3.4:** Step & Repeat binding, mark profiles, localized preset/profile names and an in-app mark profile editor.
- **1.3.2:** Full millimeter/inch UI support, ICC profile information in File Info, bundled ICC plumbing and US/ANSI presets.
- **1.3.0:** Major UI redesign, working Explorer drag-and-drop via STA fix, export progress phases, OPOS marks in preview and many UX fixes.
- **1.2.x:** Context-aware suggestions, RollFed duplex, preview ruler, smart preflight, layout optimizer and memory-safe preview pipeline.
- **1.1.x:** Hot folders, CLI production automation, stronger storage rules and safer user-mode operation.
- **1.0.x:** First production line: PDF/X export, adaptive preview, CMYK inspection, license hardening, ICC management and clock protection.
- **0.x:** Early foundations: template manager, binding types, saddle-stitch MVP, print marks, creep and interactive preview.

## Roadmap

Planned future work includes a Windows Service mode for hot folders, REST API and webhooks, JDF/JMF integration, macOS/Linux builds, an OpenGL preview engine, more UI languages, more gang-run templates and regex-based multi-template routing for hot folders.

---

# imPRESS Studio — Historia wersji

## Polski

## 1.4.0 — 7 czerwca 2026 (Najnowsza, Stabilna)

*Historia operacji — dziennik sesji w prawym panelu z cofnięciem do wybranego punktu*

### Historia operacji (zakładka „Historia")

- **Zakładka „Historia" w prawym inspektorze ożyła** — wcześniej był tam tylko placeholder „pojawi się w przyszłej wersji". Teraz pokazuje chronologiczny dziennik wszystkich istotnych operacji bieżącej sesji: najnowszy wpis na górze, z godziną (`HH:mm:ss`), ikoną kategorii i krótkim opisem jednoliniowym.
- **Logowane operacje:** wczytanie pliku źródłowego (nazwa + liczba stron), dodanie plików do listy, usunięcie pliku / odznaczonych plików, wczytanie zaznaczonych (liczba stron i plików), zaplanowanie impozycji (liczba arkuszy), zakończony / anulowany / błędny eksport, łączenie plików PDF (Połącz…), zmiana szablonu, aktualizacja opcji eksportu, przypisanie pliku do slotu (drag&drop) oraz reset projektu.
- **Opisy w aktywnym języku — i przełączają się żywcem.** Każdy wpis przechowuje *klucz lokalizacyjny* i argumenty (nie gotowy tekst), więc przy zmianie języka PL ↔ EN cała historia natychmiast re-renderuje się w nowym języku, bez restartu okna. Ta sama technika co lokalizowane nazwy presetów i profili znaczników z v1.3.4 (`HistoryEntry.RefreshLocalization()` + subskrypcja `LocalizationService.PropertyChanged` w `MainWindowViewModel`).
- **Przycisk „Wyczyść"** opróżnia dziennik sesji. Lista jest ograniczona do 200 ostatnich wpisów (`MaxHistoryEntries`) — długa sesja nie rozrasta jej w nieskończoność; po przekroczeniu limitu najstarsze wpisy są odcinane. Gdy dziennik jest pusty, widać podpowiedź zamiast pustego panelu.
- **Cofnięcie do wybranej operacji (hover → ↺).** Po najechaniu na wpis pojawia się przycisk „↺", który **przywraca cały stan projektu** do momentu tej operacji: plik źródłowy, szablon, opcje eksportu, lista plików oraz zaplanowany układ arkuszy — wraz z ręcznymi zmianami slotów wykonanymi później (zostają cofnięte). Każdy wpis przechowuje lekki *snapshot* (`WorkspaceSnapshot`) złożony z referencji do niezmienialnych obiektów (`SheetLayout`/`PagePlacement` nigdy nie są mutowane w miejscu — edycja tworzy nowe instancje), więc checkpoint jest tani pamięciowo i nie wymaga głębokiego klonowania. *Ograniczenie z założenia:* wyeksportowany już plik PDF na dysku NIE jest usuwany — przywracany jest wyłącznie stan w pamięci aplikacji.
- **19 nowych kluczy lokalizacyjnych** PL + EN (`Hist.*`, w tym `Hist.RestoreTip` / `Hist.Restored`). Nowe klasy `HistoryEntry` (`ObservableObject`) i `WorkspaceSnapshot`; kolekcja `MainWindowViewModel.History` + helper `LogHistory(glyph, key, args)` (robi snapshot przez `CaptureSnapshot()`) wpinany w istniejące ścieżki operacji obok ustawiania `StatusText`, oraz `RestoreToHistoryEntry(entry)` wywoływane z przycisku ↺.

### Zwijany panel boczny (inspektor)

- **Smukły uchwyt na styku podglądu i prawego panelu** — chevron „›" zwija inspektor (podgląd odzyskuje całą szerokość), „‹" przywraca go. Uchwyt jest zawsze widoczny, wyśrodkowany w pionie. Stan trzymany w `MainWindowViewModel.IsInspectorCollapsed` + `ToggleInspectorCommand`; trzecia kolumna układu zmieniona na `Auto`, a panel (stała szerokość 280) chowa się przez `IsVisible`, więc kolumna kurczy się do zera bez konwerterów. Nowy klucz `Insp.ToggleTip` + styl `Button.inspector-toggle`.

### Okno aktywacji — czytelniejsze i poprawne w obu motywach

- **Status licencji koloruje się semantycznie** — zielony (tło + tekst) gdy licencja aktywna, czerwony gdy nieaktywna. Wcześniej pole statusu miało wpisane na sztywno ciemne tło (`#0a2e1a` / `#1a1a2e`), które w jasnym motywie wyglądało jak czarny blok. Teraz używa motywozależnych zasobów `SuccessSoft`/`Success` i `DangerSoft`/`Danger` (sterowane klasami stylów `lic-status.active` / `.inactive`), więc wygląda dobrze i w jasnym, i w ciemnym motywie. Ten sam tint dostał baner komunikatów (sukces/błąd).
- **Karty respektują motyw** — styl `Border.card` miał wpisane `#FFFFFF` + jasną ramkę (białe karty na ciemnym tle w trybie dark). Przepięty na `{DynamicResource Surface}` / `Border`.
- **Wczytywanie pliku .lic nad kluczem aktywacyjnym** — sekcja „Plik licencji" przeniesiona ponad pole klucza (to szybsza, główna ścieżka). Przyciski przemianowane z „JSON" na właściwe rozszerzenie **.lic** (ten sam plik JSON niesie zarówno fingerprint maszyny, jak i licencję): „Wczytaj plik .lic…" oraz „Eksportuj .lic…". Obie ścieżki (wczytaj plik / wklej klucz) mają teraz spójny przycisk akcji w kolorze akcentu i krótkie podpowiedzi.
- Nowy klucz `Act.LoadFileHint`, przeredagowane `Act.OrLoadFile` / `Act.LoadFileBtn` / `Act.ExportJsonBtn` / `Act.KeyHint` (PL + EN). Logika walidacji/aktywacji bez zmian — to wyłącznie warstwa UI.

Build: 0 warnings / 0 errors. Bez zmian w silniku impozycji i eksportu — funkcja czysto UI/obserwacyjna.

## 1.3.6 — 7 czerwca 2026 (Hotfix)

*Poprawka aktywacji licencji*

### Naprawione

- **Błędna blokada „Plik kontrolny licencji został zmodyfikowany"** przy zmianie licencji na tym samym komputerze (np. trial → płatna, ponowne wydanie). Strażnik cofania zegara (`LicenseClockGuard`) zapisuje plik kontrolny powiązany z podpisem konkretnej licencji; przy wgrywaniu *innej* licencji stary plik nie pasował i był traktowany jak sabotaż. Teraz niedopasowany plik kontrolny jest rozpoznawany jako „należący do innej licencji" i zapisywany od nowa — sama blokada cofania zegara działa bez zmian. Plik kontrolny jest dodatkowo czyszczony przy każdej aktywacji.

### Dodane

- **Przypomnienie o restarcie** — po wczytaniu klucza lub pliku licencji aplikacja informuje, że należy ją zamknąć i uruchomić ponownie, aby licencja zaczęła obowiązywać.
- **Panel admina (serwer licencji)** — w akcjach przy każdej licencji doszły przyciski *Pobierz plik .lic* oraz *Kopiuj klucz do schowka*, obok wysyłki ponownej i blokady.

Hotfix do 1.3.5 — bez zmian w silniku impozycji. Build: 0 warnings / 0 errors.

## 1.3.5 — 6 czerwca 2026 (Stabilna)

*Eksport identyfikatora do .lic, łączenie plików PDF*

### Eksport identyfikatora maszyny do pliku .lic

- **Nowy przycisk *Eksportuj JSON…*** w oknie *Aktywacja licencji*, pod przyciskiem *Kopiuj*. Zapisuje identyfikator sprzętowy do pliku `.lic` (natywne okno „Zapisz jako", rozszerzenie `.lic` a nie `.json`) — wygodniejsze niż ręczne kopiowanie długiego ciągu, gdy zamawiasz licencję. Plik zawiera składniki `MAC`/`DISK`/`UUID` oraz gotowy ciąg `fingerprint` (to samo, co kopiuje przycisk Kopiuj) plus nazwę maszyny i znacznik czasu eksportu
- **Strona z generatorem licencji próbnej** (impressstudio.pl) przyjmuje teraz ten plik — zamiast wklejać identyfikator, wgrywasz `.lic` i pole wypełnia się samo. Obsługa po obu stronach: wklejenie ciągu nadal działa

### Łączenie plików PDF (Połącz…)

- **Przycisk *Połącz…*** w panelu plików źródłowych, obok *+ Dodaj*. Aktywny gdy zaznaczysz co najmniej dwa pliki. Scala je w jeden PDF i zapisuje na dysk przez natywne okno „Zapisz jako" — operacja samodzielna, niezależna od impozycji (nie ładuje wyniku do projektu)
- **Sugerowana nazwa** według wzorca `merged_<data>_<liczba_plików>_<liczba_stron>.pdf` (np. `merged_2026-06-06_3_48.pdf`) — liczba stron liczona od razu z analizy, bez wcześniejszego scalania. Pliki łączone w kolejności z listy. Wykorzystuje istniejący silnik scalania, ten sam co dotychczasowe automatyczne łączenie zaznaczonych plików

Build: 0 warnings / 0 errors. Nowe klucze lokalizacyjne PL + EN dla obu funkcji. Część serwerowa (panel admina trial-servera) zyskała generator licencji płatnych — trial / 6 mies. / rok / wieczysta / custom — niezależnie od aplikacji desktopowej.

## 1.3.4 — 24 maja 2026 (Stabilna)

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

Build: 0 warnings / 0 errors. Testy: **146/146 zielone** (+18 od v1.3.2 — `MarksProfileResolverTests`, `SlugFormatterTests`, `StepAndRepeatStrategyTests`). Smoke test: aplikacja startuje czysto, 5 wbudowanych profili znaczników seeduje się do `%APPDATA%`, legacy szablony renderują się identycznie jak przed migracją, przełączenie języka żywcem aktualizuje labelki presetów i nazwy profili.

## 1.3.2 — 23 maja 2026 (Stabilna)

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

Build: 0 warnings / 0 errors. Smoke test: aplikacja startuje czysto, logi bez wyjątków. Reguły rekomendacji w narracyjnych "Action" templates (np. `"ustaw bleed ≥ 3 mm"`) celowo zostawione z mm jako branżową konstantą — to porada, nie zmierzona wartość.

## 1.3.1 — 21 maja 2026 (Stabilna)

*Dopracowanie topbara — przyciski Planuj i Eksportuj czytelniejsze*

### Przyciski akcji w topbarze

- **Przycisk *Planuj* dostał własny styl** zamiast neutralnego ghost — jasnoniebieskie tło (`#DBF1FF`), granatowy tekst i ikona (`#004AA9`) oraz subtelna, półprzezroczysta obwódka (`#006AC5` z ~25% alfą). Czytelny jako akcja, ale wizualnie nie konkuruje z głównym CTA *Eksportuj*. Nowa klasa stylu `Button.plan` w `App.axaml` — nie rusza pozostałych przycisków ghost
- **Tekst przycisku *Eksportuj* wymuszony na biały** (label + skrót `Ctrl+E`) — pełny kontrast na akcentowym tle niezależnie od motywu

Wydanie kosmetyczne — bez zmian w silniku impozycji ani eksportu. Build: 0 warnings / 0 errors. Testy: 123/123 zielone.

## 1.3.0 — 18 maja 2026 (Stabilna)

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

Build: 0 warnings / 0 errors. Testy: 123/123 zielone (zaktualizowany `StickerSize_GetsRollFedBindingWithCropMarksNoOposByDefault` po zmianie defaultu CutterMarks).

## 1.2.4 — 17 maja 2026 (Stabilna)

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

Build: 0 warnings / 0 errors. Testy: 123/123 zielone (było 109, +14 nowych — context-aware suggestions, RollFed duplex pairing, display units). Jeden istniejący test `MultipleSourcePages_CycledRoundRobin` zaktualizowany o jawne `Duplex=false` (wcześniej polegał na ignorowanym Duplex w RollFed).

## 1.2.3 — 17 maja 2026 (Stabilna)

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

Build: 0 warnings / 0 errors. Testy: 109/109 zielone (było 66 przed sesją, +43 nowe — content-bleed, summary generator, suggestion engine, layout optimizer, ganging duplex).

## 1.2.2 — 16 maja 2026 (Stabilna)

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

## 1.2.1 — 16 maja 2026 (Stabilna)

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

## 1.2.0 — 15 maja 2026 (Stabilna)

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

## 1.1.1 — 10 maja 2026 (Stabilna)

*„Run as User” — koniec zapisu do Program Files, hardening UX hot folderów*

### Krytyczne: aplikacja nie pisze już do Program Files

- **Wszystkie dane dynamiczne przeniesione do `%APPDATA%` / `%LOCALAPPDATA%`** — wcześniej licencje, szablony i niektóre cache zapisywały się do `C:\Program Files\imPRESS Studio\`, co dawało `UnauthorizedAccessException` u użytkowników bez admina, kolizje z UAC przy aktywacji licencji oraz utratę danych przy aktualizacji aplikacji. Aplikacja działa teraz poprawnie pod zwykłym kontem Windows bez podnoszonych uprawnień
- **Nowy centralny helper `AppPaths`** — single source of truth dla wszystkich ścieżek. Roaming (`%APPDATA%\imPRESS Studio\`): `settings.json`, `hotfolders.json`, `templates/`, `presets/`, `licenses/`, `icc/` (user-imported), `export-presets/`, `localization/`. Local-only (`%LOCALAPPDATA%\imPRESS Studio\`): `logs/`, `state/hotfolder.db`, `cache/`, `temp/`, `recovery/`, `runtime/spool/`, `license.stamp`. Bundled assety (read-only ICC, default template) zostają w `{install}/icc/` i `{install}/config/` i nigdy nie są zapisywane podczas runtime'u
- **Migracja 1.0.x → 1.1.1 (idempotentna, jednorazowa)** — przy pierwszym starcie kopiuje stary `Program Files\imPRESS Studio\config\license.json` do `%APPDATA%\imPRESS Studio\licenses\license.json`, kopiuje stare szablony do `%APPDATA%\imPRESS Studio\templates\`, przenosi poprzednie `%LocalAppData%\ImpositionApp\*` (logi, state, cache) do `%LocalAppData%\imPRESS Studio\*`. Operator nie musi ręcznie nic przenosić — wszystko działa po update'cie
- **Ujednolicone nazwy katalogów** — usunięto inconsistency `ImpositionApp` vs `imPRESS Studio` w kilku miejscach (logi, ledger, dialog "Otwórz logi"). Wszędzie teraz używana jest jedna nazwa: `imPRESS Studio`
- **User-imported ICC profiles** trafiają teraz do `%APPDATA%\imPRESS Studio\icc\`. Bundled profile dostarczone z instalatorem dalej w `{install}/icc/` (read-only), oba katalogi merge'owane w combobox eksportu z user-priority na konflikty nazw

### Hot folder UX po realnych testach produkcyjnych

- **Auto-default `.work`/`.archive`/`.error`** — pola Folder roboczy / archiwum / błędów w dialogu edycji nie są już wymagane. Gdy puste, runner sam tworzy `.work`, `.archive`, `.error` jako podkatalogi folderu wejściowego (z dropką, więc filewatcher i tak ich nie matchuje). Dialog pokazuje wyliczoną wartość pod każdym blank polem: *"Domyślnie zostanie utworzony: D:\HF\input\.work"* — operator widzi co się stanie, zanim kliknie Zapisz
- **Realtime walidacja per-pole** w dialogu Dodaj/Edytuj — czerwone gwiazdki `*` przy wymaganych polach (Nazwa, Wejście, Wyjście, Szablon), pod każdym polem mała czerwona etykieta z konkretnym błędem. Walidacja na każdy keystroke; przycisk *Zapisz* jest disabled dopóki `IsValid = false`. Koniec ze zbiorczym banner-em po kliknięciu Zapisz — wiesz od razu które pole jest złe
- **Banner statusu licencji w managerze** — gdy licencja jest nieprawidłowa lub wygasła, czerwony banner ⚠ na górze okna z treścią błędu i przyciskiem *Otwórz menedżer licencji*, który otwiera dialog aktywacji bez wychodzenia z managera. Po pomyślnej aktywacji banner znika i Start staje się dostępny. Wcześniej operator widział tylko warning w logu i nie wiedział czemu Start nic nie robi
- **Banner odzyskania zadań po crashu** — po starcie aplikacji, jeżeli runner odzyskał osierocone zadania z poprzedniej sesji (proces padł w trakcie eksportu), pojawia się niebieski banner: *"Odzyskano N przerwane zadanie/zadania hot folderów po poprzednim zamknięciu aplikacji."*
- **Start/Restart disabled przy nieprawidłowej licencji** — komendy mają `CanExecute = HasSelection && LicenseValid`, więc przyciski są wyszarzone. Wcześniej kliknięcie Start przy złej licencji zapisywało wyjątek tylko do loga, bez czytelnego sygnału w UI

### Hardening backendu — fail fast zamiast cichych crashy

- **Hard guard clauses na każdym I/O boundary** — `HotFolderRunner.EnsureFolders`, `FileTransitions.MoveToWork/ApplyOnSuccess/ApplyOnFailure/ReturnToInput`, `HotFolderJobProcessor.LoadTemplateAsync`, oba watcher sources (`FileSystemWatcher` + polling) walidują teraz: `ArgumentException.ThrowIfNullOrWhiteSpace`, `Path.IsPathFullyQualified`, brak invalid characters. Jeden z testów zlapal `Directory.CreateDirectory("")` mimo walidacji UI — backend nie ufa już upstream'owi i sam fail-fastuje z czytelnym komunikatem
- **Walidator hot folderów rozluźniony dla dotted defaults** — wcześniejsza zasada *"work/archive/error nie mogą leżeć wewnątrz input"* jest dalej egzekwowana, ale teraz akceptuje single-level subdir o nazwie `.work`, `.archive`, `.error` jako runner-managed exception (i tak są wykluczone z FSW przez nazwę z kropką + explicit check w runner)
- **Atomic write i rotacja uszkodzonych configów** — `AppSettings.Save()` używa teraz `.tmp` + `File.Move`, więc crash w trakcie zapisu nie zostawia półpustego pliku settings.json. `AppSettings.Load()` przy `JsonException` rotuje plik do `.broken-yyyyMMdd-HHmmss` i wraca z defaultami zamiast crashować. `HotFolderConfigStore` już to miał — teraz spójna polityka w całej aplikacji
- **Startup summary log** — pierwsza linia w logu pokazuje teraz wszystkie kluczowe ścieżki: roaming, local, logs, licenses, state, hotfolders config. Diagnostyka problemów u klienta jest natychmiastowa
- **Mniej szumu w logu** — komunikat *"Hot folder config not found at ...; starting empty."* loguje się teraz tylko raz per process. Wcześniej manager VM polluował logi tym wpisem co 1,5 sekundy (interval timera odświeżania). Reszta typowo śmieciowych komunikatów też przejrzana

### Drobne

- `HotFolderStatusSnapshot` rozszerzony o pole `RecoveredOrphansAtStart` — manager VM agreguje tę liczbę między runnerami żeby renderować banner odzyskania
- `HotFolderManagerViewModel` bierze teraz `LicenseValidator` jako dependency, sprawdza licencję przy `InitializeAsync` i po zamknięciu `ActivationDialog`
- Wersja `1.1.0 → 1.1.1` (patch — runtime/UX hardening, brak nowych funkcji ani breaking changes)

### Uwagi przy aktualizacji

- **Operator nic nie musi robić** — migracja licencji i szablonów odbywa się automatycznie przy pierwszym uruchomieniu 1.1.1. Po update obie ścieżki (stara i nowa) chwilowo współistnieją; nowa wygrywa, stara jest opcjonalnie usuwana (failure OK gdy brak admina). Po następnej deinstalacji 1.0.x zostawiona kopia w Program Files znika
- **Istniejące hot foldery działają bez zmian** — config `hotfolders.json` przechowuje tylko ścieżki które były wpisane wcześniej; runner przy starcie wypełnia blank pola (`.work`/`.archive`/`.error`) defaultami. Konfiguracje sprzed 1.1.1 z explicit ścieżkami nie są dotykane
- Pliki w starych lokalizacjach (`%LocalAppData%\ImpositionApp\logs` itd.) nie są kasowane — operator może je sprawdzić i ręcznie usunąć po zweryfikowaniu że nowa lokalizacja działa

## 1.1.0 — 9 maja 2026 (Stabilna)

*„Hot Folders” — automatyczna impozycja w trybie produkcyjnym*

### Nowa funkcja flagowa: hot foldery

- **Pełny system hot folderów dla drukarni produkcyjnych** — wskazujesz folder wejściowy, aplikacja sama wykrywa wrzucone pliki PDF, nakłada wybrany szablon, eksportuje gotowy arkusz do folderu wyjściowego i archiwizuje oryginał. Wszystko w tle, bez interakcji użytkownika. Idealne pod workflow „operator wrzuca PDF z DTP, ripper bierze gotowy arkusz”
- **Wiele hotfolderów jednocześnie** — każdy z własnym szablonem (np. *Saddle A4 → 4-up*, *Wizytówki gang-run*, *Broszura A5*). Każdy runner jest izolowany — błąd na jednym folderze nie zatrzymuje pozostałych
- **Wielowarstwowa detekcja zakończenia kopiowania** (`FileStabilityProbe`) — N kolejnych pomiarów rozmiaru + `LastWriteTimeUtc` + próba otwarcia z `FileShare.None`. Eliminuje race condition gdy kopiowanie 200 MB PDF przez SMB jeszcze trwa, a `FileSystemWatcher` już zgłosił `Created`. Tolerancja na Defendera trzymającego exclusive lock na 2-5 sekund po zakończeniu kopiowania
- **Polling fallback dla SMB/DFS** — gdy `FileSystemWatcher` kłamie na share'ach sieciowych, drugi mechanizm robi snapshot diff folderu co N sekund. Oba źródła aktywne równolegle, deduplikator usuwa duplikaty
- **Recovery po crashu** — gdy aplikacja padnie w trakcie eksportu, ledger SQLite trzyma rekord `reserved`; przy następnym starcie pliki w `.work` wracają do `input` i są przetwarzane od nowa. Gwarancja zero-loss
- **Ledger SQLite** w `%LocalAppData%\ImpositionApp\state\hotfolder.db` z trybem WAL — historia każdego joba (kto, kiedy, ile prób, czy sukces, output path). Auto-prune wpisów starszych niż 30 dni
- **Anty-duplikat oparty na fingerprincie** — SHA-256 z pierwszego 1 MB pliku + rozmiar + mtime. Re-export tego samego pliku po edycji = inny fingerprint = nowy job. Ten sam plik wrzucony dwa razy bez zmian = pominięty
- **Retry przez Polly v8** — exponential backoff z jitter, osobne polityki dla I/O (5×, 200ms base), eksportu (3×, 2s base) i ledgera (8×, 50ms — chroni przed `SQLITE_BUSY`). Deterministyczne błędy preflight nie są retry'owane
- **Globalny limit procesów Ghostscript** (`GhostscriptThrottle` z `SemaphoreSlim`) — bez tego N hotfolderów × M zadań × ~500 MB RAM gs zabijało maszynę. Domyślnie max 2 procesy gs jednocześnie, konfigurowalne
- **Walidacja licencji per job + okresowa** — host sprawdza licencję przy starcie i co 60 minut w tle. Każdy export waliduje licencję jeszcze raz tuż przed startem. Wygaśnięcie w trakcie 24/7 pracy zatrzyma runnery z czytelnym komunikatem

### Interfejs zarządzania

- **Nowa pozycja menu: Narzędzia → Hot foldery…** (*Tools → Hot Folders* w EN). Świadomie nie dodajemy przycisku do głównego paska — to funkcja produkcyjna, nie codzienna akcja obok *Eksportuj*
- **Okno managera** z listą skonfigurowanych hot folderów: nazwa, status (Bezczynny / Uruchamianie / Działa / Zatrzymany / Błąd), folder wejściowy/wyjściowy, szablon, licznik OK/błędów/kolejki. Auto-refresh co 1,5 sekundy przez `DispatcherTimer`
- **Akcje na liście**: Dodaj, Edytuj, Usuń, Start, Stop, Restart, Otwórz wejście, Otwórz wyjście, Otwórz logi (skrót do `%LocalAppData%\ImpositionApp\logs`)
- **Dialog edycji** z folder/file pickerami dla wszystkich ścieżek (input / output / archive / error / work / template / Ghostscript). Sekcje: Podstawowe, Foldery, Szablon i filtry, Działanie, Eksport. Walidacja na Save z banner-em zawierającym listę bullet-błędów (puste pola, ścieżki nieabsolutne, nieistniejący szablon, foldery nakładające się, brak gs przy włączonym PDF/X)
- **Banner ostatniego błędu** — gdy zaznaczony hot folder ma `LastError`, pojawia się czerwony pasek z treścią. Operator widzi co się stało bez nurkowania w logi
- **Pełna lokalizacja PL/EN** — wszystkie etykiety, statusy, komunikaty walidacji, opcje enum (*Po sukcesie: Nic / Archiwizuj / Usuń*) tłumaczone

### CLI

- **Komendy `hotfolder run/list/start/stop/reload`** — `imPRESS Studio.exe hotfolder run` uruchamia daemon w foreground (Ctrl+C zatrzymuje), gotowe pod uruchomienie jako Windows Service w przyszłej wersji. `hotfolder list` pokazuje status wszystkich folderów, `start`/`stop` przyjmują GUID hot folderu, `reload` wczytuje `hotfolders.json` ponownie i uzgadnia stan (start nowych, stop usuniętych, restart zmienionych)

### Konfiguracja i przechowywanie

- **Plik konfiguracji** `%APPDATA%\imPRESS Studio\hotfolders.json` — atomicznie zapisywany przez `.tmp` + `File.Move`. Edytowalny ręcznie, walidowany przy każdym wczytaniu. Złamana składnia rotuje plik do `.broken-{timestamp}` zamiast pożerać konfigurację
- **Walidator** wymusza absolutne ścieżki, niepustość pól wymaganych, istnienie szablonu na dysku i — krytyczne — odrzuca konfigurację gdzie `workPath`/`archivePath`/`errorPath`/`outputPath` leży *wewnątrz* `inputPath` (klasyczny błąd początkujących powodujący nieskończoną pętlę: aplikacja przerabia własne wyjścia)
- **Long path support** — manifest aplikacji deklaruje `longPathAware=true` i `perMonitorV2` DPI awareness. Hot foldery na głębokich mapowanych dyskach nie są już ograniczone do 260 znaków

### Architektura i zależności

- Nowy moduł `Imposition.HotFolder` — Configuration / Watching / Queueing / State / Processing / Hosting / Diagnostics / DependencyInjection. Zero powiązań z UI, w pełni używalne z CLI, gotowe pod future REST API / Windows Service
- Dodane pakiety NuGet: `Microsoft.Data.Sqlite 8.0.10` (ledger), `Polly` + `Polly.Core 8.4.2` (retry/backoff), `Microsoft.Extensions.Hosting 8.0.0` (`IHostedService` dla cyklu życia hosta)
- `HotFolderHost` implementuje `IHostedService` — gotowy pod hostowanie w generic host. Startowany z `Program.Main` przed Avalonią (GUI) lub jako foreground daemon (CLI). Drugi tryb jest fundamentem pod uruchomienie jako Windows Service w przyszłej wersji
- Wersja aplikacji `1.0.8 → 1.1.0` (minor bump zgodnie z SemVer — nowa, kompatybilna funkcjonalność)

### Odporność produkcyjna

- **Atomowa state machine plików**: `input → .work/{jobId}__file.pdf → output + archive (lub error)`. `File.Move` na NTFS atomowy w obrębie woluminu; po przeniesieniu do `.work` plik jest niewidzialny dla `FileSystemWatcher`, co eliminuje duplikaty z innych instancji aplikacji
- **Backoff exponential z jitter** dla wszystkich operacji I/O — nie zalewamy NAS-a powtórzonymi requestami w czasie krótkiej awarii sieci
- **Top-level try/catch w każdej pętli `Task.Run`** — żaden wyjątek z producenta/konsumenta/poller'a/recovery loopu nie wykończy procesu w ciszy
- **Recreate watchera po `InternalBufferOverflowException`** + automatyczny full rescan folderu — bez tego po overflow gubilibyśmy eventy dla brakujących plików na zawsze. Dodatkowy auto-recovery loop po dowolnym `FSW.Error` z exponential backoff
- **Channel z bounded capacity i `FullMode=Wait`** dla kolejki jobów — jobów nie tracimy, ale producent czeka asynchronicznie gdy konsument utknie. Pod skrajnym DDoS-em (10000 plików nagle) nie wybuchamy z OOM

### Uwagi przy aktualizacji

- **Bez breaking changes** — istniejący workflow (Eksportuj, CLI `impose`, szablony, licencje) działa identycznie. Hot foldery to dodatek, włącza go się przez menu
- **Hot foldery wymagają ważnej licencji** — host nie wystartuje runnerów gdy licencja jest niepoprawna lub wygasła. To samo dotyczy `hotfolder run` z CLI
- Pierwsze uruchomienie tworzy `%APPDATA%\imPRESS Studio\hotfolders.json` oraz `%LocalAppData%\ImpositionApp\state\hotfolder.db` przy pierwszym save w UI — pliki nie powstają same z siebie przy update

## 1.0.8 — 26 kwietnia 2026 (Stabilna)

*„Color & Clock” — zarządzanie ICC + ochrona przed cofnięciem zegara*

### Nowe funkcje

- **Pełne zarządzanie profilami ICC w oknie eksportu** — pole tekstowe zastąpione listą rozwijaną z auto-skanowaniem folderu `icc/` w katalogu instalacji. Pozycja *(brak)* dla eksportu bez konwersji. Przycisk *Wczytaj…* otwiera systemowy file picker (filtr `*.icc / *.icm`), wybrany profil jest kopiowany do `icc/` i automatycznie wybrany. Przycisk *Folder* otwiera katalog w Eksploratorze do ręcznych dodatków. Pod listą podpowiedź pokazuje liczbę dostępnych profili i ścieżkę
- **Ochrona licencji przed cofnięciem zegara** — nowy `LicenseClockGuard` przy każdej udanej walidacji zapisuje HMAC-podpisany znacznik czasu w `%LocalAppData%\imPRESS Studio\license.stamp`. Klucz HMAC pochodzi z podpisu licencji (więc znacznik jest licencja-specific i nie da się go skopiować z innej maszyny). Przy następnej walidacji jeśli zegar systemowy cofnął się o więcej niż 2h przed zapisaną wartość — eksport zablokowany z komunikatem *„Wykryto cofnięcie zegara systemowego”*. Tolerancja 2h na driftu NTP / DST. Edycja pliku `.stamp` wykrywana przez weryfikację HMAC i traktowana jako tampering

### Zmiany

- **Email kontaktowy** w oknie *O programie* zaktualizowany na `impress_studio@proton.me` (poprzednio `wojciech.bujacz@hotmail.com`)

## 1.0.7 — 26 kwietnia 2026 (Stabilna)

*„License Hardening” — szczelny gate eksportu, nowy fingerprint i tolerancja sprzętowa*

### Krytyczne poprawki

- **Eksport bez licencji — załatany** — kliknięcie *Eksportuj* bez ważnej licencji generowało plik PDF mimo wszystko. Teraz `MainWindowViewModel.ExportAsync` twardo waliduje licencję *z dysku* na każde wywołanie (nie tylko cached `App.LicenseStatus` z momentu startu) — zamyka okno, w którym ktoś usuwał plik mid-session albo licencja wygasała w trakcie pracy. Identyczny gate w trybie CLI (`impose`): exit code 2 + komunikat z hintem do `license activate`

### Nowy fingerprint sprzętowy

- **Hostname usunięty** — nazwa komputera była nietrwałym identyfikatorem (zmiana 5-sekundowa, świetnie obchodzona). Już nie ma żadnego udziału w fingerprint
- **3 niezależne komponenty**: SHA-256 z (a) MAC adresu pierwszego fizycznego NIC z filtrem na virtuals (VMware/Hyper-V/Bluetooth/loopback są pomijane), (b) numeru seryjnego dysku systemowego (`InterfaceType ≠ USB AND MediaType LIKE 'Fixed%'` — pendrive się nie liczy), (c) UUID systemowego z SMBIOS (`Win32_ComputerSystemProduct`)
- **Tolerancja 2 z 3** — licencja waliduje się gdy ≥ 2 z 3 komponentów się zgadza. Wymiana karty sieciowej, dysku albo czyszczenie BIOS-a — pojedyncza zmiana nie wywala licencji. Dwie zmiany jednocześnie (`1/3`) → komunikat *„licencja jest przypisana do innego komputera (1/3 elementow zgodnych — wymagana ponowna aktywacja)"*
- **Backward compat** — stare licencje pre-1.0.7 z pojedynczym `MachineHash` nadal walidują się normalnie przez fallback na composite. Nikt nie traci dostępu

### Narzędzia wydawcy

- **Pole *Identyfikator komputera* w oknie aktywacji** pokazuje teraz string w formacie `MAC=...|DISK=...|UUID=...` — klient kopiuje całość i wysyła wydawcy
- **Skrypt PowerShell `tools/Issue-License.ps1`** — wrapper na CLI. Wydawca (Ty) wpisuje:

  ```
  .\tools\Issue-License.ps1 -Name "Drukarnia ABC" -Email "biuro@abc.pl" `
      -Edition Standard -Days 365 `
      -Fingerprint "MAC=A1B2...|DISK=C3D4...|UUID=E5F6..."
  ```

  Skrypt: lokalizuje exe, w razie braku auto-generuje parę kluczy RSA, wywołuje `license issue`, parsuje activation key z output i zapisuje do `issued/license_<slug>.key.txt` gotowy do maila. Ostrzega gdy < 2 komponentów (brak budżetu na tolerancję)
- CLI `license fingerprint` pokazuje teraz wszystkie 3 hashy + jednolinijkowy format do polecenia `issue`; `license issue` akceptuje `--mac`/`--disk`/`--uuid` (zalecane) lub `--machine` (legacy strict)

### Uwagi dla wydawcy

- Po pierwszym `license keygen` wklej zawartość `license_public.xml` do `LicenseValidator.PublicKeyXml` i przebuduj. Klucz prywatny **nigdy** nie idzie z aplikacją

## 1.0.6 — 26 kwietnia 2026 (Stabilna)

*„Progressive Zoom” — natychmiastowy podgląd przy powiększaniu*

### Wydajność podglądu

- **Progressive load (jak Google Maps)** — gdy użytkownik powiększa stronę i nowa skala jest jeszcze rasteryzowana w tle, canvas natychmiast pokazuje najlepszy wcześniej zcache'owany bitmap (przeskalowany do nowego rozmiaru). Wynik jest chwilowo miękki, ale widoczny od razu — ostry render dochodzi po 0,5–2s zamiast pustego placeholdera
- **Coarsene scale buckets** — zamiast 19 poziomów rozdzielczości (skok co 0,25) tylko 5 dyskretnych poziomów: `1.5 / 2.0 / 3.0 / 4.5 / 6.0×`. Wiele kroków zoomu wpada w ten sam bucket, znacznie redukując liczbę pełnych re-rasteryzacji PDF-a
- **Cancel-na-zoom-spam** — gdy użytkownik szybko zoomuje 1×→2×→3×→4×, stare requesty kończą się w tle ale nie odświeżają już canvasa. Bez tego każda kropla scrolla wywoływała pełny repaint na pośrednim, już nieaktualnym poziomie
- **Filter cache po referencji bitmapy** — cache filtra kanałów CMYK + overprint trzyma teraz klucz na *referencji* źródłowego bitmapu, nie indeksie strony. Eliminuje subtelny stale-bitmap bug przy zmianie skali z aktywnym filtrem

### Uwagi techniczne

- PDFium/Docnet rasteryzuje per-stronę na CPU — to jest faktyczne wąskie gardło (przy A4 6× to ~18 megapikseli per strona). Hardware nie pomoże, dlatego optymalizacja wchodzi w warstwę pipeline'u rasteryzacji i schedulowania, nie renderingu Skii (która już używa GPU pod spodem). Plik 100 MB i tak będzie wolniejszy niż 11 MB — ale teraz nigdy nie zobaczysz pustego ekranu, tylko płynne dochodzenie do ostrości

## 1.0.5 — 26 kwietnia 2026 (Stabilna)

*„Ink Inspector” — separacja kanałów CMYK + próbnik koloru*

### Nowe funkcje

- **Toggle kanałów CMYK** — cztery checkboxy w pasku opcji podglądu (*C / M / Y / K*). Wyłączenie któregokolwiek zdejmuje wybrany ink z arkusza, pozwalając obejrzeć separację. Implementacja przez konwersję RGB→CMYK→RGB per pixel z zerowaniem wybranego kanału. Wynik cachowany per (strona, maska kanałów, overprint), więc przełączanie jest natychmiastowe
- **Próbnik koloru (eyedropper)** — pływający panel w prawym dolnym rogu canvasa pokazuje pod kursorem: kwadrat z kolorem, wartości `R G B` (0-255) oraz `C M Y K` (0-100%). Pomiar bierze piksel z surowego renderu, niezależnie od aktywnego filtra kanałów

### Zmiany

- Pasek opcji podglądu poszerzony o sekcję *Kanały:* z separatorem od dotychczasowych togglów (Spread / Marks / Creep / Overprint)
- Litery C/M/Y/K w checkboxach kolorowane zgodnie z kanałem (cyan/magenta/yellow/black) — wizualna mnemonika

### Uwagi

- **Symulacja, nie prawdziwa separacja** — Docnet/PDFium daje composite RGB, nie ma sposobu odzyskać oryginalne plates bez ponownego przepuszczenia PDF-a przez Ghostscript z urządzeniem `tiffsep`. Filtr per-pixel daje plausible „pokaż mi tylko cyjan" wystarczająco wiernie do proofingu wzrokowego — do akceptacji prepress użyj eksportu PDF/X i zewnętrznego RIP-a

## 1.0.4 — 26 kwietnia 2026 (Stabilna)

*„Adaptive Preview” — szybszy podgląd dużych PDF-ów*

### Nowe funkcje

- **Równoległa rasteryzacja stron** — silnik podglądu (`PdfPreviewRenderer`) używa puli wątków ograniczonej do `Math.Min(4, ProcessorCount-1)` przez `SemaphoreSlim`. Wcześniej każda strona renderowała się sekwencyjnie; teraz na 4-rdzeniowym CPU 4 strony liczą się jednocześnie
- **Pre-rendering sąsiednich arkuszy** — przy zmianie aktualnego arkusza w tle ładują się strony arkuszy `−1`, `+1`, `+2`. Nawigacja strzałkami / kliknięciem miniatury jest natychmiastowa zamiast czekania ~sekunda na każdy nowy arkusz
- **Kwantyzacja zoomu (cache buckets)** — skala renderu zaokrąglana do siatki 0,25× i ograniczana do max 6×. Zoom 1,01× → 1,02× nie wywołuje już ponownej rasteryzacji, bo trafia w ten sam bucket cache
- **LRU cache z limitem 80 bitmap** — przy przekroczeniu limitu pamięci cache wycina 10% najstarszych wpisów. Wcześniej cache rósł nieograniczenie i zjadał kilkaset MB przy długich sesjach

### Zmiany

- Avalonia 11 / Skia już używa GPU (Direct3D 11 / OpenGL na Windows) — nie wprowadzono dodatkowej warstwy OpenGL, bo wąskim gardłem nie był rendering canvasa, lecz CPU-bound rasteryzacja PDF przez Docnet/PDFium. Optymalizacje skupione tam, gdzie liczy się czas
- `RenderAndCache` nie blokuje już Task.Run wątku w nieskończoność — semafor gwarantuje, że szybki tryb prefetch nie zagłodzi UI

### Poprawki

- **Strict preflight + dialog raportu** — gdy dostępny jest interaktywny dialog raportu (tryb UI), `StrictPreflight` nie wyrzuca już wyjątku przed pokazaniem raportu. Użytkownik widzi listę 5 ostrzeżeń i sam decyduje. CLI bez callbacka nadal honoruje strict
- **Race condition w PreflightReportDialog** — dialog tworzony jest teraz przez `Dispatcher.UIThread.InvokeAsync` (pipeline wywołuje callback z wątku roboczego)
- **installer.iss** — guard `#if !DirExists()` z czytelnym komunikatem zamiast cryptycznego „No files found" gdy katalog `publish/` nie istnieje

## 1.0.3 — 26 kwietnia 2026 (Stabilna)

*„Print-Ready” — PDF/X-1a, raport preflight i numeracja w podglądzie*

### Nowe funkcje

- **Eksport PDF/X-1a:2001** — obok PDF/X-4 dostępny jest teraz starszy standard *PDF/X-1a* wymagany przez wiele drukarni offsetowych. PDF 1.4, tylko CMYK, bez przezroczystości. Wybór standardu z listy rozwijanej w oknie eksportu (Brak / X-1a / X-4)
- **Raport preflight przed eksportem** — gdy walidacja źródłowego PDF-u znajdzie ostrzeżenia (RGB-only zamiast CMYK, brakujący profil ICC, transparencja w trybie X-1a, zaszyfrowany plik, niezgodny rozmiar trim, PDF wyższej wersji niż docelowy standard), aplikacja pokazuje czytelny dialog z listą znalezisk pogrupowanych po wadze (błąd / ostrzeżenie / info). Użytkownik może kontynuować lub anulować
- **Numerowanie arkuszy widoczne na podglądzie** — wcześniej numer arkusza był stemplowany dopiero w eksportowanym PDF. Teraz nakładka pojawia się też na *ImpositionCanvas* w czasie rzeczywistym — gdy zmieniasz pozycję lub format w oknie eksportu, podgląd aktualizuje się natychmiast. Renderowanie w pełni wektorowe (FormattedText), bez wpływu na FPS przy zoomie
- **Rozszerzona walidacja preflight** — nowe reguły: `RGB_ONLY` (źródło tylko RGB), `NO_ICC` (obrazy bez ICC), `ENCRYPTED` (PDF zaszyfrowany), `X1A_TRANSPARENCY`, `X1A_OCG`, `X1A_VERSION`, `X4_VERSION`

### Zmiany

- Pole *Konwertuj do PDF/X-4* w oknie eksportu zastąpione listą rozwijaną *Standard PDF/X*
- `ExportOptions.ConvertToPdfX4` zachowane jako shim dla kompatybilności — pod spodem ustawia `PdfXMode`
- Argumenty Ghostscript dobierane automatycznie pod docelowy standard (`CompatibilityLevel=1.4` dla X-1a, `1.6` dla X-4)
- `ExportPipeline.RunAsync` przyjmuje opcjonalny callback *confirmAfterPreflight* — UI pokazuje raport, CLI pomija dialog

## 1.0.2 — 26 kwietnia 2026 (Stabilna)

*„Numbering & Persistence” — numerowanie arkuszy i zapamiętywanie opcji eksportu*

### Nowe funkcje

- **Numerowanie arkuszy** — opcja w oknie eksportu stempluje numer arkusza na każdym wyjściowym arkuszu. Konfigurowalne: pozycja (6 wariantów: rogi i środki góra/dół), format (np. `{0} / {1}`, `Arkusz {0}`), domyślnie *Prawy dolny*
- **Wybierak pliku Ghostscript** — przycisk `...` obok pola ścieżki otwiera systemowy dialog z filtrem na `gswin64c.exe` / `gswin32c.exe`
- **Instalator Inno Setup** — pełny kreator z wyborem katalogu instalacji, folderu w menu Start, opcjonalnym skrótem na pulpicie. Czysta deinstalacja przez Panel Sterowania
- **Auto-aktualizacja przez GitHub Releases** — przy starcie aplikacja sprawdza najnowszy tag na `dnblsr-lab/imPRESS-Studio` i oferuje pobranie nowszego instalatora

### Poprawki

- **Trwałość opcji eksportu** — ścieżka do Ghostscripta, profil ICC, ustawienia preflight i zakres arkuszy są teraz zapisywane w `%APPDATA%\imPRESS Studio\settings.json` i przywracane przy następnym otwarciu okna eksportu (wcześniej każde otwarcie czyściło pola)

### Zmiany

- Wersjonowanie zgodne z SemVer — z `1.01` na `1.0.2` (poprzednie `1.01` traktowane jako `1.0.1`)
- Velopack zastąpiony przez Inno Setup — instalator daje pełną kontrolę użytkownikowi nad miejscem instalacji i skrótami
- Okno eksportu rozszerzone o sekcję *Numerowanie stron* + *ScrollViewer* dla wygody przy mniejszych rozdzielczościach

## 1.01 — 19 kwietnia 2026 (Stabilna)

*„Polish Pass” — menu, ustawienia i gang-run*

### Nowe funkcje

- **Pasek menu aplikacji** — *Plik*, *Edycja*, *Widok*, *Narzędzia*, *Ustawienia*, *Pomoc* ze skrótami klawiaturowymi widocznymi w pozycjach menu
- **Okno ustawień aplikacji** — wybór języka interfejsu (polski / angielski), konfiguracja zapisywana w `%APPDATA%\imPRESS Studio\settings.json`
- **Infrastruktura tłumaczeń (i18n)** — słowniki PL/EN, markup `{loc:Loc Key}` w XAML, serwis `LocalizationService` z powiadamianiem o zmianie języka
- **Szablony gang-run 3+** — *DL Ulotka 3-up* (99×210, 3×1 na SRA3), *Wizytówki 6-up* (90×50, 3×2), *Wizytówki 8-up* (85×55, 4×2), *Wizytówki 12-up* (85×55, 4×3) z walidacją mieszczenia na arkuszu

### Zmiany

- Przycisk „O programie” przeniesiony z nagłówka do menu *Pomoc* — czystszy layout paska tytułowego
- Wybór języka usunięty z okna „O programie” i przeniesiony do nowego okna *Ustawienia*
- Wersja aplikacji podniesiona z 1.00 na 1.01 (okno „O programie” oraz zasoby)
- Pełny przegląd tekstów w UI — uzupełnione polskie znaki diakrytyczne w komunikatach statusowych, tooltipach i oknach dialogowych

## 1.00 — 18 kwietnia 2026 (Stabilna)

*„First Impress” — pierwsza oficjalna wersja produkcyjna*

### Nowe funkcje

- **System licencjonowania** — aktywacja kluczem Base64 lub plikiem `.json`, powiązanie z hashem sprzętu, obsługa edycji Trial / Standard / Professional, wygasanie z 14-dniowym ostrzeżeniem
- **Okno aktywacji licencji** — pokazuje identyfikator maszyny z przyciskiem kopiowania, pole na klucz aktywacyjny, opcja wczytania pliku licencji
- **Ikona aplikacji** — nowa ikona programu (`ikona.ico`) i logo (`logo.png`) osadzone w exe i oknach dialogowych
- **CLI licencyjne** — komendy `license keygen`, `license fingerprint`, `license issue`, `license activate`, `license info`
- **Panel informacji o pliku PDF** — dialog z ikoną „i”, pełne metadane (wymiary, trim, czcionki, kolory, przezroczystości, szyfrowanie, OCG, tagowanie)
- **Rebranding** — zmiana nazwy z „Imposition Studio” na „imPRESS Studio” we wszystkich oknach, pliku wykonywalnym i CLI
- **Rozszerzony lewy panel** — z 320 do 380 px dla lepszej czytelności kart i etykiet

### Zmiany

- Plik wykonywalny przemianowany z `ImpositionApp.exe` na `imPRESS Studio.exe`
- Wszystkie okna dialogowe otrzymały spójną ikonę w tytule
- Przeorganizowany układ okna „O programie” z nowym logo

### Bezpieczeństwo

- Weryfikacja licencji podpisem RSA-2048 — klucz prywatny nigdy nie trafia do aplikacji
- Hash sprzętu SHA-256 z numeru seryjnego płyty głównej, dysku i nazwy komputera

## 0.9 — 10 kwietnia 2026 (Beta)

*„Release Candidate” — ostatnie testy przed wydaniem 1.0*

### Nowe funkcje

- **Multi-file loader** — obsługa wielu plików PDF jednocześnie z checkboxami do selektywnego wczytania
- **Scalanie plików źródłowych** — zaznaczenie kilku plików łączy je w jeden dokument przed impozycją
- **Zmiana kolejności plików** — strzałki ▲▼ do układania listy przed scaleniem
- **Info o pliku** — okno z pełnymi metadanymi wybranego PDF-a
- **Reset all** — przycisk czyszczący projekt do ustawień początkowych

### Zmiany

- Przebudowany panel plików źródłowych — lista zastąpiła pojedynczy wybór
- Przycisk „Wczytaj zaznaczone” zamiast „Wczytaj plik”

### Poprawki

- Naprawiono zawieszenie przy próbie wczytania zaszyfrowanego PDF-a
- Poprawiono wyświetlanie nazwy szablonu w pasku tytułu

## 0.8 — 28 marca 2026 (Beta)

*„Polish & Stats” — dopracowanie UI i panel statystyk*

### Nowe funkcje

- **Panel statystyk** — powierzchnia papieru, wykorzystanie, odpad, szacowanie kosztów, waga
- **NumericUpDown** dla ceny za arkusz i gramatury papieru — kalkulacje w czasie rzeczywistym
- **Progress bary** dla wykorzystania papieru i odpadu
- **Karta informacji o oprawie** — typ, stron na sygnaturę, spad, rynienka
- **Pasek miniatur arkuszy** — szybka nawigacja na dole okna podglądu

### Zmiany

- Przebudowane menu nagłówka — przyciski *Szablon*, *Opcje eksportu*, *Pomoc*, *Licencja*, *O programie*
- Wyraźne rozdzielenie trzech paneli kolorami i liniami

## 0.7 — 15 marca 2026 (Beta)

*„Preview Master” — interaktywny podgląd arkuszy*

### Nowe funkcje

- **Interaktywny podgląd** na `ImpositionCanvas` — rendering stron na arkuszu z pełną precyzją drukarską
- **Zoom względem kursora** — przybliżenie kółkiem myszy skupia się na wskazywanym miejscu
- **Pan (przesuwanie)** — przeciąganie myszą po arkuszu
- **Obrót widoku** o 90° (klawisz `R`)
- **Tryb rozkładówki** — przód i tył arkusza obok siebie
- **Wizualizacja pełzania (creep)** — pomarańczowe linie przerywane
- **Symulacja nadruku** — multiply blend dla ciemnienia CMYK
- **Skróty klawiaturowe** — `Ctrl+P`, `Ctrl+E`, `Ctrl+O`, `Ctrl+0`, `F`, `R`, `←`/`→`

## 0.6 — 25 lutego 2026 (Beta)

*„Print Marks & Creep”*

### Nowe funkcje

- **Znaczniki drukarskie** — linie cięcia, pasery (registration marks), paski kolorów CMYK, znaczniki bigowania
- **Kompensacja pełzania** dla oprawy zeszytowej — z konfiguracją grubości papieru
- **Opcje falcowania** — Z-fold i harmonijka z konfigurowalną liczbą paneli
- **Własne rozmiary papieru** — dodawanie rozmiarów poza listą predefiniowaną

### Zmiany

- Rozszerzona obsługa marginesów: spad, strefa bezpieczna, rynienka, grzbiet, margines arkusza

## 0.5 — 10 lutego 2026 (Beta)

*„PDF/X-4 & Ghostscript”*

### Nowe funkcje

- **Konwersja do PDF/X-4** przez Ghostscript — standard ISO 15930-7
- **Obsługa profili ICC** — ISOcoated\_v2\_eci i inne dostarczane w folderze `icc/`
- **Ścisły preflight** — ostrzeżenia przerywają eksport
- **Zakres arkuszy** w eksporcie — `1-5`, `2,4,6`, kombinacje
- **Tryb CLI** — komenda `impose` do zadań wsadowych

## 0.4 — 20 stycznia 2026 (Beta)

*„Template Manager”*

### Nowe funkcje

- **Menedżer szablonów** z pełną edycją parametrów
- **Import/eksport szablonów JSON** — wymiana między stanowiskami
- **Gotowe szablony** dla typowych publikacji (broszura A5, ulotka A4, wizytówki)
- **Walidacja szablonu** przed uruchomieniem impozycji

## 0.3 — 8 stycznia 2026 (Beta)

*„Binding Types”*

### Nowe funkcje

- **Oprawa klejona** (perfect bound) z obsługą grzbietu
- **Falcowanie** — wstępna implementacja Z-fold
- **Sygnatury** — konfigurowalne 4/8/16 stron na składkę

## 0.2 — 18 grudnia 2025 (Beta)

*„Saddle Stitch MVP”*

### Nowe funkcje

- **Pierwsza działająca impozycja** — oprawa zeszytowa z układem 2×1
- **Eksport PDF** przez PdfSharpCore
- **Drag & drop** plików PDF na okno aplikacji
- **Pasek statusu** z informacją o stanie operacji

## 0.1 — 1 grudnia 2025 (Alpha)

*„Genesis” — szkielet aplikacji*

### Nowe funkcje

- **Podstawowa architektura** — Avalonia 11 + .NET 8 + MVVM
- **Podział na projekty**: `Imposition.Core`, `Imposition.Pdf`, `Imposition.Export`, `Imposition.Ui`, `Imposition.Utils`, `Imposition.App`
- **Wczytywanie PDF-ów** przez PdfPig — analiza stron, czcionek, metadanych
- **Kontener DI** oparty na Microsoft.Extensions.DependencyInjection
- **Logowanie** Serilog z plikowym i konsolowym sinkiem

## Plany rozwoju — 2026+

*Funkcje planowane w przyszłych wersjach*

### Na roadmapie

- **Wersja Windows Service** dla hot folderów — daemon poza kontekstem zalogowanego użytkownika, autostart przy boot, zarządzanie przez SCM. Obecnie hot folder host można uruchomić z CLI (`imPRESS Studio.exe hotfolder run`) jako foreground proces — Windows Service wrapper to naturalne rozszerzenie
- **REST API + webhooki** dla hot folderów — POST `/jobs` z PDF + JSON config, webhook on completion, integracja z MIS
- **Integracja JDF/JMF** — komunikacja z systemami MIS/ERP, parser job ticketów dla hot folderów
- Wersja **macOS** i **Linux** (Avalonia jest cross-platform)
- **Silnik preview OpenGL** dla szybszego renderu dużych arkuszy
- **Kolejne języki UI** — niemiecki i czeski (PL/EN już dostępne)
- **Więcej szablonów gang-run** — etykiety, naklejki, karty lojalnościowe
- **Multi-template per hot folder** z regex routingiem (np. `A4_*.pdf → 4-up`, `A3_*.pdf → 8-up`)
