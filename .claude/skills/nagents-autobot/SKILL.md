---
name: nagents-autobot
description: >-
  Wiązania procesu AutoBot dla projektu nAgents: format ID tematu, zapis dispatchu,
  checklisty ról Operator/Evaluator/Final Control, allowlisty, izolacja przez worktree,
  plan testów oparty na scenarios.md, effort per rola, limit rund, PASS-WITH-NOTES,
  manual resume, watchdog, kontrakt raportu, szablon pytania ABC, rejestr ECHO oraz
  siedem twardych barier. Używaj razem ze skillem `autobots` przy rozpoczęciu pracy,
  przejęciu tematu, dispatchu, kontroli statusu i przygotowaniu integracji w nAgents.
---

# nAgents — wiązania procesu AutoBot

Towarzysz skilla `autobots`. Szkielet mówi *jak* prowadzić proces; ten dokument
mówi *czym dokładnie* są w nAgents ID, dispatch, role, allowlista, izolacja,
testy, limity, bariery i rejestry.

**Kolejność:** przeczytaj `autobots`, potem ten dokument. Przy konflikcie wygrywa
ten dokument — ale konflikt zgłoś właścicielowi, nie rozstrzygaj po cichu.

## 0. Mapa zgodności ze szkieletem

Numeracja poniżej odpowiada numeracji szkieletu, żeby dało się sprawdzić, że
**żadna zasada nie została pominięta**. Szkielet jest w całości projektowo
neutralny — jedyne odwołanie do innego projektu to przykład `civ-autobot`
w zdaniu zamykającym. Nic nie zostało wyłączone.

| Szkielet | Wiązanie w nAgents |
|---|---|
| 1 · Zasada nadrzędna | §1 — definicja `READY_FOR_DEPLOY` i lista rzeczy, które nim nie są |
| 2 · Start sesji | §2 — kolejność czytania, `docs/process/zmiana-procesu.md`, meldunek |
| 3 · Orkiestracja | §3 — domyślnie wyłączona, warunek włączenia |
| 4 · Routing ról | §4 — checklisty per rola |
| 5 · Pętla | §5 — ID, zapis dispatchu, limit 3, PASS-WITH-NOTES, resume, pauza |
| 6 · ABC/ECHO | §6 — kiedy obowiązkowe, szablon pytania, rejestr |
| 7 · Kontrakt raportu | §7 — domeny i statusy projektu |
| 8 · Watchdog | §8 — ZWIS 20 min, pula 2, obsadzanie slotów |
| 9 · Dobre praktyki | §9 — siedem barier, recon, przegląd diffu, bramka push |
| 10 · Meldunek startowy | §10 — wzór dla tego projektu |

---

## 1. Zasada nadrzędna

Każdy temat ma: pełne ID, jawny `GOAL`, mierzalne kryteria końca wskazujące
numery scenariuszy, allowlistę, izolację i plan testów. Bez kompletu — nie ma dispatchu.

### `READY_FOR_DEPLOY` w nAgents oznacza łącznie

- zmiana wyłącznie w zatwierdzonej allowliście
- `pytest` zielony w całości
- **scenariusze z kryteriów końca przechodzą sprawdzone ręcznie**, nie tylko automatycznie
- żadna z siedmiu barier (§9) nienaruszona
- brak nowych wartości sekretów w repozytorium
- `docs/process/tematy.md` zaktualizowany
- zmiana faktycznie zintegrowana przez orkiestratora

### Co NIE oznacza zakończenia

Wymieniam wprost, bo to najczęstsze źródło fałszywego „gotowe":

| To nie jest dowód | Dlaczego |
|---|---|
| Raport `PASS` | Raport opisuje pracę, nie jej skutek w repozytorium |
| Istnienie gałęzi lub worktree | Nazwa katalogu nie jest stanem |
| Commit | Commit to zapis, nie integracja |
| Widoczny status subagenta | UI pokazuje przebieg, nie wynik |
| Deklaracja agenta „zrobione" | Deklaracja bez artefaktu jest niczym |
| Brak artefaktu | Brak dowodu to nie jest dowód |

**Push pozostaje osobną bramką** po `READY_FOR_DEPLOY`, wymaga wyraźnego polecenia
właściciela i idzie **wyłącznie na gałąź przez niego wskazaną**.

## 2. Start sesji

### Kolejność czytania — obowiązkowa

| # | Plik | Po co |
|---|---|---|
| 1 | `CLAUDE.md` | mapa źródeł i norma procesu |
| 2 | `docs/process/tematy.md` | rejestr tematów: co aktywne, co zablokowane |
| 3 | `docs/process/handoff.md` | bieżący handoff |
| 4 | `docs/spec/README.md` | skrót projektu i etapów |
| 5 | `docs/spec/decisions.md` | decyzje, w tym **otwarte i blokujące** |
| 6 | `docs/spec/scenarios.md` | scenariusze — źródło planów testów |
| 7 | `docs/spec/0X-mvpX.md` | specyfikacja bieżącego etapu |

**Nie zaczynaj** od starego handoffu, samego czatu, artefaktów zamkniętych tematów
ani od `docs/nota-*.md` — notatki decyzyjne są historią rozważań, nie routingiem.

### Zmiana samego mechanizmu procesu

Szkielet wymaga osobnego dokumentu na wypadek modyfikowania AutoBota zamiast kodu
produktu. W nAgents jest nim **[`docs/process/zmiana-procesu.md`](../../../docs/process/zmiana-procesu.md)**.
Przeczytaj go **przed** dotknięciem `.claude/skills/**`, `CLAUDE.md` lub
`docs/process/**` — te ścieżki mają własny, ostrzejszy tryb.

## 3. Orkiestracja wieloagentowa

**Domyślnie WYŁĄCZONA.** Wymaga jednoczesnego spełnienia dwóch warunków:

1. narzędzie pozwalające przypisać model/effort per rola jest dostępne
2. właściciel dał **jawną zgodę na tę sesję**

Zgodą jest zdanie w rodzaju „zgoda na orkiestrację wieloagentową w tej sesji".
**Brak takiego zdania = brak zgody.** Nie domniemywaj jej z rozmiaru tematu,
z pośpiechu ani z tego, że poprzednia sesja ją miała.

Bez obu warunków role różnicujemy **wyłącznie treścią promptu**, w jednym wątku.

### Effort per rola — gdy orkiestracja jest włączona

| Rola | Effort |
|---|---|
| Operator | wysoki |
| Evaluator | wysoki |
| Final Control | wysoki |
| Orkiestrator | model sesji |

## 4. Routing ról i checklisty

```text
Operator → Evaluator → Final Control → integracja → READY_FOR_DEPLOY → bramka push
```

### Operator
Wykonuje jeden temat w izolacji, wyłącznie w allowliście.
**Nie ocenia własnej pracy, nie integruje, nie pushuje.**

Kończy raportem terminalnym z kontraktu §7. Jeśli natrafi na decyzję
produktową — zatrzymuje się ze statusem `DECISION_REQUIRED`, nie rozstrzyga sam.

### Evaluator — checklista dla nAgents
Niezależny adwokat diabła. **Nie integruje, nie publikuje.** Sprawdza:

1. Czy diff mieści się w allowliście — co do pliku
2. Czy nie narusza żadnej z siedmiu barier (§9)
3. Czy scenariusze z kryteriów końca faktycznie przechodzą — **nie czy raport tak twierdzi**
4. Czy zmiana dotyka uprawnień choćby pośrednio; jeśli tak, czy A2 i A3 przechodzą
5. Czy w diffie nie ma wartości sekretów, także w testach i przykładach
6. Czy nie ma usunięć, których GOAL nie wymagał
7. Czy nie nakłada się z drugim aktywnym tematem
8. Czy `pytest` jest zielony na faktycznym stanie drzewa, a nie w raporcie

### Final Control — checklista dla nAgents
Zawsze **osobny subagent**, nigdy główny agent. **Nie wystawia `READY_FOR_DEPLOY`.**
Kontroluje kompletność śladu:

1. Czy istnieje zapis dispatchu (§5) i czy GOAL się nie zmienił po drodze
2. Czy ID jest to samo we wszystkich rundach
3. Czy werdykt Evaluatora jest oparty na artefaktach, nie na deklaracjach
4. Czy `PASS-WITH-NOTES` nie ukrywa uwagi dotyczącej GOAL, testów, zakresu,
   bezpieczeństwa, dowodu lub gotowości do integracji
5. Czy licznik rund się zgadza i nie został po cichu zresetowany
6. Czy `docs/process/tematy.md` odzwierciedla stan faktyczny

### Orkiestrator
Działa w głównym czacie. Integruje **wyłącznie zatwierdzoną allowlistę**,
per plik, w razie potrzeby per hunk. Jako **jedyny** wystawia `READY_FOR_DEPLOY`,
i dopiero **po faktycznej integracji** — nie po pozytywnym Final Control.

Przed integracją sprawdza rzeczywisty stan: `git status`, `git diff`, wynik testów,
raporty, allowlistę.

### Właściciel
Odpowiada na decyzje **wyłącznie w głównym czacie orkiestratora**.
Subagenty są kanałami technicznymi — nie prowadź z nimi osobnych rozstrzygnięć
produktowych i nie przyjmuj od nich decyzji za właściciela.

## 5. Pętla

```text
dispatch → Operator → Evaluator → Final Control → integracja → READY_FOR_DEPLOY
                 ↑          ↑             ↑
                 └──────────┴─────────────┘
      FAIL / BLOCK / TIMEOUT / INFRA / ZWIS / brak dowodu
```

### Format ID

```
NAG-<ETAP>-<NNN>-<slug>
```

`ETAP` ∈ `MVP1` `MVP2` `MVP3` `MVP4` `PROC` `INFRA`.
`NNN` trzycyfrowy, rosnący w obrębie etapu, **nigdy nieużywany ponownie**.
ID jest niezmienne przez wszystkie rundy. Pytanie decyzyjne:
`NAG-MVP1-003-uprawnienia-Q2` — nigdy samo „Q2".

### Zapis dispatchu — przed dispatchem, nie po

Plik `docs/process/dispatch/<ID>.md`, tworzony **zanim** ruszy Operator.
Szablon: `docs/process/dispatch/SZABLON.md`. Zawiera pełne ID, GOAL, kryteria
końca z numerami scenariuszy, zakres, allowlistę, izolację, bazę worktree
i plan testów.

**Dispatch bez tego pliku jest naruszeniem procesu.** Bez niego nie da się
później sprawdzić, czy GOAL się nie przesunął w trakcie.

### Przebieg

1. Zapis dispatchu → Operator
2. Terminalny raport Operatora → **natychmiast** zamknij przebieg i uruchom
   Evaluatora dla tego samego ID
3. `PASS` uruchamia Final Control **bez czekania na dodatkową zgodę**
4. Pozytywny Final Control → orkiestrator sprawdza faktyczny stan repozytorium
   i integruje
5. Dopiero po **faktycznej integracji** orkiestrator zapisuje `READY_FOR_DEPLOY`
6. Każdy `FAIL`, `BLOCK`, `TIMEOUT`, `INFRA`, `ZWIS`, brak artefaktu lub błąd
   izolacji wraca do Operatora, potem Evaluatora i Final Control — z tym samym ID

### `PASS-WITH-NOTES`

**Nie kończy procesu**, jeśli uwagi dotyczą któregokolwiek z: kryterium GOAL,
testów, zakresu, bezpieczeństwa, dowodu lub gotowości do integracji.
W takim wypadku temat wraca do Operatora jak przy `FAIL`.

Kończy proces tylko wtedy, gdy uwagi są kosmetyczne i zapisane jako osobny temat.

### Limit rund

**3 rundy na temat.** Szkielet podaje 5 jako przykład konfigurowalny; w nAgents
przyjmujemy 3, bo przy jednej osobie technicznej wcześniejsza eskalacja jest
tańsza niż czwarta pętla.

Po trzeciej rundzie orkiestrator **zatrzymuje temat i zgłasza właścicielowi**:
co próbowano, co zawiodło, jakie są warianty. **Cichy reset licznika jest
naruszeniem procesu.**

### Manual resume

Wymaga jawnej decyzji właściciela i **zachowuje ID, licznik rund i ostatni werdykt**.
Wznowienie nie jest nowym tematem i nie zeruje historii.

### Pauza

Jedyna normalna pauza to **oczekiwanie na decyzję właściciela**.
Pauzuje **wyłącznie temat, który jej wymaga** — pozostałe niezależne tematy
pracują dalej. Zatrzymywanie całej pracy z powodu jednego pytania jest błędem.

## 6. Dyscyplina ABC/ECHO

### Kiedy formalna decyzja jest obowiązkowa

Gdy zmiana dotyka realnego kompromisu produktowego lub architektonicznego —
w nAgents konkretnie:

- modelu uprawnień lub sposobu ich egzekwowania
- zakresu danych wysyłanych do dostawcy modelu
- budżetów, limitów i zachowania po przekroczeniu
- retencji, audytu lub realizacji praw osób
- topologii agentów (**D-010**) i rezydencji pamięci (**D-011**)
- wyboru silnika, bramy modeli lub bazy

Dla drobnej implementacji **w ramach** już przyjętej decyzji — nie jest wymagana.

### Szablon pytania — wszystkie pola obowiązkowe

```text
PYTANIE: NAG-<ETAP>-<NNN>-<slug>-Q<n>
SYTUACJA:      <co się dzieje, stan faktyczny>
CEL PYTANIA:   <co rozstrzygamy>
DLACZEGO TERAZ:<co blokuje, jeśli nie rozstrzygniemy>

WARIANT A: <opis>
  ZA:      1) …  2) …          ← minimum dwa
  PRZECIW: 1) …  2) …          ← minimum dwa
WARIANT B: <opis>
  ZA:      1) …  2) …
  PRZECIW: 1) …  2) …
WARIANT C: <opis>
  ZA:      1) …  2) …
  PRZECIW: 1) …  2) …

REKOMENDACJA: <litera + jedno zdanie uzasadnienia>
KONSEKWENCJE IMPLEMENTACYJNE: <co trzeba będzie napisać albo przepisać>
KONSEKWENCJE TESTOWE: <które scenariusze się zmieniają lub dochodzą>
```

Pytanie bez dwóch argumentów za i dwóch przeciw dla **każdego** wariantu
jest niekompletne — uzupełnij przed zadaniem.

### ECHO

**Nie zamieniaj odpowiedzi „chyba", luźnej rozmowy ani rekomendacji agenta
w formalną decyzję.** Rekomendacja nie staje się decyzją przez brak sprzeciwu.

ECHO zapisuje się **dopiero po jednoznacznej odpowiedzi literą**, w
`docs/process/echo.md`, w formacie `NAG-MVP1-003-Q2 = B` z datą i autorem.
Dopiero potem kontynuuj ten sam ID.

Nie numeruj pytań ponownie tak, by kolidowały z wcześniejszymi.

| Artefakt | Plik | Kiedy |
|---|---|---|
| ECHO | `docs/process/echo.md` | zawsze po odpowiedzi literą |
| ADR | `docs/spec/decisions.md` | dodatkowo, gdy decyzja zmienia architekturę |

## 7. Kontrakt raportu

```text
STATUS: PASS | PASS-WITH-NOTES | FAIL | BLOCK | TIMEOUT | INFRA | DECISION_REQUIRED
DOMAIN: PRODUKT | PROCES | INFRA | INFORMACYJNY
TEMAT:  NAG-<ETAP>-<NNN>-<slug>
GOAL:   <jedno zdanie>
ZMIANY: <pliki z allowlisty + SHA commita albo „brak zmian">
TESTY:  <wynik pytest + numery scenariuszy + wynik sprawdzenia ręcznego>
BLOKADY: <lista albo „brak">
NASTĘPNY KROK: <kolejna bramka>
DEPLOY/PUSH: NIE WYKONANO
```

`DEPLOY/PUSH` domyślnie `NIE WYKONANO`. `WYKONANO` wpisuje wyłącznie orkiestrator,
po jawnym poleceniu właściciela i wyłącznie na wskazaną gałąź.

**Status nie zmienia się** na podstawie nazwy worktree, interfejsu, deklaracji
agenta ani nieistniejącego raportu.

### Domeny

| Domena | Co obejmuje |
|---|---|
| `PRODUKT` | kod uprzęży, rejestr, migracje, interfejs |
| `PROCES` | `.claude/skills/**`, `CLAUDE.md`, `docs/process/**` |
| `INFRA` | wdrożenie, kontenery, baza, brama modeli, kopie zapasowe |
| `INFORMACYJNY` | analiza, rozpoznanie, dokumentacja bez zmiany zachowania |

## 8. Watchdog

| Parametr | Wartość |
|---|---|
| Jeden temat | **jeden aktywny przebieg Operatora** |
| Brak ruchu → `ZWIS` | 20 minut |
| Aktywna pula tematów | **2 równolegle** |

Pula wynosi 2, bo wszystko przegląda jedna osoba; trzeci równoległy temat
przekracza pojemność przeglądu i kończy się integracją bez realnej kontroli.

**Obsadzanie slotów:** gdy istnieje niezablokowana praca, a slot jest wolny —
obsadź go. Zostawienie wolnego zasobu przez przeoczenie jest błędem tak samo
jak przeciążenie.

**Po raporcie terminalnym** zwolnij slot i uruchom następny etap natychmiast.

**Przy `ZWIS`:** sprawdź transcript, stan repozytorium, worktree i artefakty
**zamiast zgadywać**. Nie anuluj i nie restartuj w ciemno — orkiestrator
przejmuje temat.

## 9. Dobre praktyki i twarde bariery

### Siedem barier — naruszenie oznacza `FAIL`

Niezależnie od jakości reszty pracy. Wynikają z modelu bezpieczeństwa
w `docs/spec/00-architektura.md`.

1. **Żadnych wartości sekretów w repozytorium.** W rejestrze wyłącznie `vault_ref`.
   Sekret w diffie = `FAIL` i rotacja klucza.
2. **Agent rodzaju `stanowiskowy` nie ma własnych poświadczeń.** Niepusta lista
   `secrets` = `FAIL` na poziomie walidatora rejestru.
3. **Brak dostępu zwraca 404, nie 403.** Zmiana tego zachowania wymaga ECHO.
4. **Domyślna odmowa.** Kod dodający ścieżkę „wszyscy mogą, chyba że" = `FAIL`.
5. **Nigdy `git add -A` ani `git add .`** — integracja allowlist-only, per plik,
   w razie potrzeby per hunk. Szczególnie niebezpieczne w drzewie współdzielonym
   albo przy cudzej pracy.
6. **Żadnych prawdziwych danych osobowych poza `prod`.**
7. **Push wyłącznie na gałąź wskazaną przez właściciela.**

### Allowlisty

| Obszar | Typowa allowlista |
|---|---|
| Uprząż — logika | `app/**`, `tests/**` |
| Rejestr agentów | `registry/agents.yaml`, `app/registry/**` |
| Migracje | `migrations/**`, `app/models/**` |
| Wiedza | `knowledge/**` |
| Dokumentacja | `docs/spec/**` (poza `decisions.md`) |
| Proces | `docs/process/**`, `.claude/skills/**`, `CLAUDE.md` |

**Nigdy w allowliście:** `.env*`, dowolny plik z wartościami sekretów,
`docs/spec/decisions.md` (zmienia go wyłącznie orkiestrator po ECHO),
`.git/**`, konfiguracja produkcyjna bez jawnej zgody właściciela.

### Izolacja

```
worktree:  ../nagents-<ID>
branch:    auto/<ID>
baza:      gałąź robocza wskazana przez właściciela — nie zakładaj `main`
```

Worktree usuwany po integracji albo po zamknięciu tematu.

### Plan testów

```text
1. pytest -q                       # całość zielona
2. pytest tests/<obszar> -v        # obszar tematu
3. scenariusze z kryteriów końca   # ręcznie, na dev
4. przy zmianie uprawnień: A2 i A3 obowiązkowo, bezwarunkowo
5. przy zmianie wiedzy: pełny zestaw testów agenta (od MVP3)
```

Punkt 4 obowiązuje także wtedy, gdy temat dotyka uprawnień tylko pośrednio.

### Recon przed kodowaniem

```bash
git status && git branch --show-current
grep -rn "<pojęcie z GOAL>" app/ docs/spec/
```

Plus lektura: rejestr tematów (kolizje z aktywnym), dziennik decyzji
(czy decyzja już to przesądza), scenariusze (czy scenariusz istnieje).

### Przed integracją

Przejrzyj diff **również pod kątem usunięć**, nakładania się z drugim aktywnym
tematem i regresji względem pracy równoległej. Usunięcie, którego GOAL nie
wymagał, jest sygnałem ostrzegawczym.

## 10. Meldunek startowy

```text
Przeczytałem: CLAUDE.md, rejestr tematów, handoff, README specyfikacji,
dziennik decyzji, scenariusze, specyfikację bieżącego etapu.

Stan: <etap; tematy aktywne z pełnym ID i statusem>
Blokady: <lista, w tym decyzje otwarte>
Następna bramka: <co dokładnie>
Orkiestracja wieloagentowa: <wyłączona / włączona zgodą z dnia …>

Nie zaczynam zmian, dopóki nie potwierdzę właściwego ID, GOAL, allowlisty
i decyzji wymaganych od właściciela. Pracuję wyłącznie w bieżącym, czystym
worktree.
```
