# 8gent — manifest paczki dokumentacji

Status: aktualizacja marki dokumentacji przygotowana do kontrolowanej publikacji
Branch docelowy publikacji: `docs/rename-8gent-2026-09-15`
Repozytorium: `maciejsieracki/nAgents`

## Zawartość główna

Poniższe dziewięć ksiąg zachowuje treść i identyfikatory pakietów, które przeszły
Operator → niezależny Evaluator → Final Control → lokalny P7. Ta gałąź dodaje
wyłącznie warstwę marki `8gent` w bieżących tekstach oraz osobną politykę
nazewnictwa; historyczne stagingi i raporty evidence pozostają niezmienione.
Hashe poniżej dotyczą plików z tej gałęzi. Nagłówki `stagingowy` w kopiach są
zachowane celowo, aby nie zgubić proweniencji pakietów.

| Plik | Zakres | SHA-256 |
|---|---|---|
| `NAGENTS-SPEC.md` | architektura, zakres MVP, bezpieczeństwo, scenariusze | `ac62ce18a6a0c825cba512ba22dd5bf31a8f803a864eb21f4f2c5278b58483cf` |
| `NAGENTS-DECISIONS.md` | decyzje D-001…D-013, ECHO, pytania, historia | `2640ee7083f075bf2543787e1b0caf8576dfb892f7522753f945981d8f70c58e` |
| `NAGENTS-PROCESS.md` | role, pętla, allowlista, evidence, watchdog, P1–P7 | `d81465d36951b27b7148d2d0ac43c69256ee82c7277d974ca4d96eeacfe09243` |
| `NAGENTS-HANDOFF.md` | format przekazania, blokady i następne bramki | `d00734d350829ab6b9c58ee1b78d366e13296ba7e4daccb5b9351d6dfe419a5e` |
| `NAGENTS-RESEARCH.md` | research, źródła, porównania i korekty | `6c39b0fd5f0aa8d4cc867452fa5f0d1babb15743d88db874efbfcb92d42169d1` |
| `NAGENTS-USER-GUIDE.md` | instrukcja pracownika i granica pracownik–administrator | `5dbbb7f2249453889c376dd23a8e710bbb8e0141985cab2509ad3f318e66fd80` |
| `AUTOBOT-PROJECT.md` | kontrakt AutoBot Monitor, routing i statusy | `75f70419a6dd63ce331252412990504eb8003595abdc919e13a818e1205ccb26` |
| `docs/ABM-HISTORY.md` | historyczny indeks kart, runów, eventów i artefaktów ABM | `27b73324a3af9c3ee8827bc2b75a191062d5e84f8129ae1ad1e26c83675a9dec` |
| `docs/ABM-LIFECYCLE.md` | manual installation/uninstall/rollback/backup | `ff7d0c8845a6f2fd5ecf03c3b10fdabbd64faafc3e9c4a7c3c9bdc77fbbd2b07` |

## Plik wejściowy dla następnego agenta

`NAGENTS-PROJECT.md` jest kanonicznym indeksem wejściowym. Zawiera:

- kolejność czytania plików;
- mapę pytanie → pakiet → sekcja → źródło pierwszeństwa;
- procedurę wyszukiwania po identyfikatorach `SPEC-*`, `DEC-*`, `PROCESS-*`,
  `HANDOFF-*`, `RESEARCH-*`, `USER-*`, `ABM-*` i `LIFE-*`;
- rozdzielenie faktów z dokumentów od świeżego stanu Git/Kanbana/usług;
- granice 8gent, AutoBot Monitor i The-Game;
- zasady zachowania dokumentów historycznych i sprzątania.

## Dowody i odtwarzalność

Do paczki dołączone są:

- wszystkie katalogi `docs/process/staging/NAG-CONSOLIDATE-*`;
- `NAGENTS-CONSOLIDATION-PLAN.md`;
- `docs/process/NAGENTS-DOCS-CONSOLIDATION-PLAN.md`;
- `docs/process/NAGENTS-BOARD-SEPARATION-HANDOFF.md`;
- `docs/NAGENTS-DOCUMENTATION-PACKAGE-CONTENTS.sha256`;
- `docs/N8GENT-BRAND-NAMING.md`;
- `docs/SECURITY-REDACTIONS.md`;
- audyt P1–P5 w `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/`;
- `docs/CONSOLIDATION-CLEANUP-MANIFEST.md`.

Do archiwum trafia również bezpieczny snapshot dokumentacji i maszynowych
manifestów z bieżącego checkoutu 8gent (`*.md` oraz audytowe `*.json`,
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
