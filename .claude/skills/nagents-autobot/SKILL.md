---
name: nagents-autobot
description: >-
  Kompletny proces pracy w projekcie nAgents — samowystarczalny, nie wymaga innych
  skilli. Role Operator/Evaluator/Final Control/Orkiestrator, pętla tematu z pełnym ID,
  zapis dispatchu, allowlisty, izolacja przez worktree, plan testów oparty na
  scenariuszach, siedem twardych barier, dyscyplina decyzyjna ABC/ECHO, kontrakt
  raportu, watchdog, dyscyplina źródeł i zakresu, konwencje repozytorium oraz szybki
  start. Używaj przy każdym wejściu w projekt, przejęciu tematu, dispatchu, kontroli
  statusu, przygotowaniu integracji i zmianie samego procesu.
---

# nAgents AutoBot — proces pracy

**Ten dokument jest kompletny.** Nie odsyła do innego skilla procesowego i nie
wymaga go do działania. Wywodzi się z uniwersalnego szkieletu AutoBot (kopia
źródłowa: `docs/process/zrodla/autobots-szkielet-uniwersalny.md`), ale wartości
konfigurowalne są tu już rozstrzygnięte, a rzeczy nieprzydatne w tym projekcie
usunięte.

---

## 0. Szybki start — pięć minut

```text
1. Przeczytaj ten dokument w całości. Jest długi, ale krótszy niż koszt błędu.
2. Przeczytaj siedem plików z §2 w podanej kolejności.
3. Sprawdź stan:  git status && git branch --show-current
4. Napisz meldunek startowy (§2.3) i CZEKAJ na potwierdzenie właściciela.
5. Nie dotykaj kodu przed potwierdzeniem ID, GOAL i allowlisty.
```

### Pięć rzeczy do wiedzy od razu

1. **To nie jest projekt agenta.** Budujemy warstwę zarządzania nad Hermesem.
   Jeśli piszesz kod robiący to, co Hermes już robi — zatrzymaj się.
2. **Domyślna odmowa wszędzie** — uprawnienia, narzędzia, dane.
   Nigdy „wszyscy mogą, chyba że".
3. **Dwie decyzje są otwarte i blokujące** — D-010 (topologia agentów)
   i D-011 (rezydencja pamięci). Nie rozstrzygaj ich w kodzie.
4. **Orkiestracja wieloagentowa jest wyłączona**, dopóki właściciel nie włączy
   jej jawnie na daną sesję (§11).
5. **Siedem barier (§7) oznacza `FAIL`**, niezależnie od jakości reszty pracy.

### Trzy najczęstsze sposoby zepsucia tego projektu

| Sposób | Objaw | Zapobieganie |
|---|---|---|
| Rozjazd zakresu | temat rośnie w trakcie rundy | §13.3 — nowy pomysł to nowy temat |
| Fałszywe „gotowe" | raport `PASS` bez sprawdzonego scenariusza | §1.2 — co nie jest dowodem |
| Cicha decyzja w kodzie | wybór o danych zapadł w implementacji | §14.2 — ujawnij wybór wcześniej |

## 1. Zasada nadrzędna

Każdy temat ma: **pełne ID, jawny `GOAL`, mierzalne kryteria końca wskazujące
numery scenariuszy, allowlistę, izolację i plan testów.** Bez kompletu nie ma dispatchu.

### 1.1 `READY_FOR_DEPLOY` oznacza łącznie

- zmiana wyłącznie w zatwierdzonej allowliście
- `pytest` zielony w całości
- **scenariusze z kryteriów końca sprawdzone ręcznie**, nie tylko automatycznie
- żadna z siedmiu barier nienaruszona
- brak nowych wartości sekretów w repozytorium
- `docs/process/tematy.md` odzwierciedla stan faktyczny
- zmiana **faktycznie zintegrowana** przez orkiestratora

**Push pozostaje osobną bramką** po `READY_FOR_DEPLOY`. Wymaga wyraźnego polecenia
właściciela i idzie **wyłącznie na gałąź przez niego wskazaną**. Operator, Evaluator
i Final Control nigdy nie pushują.

### 1.2 Co NIE jest dowodem zakończenia

Najczęstsze źródło fałszywego „gotowe":

| To nie jest dowód | Dlaczego |
|---|---|
| Raport `PASS` | opisuje pracę, nie jej skutek w repozytorium |
| Gałąź albo worktree | nazwa katalogu nie jest stanem |
| Commit | zapis, nie integracja |
| Widoczny status subagenta | interfejs pokazuje przebieg, nie wynik |
| Deklaracja „zrobione" | deklaracja bez artefaktu jest niczym |
| Brak artefaktu | brak dowodu to nie jest dowód |

## 2. Start sesji

### 2.1 Kolejność czytania — obowiązkowa

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
ani od `docs/nota-*.md`. Notatki decyzyjne są historią rozważań, nie routingiem.

### 2.2 Gdy zmieniasz sam proces

Przed dotknięciem `.claude/skills/**`, `CLAUDE.md` lub `docs/process/**` przeczytaj
**`docs/process/zmiana-procesu.md`**. Te ścieżki mają własny, ostrzejszy tryb —
błąd w kodzie produktu wyłapie Evaluator, błąd w definicji Evaluatora nie wyłapie nikt.

### 2.3 Meldunek startowy

```text
Przeczytałem: CLAUDE.md, rejestr tematów, handoff, README specyfikacji,
dziennik decyzji, scenariusze, specyfikację bieżącego etapu.

Stan: <etap; tematy aktywne z pełnym ID i statusem>
Blokady: <lista, w tym decyzje otwarte>
Następna bramka: <co dokładnie>
Orkiestracja wieloagentowa: <wyłączona / włączona zgodą z dnia …>

Nie zaczynam zmian, dopóki nie potwierdzę ID, GOAL, allowlisty i decyzji
wymaganych od właściciela. Pracuję wyłącznie w bieżącym, czystym worktree.
```

## 3. Role

```text
Operator → Evaluator → Final Control → integracja → READY_FOR_DEPLOY → bramka push
```

### 3.1 Operator
Wykonuje **jeden** temat w izolacji, wyłącznie w allowliście.
**Nie ocenia własnej pracy, nie integruje, nie pushuje.**

Kończy raportem terminalnym (§9). Gdy natrafi na decyzję produktową — zatrzymuje się
ze statusem `DECISION_REQUIRED`, nie rozstrzyga sam.

### 3.2 Evaluator — niezależny adwokat diabła
**Nie integruje, nie publikuje.** Sprawdza:

1. Czy diff mieści się w allowliście — co do pliku
2. Czy nie narusza żadnej z siedmiu barier (§7)
3. Czy scenariusze z kryteriów końca **faktycznie** przechodzą — nie czy raport tak twierdzi
4. Czy temat dotyka uprawnień choćby pośrednio; jeśli tak — czy A2 i A3 przechodzą
5. Czy w diffie nie ma wartości sekretów, także w testach i przykładach
6. Czy nie ma usunięć, których `GOAL` nie wymagał
7. Czy nie nakłada się z drugim aktywnym tematem
8. Czy `pytest` jest zielony na faktycznym stanie drzewa, a nie w raporcie

### 3.3 Final Control
Zawsze **osobny subagent**, nigdy główny agent. **Nie wystawia `READY_FOR_DEPLOY`.**
Kontroluje kompletność śladu:

1. Czy istnieje zapis dispatchu i czy `GOAL` nie zmienił się po drodze
2. Czy ID jest to samo we wszystkich rundach
3. Czy werdykt Evaluatora opiera się na artefaktach, nie na deklaracjach
4. Czy `PASS-WITH-NOTES` nie ukrywa uwagi dotyczącej GOAL, testów, zakresu,
   bezpieczeństwa, dowodu lub gotowości do integracji
5. Czy licznik rund się zgadza i nie został po cichu zresetowany
6. Czy `docs/process/tematy.md` odzwierciedla stan faktyczny

### 3.4 Orkiestrator
Działa w głównym czacie. Integruje **wyłącznie zatwierdzoną allowlistę**, per plik,
w razie potrzeby per hunk. Jako **jedyny** wystawia `READY_FOR_DEPLOY` — i dopiero
**po faktycznej integracji**, nie po pozytywnym Final Control.

Przed integracją sprawdza rzeczywisty stan: `git status`, `git diff`, wynik testów,
raporty, allowlistę.

### 3.5 Właściciel
Odpowiada na decyzje **wyłącznie w głównym czacie orkiestratora**.
Subagenty są kanałami technicznymi — nie prowadź z nimi osobnych rozstrzygnięć
produktowych i nie przyjmuj decyzji za właściciela.

## 4. Pętla tematu

```text
dispatch → Operator → Evaluator → Final Control → integracja → READY_FOR_DEPLOY
                 ↑          ↑             ↑
                 └──────────┴─────────────┘
      FAIL / BLOCK / TIMEOUT / INFRA / ZWIS / brak dowodu
```

### 4.1 Format ID

```
NAG-<ETAP>-<NNN>-<slug>
```

`ETAP` ∈ `MVP1` `MVP2` `MVP3` `MVP4` `PROC` `INFRA`.
`NNN` trzycyfrowy, rosnący w obrębie etapu, **nigdy nieużywany ponownie**.
ID jest niezmienne przez wszystkie rundy.

Pytanie decyzyjne: `NAG-MVP1-003-uprawnienia-Q2` — **nigdy samo „Q2"**.
Nie numeruj pytań tak, by kolidowały z wcześniejszymi.

### 4.2 Zapis dispatchu — przed dispatchem, nie po

Plik `docs/process/dispatch/<PEŁNE-ID>.md`, tworzony **zanim** ruszy Operator.
Szablon: `docs/process/dispatch/SZABLON.md`.

**Dispatch bez tego pliku jest naruszeniem procesu** — bez niego nie da się później
sprawdzić, czy `GOAL` nie przesunął się w trakcie.

### 4.3 Przebieg

1. Zapis dispatchu → Operator
2. Terminalny raport Operatora → **natychmiast** zamknij przebieg i uruchom
   Evaluatora dla tego samego ID
3. `PASS` uruchamia Final Control **bez czekania na dodatkową zgodę**
4. Pozytywny Final Control → orkiestrator sprawdza faktyczny stan i integruje
5. Dopiero po **faktycznej integracji** orkiestrator zapisuje `READY_FOR_DEPLOY`
6. Każdy `FAIL`, `BLOCK`, `TIMEOUT`, `INFRA`, `ZWIS`, brak artefaktu lub błąd
   izolacji wraca do Operatora, potem Evaluatora i Final Control — z tym samym ID

### 4.4 `PASS-WITH-NOTES`

**Nie kończy procesu**, jeśli uwagi dotyczą któregokolwiek z: kryterium `GOAL`,
testów, zakresu, bezpieczeństwa, dowodu lub gotowości do integracji.
Wtedy temat wraca do Operatora jak przy `FAIL`.

Kończy proces tylko wtedy, gdy uwagi są kosmetyczne i zapisane jako osobny temat.

### 4.5 Limit rund: 3

Po trzeciej rundzie orkiestrator **zatrzymuje temat i zgłasza właścicielowi**:
co próbowano, co zawiodło, jakie są warianty.

**Cichy reset licznika jest naruszeniem procesu.** Licznik ma boleć — to jego funkcja.
Przenumerowanie tematu w celu wyzerowania licznika jest tym samym naruszeniem.

### 4.6 Manual resume

Wymaga jawnej decyzji właściciela i **zachowuje ID, licznik rund oraz ostatni werdykt**.
Wznowienie nie jest nowym tematem i nie zeruje historii.

### 4.7 Pauza

Jedyna normalna pauza to **oczekiwanie na decyzję właściciela**.
Pauzuje **wyłącznie temat, który jej wymaga** — pozostałe niezależne tematy pracują dalej.
Zatrzymywanie całej pracy z powodu jednego pytania jest błędem.

## 5. Allowlista, izolacja, testy

### 5.1 Allowlisty

| Obszar | Typowa allowlista |
|---|---|
| Uprząż — logika | `app/**`, `tests/**` |
| Rejestr agentów | `registry/agents.yaml`, `app/registry/**` |
| Migracje | `migrations/**`, `app/models/**` |
| Wiedza | `knowledge/**` |
| Dokumentacja | `docs/spec/**` (poza `decisions.md`) |
| Proces | `docs/process/**`, `.claude/skills/**`, `CLAUDE.md` |

**Nigdy w allowliście:** `.env*`, dowolny plik z wartościami sekretów,
`docs/spec/decisions.md` (zmienia go wyłącznie orkiestrator po ECHO), `.git/**`,
konfiguracja produkcyjna bez jawnej zgody właściciela.

**Zmiana procesu nigdy nie jedzie w allowliście tematu produktowego** — nawet
jednolinijkowa. To osobny temat w domenie `PROCES`.

### 5.2 Izolacja

```
worktree:  ../nagents-<ID>
branch:    auto/<ID>
baza:      gałąź robocza wskazana przez właściciela — nie zakładaj `main`
```

Jeden temat = jeden worktree = jeden aktywny przebieg Operatora.
Worktree usuwany po integracji albo po zamknięciu tematu.

### 5.3 Plan testów

```text
1. pytest -q                       # całość zielona
2. pytest tests/<obszar> -v        # obszar tematu
3. scenariusze z kryteriów końca   # ręcznie, na dev
4. przy zmianie uprawnień: A2 i A3 obowiązkowo, bezwarunkowo
5. przy zmianie wiedzy: pełny zestaw testów agenta (od MVP3)
```

Punkt 4 obowiązuje także wtedy, gdy temat dotyka uprawnień tylko pośrednio.

### 5.4 Recon przed kodowaniem

```bash
git status && git branch --show-current
grep -rn "<pojęcie z GOAL>" app/ docs/spec/
```

Plus lektura: rejestr tematów (kolizje), dziennik decyzji (czy decyzja to przesądza),
scenariusze (czy scenariusz istnieje — jeśli nie, dopisz go **przed** startem tematu).

### 5.5 Przed integracją

Przejrzyj diff **również pod kątem usunięć**, nakładania się z drugim aktywnym
tematem i regresji względem pracy równoległej. Usunięcie, którego `GOAL` nie wymagał,
jest sygnałem ostrzegawczym.

## 6. Kryteria końca

Każdy temat ma jednozdaniowy `GOAL` i kryteria końca **wskazujące numery scenariuszy**
z `docs/spec/scenarios.md`.

```text
GOAL: Pracownik spoza grupy nie dobija się do agenta ani przez interfejs,
      ani z jego pominięciem.
KONIEC: scenariusze A2 i A3 przechodzą; wpisy `deny` widoczne w audycie;
        testy tests/test_permissions.py zielone.
```

**Kryterium bez numeru scenariusza jest niekompletne.**

## 7. Siedem twardych barier

Naruszenie oznacza **natychmiastowy `FAIL`**, niezależnie od jakości reszty pracy.
Wynikają z modelu bezpieczeństwa w `docs/spec/00-architektura.md`.

1. **Żadnych wartości sekretów w repozytorium.** W rejestrze wyłącznie `vault_ref`.
   Sekret w diffie = `FAIL` i rotacja klucza.
2. **Agent rodzaju `stanowiskowy` nie ma własnych poświadczeń.** Niepusta lista
   `secrets` = `FAIL` na poziomie walidatora rejestru.
3. **Brak dostępu zwraca 404, nie 403.** Zmiana tego zachowania wymaga ECHO.
4. **Domyślna odmowa.** Kod dodający ścieżkę „wszyscy mogą, chyba że" = `FAIL`.
5. **Nigdy `git add -A` ani `git add .`** — integracja allowlist-only, per plik,
   w razie potrzeby per hunk. Szczególnie niebezpieczne w drzewie współdzielonym.
6. **Żadnych prawdziwych danych osobowych poza `prod`.**
7. **Push wyłącznie na gałąź wskazaną przez właściciela.**

**Osłabienie, usunięcie albo dodanie wyjątku do którejkolwiek wymaga ECHO** —
patrz `docs/process/zmiana-procesu.md`.

## 8. Decyzje — ABC/ECHO

### 8.1 Kiedy formalna decyzja jest obowiązkowa

Gdy zmiana dotyka realnego kompromisu — w nAgents konkretnie:

- modelu uprawnień lub sposobu ich egzekwowania
- zakresu danych wysyłanych do dostawcy modelu
- budżetów, limitów i zachowania po przekroczeniu
- retencji, audytu lub realizacji praw osób
- topologii agentów (**D-010**) i rezydencji pamięci (**D-011**)
- wyboru silnika, bramy modeli lub bazy
- którejkolwiek z siedmiu barier

Dla drobnej implementacji **w ramach** przyjętej decyzji — nie jest wymagana.

### 8.2 Szablon pytania — wszystkie pola obowiązkowe

```text
PYTANIE: NAG-<ETAP>-<NNN>-<slug>-Q<n>
SYTUACJA:       <stan faktyczny>
CEL PYTANIA:    <co rozstrzygamy>
DLACZEGO TERAZ: <co blokuje, jeśli nie rozstrzygniemy>

WARIANT A: <opis>
  ZA:      1) …  2) …          ← minimum dwa
  PRZECIW: 1) …  2) …          ← minimum dwa
WARIANT B: <opis>
  ZA:      1) …  2) …
  PRZECIW: 1) …  2) …
WARIANT C: <opis>
  ZA:      1) …  2) …
  PRZECIW: 1) …  2) …

REKOMENDACJA: <litera + jedno zdanie>
KONSEKWENCJE IMPLEMENTACYJNE: <co trzeba napisać albo przepisać>
KONSEKWENCJE TESTOWE: <które scenariusze się zmieniają lub dochodzą>
```

Pytanie bez dwóch argumentów za i dwóch przeciw dla **każdego** wariantu jest
niekompletne — uzupełnij przed zadaniem.

### 8.3 ECHO

**Nie zamieniaj odpowiedzi „chyba", luźnej rozmowy ani rekomendacji agenta
w formalną decyzję.** Rekomendacja nie staje się decyzją przez brak sprzeciwu.

ECHO zapisuje się **dopiero po jednoznacznej odpowiedzi literą**, w
`docs/process/echo.md`, w formacie `NAG-MVP1-003-Q2 = B` z datą i autorem.
Dopiero potem kontynuuj ten sam ID.

| Artefakt | Plik | Kiedy |
|---|---|---|
| ECHO | `docs/process/echo.md` | zawsze po odpowiedzi literą |
| ADR | `docs/spec/decisions.md` | dodatkowo, gdy decyzja zmienia architekturę |

## 9. Kontrakt raportu

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

**Status nie zmienia się** na podstawie nazwy worktree, interfejsu, deklaracji agenta
ani nieistniejącego raportu.

### Domeny

| Domena | Co obejmuje |
|---|---|
| `PRODUKT` | kod uprzęży, rejestr, migracje, interfejs |
| `PROCES` | `.claude/skills/**`, `CLAUDE.md`, `docs/process/**` |
| `INFRA` | wdrożenie, kontenery, baza, brama modeli, kopie zapasowe |
| `INFORMACYJNY` | analiza, rozpoznanie, dokumentacja bez zmiany zachowania |

## 10. Watchdog i pojemność

| Parametr | Wartość |
|---|---|
| Jeden temat | **jeden aktywny przebieg Operatora** |
| Brak ruchu → `ZWIS` | **20 minut** |
| Aktywna pula tematów | **2 równolegle** |

Pula wynosi 2, bo wszystko przegląda jedna osoba; trzeci równoległy temat przekracza
pojemność przeglądu i kończy się integracją bez realnej kontroli.

**Obsadzanie slotów:** gdy istnieje niezablokowana praca, a slot jest wolny — obsadź go.
Zostawienie wolnego zasobu przez przeoczenie jest błędem tak samo jak przeciążenie.

**Po raporcie terminalnym** zwolnij slot i uruchom następny etap natychmiast.

**Przy `ZWIS`:** sprawdź transcript, stan repozytorium, worktree i artefakty
**zamiast zgadywać**. Nie anuluj i nie restartuj w ciemno — orkiestrator przejmuje temat.

## 11. Orkiestracja wieloagentowa i delegowanie

### 11.1 Norma tego projektu: orkiestrator nie wykonuje pracy sam

Decyzja właściciela z 2026-08-22, zapisana jako **ECHO-001**:

> W tym projekcie **cała praca wykonawcza idzie do subagentów.** Orkiestrator
> prowadzi rozmowę z właścicielem, przygotowuje dispatch, integruje i wystawia
> `READY_FOR_DEPLOY` — ale nie pisze kodu ani nie prowadzi analizy samodzielnie.

To jest odwrócenie domyślnego ustawienia ze szkieletu, gdzie orkiestracja była
opcjonalna. **W nAgents jest normą.**

### 11.2 Przydział modeli — ustalony

| Rola | Model | Effort |
|---|---|---|
| Operator | **Sonnet 5** | **wysoki** |
| Evaluator | **Sonnet 5** | **wysoki** |
| Final Control | **Sonnet 5** | **wysoki** |
| Orkiestrator | model sesji | — |

Przydział jest rozstrzygnięty i **nie wymaga potwierdzania przy każdym dispatchu.**
Zmiana wymaga ECHO.

### 11.3 Zawsze przez workflow — nigdy przez zwykłego subagenta

Decyzja właściciela z 2026-08-22, zapisana jako **ECHO-002**:

> **Każde zlecenie pracy subagentowi idzie przez narzędzie workflow.**
> Powód jest techniczny: zwykłe wywołanie subagenta przyjmuje wyłącznie model,
> a **effort da się przypisać tylko w workflow**. Bez workflow nie da się
> zrealizować przydziału z §11.2, więc dispatch poza workflow jest naruszeniem
> procesu — nawet dla pojedynczego, drobnego zadania.

| Sytuacja | Narzędzie |
|---|---|
| Jedno zadanie, jedna rola | **workflow** z jednym wywołaniem `agent()` |
| Jeden temat, pełna pętla | **workflow**: etapy Operator → Evaluator → Final Control |
| Kilka tematów zebranych naraz | **workflow** z fan-outem — nie kolejka wywołań |
| Analiza wymagająca wielu perspektyw | **workflow** z równoległymi rolami |

Nie ma wiersza „bez workflow". Jeden agent to nadal workflow — po prostu z jednym
wywołaniem.

Przy workflow obowiązuje ta sama pętla: Operator → Evaluator → Final Control.
Etapy workflow **odwzorowują role, nie zastępują ich** — nazwa etapu ma odpowiadać
roli, a `label` wywołania ma zawierać rolę i temat.

**Każde wywołanie `agent()` musi mieć jawnie podane `model` i `effort`.**
Pominięcie któregokolwiek oznacza, że przydział z §11.2 nie został zastosowany.

### 11.4 Co orkiestrator robi sam

Wyjątki od §11.1, bo z definicji nie da się ich delegować:

- rozmowa z właścicielem i przyjmowanie decyzji
- zapis ECHO i aktualizacja rejestrów procesu
- przygotowanie dispatchu i allowlisty
- integracja zatwierdzonej pracy i wystawienie `READY_FOR_DEPLOY`
- push po jawnym poleceniu

**Wszystko poza tą listą jest delegowane.**

### 11.5 Gdy zgoda zostanie cofnięta

Właściciel może wrócić do trybu jednowątkowego zdaniem w rozmowie.
Wtedy role różnicujemy **wyłącznie treścią promptu**, w jednym wątku,
a §11.1 przestaje obowiązywać do odwołania.

## 12. Dyscyplina źródeł i korekt

### 12.1 Hierarchia źródeł

Projekt opiera się na ocenie cudzych narzędzi. Dwa razy zapisano w dokumentach
nieprawdę, opierając się na źródle niższego rzędu. Stąd twarda kolejność:

| Rząd | Źródło | Status |
|---|---|---|
| 1 | oficjalna dokumentacja narzędzia | **rozstrzygające** |
| 2 | repozytorium i zgłoszenia błędów | rozstrzygające dla stanu faktycznego |
| 3 | wpis producenta, notatka o wydaniu | wiarygodne, ale marketing |
| 4 | artykuł branżowy, podsumowanie | poszlaka — sprawdź w rzędzie 1 |
| 5 | film promocyjny, materiał sprzedażowy | **nigdy jako podstawa decyzji** |

**Zanim wpiszesz cechę narzędzia do dokumentu decyzyjnego, sprawdź ją w rzędzie 1 lub 2.**
Fakt z rzędu 4 lub 5 zapisuj jawnie jako niepotwierdzony.

### 12.2 Przypadki, które tę regułę wywołały

| Błąd | Skąd | Jak było naprawdę |
|---|---|---|
| „Eve nie ma kanału do Teams" | opis repozytorium z przykładem dla Slacka | dokumentacja wymienia Teams jako wbudowany |
| „Hermes ma panel administracyjny" | podsumowanie w wyszukiwarce | dokumentacja mówi wprost, że panelu nie ma |

Pierwszy zawężał przewagę jednego narzędzia z dwóch punktów do jednego — czyli
zmieniał wynik porównania.

### 12.3 Jak korygować własny błąd

1. **Popraw tam, gdzie mieszka ustalenie** — nie tylko w rozmowie.
   Dokument z nieprawdą przeżyje rozmowę.
2. **Zostaw ślad korekty**, nie ciche nadpisanie. Czytelnik musi wiedzieć,
   że wcześniejsza wersja mówiła inaczej.
3. **Nazwij skutek dla decyzji.** „To był błąd" bez „a to zmienia rekomendację
   o tyle" jest bezużyteczne.
4. **Nie rozwodź się.** Jedno zdanie o pomyłce, reszta o konsekwencji.

## 13. Dyscyplina zakresu

### 13.1 Nie gonimy parytetu

Projekt istnieje obok gotowych platform komercyjnych, które mają więcej funkcji
i zawsze będą miały. **Gonienie parytetu funkcja po funkcji zamienia projekt
na trzy tygodnie w projekt na pół roku.**

Budujemy pod listę wymagań właściciela, nie pod to, co widać na cudzym demie.
Funkcja nierealizująca żadnego wymagania ani scenariusza **nie wchodzi** — idzie
do `docs/spec/decisions.md` jako rozważona i odrzucona.

### 13.2 Czego ten projekt nie robi

- **nie jest silnikiem agenta** — tym jest Hermes
- **nie jest komunikatorem** — tym jest Teams
- **nie przechowuje pamięci agenta** — robi to Hermes i dostawca pamięci
- **nie hostuje modeli** — te są po API, wymienne
- **nie zawiera niczego specyficznego dla NASTER w kodzie** — konfiguracja i wiedza,
  nigdy kod

Temat naruszający którykolwiek punkt wymaga pytania ABC, nie decyzji Operatora.

### 13.3 Rozjazd w trakcie tematu

Gdy pojawi się pomysł spoza `GOAL` — **zapisz go jako nowy temat w rejestrze
i wróć do swojego.** Nie poszerzaj allowlisty w biegu.

## 14. Cztery pliki trwałej prawdy

### 14.1 Pliki

Przy jednej osobie technicznej to jedyna obrona przed tym, że cała wiedza
o projekcie mieszka w jednej głowie.

| Plik | Zawartość |
|---|---|
| `docs/spec/00-architektura.md` | co budujemy i dlaczego tak |
| `docs/spec/decisions.md` | wybory dotyczące kosztu, danych, dostępu, odwracalności |
| `docs/spec/scenarios.md` | sytuacje do obsłużenia — źródło testów |
| `CLAUDE.md` | jak pracujemy; krótkie i zmienne |

**Wybór trafia do `decisions.md` zanim powstanie realizujący go kod.**
Decyzja udokumentowana po fakcie jest opisem, nie decyzją.

### 14.2 Ujawnianie wyborów zwykłym językiem

Gdy pojawi się wybór dotyczący danych, kosztu, prywatności, przenośności, wdrożenia
lub utrzymania — **nie podejmuj go po cichu w kodzie.** Przedstaw właścicielowi:

- dwie lub trzy realne opcje, zwykłym językiem
- rekomendację z jednym zdaniem uzasadnienia
- **co staje się łatwiejsze, a co trudniejsze do zmiany później**

Ostatni punkt jest najważniejszy i najczęściej pomijany. Właściciel podejmuje
decyzje o odwracalności, nie o składni.

Wybór spełniający kryteria z §8.1 idzie pełnym trybem ABC/ECHO.

### 14.3 Kopia, której nie odtworzono, nie jest kopią

**Odtworzenie musi zostać przećwiczone przed uznaniem tematu za zamknięty.**
Deklaracja „mamy backup" bez udokumentowanego odtworzenia to `FAIL`.

## 15. Konwencje repozytorium

### 15.1 Gdzie co trafia

| Rodzaj | Miejsce | Uwagi |
|---|---|---|
| Specyfikacja techniczna | `docs/spec/` | trwała, wersjonowana, źródło prawdy |
| Rejestry procesu | `docs/process/` | tematy, handoff, ECHO, dispatch |
| Materiały źródłowe | `docs/process/zrodla/` | cudze dokumenty — **nie modyfikujemy** |
| Notatki decyzyjne | `docs/nota-*.md` | **historia rozważań, nie routing** |
| Dokument do pokazania | artefakt + kopia w repo | artefakt do czytania, repo do trwałości |

**Notatki `nota-*` są zamrożone.** Nie aktualizuj ich, gdy ustalenie się zmieni —
zmienia się `decisions.md` i specyfikacja. Notatka pokazuje, co wiedzieliśmy wtedy,
i to jest jej wartość.

### 15.2 Język

Dokumentacja, komentarze w rejestrze i komunikaty dla użytkownika — **po polsku**.
Nazwy techniczne, pola bazy, ścieżki i identyfikatory — po angielsku,
bez polskich znaków.

### 15.3 Commity

- opis po polsku, bez znaków diakrytycznych w treści commita
- pierwszy wiersz: `<obszar>: <co się zmienia>` — `process:`, `docs:`, `app:`
- w treści: **co i dlaczego**, nie jak
- **nigdy identyfikator modelu ani nazwa narzędzia** w artefaktach wypychanych do repo

### 15.4 Gałąź

Push wyłącznie na gałąź wskazaną przez właściciela (bariera 7).
**Nie zakładaj `main`.** Gałąź robocza jest w `CLAUDE.md`; jeśli jej tam nie ma
albo wygląda na nieaktualną — zapytaj, nie zgaduj.

### 15.5 Koszt

Rachunek za modele przewyższa koszt infrastruktury o rząd wielkości.
**Optymalizuj dobór modeli i wielkość kontekstu, nie rozmiar serwera.**
Warstwa rozmowy chodzi na modelu tanim; analiza nocna może być wolna i dokładna.

---

## Pochodzenie

Dokument wywodzi się z uniwersalnego szkieletu procesu AutoBot, dostarczonego
przez właściciela i napisanego pierwotnie dla innego projektu. Kopia źródłowa
leży w `docs/process/zrodla/autobots-szkielet-uniwersalny.md` i **nie jest
modyfikowana** — służy jako odniesienie przy sporze o brzmienie zasady.

Względem szkieletu ta wersja:

- **rozstrzyga** wartości pozostawione jako konfigurowalne: limit rund 3 zamiast
  przykładowych 5, pula 2 tematy, próg `ZWIS` 20 minut, domeny i statusy raportu,
  effort per rola, konkretny punkt startowy zamiast „README albo CLAUDE albo AGENTS"
- **uzupełnia** o rzeczy, których szkielet wymagał, ale nie definiował: zapis
  dispatchu jako plik, checklisty Evaluatora i Final Control, szablon pytania ABC,
  jawną listę tego, co nie jest dowodem zakończenia
- **dodaje** praktyki wypracowane w tym projekcie: dyscyplinę źródeł i korekt (§12),
  dyscyplinę zakresu (§13), cztery pliki trwałej prawdy (§14), konwencje
  repozytorium (§15), szybki start (§0)
- **usuwa** warstwę „ustal per projekt", odwołania do innych projektów i wskazówki
  o kopiowalności szkieletu — tu są już bezużyteczne

Zmiana tego dokumentu podlega trybowi z `docs/process/zmiana-procesu.md`.
