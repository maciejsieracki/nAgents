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
9. Czy `GOAL` w raporcie zgadza się z `GOAL` z zapisu dispatchu i czy numery
   scenariuszy w raporcie odpowiadają kryteriom końca — **rozbieżność jest sygnałem
   utraty kontekstu przez Operatora**, niezależnie od wyniku `PASS`/`FAIL`
10. Gdy temat był dzielony na węzły (§11.3.2) i choć jeden ma `FAIL` — wskazuje
    **dokładnie jeden** wadliwy węzeł i precyzyjną poprawkę wyłącznie dla niego.
    Węzły z `PASS` nie wracają razem z nim

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
7. Przy temacie dzielonym na węzły — **ustala, który węzeł był najsłabszy**
   (dostał `FAIL` choć raz albo wymagał najwięcej rund) i przekazuje to
   orkiestratorowi do zapisu (§9.2)

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
   Przy temacie dzielonym na węzły (§11.3.2) wraca **wyłącznie węzeł wskazany
   przez Evaluatora** — reszta tematu stoi nietknięta

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

#### Kto rozstrzyga — właściciel czy orkiestrator

**Nie każda otwarta kwestia jest pytaniem do właściciela.** Podział:

| Rodzaj | Kto | Przykłady |
|---|---|---|
| Pieniądze, prawo, ludzie, ryzyko, zakres | **Właściciel** — pytanie ABC | ile agent może wydać, czy dane klientów idą do modelu, kto jest w pilocie, co robimy z monitoringiem pracowników |
| Technika bez konsekwencji dla powyższych | **Orkiestrator** — decyduje i **informuje**, nie pyta | gdzie stoi baza, na której wersji zależności budujemy, jak nazywamy gałęzie, gdzie leżą kopie |

Pytanie techniczne postawione właścicielowi **nie jest ostrożnością — jest przerzuceniem
na niego decyzji, do której nie ma podstaw.** Kosztuje jego czas i opóźnia pracę.
Gdy technika ma konsekwencję dla pieniędzy, prawa lub ryzyka — pytaj o **konsekwencję**,
nie o mechanizm.

#### Test zrozumiałości — przed wysłaniem pytania

Pytanie idzie do właściciela dopiero, gdy przechodzi wszystkie trzy:

1. Czy da się je przeczytać na głos osobie spoza projektu i dostać sensowną odpowiedź?
2. Czy w treści pytania **nie ma** numeru paragrafu, ścieżki pliku, nazwy narzędzia
   ani identyfikatora wewnętrznego? Te idą do odnośnika, nie do zdania.
3. Czy warianty różnią się **skutkiem dla firmy**, a nie sposobem wykonania?

**Przypadek, który tę regułę wywołał** (2026-08-22): zestaw 27 pytań przeszedł kontrolę
formy, ale właściciel odpowiedział „zadałeś je technicznym językiem, że ja w ogóle nie
wiem, o co chodzi". Kontrola sprawdzała kompletność wariantów i argumentów — nie
sprawdzała, czy adresat je rozumie. Około siedemnastu z nich w ogóle nie powinno do
niego trafić.

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

### 9.1 Zasada czystości — co wraca do orkiestratora

Raport terminalny niesie **destylat, nie surowe dane**: ścieżki plików i SHA zamiast
wklejonego diffu, wynik testu zamiast pełnego logu, jedno zdanie cytatu zamiast strony
źródła. Surowe materiały zostają w worktree Operatora — orkiestrator sięga po nie sam,
jeśli musi zweryfikować, ale domyślnie pracuje na destylacie z pól `ZMIANY` i `TESTY`.

**Limit twardy: 400 słów na raport węzła.** Zasada bez liczby jest apelem, nie regułą.
Przekroczenie to `PASS-WITH-NOTES`, nie `FAIL` — ale wraca do poprawy, bo raport, którego
nikt nie przeczyta w całości, nie pełni swojej funkcji.

### 9.2 Metryka wdrożenia przy dekompozycji

Gdy temat przeszedł przez bramę triage (§11.3.2), **Final Control ustala** przy zamknięciu,
który węzeł był najsłabszy, a **orkiestrator zapisuje** to jednym zdaniem w
`docs/process/tematy.md`. Rozdzielenie jest celowe: Final Control ma dane z przeglądu rund,
ale tylko orkiestrator zamyka temat.

Żaden węzeł nie miał `FAIL` → wpisz „brak, wszystkie węzły `PASS` za pierwszym razem".
To nie jest osobny artefakt — jedno zdanie w istniejącym rejestrze. Po kilku tematach
widać, co się psuje najczęściej.

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

### 11.3.1 Dobierz szerokość fan-outu do faktycznego limitu

Workflow uruchamia równolegle **najwyżej `min(16, liczba_CPU − 2)`** agentów.
Nadmiar czeka w kolejce. **Sprawdź limit, zanim zaplanujesz rozrzut:**

```bash
nproc        # limit = min(16, nproc - 2)
```

Wzór czyta się jako „mniejsza z dwóch liczb": sufit 16 oraz rdzenie minus 2 rezerwy
dla orkiestratora i systemu. Sufit zaczyna cokolwiek znaczyć **dopiero od 18 rdzeni** —
poniżej wiąże człon z procesorów.

| Rdzenie | Rdzenie − 2 | Sufit | **Limit** |
|---|---|---|---|
| 4 | 2 | 16 | **2** |
| 8 | 6 | 16 | **6** |
| 16 | 14 | 16 | **14** |
| 18 i więcej | ≥16 | 16 | **16** |

**Limit nie zależy od obciążenia maszyny.** Rezerwa dwóch rdzeni jest odejmowana
z góry, niezależnie od tego, czy cokolwiek je zajmuje. Zmierzone 2026-08-22:
przy `loadavg` 0.08 (maszyna praktycznie bezczynna) i braku dławienia cgroup
limit nadal wynosił 2. Czekanie na „spokojniejszą porę" niczego nie zmieni —
zmienia to wyłącznie większy kontener.

Uwaga o proporcji: przy 4 rdzeniach rezerwa zjada połowę mocy, przy 8 już ćwierć.
Przeskok z 4 na 8 rdzeni **potraja** liczbę równoległych agentów.

W kontenerze o 4 CPU limit wynosi **2**. Zlecenie siedmiu agentów nie daje wtedy
siedmiokrotnego przyspieszenia — daje cztery fale po dwóch, plus koszt
przełączania i siedem razy powtórzony wstęp do promptu.

**Reguła:** liczba równoległych wywołań w jednej fali powinna odpowiadać limitowi.
Gdy zadań jest więcej niż miejsc, **łącz je w grubsze paczki** zamiast mnożyć
cienkich agentów. Cztery paczki przy limicie dwóch kończą się szybciej niż
siedem drobnych.

Zaobserwowane w praktyce (2026-08-22, workflow `nagents-pytania-abc`): siedmiu
Operatorów przy limicie 2 wykonywało się falami po dwóch — trzeci startował
dokładnie w chwili zakończenia pierwszego.

**Uwaga o zbieżności:** limit techniczny (2) zgadza się z pulą tematów z §10 (2),
ustaloną z zupełnie innego powodu — pojemności przeglądu jednej osoby.
Przy zmianie któregokolwiek sprawdź, czy drugi nadal ma sens.

### 11.3.2 Brama triage — dzielić temat na węzły czy nie

Przed dispatchem odpowiedz na dwa pytania:

| Pytanie | Próg |
|---|---|
| Czy temat ma co najmniej dwa niezależne obszary z tabeli §5.1, więcej niż 3 scenariusze w kryteriach końca, albo więcej niż 6 plików w allowliście? | dowolny z trzech |
| Czy przetworzenie w jednym ciągu grozi przepełnieniem kontekstu? | surowe dane powyżej ok. 3000 tokenów |

**Choć jedno „tak" i kroki nie są sekwencyjnie zależne** → podziel na węzły.
Kroki zależne (krok 2 potrzebuje wyniku kroku 1) **nie dzielą się**, niezależnie od progów.
Obie odpowiedzi „nie" → jeden Operator, bez podziału.

Szerokość fan-outu dobierz według §11.3.1 — nie według liczby z zewnętrznych protokołów.
Trzy węzły przy limicie dwóch to dwie fale, nie trzy równoległe strumienie.

**ID węzła:** ID rodzica z sufiksem litery — `NAG-MVP1-003-a`, `-b`, `-c`.
Węzeł nie dostaje osobnego wpisu w rejestrze. **Licznik rund (§4.5) liczy się dla całego
tematu**, nie osobno dla węzła.

Każdy węzeł ma **binarne kryterium sukcesu** obok numeru scenariusza z §6 — sprawdzalne
`PRAWDA`/`FAŁSZ`. Brak formy binarnej jest niekompletnością tak samo jak brak numeru scenariusza.

*Trzeciego kryterium triage z protokołów zewnętrznych — „wynik krytyczny wymaga zewnętrznej
walidacji" — nie wprowadzamy jako osobnej bramki: u nas obowiązuje bezwarunkowo dla każdego
tematu przez Evaluatora (§3.2). Osobna bramka sugerowałaby fałszywie, że tematy nieoznaczone
jako krytyczne tej walidacji nie przechodzą.*

### 11.3.3 Matryca węzła — cztery pola, wszystkie obowiązkowe

Zapis dispatchu dla każdego węzła zawiera:

| Pole | Co to jest |
|---|---|
| **Zadanie** | wąski zakres, jedno zdanie |
| **Reguła anty-halucynacyjna** | konkretny sposób oszukania siebie, którego **zakazujemy** |
| **Binarne kryterium** | sprawdzalne `PRAWDA`/`FAŁSZ` |
| **Procedura naprawcza** | co dokładnie robi Evaluator przy `FAIL` — zapisane z góry, nie improwizowane |

Drugie pole jest tym, którego nam brakowało najbardziej. **Kryterium sukcesu sprawdza,
czy wynik jest kompletny. Reguła anty-halucynacyjna zakazuje sposobu, w jaki agent
oszuka sam siebie.** To są dwie różne rzeczy.

#### Nasze tryby halucynacji — obserwowane, nie hipotetyczne

| Tryb | Przypadek z tego projektu | Reguła zakazująca |
|---|---|---|
| Cecha narzędzia z podsumowania | „Hermes ma panel administracyjny" | Zakaz opisywania cechy narzędzia bez odwołania do źródła rzędu 1 lub 2 (§12.1) |
| Wniosek z opisu zamiast z dokumentacji | „Eve nie ma kanału Teams" — zmieniło wynik porównania | jak wyżej |
| Deklaracja zamiast artefaktu | „zleciłem uzupełnienie" — nie zlecono | Zakaz raportowania czynności bez identyfikatora zadania albo SHA |
| Pytanie o rzecz rozstrzygniętą | wariant sprzeczny z D-006 | Zakaz proponowania wariantu bez sprawdzenia dziennika decyzji |

Żaden z nich nie został złapany przez regułę procesu, bo takiej reguły nie było.

#### Węzły dla tematu kodującego

| Węzeł | Zakres | Reguła anty-halucynacyjna |
|---|---|---|
| `-a` **Logika** | `app/**` | Zakaz `Any` bez uzasadnienia w komentarzu. Zakaz `except: pass` i łapania `Exception` bez ponownego rzucenia albo zalogowania. Zakaz `# TODO` w kodzie idącym do integracji |
| `-b` **Testy** | `tests/**`, pisane z `scenarios.md` | Zakaz `assert wynik` i `assert wynik is not None` jako jedynej asercji. Przy uprawnieniach obowiązkowo testy negatywne A2 i A3 |
| `-c` **Bezpieczeństwo** | siedem barier, sekrety, zapytania | **Zakaz twierdzenia „uprawnienia są sprawdzane" bez wskazania linii wywołania `can_use`.** Zakaz deklaracji o bezpieczeństwie bez nazwania wektora. Wymóg podania liczby zapytań do bazy na żądanie |

**Testy pisze inny węzeł niż kod, z `scenarios.md`, bez wglądu w implementację.**
Agent, który napisał kod, pisze testy sprawdzające to, co kod robi — a nie to, czego
wymaga scenariusz. Testy przechodzą, wymaganie nie jest zrealizowane, wszystko świeci
na zielono i nikt tego nie łapie.

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

#### Rejestr języka w tekstach dla właściciela

Dokument, pytanie albo raport adresowany do właściciela pisze się **językiem skutków,
nie mechanizmów**. Obowiązują zakazy:

| Zakaz | Zamiast tego |
|---|---|
| Nazwa narzędzia albo biblioteki w zdaniu głównym | co to daje firmie |
| Numer paragrafu, ID tematu, ścieżka pliku w treści | odnośnik na końcu akapitu |
| Skrót bez rozwinięcia przy pierwszym użyciu | pełne określenie, skrót w nawiasie |
| „Zaimplementujemy", „skonfigurujemy", „wdrożymy warstwę" | co się zmieni w pracy ludzi |

Sprawdzian: **usuń z tekstu wszystkie nazwy własne narzędzi. Jeśli zdanie przestaje
cokolwiek znaczyć — było napisane o mechanizmie, nie o skutku.**

To nie dotyczy dokumentacji technicznej w `docs/spec/` ani tego skilla — te są dla
agentów i dla osoby technicznej, więc żargon jest tam właściwy.

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

## 16. Wzorzec promptu dla subagenta

Gotowy szablon zlecenia. Kopiuj i wypełnij — pola odpowiadają matrycy z §11.3.3.

```text
KONTEKST PROJEKTU
Przeczytaj obowiązkowo, w tej kolejności:
  CLAUDE.md, docs/process/tematy.md, docs/spec/00-architektura.md,
  docs/spec/decisions.md, docs/spec/scenarios.md, <specyfikacja etapu>
nAgents to warstwa zarządzania nad flotą Hermesów. Nie jest silnikiem agenta.

TWOJA ROLA: Operator | Evaluator | Final Control
TEMAT:      NAG-<ETAP>-<NNN>-<slug>[-<litera węzła>]

ZADANIE
<wąski zakres, jedno zdanie — co ma być prawdą po zakończeniu>

REGUŁA ANTY-HALUCYNACYJNA
<konkretny sposób oszukania siebie, którego zakazujemy — patrz §11.3.3>

BINARNE KRYTERIUM SUKCESU
<sprawdzalne PRAWDA/FAŁSZ; twój wynik zostanie wobec niego zweryfikowany>
Dodatkowo: scenariusze <numery ze scenarios.md> muszą przechodzić.

ALLOWLISTA
<ścieżki, per plik lub katalog>
Zakazane bezwzględnie: .env*, docs/spec/decisions.md, .git/**

OGRANICZENIA WYJŚCIA
- maksymalnie 400 słów w raporcie (§9.1)
- destylat, nie surowe dane: ścieżki i SHA zamiast diffu, wynik testu zamiast logu
- nie edytujesz plików spoza allowlisty
- przy decyzji produktowej zatrzymujesz się ze statusem DECISION_REQUIRED

FORMAT ODPOWIEDZI
STATUS / DOMAIN / TEMAT / GOAL / ZMIANY / TESTY / BLOKADY / NASTĘPNY KROK
DEPLOY/PUSH: NIE WYKONANO
```

**Wywołanie zawsze przez workflow, z jawnym `model` i `effort`** (§11.2, §11.3).

### 16.1 Czego w prompcie nie może zabraknąć

| Pole | Co się dzieje przy braku |
|---|---|
| Reguła anty-halucynacyjna | agent wypełni lukę domysłem i nie zauważy, że zgaduje |
| Binarne kryterium | krytyk nie ma wobec czego orzekać, ocena robi się uznaniowa |
| Limit słów | do syntezy trafiają surowe dane i zatruwają kontekst orkiestratora |
| Allowlista | zmiana wychodzi poza zakres tematu, integracja staje się ryzykowna |
| Kolejność czytania | agent zaczyna od przypadkowego pliku i buduje na nieaktualnym stanie |

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
