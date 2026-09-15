# NAG-DOCS-CONSOLIDATION-Q1 — P5 indeks

STATUS: PASS
DOMAIN: INFORMACYJNY
TEMAT: NAG-DOCS-P5-INDEX-UPDATE-Q1
GOAL: Zaktualizować wyłącznie `NAGENTS-PROJECT.md`, aby kierował do właściwego pakietu i sekcji, rozróżniał snapshot lokalny, zdalny i runtime oraz zachował readback i proweniencję P1–P4.
OBSERVED_AT: 2026-09-14T15:07:31+00:00

## Zakres i allowlista

Zmieniono wyłącznie:

- `NAGENTS-PROJECT.md` — indeks nawigacyjny;
- `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P5-index.md` — ten raport.

Nie zmieniono źródeł P1–P4, nie usunięto ani nie przeniesiono dokumentów,
nie wykonano `git add`, commit, push, merge, deployu, fetch, pull, resetu,
stashu, clean ani restartu usługi.

## Wykonane uzupełnienia

Indeks zawiera teraz:

1. kolejność: indeks jako mapa, następnie `CLAUDE.md`, `tematy.md`, bieżący
   handoff, `docs/spec/README.md`, decyzje, scenariusze, właściwy MVP,
   dispatch/raport/artefakt i na końcu świeży readback;
2. mapę 16 pytań P3 w układzie pytanie → pakiet → sekcja, z rozróżnieniem
   pakietów obecnych i kandydatów P6;
3. lokalne/zdalne liczniki P1/P2, nazwane refy i 12 różnic remote↔local;
4. słownik statusów P4 oraz rozkład statusów rekordów i rodzin ścieżek;
5. granice nAgents, AutoBot Monitor i The-Game oraz statusy
   `OWNER_DECISION_REQUIRED`, `LIVE_READBACK_REQUIRED`, `INFRA/DECISION_REQUIRED`
   i `SEPARATE_PROJECT`;
6. zasady ograniczania kontekstu: metadane przed treścią, wyłączenia P1,
   exact-hash bez ponownego ładowania kopii, jeden temat/run naraz i brak
   traktowania raportu/UI/`queued` jako dowodu;
7. readback trzech driftów i wszystkich 12 rodzin wariantów P4;
8. linkowany rejestr artefaktów P1–P5 oraz minimalny readback przed użyciem.

Liczby wpisane do indeksu są zgodne z `P2-counts.json`: 695 rekordów lokalnych,
104 GitHub, 34 zewnętrzne, 833 razem, `LOCAL_ONLY` 132 i `REMOTE_ONLY` 0.
P4 pozostaje `PASS_WITH_EXPLICIT_OWNER_GATES`: 833 rekordy, 254 rodziny,
272 unikalne SHA-256, 105 rodzin exact-hash, 561 nadmiarowych rekordów,
12 rodzin wariantów i 3 pliki z driftem.

## Celowany readback przed zmianą

Ponownie odczytano bajty i metadane trzech plików z driftem oraz dwunastu rodzin
hash-variant wskazanych w P4. Potwierdzono m.in. bieżące hashe: ABM
`AUTOBOT-KANBAN.md` `64b99a0fbf76`, ABM `docs/ABM-CARD-TAGGING-GUIDE.md`
`c109d5f6015c`, a `NAGENTS-PROJECT.md` przed P5 `bd0637953c52`.
Dla wariantów zachowano hashe i liczniki linii w indeksie; nie wybrano zwycięzcy
na podstawie podobieństwa.

## Testy i readback końcowy

- `git diff --check`: PASS.
- Relacje Markdown: 36 odnośników, 26 unikalnych; po utworzeniu tego raportu
  wszystkie cele względne istnieją: PASS.
- JSON: odczyt i walidacja `P1-sources.json`, `P2-counts.json`,
  `P3-source-map.json` i `P4-classification.json`: PASS.
- Odtworzenie P2: `695 + 104 + 34 = 833`, `132 LOCAL_ONLY`, `0 REMOTE_ONLY`:
  PASS.
- Odtworzenie P4: `607 + 226 = 833`, `85 + 169 = 254`, rozkład statusów
  rekordów sumuje się do 833, a rozkład statusów rodzin do 254: PASS.
- P4 remote readback: 104/104 hashy zweryfikowanych read-only, 92 zgodne z
  bieżącym lokalnym snapshotem, 12 różne, 0 różnic zbioru linków: PASS.
- Skan treści zmienionych przez P5 pod kątem wartości credentiali, tokenów,
  kluczy, haseł, PII i realnych adresów IP: 0 dopasowań: PASS.
- P1/P4 safety: brak odczytu sekretów, credentiali, `state.db`, sesji i
  surowych logów; brak kopiowania PII; źródła P1–P4 pozostawiono bez zmian.

Końcowy SHA-256 `NAGENTS-PROJECT.md`:
`6d6f324135bd1c64c27548fb38b1d28ce068132108ce5afb50185d84e59b5d58`.

## Blokady i następny krok

P5 nie rozstrzyga bram właścicielskich dotyczących integracji, publikacji ani
przyszłych pakietów `NAGENTS-*`. Następny krok: P6 przygotowuje macierz
sekcja → źródło → pakiet → test → rollback; przed tą fazą należy ponowić
readback, ponieważ P2/P4 są snapshotami.

DEPLOY/PUSH: NIE WYKONANO
