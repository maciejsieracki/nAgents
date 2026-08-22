# nAgents — punkt startowy

Warstwa zarządzania nad flotą instancji Hermesa. Ten plik jest **źródłem prawdy
dla procesu** i pierwszą rzeczą do przeczytania w każdej sesji.

## Kolejność czytania — obowiązkowa

| # | Plik | Po co |
|---|---|---|
| 1 | ten plik | mapa źródeł i norma procesu |
| 2 | [`docs/process/tematy.md`](docs/process/tematy.md) | rejestr tematów: co aktywne, co zablokowane |
| 3 | [`docs/process/handoff.md`](docs/process/handoff.md) | bieżący handoff |
| 4 | [`docs/spec/README.md`](docs/spec/README.md) | skrót projektu i etapów |
| 5 | [`docs/spec/decisions.md`](docs/spec/decisions.md) | decyzje, w tym **otwarte i blokujące** |
| 6 | [`docs/spec/scenarios.md`](docs/spec/scenarios.md) | scenariusze — źródło planów testów |
| 7 | `docs/spec/0X-mvpX.md` | specyfikacja bieżącego etapu |

**Nie zaczynaj** od samego czatu, starego handoffu ani od `docs/nota-*.md`.
Notatki decyzyjne są historią rozważań — pokazują, *dlaczego* projekt wygląda
tak, jak wygląda, ale nie są aktywnym routingiem.

## Proces

Projekt pracuje procesem **AutoBot**. Dwa skille, czytane w tej kolejności:

1. [`.claude/skills/autobots/SKILL.md`](.claude/skills/autobots/SKILL.md) — szkielet
   uniwersalny: role, pętla, dyscyplina ABC/ECHO, watchdog, kontrakt raportu
2. [`.claude/skills/nagents-autobot/SKILL.md`](.claude/skills/nagents-autobot/SKILL.md) —
   wiązania dla tego projektu: format ID, allowlisty, izolacja, testy, limity, bariery

Przy konflikcie wygrywa skill projektowy — ale konflikt zgłoś właścicielowi,
nie rozstrzygaj po cichu.

## Czym ten system jest, a czym nie

**Jest** warstwą zarządzania: kto to jest, do czego ma prawo, ile mu wolno wydać,
co po sobie zostawił.

**Nie jest** silnikiem agenta — tym jest Hermes. Nie jest komunikatorem — tym jest
Teams. Nie hostuje modeli. **Nie goni parytetu funkcji z gotowymi platformami.**

## Siedem barier, których nie wolno przekroczyć

Wynikają z modelu bezpieczeństwa w [`docs/spec/00-architektura.md`](docs/spec/00-architektura.md).
Naruszenie którejkolwiek oznacza `FAIL`, niezależnie od jakości reszty pracy.

1. Żadnych wartości sekretów w repozytorium — wyłącznie `vault_ref`
2. Agent rodzaju `stanowiskowy` nie ma własnych poświadczeń do systemów firmowych
3. Brak dostępu zwraca **404, nie 403** — komunikat o odmowie ujawnia istnienie zasobu
4. Domyślna odmowa — nigdy „wszyscy mogą, chyba że"
5. Nigdy `git add -A` ani `git add .` — integracja allowlist-only
6. Żadnych prawdziwych danych osobowych poza `prod`
7. Push wyłącznie na gałąź wskazaną przez właściciela

## Stan na dziś

- **Etap:** przed MVP1. Dokumentacja gotowa, kod nie istnieje.
- **Gałąź robocza:** `claude/git-connection-9sz6dg`
- **Blokada:** decyzja **D-011** — rezydencja wspólnej pamięci. MVP1 i MVP2
  działają bez niej; MVP3 nie startuje bez odpowiedzi.
- **Decyzja odroczona:** **D-010** — topologia agentów. Rozstrzygnięcie po MVP1,
  na danych z realnego użycia. Rejestr obsługuje oba warianty.
- **Zależność zewnętrzna:** rejestracja aplikacji w Entra ID wymaga uprawnień
  administratora dzierżawy. Blokuje MVP1 od dnia trzeciego — załatwić wcześniej.

## Zasady pracy z właścicielem

- Decyzje zapadają **wyłącznie w głównym wątku**. Subagenty są kanałami
  technicznymi, nie partnerami do rozstrzygnięć produktowych.
- Odpowiedź „chyba tak" nie jest decyzją. ECHO zapisuje się dopiero po
  jednoznacznej odpowiedzi literą — patrz [`docs/process/echo.md`](docs/process/echo.md).
- Orkiestracja wieloagentowa jest **domyślnie wyłączona** i wymaga jawnej
  zgody na daną sesję.
- Każdy wybór dotyczący kosztu, danych, dostępu lub odwracalności trafia do
  dziennika decyzji **zanim** powstanie realizujący go kod.
