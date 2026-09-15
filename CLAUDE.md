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

Opcjonalnie, gdy chcesz zrozumieć **dlaczego** projekt wygląda tak, jak wygląda:
[`docs/process/pamiec.md`](docs/process/pamiec.md) — kondensat rozumowania, warianty
odrzucone, pytania i odpowiedzi, popełnione błędy. To kontekst, nie routing.

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

## Główny kierunek produktu: web-first

**NAgents jest głównym projektem.** AutoBot Router, AutoBot Monitor, integracje z
Hermesem i przyszła nakładka Desktopu są elementami NAgents, nie osobnymi
produktami nadrzędnymi.

### Kolejność dostarczenia

1. **Etap pierwszy — bezpieczny dostęp webowy.** Znaleźć i potwierdzić prosty,
   bezpieczny sposób pracy przez serwerową wersję webową Hermesa albo przez
   webową warstwę NAgents. Pracownik ma otworzyć gotowy profil i czat bez
   znajomości gatewaya, serwera, profilu technicznego ani procesu wdrożenia.
   Zamknięcie przeglądarki nie może zatrzymać pracy serwera.
2. **Etap drugi — Desktop jako dodatek.** Dopiero po ustabilizowaniu ścieżki
   webowej przygotować nakładkę na Desktop Hermesa albo dostosować jego
   istniejącą powierzchnię do potrzeb NAgents. Desktop nie może być warunkiem
   życia procesu ani głównym miejscem zarządzania NAgents.

### Podział ról użytkowników

- **Pracownik** dostaje gotowy, przydzielony profil i czat. Otwiera rozmowę i
  pracuje; nie konfiguruje gatewaya, serwera, modeli, poświadczeń ani routingu.
- **Administrator** zarządza profilami, agentami, nadaniami, modelami,
  limitami, poświadczeniami, obiegiem i ustawieniami zaawansowanymi wyłącznie
  przez webową powierzchnię administracyjną NAgents/Hermesa albo terminal.
- **Serwer** jest właścicielem wykonania, sesji, kolejki, pamięci i audytu.
  Przeglądarka oraz Desktop są klientami, nie rodzicami procesu.

Hierarchia i użyteczność agentów mają być dostępne przede wszystkim w webie.
Widoczność elementu w interfejsie nie zastępuje kontroli dostępu na serwerze.

### Pomocnik procesu podczas nieobecności właściciela

Wariant A jest przyjęty: serwerowy pomocnik procesu odbiera dyspozycje Crona i
prowadzi jednoznaczne, wcześniej zatwierdzone przejścia Kanbana bez zależności
od Desktopu. Nie tworzy nowego zakresu ani nie rozstrzyga kwestii właściciela.
Przy braku dowodu lub niejednoznaczności zatrzymuje tylko właściwy strumień i
eskaluje go przez webową powierzchnię NAgents. Pętla jest uznana za działającą
dopiero po canary z zamkniętym Desktopem i readbackiem następnej fazy.

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
  zgody. W tym projekcie zgoda **została udzielona bezterminowo** decyzją
  **ECHO-001**, potwierdzoną **ECHO-003** — nie pytaj o nią co sesję.
  Reguła domyślna dotyczy projektów, w których takie ECHO nie zapadło.
- Każdy wybór dotyczący kosztu, danych, dostępu lub odwracalności trafia do
  dziennika decyzji **zanim** powstanie realizujący go kod.
