# NAGENTS-RESEARCH.md — pakiet rozeznania i źródeł

STATUS: `STAGING_ONLY`
DOMAIN: `INFORMACYJNY`
TOPIC: `NAG-CONSOLIDATE-RESEARCH-Q1`
TASK_ID: `t_75595cef`
P4_STATUS: `PASS_WITH_EXPLICIT_OWNER_GATES`
GENERATED_AT_UTC: `2026-09-14T20:28:42Z`
SOURCE_OF_TRUTH: `false`
CURRENTNESS: `not_live_state`

Ten plik jest pakietem roboczym do niezależnej oceny. Nie jest bieżącym routingiem, rejestrem decyzji, opisem uruchomionego runtime ani zgodą na integrację/deployment. Stare tezy pozostają oznaczone datą i statusem; samo przeniesienie ich do tego pliku nie awansuje ich do live state.

## 0. Jak czytać pakiet

Każda teza ma jawne pola `TYPE`, `DATE/RANGE`, `RANK`, `SOURCE_STATUS`, `STATE/CURRENTNESS`, `SOURCE_PATH` i `LOCATOR`.

- `SOURCE` — fakt lub deklaracja zachowana w źródle/snapshotcie; nie oznacza, że fakt jest aktualny.
- `ANALYSIS` — wniosek operatora z oznaczonych źródeł; nie jest decyzją.
- `DECISION` — wyłącznie wskaźnik do decyzji kanonicznej albo jawna bramka właściciela; ten pakiet nie tworzy nowej decyzji.
- `HISTORY_ONLY` / `HISTORY` — materiał historyczny; nie zasila live state.
- `SOURCE_SNAPSHOT` / `LEGAL_SNAPSHOT` / `PRIMARY_TECHNICAL_SNAPSHOT` — dated capture; przed użyciem operacyjnym wymaga ponownego readbacku.
- `CANONICAL_POINTER` — odczyt ścieżki kanonicznej; treść decyzji pozostaje w `docs/spec/decisions.md` i protokole ECHO.
- `OWNER_GATE` — brak decyzji; potrzebna osobna zgoda właściciela.

Ranga nie zastępuje dowodu: `PRIMARY_LEGAL_SNAPSHOT` ma większą wagę dla zobowiązań prawnych niż marketing, ale nadal jest snapshotem; `HISTORY` opisuje przebieg myślenia; `PLAN_ONLY` opisuje kryterium pakietowania.

## 0A. Aktualizacja po decyzji D-014

Bieżący research platformowy jest zebrany w
[`docs/OPENCLAW-STRATEGY.md`](docs/OPENCLAW-STRATEGY.md). OpenClaw zastępuje
Hermesa jako wybrany runtime 8gent; OpenRouter i OpenMonitor nie są wybranymi
elementami architektury. Dotychczasowe materiały Hermes/LiteLLM pozostają
`HISTORY` lub `EVIDENCE` i mogą zasilać mapowanie oraz przyszły plugin AutoBot
Monitor, ale nie są bieżącym dowodem capability ani instalacji.

Nie wykonano jeszcze inventory wersji OpenClaw, testu Gatewaya, wyboru auth,
canary ani plugin spike. Te braki są jawne i pozostają `OWNER_GATE`/`GAP TO
VERIFY`, a nie `PASS`.

## 1. Macierz wymagań P6

| Sekcja | Zakres | Główne źródła | Wymóg proweniencji | Stan |
|---|---|---|---|---|
| `RESEARCH-01` | historyczne rozeznanie i warianty 8gent | NAG-RESEARCH, `docs/process/pamiec.md` | 11 families / 88 records, dated locators | `PASS` |
| `RESEARCH-02` | appto: funkcje, ceny, legal, wnioski | NAG-APPT0, `docs/nota-06`, `docs/nota-07`, `appto-*` | 8 families / 96 records, source vs analysis | `PASS` |
| `RESEARCH-03` | OAuth/Entra/Graph/Hermes snapshots | NAG-SOURCES, P4 | 5 / 5 exact records, hash/link/status | `PASS` |
| `RESEARCH-04` | korekty, lekcje, pewność i currentness | `pamiec.md`, dated notes, P4 | correction trail, no history→live promotion | `PASS` |
| `RESEARCH-05` | granice pakietu i routing dalszej pracy | P6, P4, decisions/ECHO | no decision/integration/deployment side effect | `PASS` |

P6 locator: `NAGENTS-CONSOLIDATION-PLAN.md`, §4.5, lines 140–148. P4 locator: `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json`, `observed_at_utc` z metadanych P4.

## 2. P4 rollup i zakres proweniencji

| Grupa | P4 status | Path families | Records | P4 source of truth | Target package |
|---|---|---:|---:|---|---|
| NAG-RESEARCH | HISTORY | 11 | 88 | docs/process/pamiec.md oraz właściwe źródła pierwotne; nie jest źródłem bieżącego routingu | NAGENTS-RESEARCH.md; decyzje tylko po wpisie w decisions/ECHO |
| NAG-APPT0 | SOURCE | 8 | 96 | P3 kat. 13/history + jawne źródła appto w docs/process/zrodla/ | NAGENTS-RESEARCH.md; ewentualne integracje tylko po decyzji |
| NAG-SOURCES | SOURCE | 5 | 5 | źródła pierwotne z datą, nie bieżący routing 8gent | NAGENTS-INTEGRATIONS.md / NAGENTS-RESEARCH.md jako cytowane odnośniki |

Wymagane liczniki zostały zachowane bez deduplikacji: `NAG-RESEARCH = 11/88`, `NAG-APPT0 = 8/96`, `NAG-SOURCES = 5/5`. Rekord P4 pozostaje jednostką proweniencji; wspólny hash z wielu checkoutów nie jest powodem do usunięcia rekordów.

### 2.1 Ledger path-family: hash, data, status, link

| PF | Grupa | Ścieżka | P4 status | Records | Preferred SHA-256 | Data/range | Readback i link |
|---|---|---|---|---:|---|---|---|
| PF-0181 | NAG-RESEARCH | `LLM-OPEN-SOURCE-nAgents-HANDOFF.md` | HISTORY | 1 | `6b5618d914799aa20daa0a411785908099f691ada8c8fb2a5e59d3fbdc9e81b5` | UNKNOWN_IN_ALLOWED_SCOPE | P4_ONLY_NO_RAW_COPY; [link](../../audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json#PF-0181) |
| PF-0193 | NAG-RESEARCH | `SERWERY-nAgents-ustalenia.md` | HISTORY | 1 | `8f83d7c80680d29260a2e2dd8befc5fdb32ff10780e44dbb1f459b59d381c7a0` | UNKNOWN_IN_ALLOWED_SCOPE | P4_ONLY_NO_RAW_COPY; [link](../../audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json#PF-0193) |
| PF-0196 | NAG-RESEARCH | `WLASNY-LLM-nAgents-porownanie.md` | HISTORY | 1 | `89671ba45a012296cf22a196533bc12be09f5082a3fb7bcfae938b5d9f47f5d2` | UNKNOWN_IN_ALLOWED_SCOPE | P4_ONLY_NO_RAW_COPY; [link](../../audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json#PF-0196) |
| PF-0197 | NAG-RESEARCH | `ZALACZNIK-B-serwer-OVH.md` | HISTORY | 1 | `fb8c7e6bea168d1583fb2ac08a40e04f4fe1aa96fd331d8c5a7602311b3ba6f7` | UNKNOWN_IN_ALLOWED_SCOPE | P4_ONLY_NO_RAW_COPY; [link](../../audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json#PF-0197) |
| PF-0198 | NAG-RESEARCH | `docs/nota-01-trzy-drogi-do-agenta.md` | HISTORY | 12 | `0927f3dc6168ef3a15fb5b805c0dfa6d02ab0f3b2d1127bd74b1bc9e09962461` | 2026-08-22 | LOCAL_READBACK; [link](../../../nota-01-trzy-drogi-do-agenta.md) |
| PF-0199 | NAG-RESEARCH | `docs/nota-02-uprzaz-dla-agentow.md` | HISTORY | 12 | `9af3e69b558f18b1c61551a264e6137d30811fb6512ab43f03d7adb488824d28` | 2026-08-22 | LOCAL_READBACK; [link](../../../nota-02-uprzaz-dla-agentow.md) |
| PF-0200 | NAG-RESEARCH | `docs/nota-02a-aneks-profile-i-zakres-v1.md` | HISTORY | 12 | `62d4e8311ec5017ff82abd571f4aef8c6eff89e842cf9fee410ed629d1bbc64a` | 2026-08-22 | LOCAL_READBACK; [link](../../../nota-02a-aneks-profile-i-zakres-v1.md) |
| PF-0201 | NAG-RESEARCH | `docs/nota-03-topologia-27-agentow.md` | HISTORY | 12 | `d640062145f814e6bbc562cbad4f20718a4255fc2b76e1bae293884c7ae2f56e` | 2026-08-22 | LOCAL_READBACK; [link](../../../nota-03-topologia-27-agentow.md) |
| PF-0202 | NAG-RESEARCH | `docs/nota-04-korekty-i-nowe-materialy.md` | HISTORY | 12 | `c64e83b0f41b0758d6f0cbb2dfda3b794ef0520bf2631d59cef5793b340142c2` | 2026-08-22 | LOCAL_READBACK; [link](../../../nota-04-korekty-i-nowe-materialy.md) |
| PF-0203 | NAG-RESEARCH | `docs/nota-05-kupic-czy-zbudowac.md` | HISTORY | 12 | `a9745f9eab8b7f0f73bcfb6eb14dc6dc82d0291786adc4cef4e8236cbb4b4f81` | 2026-08-22 | LOCAL_READBACK; [link](../../../nota-05-kupic-czy-zbudowac.md) |
| PF-0206 | NAG-RESEARCH | `docs/nota-08-wybory-otwarte.md` | HISTORY | 12 | `b05ce062506e6ac5d50309c504a6bf0a32f5257e925c1b78ad27d3ccb86625d9` | 2026-08-25 | LOCAL_READBACK; [link](../../../nota-08-wybory-otwarte.md) |
| PF-0204 | NAG-APPT0 | `docs/nota-06-appto-research.md` | SOURCE | 12 | `19eab8b23a2a1045c268180af2fec877296537cfdaf9f3b42c323612ddb56d6a` | 2026-08-25 | LOCAL_READBACK; [link](../../../nota-06-appto-research.md) |
| PF-0205 | NAG-APPT0 | `docs/nota-07-katalog-funkcji.md` | SOURCE | 12 | `8925a223f6e207668f2ba1c6c1334ccc1c18c9b2834f4ff437c04fe16ae3467c` | 2026-08-25 | LOCAL_READBACK; [link](../../../nota-07-katalog-funkcji.md) |
| PF-0228 | NAG-APPT0 | `docs/process/zrodla/appto-cennik-pl.md` | SOURCE | 12 | `6d3f67d1cc180b13a05e66e119192ecdebc930b171a2c537f8805ab53ceda8c7` | 2026-08-25 | LOCAL_READBACK; <https://www.appto.ai/pl/cennik/> |
| PF-0229 | NAG-APPT0 | `docs/process/zrodla/appto-integracje-pl.md` | SOURCE | 12 | `d65d393a1480eb0c6e35bf67e0a56a42a3bd8c1006549789bfbc5736487fb254` | 2026-08-25 | LOCAL_READBACK; <https://www.appto.ai/pl/integracje/> |
| PF-0230 | NAG-APPT0 | `docs/process/zrodla/appto-polityka-prywatnosci-pl.md` | SOURCE | 12 | `ca04a6ae3e197367bb15cef2fff66bdf1ec95daef8b1c6ebe991edf56f8bff6b` | 2026-08-25 | LOCAL_READBACK; <https://www.appto.ai/pl/polityka-prywatnosci/> |
| PF-0231 | NAG-APPT0 | `docs/process/zrodla/appto-regulamin-pl.md` | SOURCE | 12 | `7e3e5ff77847701d11c8e57e6a81b38e3de061a5ce39a27388148f2590a0b160` | 2026-08-25 | LOCAL_READBACK; <https://www.appto.ai/pl/regulamin/> |
| PF-0232 | NAG-APPT0 | `docs/process/zrodla/appto-strona-glowna-pl.md` | SOURCE | 12 | `01366cefcc214319fe55494c913fdc29735eff8ee573685fd4d7cea61aaa34ef` | 2026-08-25 | LOCAL_READBACK; <https://www.appto.ai/pl/> |
| PF-0233 | NAG-APPT0 | `docs/process/zrodla/appto-wdrozenie-kohortowe-pl.md` | SOURCE | 12 | `9851abbe210fada59f3ed0616f27ab9361e3cf83c400965dade1ab5664a68e33` | 2026-08-25 | LOCAL_READBACK; <https://www.appto.ai/pl/wdrozenie-kohortowe/> |
| PF-0250 | NAG-SOURCES | `nagents-2026-09-10/sources/10.md` | SOURCE | 1 | `8a7379fa41f8a1c86e27baac1fc2e999b5e34f0919c18c257852013f9591c160` | 2026-09-10 | P4_ONLY_NO_RAW_COPY; [link](../../audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json#PF-0250) |
| PF-0251 | NAG-SOURCES | `nagents-2026-09-10/sources/6.md` | SOURCE | 1 | `fbfa499b1f17e85f3dd2c85cc69583166455643628529fc1727eb5d8e9a18177` | 2026-09-10 | P4_ONLY_NO_RAW_COPY; [link](../../audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json#PF-0251) |
| PF-0252 | NAG-SOURCES | `nagents-2026-09-10/sources/7.md` | SOURCE | 1 | `cc84dc63618f735aae25bc365b677705f192d0c2f420cba650713901dd69b8c2` | 2026-09-10 | P4_ONLY_NO_RAW_COPY; [link](../../audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json#PF-0252) |
| PF-0253 | NAG-SOURCES | `nagents-2026-09-10/sources/8.md` | SOURCE | 1 | `e3b0c44298fc1c149afbf4c8996fb92427ae41e4649b934ca495991b7852b855` | 2026-09-10 | P4_ONLY_NO_RAW_COPY; [link](../../audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json#PF-0253) |
| PF-0254 | NAG-SOURCES | `nagents-2026-09-10/sources/9.md` | SOURCE | 1 | `80eb2d12dfa4075ad9163f06d4278c92ccda7406970538a78156bcbb95a4d688` | 2026-09-10 | P4_ONLY_NO_RAW_COPY; [link](../../audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json#PF-0254) |

`LOCAL_READBACK` oznacza, że plik jest w dozwolonym checkoutcie i został odczytany. `P4_ONLY_NO_RAW_COPY` oznacza zewnętrzny snapshot: zachowano hash, metadane i dokładny locator z P4, ale nie kopiowano treści do repozytorium. Oryginalne URL-e technicznych snapshotów nie są dostępne w metadanych P4; pakiet nie zgaduje ich i linkuje do P4.

### 2.2 Kontrole procesu i kanon

| Ref | Ścieżka | Status | Ranga | Użycie |
|---|---|---|---|---|
| `CTRL-P6` | `NAGENTS-CONSOLIDATION-PLAN.md` | `PLAN_ONLY` | `PLAN_ONLY` | kryteria operatora |
| `CTRL-PROJECT` | `NAGENTS-PROJECT.md` | `CONSOLIDATION_CANDIDATE` | `INDEX_ONLY` | kontekst projektu, bez awansu tez |
| `CTRL-P4` | `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json` | `MACHINE_METADATA` | `P4_METADATA` | counts, hash i status |
| `CTRL-HISTORY` | `docs/process/pamiec.md` | `HISTORY` | `HISTORY_CONTEXT` | korekty i przebieg rozumowania |
| `CTRL-DECISIONS` | `docs/spec/decisions.md` | `CANONICAL` | `CANONICAL` | wyłącznie readback decyzji |
| `CTRL-ECHO` | `docs/process/echo.md` | `CANONICAL` | `CANONICAL` | protokół decyzji właściciela |

## RESEARCH-01 — historyczne rozeznanie 8gent

Źródła tej sekcji to noty i handoffy o statusie `HISTORY`. Ich wartość polega na śladzie rozumowania, wariantach i korektach; nie są źródłem aktualnej topologii, uprawnień ani stanu wdrożenia.

### SOURCE

#### R01-S01 — Nota 01 porównała trzy drogi A/B/C i zapisała preferencję A z późniejszym warunkiem dla C.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-22`  |  RANK: `HISTORY`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `HISTORY_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-01-trzy-drogi-do-agenta.md`  |  SOURCE_REF: `PF-0198`  |  LOCATOR: `§Rekomendacja; lines 12-27`
SOURCE_LINK: [PF-0198](../../../nota-01-trzy-drogi-do-agenta.md)
TEZA: To historyczny zapis rozeznania, nie aktualny wybór architektury.
CROSS_LINKS: brak

#### R01-S02 — Nota 02 zapisała pierwotną ocenę integracji Teams oraz propozycję Hermes + LiteLLM + cienki harness.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-22`  |  RANK: `HISTORY`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `SUPERSEDED_HISTORY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-02-uprzaz-dla-agentow.md`  |  SOURCE_REF: `PF-0199`  |  LOCATOR: `§Rekomendacja i korekta; lines 59-82`
SOURCE_LINK: [PF-0199](../../../nota-02-uprzaz-dla-agentow.md)
TEZA: Ocena Teams z tej noty jest później skorygowana w nocie 04; oba zapisy pozostają historią.
CROSS_LINKS: brak

#### R01-S03 — Aneks 02a rozdziela konto/profil od sandboxa plikowego i opisuje izolację stanu oraz wariant wielogatewayowy.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-22`  |  RANK: `HISTORY`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `HISTORY_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-02a-aneks-profile-i-zakres-v1.md`  |  SOURCE_REF: `PF-0200`  |  LOCATOR: `§2-§5; lines 34-119`
SOURCE_LINK: [PF-0200](../../../nota-02a-aneks-profile-i-zakres-v1.md)
TEZA: Profil jest tu granicą konfiguracji/uprawnień w procesie badawczym, nie dowodem pełnej izolacji wykonania.
CROSS_LINKS: brak

#### R01-S04 — Nota 03 opisuje wariant 27 agentów, role stanowiskowe/projektowe oraz zakaz własnych poświadczeń agentów stanowiskowych.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-22`  |  RANK: `HISTORY`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `HISTORY_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-03-topologia-27-agentow.md`  |  SOURCE_REF: `PF-0201`  |  LOCATOR: `§1-§4; lines 13-121`
SOURCE_LINK: [PF-0201](../../../nota-03-topologia-27-agentow.md)
TEZA: Liczba 27 i topologia są propozycją z historii; zabezpieczenie braku poświadczeń jest wzorcem do porównania z kanonem.
CROSS_LINKS: brak

#### R01-S05 — Nota 04 koryguje ocenę Teams i opisuje lekcję z Openbot: domyślna odmowa, audyt prób i rozdział ról.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-22`  |  RANK: `HISTORY`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `HISTORY_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-04-korekty-i-nowe-materialy.md`  |  SOURCE_REF: `PF-0202`  |  LOCATOR: `§1 i §4; lines 10-26, 110-137`
SOURCE_LINK: [PF-0202](../../../nota-04-korekty-i-nowe-materialy.md)
TEZA: To obserwacja/lekcja z materiału, nie samodzielna norma bezpieczeństwa.
CROSS_LINKS: brak

#### R01-S06 — Nota 05 zestawia budowę z zakupem i ostrzega, że tokenizacja/pseudonimizacja nie jest anonimizacją.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-22`  |  RANK: `HISTORY`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `HISTORY_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-05-kupic-czy-zbudowac.md`  |  SOURCE_REF: `PF-0203`  |  LOCATOR: `§2 i §3; lines 11-81`
SOURCE_LINK: [PF-0203](../../../nota-05-kupic-czy-zbudowac.md)
TEZA: Wynik należy czytać jako uzasadnienie historyczne; norma bezpieczeństwa jest w dokumentacji kanonicznej.
CROSS_LINKS: brak

#### R01-S07 — Nota 08 wyznacza granicę: wykonanie integracji jest pracą Hermesa, a 8gent zarządza zakresem i dostępem; MVP1 pracuje na pliku.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-25`  |  RANK: `HISTORY`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `HISTORY_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-08-wybory-otwarte.md`  |  SOURCE_REF: `PF-0206`  |  LOCATOR: `§1-§3; lines 15-103`
SOURCE_LINK: [PF-0206](../../../nota-08-wybory-otwarte.md)
TEZA: To zapis granicy zakresu i kolejności, nie zgoda na konkretną implementację integracji.
CROSS_LINKS: brak

### ANALYSIS

#### R01-A01 — Wniosek z A/B/C: dla jednego właściciela technicznego wariant A był najszybszy i odwracalny, B niósł ciężar platformy, C wymagał późniejszego pilota.
TYPE: `ANALYSIS`  |  DATE/RANGE: `2026-08-22`  |  RANK: `HISTORY_ANALYSIS`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `ANALYSIS_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-01-trzy-drogi-do-agenta.md`  |  SOURCE_REF: `PF-0198`  |  LOCATOR: `§Rekomendacja; lines 159-174`
SOURCE_LINK: [PF-0198](../../../nota-01-trzy-drogi-do-agenta.md)
TEZA: Analiza historyczna; nie jest decyzją D-001 ani D-010.
CROSS_LINKS: brak

#### R01-A02 — Granica profilu powinna odpowiadać domenie uprawnień, ale nie należy utożsamiać jej automatycznie z osobą ani z izolacją procesu.
TYPE: `ANALYSIS`  |  DATE/RANGE: `2026-08-22`  |  RANK: `HISTORY_ANALYSIS`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `ANALYSIS_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-02a-aneks-profile-i-zakres-v1.md`  |  SOURCE_REF: `PF-0200`  |  LOCATOR: `§2-§3; lines 34-68`
SOURCE_LINK: [PF-0200](../../../nota-02a-aneks-profile-i-zakres-v1.md)
TEZA: Wniosek projektowy do weryfikacji w MVP1; topologia pozostaje otwarta.
CROSS_LINKS: brak

#### R01-A03 — Zestaw not sygnalizuje wartość warstwowego kontekstu i mniejszej liczby profili, lecz nie rozstrzyga topologii przed danymi z MVP1.
TYPE: `ANALYSIS`  |  DATE/RANGE: `2026-08-22`  |  RANK: `HISTORY_ANALYSIS`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `ANALYSIS_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-05-kupic-czy-zbudowac.md`  |  SOURCE_REF: `PF-0203`  |  LOCATOR: `§5; lines 94-112`
SOURCE_LINK: [PF-0203](../../../nota-05-kupic-czy-zbudowac.md)
TEZA: To hipoteza do testu użycia, nie aktualna liczba agentów.
CROSS_LINKS: brak

#### R01-A04 — Wzorce no-credentials, pseudonimizacji i audytu są kandydatami do porównania z kanonem, ale nie mogą zastąpić wpisu w specyfikacji/decisions/ECHO.
TYPE: `ANALYSIS`  |  DATE/RANGE: `2026-08-22`  |  RANK: `HISTORY_ANALYSIS`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `ANALYSIS_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/process/pamiec.md`  |  SOURCE_REF: `CTRL-HISTORY`  |  LOCATOR: `§5; lines 142-177`
SOURCE_LINK: [CTRL-HISTORY](../../pamiec.md)
TEZA: Analiza wskazuje miejsce weryfikacji; nie przenosi normy automatycznie.
CROSS_LINKS: brak

#### R01-A05 — Rozdzielenie wykonania Hermesa od zarządzania 8gent jest stabilnym filtrem zakresu dla dalszych badań integracyjnych.
TYPE: `ANALYSIS`  |  DATE/RANGE: `2026-08-25`  |  RANK: `HISTORY_ANALYSIS`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `ANALYSIS_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-08-wybory-otwarte.md`  |  SOURCE_REF: `PF-0206`  |  LOCATOR: `§3; lines 75-103`
SOURCE_LINK: [PF-0206](../../../nota-08-wybory-otwarte.md)
TEZA: Integracje wymagają osobnego materiału INT i bramki właściciela.
CROSS_LINKS: brak

### DECISION / OWNER POINTER

#### R01-D01 — D-001 jest kanonicznym odnośnikiem decyzji o budowie własnej warstwy zarządzania nad Hermesem.
TYPE: `DECISION`  |  DATE/RANGE: `2026-09-14 readback`  |  RANK: `CANONICAL`
SOURCE_STATUS: `CANONICAL`  |  STATE/CURRENTNESS: `CANONICAL_POINTER`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/spec/decisions.md`  |  SOURCE_REF: `CTRL-DECISIONS`  |  LOCATOR: `D-001; readback 2026-09-14`
SOURCE_LINK: [CTRL-DECISIONS](../../../spec/decisions.md)
TEZA: Pakiet nie podejmuje tej decyzji ponownie i nie zmienia jej uzasadnienia.
CROSS_LINKS: brak

#### R01-D02 — D-010 pozostaje otwartą decyzją o topologii agentów; badanie nie wybiera wariantu 27.
TYPE: `DECISION`  |  DATE/RANGE: `2026-09-14 readback`  |  RANK: `CANONICAL`
SOURCE_STATUS: `CANONICAL`  |  STATE/CURRENTNESS: `CANONICAL_POINTER`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/spec/decisions.md`  |  SOURCE_REF: `CTRL-DECISIONS`  |  LOCATOR: `D-010; readback 2026-09-14`
SOURCE_LINK: [CTRL-DECISIONS](../../../spec/decisions.md)
TEZA: Nie wolno traktować historycznej liczby agentów jako live state.
CROSS_LINKS: brak

#### R01-D03 — D-011 pozostaje blokadą dla MVP3, przy zachowaniu ścieżki MVP1/MVP2 bez rozstrzygania wspólnej pamięci.
TYPE: `DECISION`  |  DATE/RANGE: `2026-09-14 readback`  |  RANK: `CANONICAL`
SOURCE_STATUS: `CANONICAL`  |  STATE/CURRENTNESS: `CANONICAL_POINTER`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/spec/decisions.md`  |  SOURCE_REF: `CTRL-DECISIONS`  |  LOCATOR: `D-011; readback 2026-09-14`
SOURCE_LINK: [CTRL-DECISIONS](../../../spec/decisions.md)
TEZA: Pakiet zapisuje granicę i nie ustala rezydencji danych.
CROSS_LINKS: brak

### Wniosek sekcji

Nie przenosimy do live state ani liczby 27 agentów, ani historycznej preferencji A/B/C, ani dawnych porównań Teams. Do dalszej pracy nadają się wzorce do weryfikacji: granica uprawnień, brak poświadczeń agentów stanowiskowych, audyt i oddzielenie zarządzania od wykonania.

## RESEARCH-02 — appto jako benchmark, nie decyzja

### 4.1 Warstwy źródła

| Plik | Data snapshotu | Ranga treści | P4 status | Link/locator |
|---|---|---|---|---|
| `docs/nota-06-appto-research.md` | 2026-08-25 | `SECONDARY_RESEARCH_SNAPSHOT` | SOURCE | ../../../nota-06-appto-research.md |
| `docs/nota-07-katalog-funkcji.md` | 2026-08-25 | `SECONDARY_RESEARCH_SNAPSHOT` | SOURCE | ../../../nota-07-katalog-funkcji.md |
| `docs/process/zrodla/appto-cennik-pl.md` | 2026-08-25 | `PRIMARY_MARKETING_SNAPSHOT` | SOURCE | https://www.appto.ai/pl/cennik/ |
| `docs/process/zrodla/appto-integracje-pl.md` | 2026-08-25 | `PRIMARY_MARKETING_SNAPSHOT` | SOURCE | https://www.appto.ai/pl/integracje/ |
| `docs/process/zrodla/appto-polityka-prywatnosci-pl.md` | 2026-08-25 | `PRIMARY_LEGAL_SNAPSHOT` | SOURCE | https://www.appto.ai/pl/polityka-prywatnosci/ |
| `docs/process/zrodla/appto-regulamin-pl.md` | 2026-08-25 | `PRIMARY_LEGAL_SNAPSHOT` | SOURCE | https://www.appto.ai/pl/regulamin/ |
| `docs/process/zrodla/appto-strona-glowna-pl.md` | 2026-08-25 | `PRIMARY_MARKETING_SNAPSHOT` | SOURCE | https://www.appto.ai/pl/ |
| `docs/process/zrodla/appto-wdrozenie-kohortowe-pl.md` | 2026-08-25 | `PRIMARY_MARKETING_SNAPSHOT` | SOURCE | https://www.appto.ai/pl/wdrozenie-kohortowe/ |

Sześć plików `docs/process/zrodla/appto-*` jest przechwyconym materiałem producenta. `docs/nota-06-appto-research.md` i `docs/nota-07-katalog-funkcji.md` są wtórnym opracowaniem i inwentarzem; nie należy mieszać ich w tabeli z primary source. Dokumenty prawne (`polityka` i `regulamin`) zachowują osobny status `PRIMARY_LEGAL_SNAPSHOT` i effective date `2026-07-06`.

### 4.2 SOURCE

### SOURCE

#### R02-S01 — Sześć przechwyconych stron appto obejmuje stronę główną, cennik, integracje, wdrożenie kohortowe oraz dokumenty prywatności i regulaminu.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-25`  |  RANK: `PRIMARY_CAPTURED_SNAPSHOT`
SOURCE_STATUS: `SOURCE`  |  STATE/CURRENTNESS: `SOURCE_SNAPSHOT`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-06-appto-research.md`  |  SOURCE_REF: `PF-0204`  |  LOCATOR: `§2-§4; lines 49-222`
SOURCE_LINK: [PF-0204](../../../nota-06-appto-research.md)
TEZA: To katalog źródeł przechwyconych i ich zakresów; daty są datami snapshotu.
CROSS_LINKS: brak

#### R02-S02 — Cennik deklaruje pakiety kredytowe i stawki snapshotu, w tym 490 netto/mies. oraz 990 netto/mies.; nie jest to aktualna oferta.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-25`  |  RANK: `PRIMARY_MARKETING_SNAPSHOT`
SOURCE_STATUS: `SOURCE`  |  STATE/CURRENTNESS: `SOURCE_SNAPSHOT`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/process/zrodla/appto-cennik-pl.md`  |  SOURCE_REF: `PF-0228`  |  LOCATOR: `§Pakiety i ceny; lines 23-45`
SOURCE_LINK: <https://www.appto.ai/pl/cennik/>
TEZA: Liczby są zachowane jako historyczny materiał porównawczy, bez wniosku zakupowego.
CROSS_LINKS: brak

#### R02-S03 — Materiały marketingowe deklarują szeroki katalog integracji; zestawienie zawiera rozbieżność między 40+ a 956.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-25`  |  RANK: `PRIMARY_MARKETING_SNAPSHOT`
SOURCE_STATUS: `SOURCE`  |  STATE/CURRENTNESS: `UNRECONCILED_SNAPSHOT`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/process/zrodla/appto-integracje-pl.md`  |  SOURCE_REF: `PF-0229`  |  LOCATOR: `§Opis i katalog; lines 13-25, 55-77`
SOURCE_LINK: <https://www.appto.ai/pl/integracje/>
TEZA: Rozbieżność pozostaje jawna; nie jest uśredniana ani korygowana przez pakiet.
CROSS_LINKS: brak

#### R02-S04 — Polityka prywatności opisuje zakres danych, transfery do państw trzecich oraz podmioty przetwarzające; dokument ma datę wejścia 2026-07-06.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-07-06 effective; captured 2026-08-25`  |  RANK: `PRIMARY_LEGAL_SNAPSHOT`
SOURCE_STATUS: `SOURCE`  |  STATE/CURRENTNESS: `LEGAL_SNAPSHOT`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/process/zrodla/appto-polityka-prywatnosci-pl.md`  |  SOURCE_REF: `PF-0230`  |  LOCATOR: `§Data, transfery, podmioty; lines 71-113`
SOURCE_LINK: <https://www.appto.ai/pl/polityka-prywatnosci/>
TEZA: To snapshot dokumentu prawnego, nie niezależna opinia prawna 8gent.
CROSS_LINKS: brak

#### R02-S05 — Regulamin wraz z umową powierzenia opisuje dostęp administracyjny, usuwanie po 30 dniach, DPA i limit odpowiedzialności; wersja ma datę 2026-07-06.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-07-06 effective; captured 2026-08-25`  |  RANK: `PRIMARY_LEGAL_SNAPSHOT`
SOURCE_STATUS: `SOURCE`  |  STATE/CURRENTNESS: `LEGAL_SNAPSHOT`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/process/zrodla/appto-regulamin-pl.md`  |  SOURCE_REF: `PF-0231`  |  LOCATOR: `§Powierzenie, dostęp, usuwanie, odpowiedzialność; lines 25-75, 145-188`
SOURCE_LINK: <https://www.appto.ai/pl/regulamin/>
TEZA: Prawne zapisy mają pierwszeństwo przed sloganem marketingowym w analizie ryzyka.
CROSS_LINKS: brak

#### R02-S06 — Strona główna i wdrożenie kohortowe deklarują funkcje, role i model wdrażania; są to twierdzenia dostawcy, nie wynik testu.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-25`  |  RANK: `PRIMARY_MARKETING_SNAPSHOT`
SOURCE_STATUS: `SOURCE`  |  STATE/CURRENTNESS: `SOURCE_SNAPSHOT`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/process/zrodla/appto-strona-glowna-pl.md`  |  SOURCE_REF: `PF-0232`  |  LOCATOR: `§Funkcje i role; lines 39-63, 129-152`
SOURCE_LINK: <https://www.appto.ai/pl/>
TEZA: Pakiet nie zamienia deklaracji w capability 8gent.
CROSS_LINKS: brak

#### R02-S07 — Katalog wtórny porządkuje 58 funkcji (54 marketingowe i 4 prawne), 26 kandydatów do planu, 25 luk i 7 odrzuceń.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-25`  |  RANK: `SECONDARY_RESEARCH_SNAPSHOT`
SOURCE_STATUS: `SOURCE`  |  STATE/CURRENTNESS: `SECONDARY_SNAPSHOT`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-07-katalog-funkcji.md`  |  SOURCE_REF: `PF-0205`  |  LOCATOR: `§1, §3, §5, §7; lines 10-45, 209-265, 371-418`
SOURCE_LINK: [PF-0205](../../../nota-07-katalog-funkcji.md)
TEZA: Liczby są inwentarzem noty 07, a nie pomiarem aktualnego systemu.
CROSS_LINKS: brak

### 4.3 ANALYSIS

### ANALYSIS

#### R02-A01 — Źródła prawne mają wyższą wagę dla przepływu danych i zobowiązań niż materiały sprzedażowe; oba poziomy muszą pozostać widoczne.
TYPE: `ANALYSIS`  |  DATE/RANGE: `2026-08-25`  |  RANK: `CROSS_SOURCE_ANALYSIS`
SOURCE_STATUS: `SOURCE`  |  STATE/CURRENTNESS: `ANALYSIS_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-06-appto-research.md`  |  SOURCE_REF: `PF-0204`  |  LOCATOR: `§4; lines 176-222`
SOURCE_LINK: [PF-0204](../../../nota-06-appto-research.md)
TEZA: To metoda czytania źródeł, nie kwalifikacja prawna.
CROSS_LINKS: brak

#### R02-A02 — Rozbieżność 40+/956 oraz napięcia marketing–legalne są nierozstrzygnięte; analiza nie produkuje jednej „poprawionej” liczby.
TYPE: `ANALYSIS`  |  DATE/RANGE: `2026-08-25`  |  RANK: `CROSS_SOURCE_ANALYSIS`
SOURCE_STATUS: `SOURCE`  |  STATE/CURRENTNESS: `ANALYSIS_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-06-appto-research.md`  |  SOURCE_REF: `PF-0204`  |  LOCATOR: `§2-§4; lines 91-222`
SOURCE_LINK: [PF-0204](../../../nota-06-appto-research.md)
TEZA: Każda przyszła teza musi wskazać konkretny dokument i datę.
CROSS_LINKS: brak

#### R02-A03 — Cennik i funkcje appto są użytecznym benchmarkiem projektowym, lecz snapshot cen nie może zasilać bieżącego budżetu ani decyzji zakupowej.
TYPE: `ANALYSIS`  |  DATE/RANGE: `2026-08-25`  |  RANK: `HISTORY_ANALYSIS`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `ANALYSIS_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-05-kupic-czy-zbudowac.md`  |  SOURCE_REF: `PF-0203`  |  LOCATOR: `§2; lines 42-60`
SOURCE_LINK: [PF-0203](../../../nota-05-kupic-czy-zbudowac.md)
TEZA: Porównanie jest odwracalne i nie tworzy zobowiązania.
CROSS_LINKS: brak

#### R02-A04 — Do dalszego projektu można przenieść wzorce: centralny kontekst, wersjonowanie umiejętności, bramkowanie akcji i audyt; nie przenosić całej listy funkcji jako wymogu parytetu.
TYPE: `ANALYSIS`  |  DATE/RANGE: `2026-08-25`  |  RANK: `SECONDARY_ANALYSIS`
SOURCE_STATUS: `SOURCE`  |  STATE/CURRENTNESS: `ANALYSIS_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-07-katalog-funkcji.md`  |  SOURCE_REF: `PF-0205`  |  LOCATOR: `§5-§7; lines 371-520`
SOURCE_LINK: [PF-0205](../../../nota-07-katalog-funkcji.md)
TEZA: Kandydaci pozostają materiałem do oceny MVP i decyzji właściciela.
CROSS_LINKS: brak

#### R02-A05 — Nota 06 wskazuje, że dostępne materiały appto nie uzasadniają pierwotnego założenia o tokenowym rozliczaniu bez wyboru modelu; korekta uzasadnienia wymaga osobnej decyzji.
TYPE: `ANALYSIS`  |  DATE/RANGE: `2026-08-25`  |  RANK: `SECONDARY_ANALYSIS`
SOURCE_STATUS: `SOURCE`  |  STATE/CURRENTNESS: `ANALYSIS_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-06-appto-research.md`  |  SOURCE_REF: `PF-0204`  |  LOCATOR: `§5; lines 226-316`
SOURCE_LINK: [PF-0204](../../../nota-06-appto-research.md)
TEZA: Nie zmieniono D-001 i nie dopisano nowej decyzji.
CROSS_LINKS: brak

#### R02-A06 — Lekcja zgodności: osobno sprawdzać prywatność, DPA, podprocesorów, retencję i transfery; nie kompensować braków marketingową listą funkcji.
TYPE: `ANALYSIS`  |  DATE/RANGE: `2026-08-25`  |  RANK: `LEGAL_RISK_ANALYSIS`
SOURCE_STATUS: `SOURCE`  |  STATE/CURRENTNESS: `ANALYSIS_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/process/zrodla/appto-regulamin-pl.md`  |  SOURCE_REF: `PF-0231`  |  LOCATOR: `§Powierzenie; lines 25-75`
SOURCE_LINK: <https://www.appto.ai/pl/regulamin/>
TEZA: Wniosek kontrolny, nie automatyczna norma dla 8gent.
CROSS_LINKS: brak

### 4.4 DECISION / owner gate

### DECISION / OWNER GATE

#### R02-D01 — D-001 pozostaje kanoniczną decyzją budowy; materiały appto są benchmarkiem, nie przesłanką do zakupu ani zmianą zakresu.
TYPE: `DECISION`  |  DATE/RANGE: `2026-09-14 readback`  |  RANK: `CANONICAL`
SOURCE_STATUS: `CANONICAL`  |  STATE/CURRENTNESS: `CANONICAL_POINTER`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/spec/decisions.md`  |  SOURCE_REF: `CTRL-DECISIONS`  |  LOCATOR: `D-001; readback 2026-09-14`
SOURCE_LINK: [CTRL-DECISIONS](../../../spec/decisions.md)
TEZA: Pakiet nie tworzy decyzji vendor/product.
CROSS_LINKS: brak

#### R02-D02 — Pakiet nie podejmuje decyzji o cenie, dostawcy ani integracji; tezy integracyjne przechodzą do materiału INT i bramki właściciela.
TYPE: `DECISION`  |  DATE/RANGE: `2026-09-14`  |  RANK: `PLAN_ONLY`
SOURCE_STATUS: `PLAN_ONLY`  |  STATE/CURRENTNESS: `OWNER_GATE`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `NAGENTS-CONSOLIDATION-PLAN.md`  |  SOURCE_REF: `CTRL-P6`  |  LOCATOR: `§4.5; lines 140-148`
SOURCE_LINK: [CTRL-P6](../../../../NAGENTS-CONSOLIDATION-PLAN.md)
TEZA: Brak decyzji jest stanem zamierzonym, nie luką w pakiecie.
CROSS_LINKS: brak

### Wniosek sekcji

Appto jest źródłem porównawczym dla wzorców produktu i zgodności. Ceny, liczniki funkcji i możliwości integracji są historycznymi snapshotami; nie są aktualnym cennikiem, budżetem, dowodem capability Hermesa ani zgodą na zakup.

## RESEARCH-03 — OAuth/Entra/Graph/Hermes

Ta sekcja nie kopiuje treści z `/home/ubuntu/handoffs`. Zewnętrzne pliki są tylko metadanymi P4 i exact record locatorami. `sources/8.md` jest pustym snapshotem i nie może być cytowany jako dowód.

### 5.1 SOURCE

### SOURCE

#### R03-S01 — P4 zachowuje pięć technicznych snapshotów NAG-SOURCES z datą archiwalną 2026-09-10: auth code flow, upload session, pusty plik, credentials Entra i Hermes Agent.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-09-10`  |  RANK: `PRIMARY_TECHNICAL_SNAPSHOT`
SOURCE_STATUS: `MACHINE_METADATA`  |  STATE/CURRENTNESS: `SOURCE_SNAPSHOT`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json`  |  SOURCE_REF: `CTRL-P4`  |  LOCATOR: `PF-0250–PF-0254; exact record metadata`
SOURCE_LINK: [CTRL-P4](../../audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json)
TEZA: W pakiecie zachowano identyfikator, hash, locator i status każdego snapshotu.
CROSS_LINKS: brak

#### R03-S02 — Snapshot sources/8.md ma 0 bajtów i 0 linii; nie dostarcza dowodu ani tezy technicznej.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-09-10`  |  RANK: `PRIMARY_TECHNICAL_SNAPSHOT`
SOURCE_STATUS: `MACHINE_METADATA`  |  STATE/CURRENTNESS: `EMPTY_SOURCE`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json`  |  SOURCE_REF: `CTRL-P4`  |  LOCATOR: `PF-0253; P4 size 0, lines 0`
SOURCE_LINK: [CTRL-P4](../../audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json)
TEZA: Pusty rekord jest jawnie zarejestrowany, ale nie jest użyty jako źródło.
CROSS_LINKS: brak

#### R03-S03 — P4 podaje dokładne metadane snapshotów: PF-0251 auth code flow, PF-0252 createUploadSession, PF-0254 Entra credentials i PF-0250 Hermes Agent.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-09-10`  |  RANK: `PRIMARY_TECHNICAL_SNAPSHOT`
SOURCE_STATUS: `MACHINE_METADATA`  |  STATE/CURRENTNESS: `SOURCE_SNAPSHOT`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json`  |  SOURCE_REF: `CTRL-P4`  |  LOCATOR: `PF-0250–PF-0254; first_heading and link_count`
SOURCE_LINK: [CTRL-P4](../../audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json)
TEZA: Opis zakresu pochodzi wyłącznie z metadanych P4; treść archiwum nie została skopiowana do checkoutu.
CROSS_LINKS: brak

### 5.2 ANALYSIS

### ANALYSIS

#### R03-A01 — Snapshot techniczny jest punktem startowym do ponownej walidacji, nie dowodem aktualnego kontraktu API, uprawnień ani zachowania runtime.
TYPE: `ANALYSIS`  |  DATE/RANGE: `2026-09-10`  |  RANK: `SNAPSHOT_ANALYSIS`
SOURCE_STATUS: `MACHINE_METADATA`  |  STATE/CURRENTNESS: `ANALYSIS_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json`  |  SOURCE_REF: `CTRL-P4`  |  LOCATOR: `P4 status and freshness fields for PF-0250–PF-0254`
SOURCE_LINK: [CTRL-P4](../../audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json)
TEZA: Weryfikacja live wymaga osobnego readbacku i nie należy do tego pakietu.
CROSS_LINKS: brak

#### R03-A02 — Najbezpieczniejszy użytek snapshotów to cytowalny locator dla przyszłego materiału INT; pusty snapshot pozostaje tylko sygnałem braku dowodu.
TYPE: `ANALYSIS`  |  DATE/RANGE: `2026-09-10`  |  RANK: `SNAPSHOT_ANALYSIS`
SOURCE_STATUS: `PLAN_ONLY`  |  STATE/CURRENTNESS: `ANALYSIS_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `NAGENTS-CONSOLIDATION-PLAN.md`  |  SOURCE_REF: `CTRL-P6`  |  LOCATOR: `§4.5; lines 140-148`
SOURCE_LINK: [CTRL-P6](../../../../NAGENTS-CONSOLIDATION-PLAN.md)
TEZA: Nie generuje endpointu, scope ani zgody na integrację.
CROSS_LINKS: brak

### 5.3 DECISION / owner gate

### DECISION / OWNER GATE

#### R03-D01 — Nie podjęto decyzji o endpointach, scope, credentials ani wdrożeniu na podstawie snapshotów NAG-SOURCES.
TYPE: `DECISION`  |  DATE/RANGE: `2026-09-14`  |  RANK: `PLAN_ONLY`
SOURCE_STATUS: `PLAN_ONLY`  |  STATE/CURRENTNESS: `OWNER_GATE`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `NAGENTS-CONSOLIDATION-PLAN.md`  |  SOURCE_REF: `CTRL-P6`  |  LOCATOR: `§4.5; lines 140-148`
SOURCE_LINK: [CTRL-P6](../../../../NAGENTS-CONSOLIDATION-PLAN.md)
TEZA: Decyzje techniczne i dostępowe pozostają w odpowiedniej bramce właściciela.
CROSS_LINKS: brak

### Wniosek sekcji

Snapshoty są dobrym wejściem do osobnego materiału `INT-*`, ale przed użyciem trzeba ponownie sprawdzić aktualne endpointy, scope, credentials i zachowanie runtime. Ten pakiet nie robi takiego testu.

## RESEARCH-04 — korekty, lekcje, pewność i currentness

### 6.1 Ślad korekt

| Korekta | Źródło i data | Status | Co wolno wnioskować |
|---|---|---|---|
| Panel Hermesa nie jest założonym kanałem administracji | `docs/nota-02-uprzaz-dla-agentow.md`, 2026-08-22; `docs/process/pamiec.md` §5 | `CORRECTED_HISTORY` | nie zakładać panelu bez bieżącej walidacji |
| Eve ma wbudowane Teams | `docs/nota-04-korekty-i-nowe-materialy.md` §1, 2026-08-22 | `CORRECTED_HISTORY` | nie używać starej tezy „Eve bez Teams” |
| Profil nie jest sandboxem plikowym | `docs/nota-02a-aneks-profile-i-zakres-v1.md` §2-§3, 2026-08-22 | `HISTORY_ONLY` | rozdzielić permission domain od process isolation |
| 27 agentów to wariant, nie live count | `docs/nota-03-topologia-27-agentow.md` i `docs/nota-05-kupic-czy-zbudowac.md`, 2026-08-22 | `HISTORY_ONLY` | D-010 pozostaje otwarte |
| DPA i transfery appto wymagają czytania dokumentów prawnych | `docs/nota-06-appto-research.md` §4, 2026-08-25 | `CORRECTED_HISTORY` | nie wnioskować z samego marketingu |
| Uzasadnienie D-001 nie wynika automatycznie z cennika appto | `docs/nota-06-appto-research.md` §5, 2026-08-25 | `OWNER_GATE` | decyzja i jej uzasadnienie wymagają osobnego wpisu |
| MVP1 opiera się na Excelu, a system/integracje są późniejsze | `docs/nota-08-wybory-otwarte.md` §3, 2026-08-25 | `CORRECTED_HISTORY` | nie opisywać historycznej wizji jako runtime |

### 6.2 SOURCE

### SOURCE

#### R04-S01 — Pamięć procesu i noty rejestrują korekty: panel Hermesa, Teams w Eve, liczba agentów, zależności MVP, DPA, tożsamość dostawcy i zakres systemu.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-22–2026-08-25`  |  RANK: `HISTORY`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `HISTORY_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/process/pamiec.md`  |  SOURCE_REF: `CTRL-HISTORY`  |  LOCATOR: `§5; lines 26-177`
SOURCE_LINK: [CTRL-HISTORY](../../pamiec.md)
TEZA: Korekty są dowodem przebiegu rozeznania, nie automatycznym nadpisaniem stanu bieżącego.
CROSS_LINKS: brak

#### R04-S02 — Nota 06 rozróżnia F (fakt), W (wniosek) i D (decyzja/propozycja) oraz wymaga, aby wnioski nie były czytane jako decyzje.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-25`  |  RANK: `SECONDARY_RESEARCH_SNAPSHOT`
SOURCE_STATUS: `SOURCE`  |  STATE/CURRENTNESS: `HISTORY_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-06-appto-research.md`  |  SOURCE_REF: `PF-0204`  |  LOCATOR: `§Legenda; lines 26-45`
SOURCE_LINK: [PF-0204](../../../nota-06-appto-research.md)
TEZA: Ten pakiet stosuje jawne pola TYPE/RANK/STATE zamiast ukrywać zmianę statusu.
CROSS_LINKS: brak

#### R04-S03 — Korekta Teams: nota 04 odrzuca wcześniejsze twierdzenie „Eve nie ma Teams” i zmienia porównanie narzędzi.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-22`  |  RANK: `HISTORY`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `CORRECTED_HISTORY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-04-korekty-i-nowe-materialy.md`  |  SOURCE_REF: `PF-0202`  |  LOCATOR: `§1; lines 10-26`
SOURCE_LINK: [PF-0202](../../../nota-04-korekty-i-nowe-materialy.md)
TEZA: W starym zapisie zachowano datę i status superseded.
CROSS_LINKS: brak

#### R04-S04 — Korekta appto: nota 06 wskazuje DPA, przeniesienie danych i tożsamość dostawcy; wcześniejsze skróty nie są wystarczającą podstawą.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-25`  |  RANK: `SECONDARY_RESEARCH_SNAPSHOT`
SOURCE_STATUS: `SOURCE`  |  STATE/CURRENTNESS: `CORRECTED_HISTORY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-06-appto-research.md`  |  SOURCE_REF: `PF-0204`  |  LOCATOR: `§4 i §6; lines 176-222, 357-430`
SOURCE_LINK: [PF-0204](../../../nota-06-appto-research.md)
TEZA: W pakiecie rozdzielono snapshot prawny od analizy.
CROSS_LINKS: brak

#### R04-S05 — Korekta zakresu: nota 08 oddziela MVP1 oparty na Excelu od późniejszego systemu i integracji.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-08-25`  |  RANK: `HISTORY`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `CORRECTED_HISTORY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/nota-08-wybory-otwarte.md`  |  SOURCE_REF: `PF-0206`  |  LOCATOR: `§3; lines 75-103`
SOURCE_LINK: [PF-0206](../../../nota-08-wybory-otwarte.md)
TEZA: Nie traktować historycznej wizji systemu jako obecnego runtime.
CROSS_LINKS: brak

### 6.3 ANALYSIS

### ANALYSIS

#### R04-A01 — Reguła currentness: HISTORY_ONLY, SOURCE_SNAPSHOT i ANALYSIS_ONLY nie mogą zasilać live state; tylko canonical pointers potwierdzają decyzję.
TYPE: `ANALYSIS`  |  DATE/RANGE: `2026-09-14`  |  RANK: `PROCESS_ANALYSIS`
SOURCE_STATUS: `HISTORY`  |  STATE/CURRENTNESS: `ANALYSIS_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/process/pamiec.md`  |  SOURCE_REF: `CTRL-HISTORY`  |  LOCATOR: `§5; lines 142-177`
SOURCE_LINK: [CTRL-HISTORY](../../pamiec.md)
TEZA: Każda teza ma status i locator, aby stara wersja nie awansowała bez readbacku.
CROSS_LINKS: brak

#### R04-A02 — Reguła pewności: liczba z primary source jest cytowalna jako snapshot, liczba z noty wtórnej jako analiza, a konflikt pozostaje UNRECONCILED.
TYPE: `ANALYSIS`  |  DATE/RANGE: `2026-09-14`  |  RANK: `PROCESS_ANALYSIS`
SOURCE_STATUS: `PLAN_ONLY`  |  STATE/CURRENTNESS: `ANALYSIS_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `NAGENTS-CONSOLIDATION-PLAN.md`  |  SOURCE_REF: `CTRL-P6`  |  LOCATOR: `§4.5; lines 140-148`
SOURCE_LINK: [CTRL-P6](../../../../NAGENTS-CONSOLIDATION-PLAN.md)
TEZA: Pakiet nie rozwiązuje konfliktów przez głosowanie ani deduplikację.
CROSS_LINKS: brak

### 6.4 DECISION / owner gate

### DECISION / OWNER GATE

#### R04-D01 — Pakiet nie ustanawia hierarchii źródeł ani nie rozstrzyga rozbieżności.
TYPE: `DECISION`  |  DATE/RANGE: `2026-09-14`  |  RANK: `PLAN_ONLY`
SOURCE_STATUS: `PLAN_ONLY`  |  STATE/CURRENTNESS: `OWNER_GATE`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `NAGENTS-CONSOLIDATION-PLAN.md`  |  SOURCE_REF: `CTRL-P6`  |  LOCATOR: `§4.5; lines 140-148`
SOURCE_LINK: [CTRL-P6](../../../../NAGENTS-CONSOLIDATION-PLAN.md)
TEZA: Pakiet nie rozstrzyga kolejności źródeł ani nie zastępuje readbacku właściwego kanonu.
CROSS_LINKS: brak

#### R04-D02 — Nie przepisywać retrospektywnie not ani źródeł; korektę statusu dopisywać w raporcie/indeksie i kierować do właściwej decyzji.
TYPE: `DECISION`  |  DATE/RANGE: `2026-09-14`  |  RANK: `PLAN_ONLY`
SOURCE_STATUS: `PLAN_ONLY`  |  STATE/CURRENTNESS: `OWNER_GATE`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `NAGENTS-CONSOLIDATION-PLAN.md`  |  SOURCE_REF: `CTRL-P6`  |  LOCATOR: `§4.5; lines 140-148`
SOURCE_LINK: [CTRL-P6](../../../../NAGENTS-CONSOLIDATION-PLAN.md)
TEZA: To zasada pakietowania, nie nowa decyzja produktowa.
CROSS_LINKS: brak

### 6.5 Reguły pewności

1. `PRIMARY_*_SNAPSHOT` = można cytować tylko z datą i linkiem do konkretnego snapshotu.
2. `SECONDARY_RESEARCH_SNAPSHOT` = wtórny wniosek; trzeba wrócić do źródła pierwotnego przed decyzją.
3. `HISTORY_ONLY` = nie zasila live state.
4. `UNRECONCILED_SNAPSHOT` = konflikt pozostaje jawny; nie wolno uśredniać ani „naprawiać” liczby.
5. `CANONICAL_POINTER` = tylko ścieżka kanoniczna może potwierdzać decyzję; sam research jej nie ustanawia.

## RESEARCH-05 — granica, routing i brak efektu ubocznego

### 7.1 SOURCE

### SOURCE

#### R05-S01 — P6 wymaga pięciu sekcji RESEARCH-01..05, proweniencji, rozdziału source/analysis/decision i jawnego statusu snapshotu.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-09-14`  |  RANK: `PLAN_ONLY`
SOURCE_STATUS: `PLAN_ONLY`  |  STATE/CURRENTNESS: `PLAN_REQUIREMENT`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `NAGENTS-CONSOLIDATION-PLAN.md`  |  SOURCE_REF: `CTRL-P6`  |  LOCATOR: `§4.5; lines 140-148`
SOURCE_LINK: [CTRL-P6](../../../../NAGENTS-CONSOLIDATION-PLAN.md)
TEZA: To kryterium wykonania operatora.
CROSS_LINKS: brak

#### R05-S02 — P4 rozdziela grupy NAG-RESEARCH/HISTORY, NAG-APPT0/SOURCE i NAG-SOURCES/SOURCE oraz wskazuje docelowy pakiet.
TYPE: `SOURCE`  |  DATE/RANGE: `2026-09-14`  |  RANK: `MACHINE_METADATA`
SOURCE_STATUS: `MACHINE_METADATA`  |  STATE/CURRENTNESS: `P4_METADATA`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json`  |  SOURCE_REF: `CTRL-P4`  |  LOCATOR: `logical_group_rollups for selected groups`
SOURCE_LINK: [CTRL-P4](../../audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json)
TEZA: Indeks zachowuje status P4 zamiast go normalizować.
CROSS_LINKS: brak

### 7.2 ANALYSIS

### ANALYSIS

#### R05-A01 — Najmniejsza bezpieczna jednostka cytowania to teza + data/range + rank + source status + locator + currentness; sam hash nie jest interpretacją treści.
TYPE: `ANALYSIS`  |  DATE/RANGE: `2026-09-14`  |  RANK: `PROCESS_ANALYSIS`
SOURCE_STATUS: `PLAN_ONLY`  |  STATE/CURRENTNESS: `ANALYSIS_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `NAGENTS-CONSOLIDATION-PLAN.md`  |  SOURCE_REF: `CTRL-P6`  |  LOCATOR: `§4.5; lines 140-148`
SOURCE_LINK: [CTRL-P6](../../../../NAGENTS-CONSOLIDATION-PLAN.md)
TEZA: Tak skonstruowano claim_index i sekcje dokumentu.
CROSS_LINKS: brak

### 7.3 DECISION / owner gate

### DECISION / OWNER GATE

#### R05-D01 — Pakiet pozostaje STAGING_ONLY: nie aktualizuje routing, live state, decyzji, integracji ani deploymentu.
TYPE: `DECISION`  |  DATE/RANGE: `2026-09-14`  |  RANK: `PLAN_ONLY`
SOURCE_STATUS: `PLAN_ONLY`  |  STATE/CURRENTNESS: `STAGING_ONLY`  |  CONFIDENCE: `udokumentowane`
SOURCE_PATH: `NAGENTS-CONSOLIDATION-PLAN.md`  |  SOURCE_REF: `CTRL-P6`  |  LOCATOR: `§4.5; lines 140-148`
SOURCE_LINK: [CTRL-P6](../../../../NAGENTS-CONSOLIDATION-PLAN.md)
TEZA: Dalszym krokiem jest niezależny Evaluator, nie publikacja.
CROSS_LINKS: brak

### 7.4 Macierz granic

| Warstwa | Traktować jako | Może | Nie może |
|---|---|---|---|
| `SOURCE` | dated evidence | dostarczyć tezę z locatoriem i hashem | potwierdzić aktualnego runtime bez readbacku |
| `ANALYSIS` | wniosek roboczy | wskazać hipotezę, lukę lub kandydacki wzorzec | stać się decyzją przez samo streszczenie |
| `DECISION` | kanoniczny pointer/bramka | wskazać `decisions/ECHO` albo potrzebę zgody | dopisywać decyzję do tego pliku |
| `INTEGRATION` | osobny strumień `INT-*` | przyjąć snapshot jako input do testu | uzyskać endpoint/scope/credentials z researchu |
| `LIVE STATE` | readback systemu/boardu | potwierdzić aktualny status | być wywiedziony z not historycznych |
| `DEPLOYMENT` | osobna zgoda i kanon | przyjąć zatwierdzony artefakt | zostać uruchomiony przez pakiet stagingowy |

### 7.5 Kontrola końcowa operatora

- Pięć sekcji `RESEARCH-01`…`RESEARCH-05` istnieje i ma oddzielne bloki `SOURCE`, `ANALYSIS`, `DECISION`.
- Każda teza ma datę/range, rangę, source status, path i locator.
- `NAG-RESEARCH` zachowuje `11/88`, `NAG-APPT0` `8/96`, `NAG-SOURCES` `5/5`; rekordy nie są kasowane przez overlap.
- `sources/8.md` jest oznaczone jako puste; nie podnosi pokrycia dowodowego.
- Nie skopiowano zewnętrznych handoffów ani żadnych sekretów, credential values, tokenów czy danych osobowych.
- Pakiet nie zmienia `docs/spec/decisions.md`, `docs/process/echo.md`, routingu, integracji, deploymentu ani żadnego źródła.
- Następna bramka: niezależny Evaluator; dopiero po jego PASS można kierować materiał do dalszej konsolidacji.

## 8. Indeks tez

Pełny indeks maszynowy, z exact record provenance, znajduje się w `coverage.json` obok tego pliku. Poniższe identyfikatory są stabilnymi locatorami: `R01-*`, `R02-*`, `R03-*`, `R04-*`, `R05-*`.
