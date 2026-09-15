# nAgents — manifest paczki dokumentacji

Status: paczka dokumentacji przygotowana do kontrolowanej publikacji
Branch docelowy publikacji: `docs/consolidated-2026-09-15`
Repozytorium: `maciejsieracki/nAgents`

## Zawartość główna

Poniższe dziewięć ksiąg jest byte-identical względem pakietów, które przeszły
Operator → niezależny Evaluator → Final Control → lokalny P7. Nagłówki
`stagingowy` w kopiach są zachowane celowo, aby nie zgubić proweniencji.

| Plik | Zakres | SHA-256 |
|---|---|---|
| `NAGENTS-SPEC.md` | architektura, zakres MVP, bezpieczeństwo, scenariusze | `3c431da703b7b53c7f4d7e6b655d92ef5283558f0161f481754edea7a6ddf855` |
| `NAGENTS-DECISIONS.md` | decyzje D-001…D-013, ECHO, pytania, historia | `cec183df199f7cbded295779cf0358e60a0db7df288b86a3d3894e5e13e329df` |
| `NAGENTS-PROCESS.md` | role, pętla, allowlista, evidence, watchdog, P1–P7 | `5643480dc51d97b4382998239eef684e98e8af7a091f06cb1c860009db1c5b22` |
| `NAGENTS-HANDOFF.md` | format przekazania, blokady i następne bramki | `15dc2cf43e2142e99126e40e28107c62ab6857cd7319a1ec23db52f5d31456ea` |
| `NAGENTS-RESEARCH.md` | research, źródła, porównania i korekty | `45440cc69e398698e7941a506459e3ff7bbc8e64cc0f149b610cf9854b1a9489` |
| `NAGENTS-USER-GUIDE.md` | instrukcja pracownika i granica pracownik–administrator | `67a1e3bbaa77244147886992aadf60df6c1139535a517679c36d8c41bb5480fe` |
| `AUTOBOT-PROJECT.md` | kontrakt AutoBot Monitor, routing i statusy | `7d97cee29c95a0a37a80f9594c5dcf880b5d81940ced842f63692091cf83cd82` |
| `docs/ABM-HISTORY.md` | historyczny indeks kart, runów, eventów i artefaktów ABM | `27b73324a3af9c3ee8827bc2b75a191062d5e84f8129ae1ad1e26c83675a9dec` |
| `docs/ABM-LIFECYCLE.md` | manual installation/uninstall/rollback/backup | `c5935a69131f2f7d04fe59130277932b844fdb7a785e7d6f50f7db21144693d2` |

## Plik wejściowy dla następnego agenta

`NAGENTS-PROJECT.md` jest kanonicznym indeksem wejściowym. Zawiera:

- kolejność czytania plików;
- mapę pytanie → pakiet → sekcja → źródło pierwszeństwa;
- procedurę wyszukiwania po identyfikatorach `SPEC-*`, `DEC-*`, `PROCESS-*`,
  `HANDOFF-*`, `RESEARCH-*`, `USER-*`, `ABM-*` i `LIFE-*`;
- rozdzielenie faktów z dokumentów od świeżego stanu Git/Kanbana/usług;
- granice nAgents, AutoBot Monitor i The-Game;
- zasady zachowania dokumentów historycznych i sprzątania.

## Dowody i odtwarzalność

Do paczki dołączone są:

- wszystkie katalogi `docs/process/staging/NAG-CONSOLIDATE-*`;
- `NAGENTS-CONSOLIDATION-PLAN.md`;
- `docs/process/NAGENTS-DOCS-CONSOLIDATION-PLAN.md`;
- `docs/process/NAGENTS-BOARD-SEPARATION-HANDOFF.md`;
- `docs/NAGENTS-DOCUMENTATION-PACKAGE-CONTENTS.sha256`;
- `docs/SECURITY-REDACTIONS.md`;
- audyt P1–P5 w `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/`;
- `docs/CONSOLIDATION-CLEANUP-MANIFEST.md`.

Do archiwum trafia również bezpieczny snapshot dokumentacji i maszynowych
manifestów z bieżącego checkoutu nAgents (`*.md` oraz audytowe `*.json`,
`*.jsonl`, `*.csv`, `*.yaml`). Nie jest to snapshot kodu ani runtime.

Raporty faz zachowują exact task/run/event/artifact IDs, statusy, hashe i
locatory. Nie są dowodem bieżącego runtime; do tego służy świeży readback.

## Wyłączone z paczki publicznej

Nie dołączono:

- sekretów, tokenów, haseł, kluczy, `.env`, `auth.json` i `state.db`;
- sesji, surowych logów, transcriptów i niesanitizowanego runtime;
- prywatnych handoffów spoza repozytorium;
- dokumentów The-Game;
- `NAGENTS-INTEGRATIONS.md`, ponieważ wybór A pozostawia integracje jako
  `OWNER_HOLD` / research-only;
- niepotwierdzonych kart i dokumentów innych projektów.

Skan przygotowanego zestawu: private-key `0`, bearer `0`, JWT `0`, IPv4 `0`,
e-mail `0`, credential-assignment `0`, NUL `0`, trailing whitespace `0`.

Znany wyjątek link-check: historyczny wpis w `CLAUDE.md` wskazuje na
nieistniejący `.claude/skills/autobots/SKILL.md`. Nie zmieniam chronionego
pliku instrukcji w ramach tej publikacji; właściwy dostępny odpowiednik
szkieletu jest w `docs/process/zrodla/autobots-szkielet-uniwersalny.md`.

## Granice publikacji

Paczka nie oznacza wdrożenia aplikacji. Nie wykonuje instalacji live, restartu
gatewaya, zmiany uprawnień, integracji Microsoft/Entra/Graph, merge ani deployu.
Publikacja na branchu jest osobnym readbackiem GitHuba; `main` nie jest zmieniany.
