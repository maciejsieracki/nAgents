# 8gent — manifest paczki dokumentacji

Status: aktualizacja architektury OpenClaw przygotowana do kontrolowanej publikacji
Branch źródłowy publikacji: `docs/openclaw-foundation-2026-09-15`
Bazowy `main` przed zmianą: `9e3abaec55673c65534a7b8a67806967bfa62df7`
Repozytorium: `maciejsieracki/nAgents`

## Zawartość główna

Poniższe księgi zachowują treść i identyfikatory wcześniejszych pakietów, które
przeszły Operator → niezależny Evaluator → Final Control → lokalny P7. Ta gałąź
wprowadza aktualną decyzję OpenClaw, mapowanie wcześniejszego wariantu i
warstwę marki `8gent`; historyczne stagingi i raporty evidence pozostają
niezmienione. Hashe poniżej dotyczą plików z tej gałęzi. Nagłówki `stagingowy` w
kopiach są zachowane celowo, aby nie zgubić proweniencji pakietów.

| Plik | Zakres | SHA-256 |
|---|---|---|
| `NAGENTS-SPEC.md` | architektura, zakres MVP, bezpieczeństwo, scenariusze | `37a7ae859c36b4dd7535e96eb143486deb56af7e0cf96e31132486cc9e9d9f2d` |
| `docs/OPENCLAW-STRATEGY.md` | aktualna podstawa OpenClaw, mapowanie Hermes → OpenClaw, AutoBot plugin boundary | `19726bb8dcb58ef3958777a030a2a1bf81c4bf767618dd3b0fcef9a7c1973ac9` |
| `NAGENTS-DECISIONS.md` | decyzje D-001…D-014, ECHO, pytania, historia | `2f09069de8e9734110429a1d16c812f2b45bb0ce25dc807de976857ad8c1fedf` |
| `NAGENTS-PROCESS.md` | role, pętla, allowlista, evidence, watchdog, P1–P7 | `56656e16506b74db6e0efafc65757c12f97f2ba2a5bbd0575ad5194cab8bb072` |
| `NAGENTS-HANDOFF.md` | format przekazania, blokady i następne bramki | `160c9615498b4759538d0a4944d79723302899b2a7eb9769a932601ddde38b1b` |
| `NAGENTS-RESEARCH.md` | research, źródła, porównania i korekty | `df8f9c7de6810fe98ea4490ba45c25814139ed46d93382e3509cd100248e7af2` |
| `NAGENTS-USER-GUIDE.md` | instrukcja pracownika i granica pracownik–administrator | `9291cb09731bb62b03c37560a81f48308d6e7b3e0dba5321268e6b41ba3d1dc3` |
| `AUTOBOT-PROJECT.md` | kontrakt AutoBot Monitor, routing i statusy | `6ff35fb9b7400e7065ecd202c39842956b0cc3a744057276f9dec5627ac0eaa7` |
| `docs/ABM-HISTORY.md` | historyczny indeks kart, runów, eventów i artefaktów ABM | `27b73324a3af9c3ee8827bc2b75a191062d5e84f8129ae1ad1e26c83675a9dec` |
| `docs/ABM-LIFECYCLE.md` | manual installation/uninstall/rollback/backup | `e82ce1431639111983fae3d8e3db7209c0441d576102cdec51954d86ceff6494` |

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
- `docs/OPENCLAW-STRATEGY.md`;
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

Paczka nie oznacza wdrożenia aplikacji ani instalacji OpenClaw. Nie wykonuje
restartu Gatewaya, zmiany uprawnień, konfiguracji providerów, integracji
Microsoft/Entra/Graph, plugin install ani deployu. Publikacja tej aktualizacji
wymaga osobnego readbacku branchu/PR; obecny dokument nie twierdzi, że zmiana
jest już na `main`.
