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

W nowym projekcie, zanim cokolwiek innego: najpierw rozpoznanie dziedziny
(§17), potem kalibracja liczb (§19), potem założenie dokumentów procesu (§20).
Dopiero wtedy pierwsze zadanie. Poniższy szybki start zakłada, że te trzy
kroki już się odbyły i proces ma już swoje wypełnienie (patrz §21 jako wzór).

```text
1. Przeczytaj ten dokument w całości. Jest długi, ale krótszy niż koszt błędu.
2. Przeczytaj pliki startowe z §2, w kolejności ustalonej dla tego projektu
   ({kolejnosc-plikow-startowych}).
3. Sprawdź stan: co dziś jest aktywne, co leży w miejscu pracy w toku
   ({miejsce-pracy-w-toku}), i czy poprzedni wykonawca nie zostawił czegoś
   nierozliczonego.
4. Napisz meldunek startowy (§2.3) i CZEKAJ na potwierdzenie właściciela.
5. Nie zaczynaj wykonania przed potwierdzeniem ID, GOAL i allowlisty
   ({mapa-obszarow-dopuszczonych-zmian}).
```

### Pięć rzeczy do wiedzy od razu

1. **Sprawdź, czy to, co masz zrobić, nie istnieje już pod spodem.** Ten proces
   jest warstwą zarządzania nad czymś, co projekt już posiada albo z czego
   korzysta. Jeśli praca odtwarza mechanizm, który gdzie indziej już działa —
   zatrzymaj się i zapytaj, zanim zaczniesz (§13).
2. **Domyślna odmowa wszędzie** — uprawnienia, narzędzia, dane.
   Nigdy „wszyscy mogą, chyba że".
3. **Decyzje otwarte i blokujące mają pierwszeństwo.** Jeśli dziennik decyzji
   projektu wskazuje sprawę jeszcze nierozstrzygniętą przez właściciela — nie
   rozstrzygaj jej samodzielnie w trakcie wykonania.
4. **Orkiestracja wieloagentowa jest wyłączona**, dopóki właściciel nie włączy
   jej jawnie na daną sesję (§11).
5. **Granice nienaruszalne tej dziedziny (§7) oznaczają `FAIL`**, niezależnie
   od jakości reszty pracy.

### Trzy najczęstsze sposoby zepsucia tego projektu

| Sposób | Objaw | Zapobieganie |
|---|---|---|
| Rozjazd zakresu | temat rośnie w trakcie rundy | §13.3 — nowy pomysł to nowy temat |
| Fałszywe „gotowe" | raport `PASS` bez sprawdzonego scenariusza | §1.2 — co nie jest dowodem |
| Cicha decyzja w wykonaniu | wybór o danych zapadł bez rozmowy z właścicielem | §14.2 — ujawnij wybór wcześniej |

## 1. Zasada nadrzędna

Każdy temat ma: **pełne ID, jawny `GOAL`, mierzalne kryteria końca wskazujące
numery scenariuszy, allowlistę, izolację i plan sprawdzenia.** Bez kompletu nie
ma dispatchu.

### 1.1 `READY_FOR_DEPLOY` oznacza łącznie

`READY_FOR_DEPLOY` zostaje jako stały znacznik stanu tego kontraktu wszędzie —
nawet tam, gdzie jego odpowiednikiem jest publikacja, złożenie pisma, wysyłka
do klienta albo wdrożenie zmiany, nie dosłowne „wdrożenie" w sensie
informatycznym. Nazwa nie zmienia się między projektami; zmienia się wyłącznie
to, co fizycznie oznacza (patrz tabela odwzorowań, §18.2, „Uznanie pracy za
obowiązującą").

- zmiana wyłącznie w zatwierdzonej allowliście ({mapa-obszarow-dopuszczonych-zmian})
- dowód wykonania ({dowod-wykonania}) przechodzi w całości — w projekcie
  informatycznym jest nim zwykle zielony zestaw testów automatycznych, patrz
  tabela odwzorowań
- **scenariusze z kryteriów końca sprawdzone ręcznie**, nie tylko automatycznie
- żadna z granic nienaruszalnych tej dziedziny nienaruszona (§7)
- brak nowych wartości sekretów w zapisach projektu
- rejestr tematów odzwierciedla stan faktyczny
- zmiana **faktycznie włączona do stanu obowiązującego** przez orkiestratora

**Przekazanie na zewnątrz pozostaje osobną bramką** po `READY_FOR_DEPLOY` —
tam, gdzie włączenie do stanu obowiązującego i faktyczne udostępnienie na
zewnątrz (wdrożenie, publikacja, złożenie, wysyłka) są dwoma różnymi, technicznie
rozdzielonymi krokami. Wymaga wyraźnego polecenia właściciela i idzie
**wyłącznie tam, gdzie on wskazał**. Operator, Evaluator i Final Control nigdy
nie wykonują tego kroku sami.

### 1.2 Co NIE jest dowodem zakończenia

Najczęstsze źródło fałszywego „gotowe":

| To nie jest dowód | Dlaczego |
|---|---|
| Raport `PASS` | opisuje pracę, nie jej skutek w stanie obowiązującym |
| Nazwa miejsca pracy w toku | nazwa miejsca pracy nie jest stanem |
| Zapis punktu kontrolnego | zapis, nie włączenie do stanu obowiązującego |
| Widoczny status zadania w narzędziu pracy | narzędzie pokazuje przebieg, nie wynik |
| Deklaracja „zrobione" | deklaracja bez artefaktu jest niczym |
| Brak artefaktu | brak dowodu to nie jest dowód |

(W nAgents: „nazwa miejsca pracy w toku" to gałąź albo worktree; „zapis punktu
kontrolnego" to commit — zapis, nie integracja; „widoczny status zadania w
narzędziu pracy" to status subagenta w interfejsie narzędzia orkiestracji.)

## 2. Start sesji

### 2.1 Kolejność czytania — obowiązkowa

Każdy projekt ustala własną, stałą kolejność plików startowych
({kolejnosc-plikow-startowych}) — raz, przy uruchomieniu procesu, nie przy
każdym temacie z osobna. Niezależnie od nazw plików w konkretnym projekcie,
kolejność czytania pokrywa te same siedem funkcji:

| # | Funkcja pliku | Po co |
|---|---|---|
| 1 | punkt startowy projektu | mapa źródeł i norma procesu |
| 2 | rejestr tematów | co aktywne, co zablokowane |
| 3 | bieżący handoff | zdjęcie aktualnej sytuacji, zastępowane w całości |
| 4 | skrót projektu i etapów | co budujemy i w jakiej kolejności |
| 5 | dziennik decyzji | decyzje, w tym **otwarte i blokujące** |
| 6 | rejestr scenariuszy | scenariusze — źródło planów sprawdzenia |
| 7 | specyfikacja bieżącego etapu | co dokładnie ma powstać teraz |

(W nAgents, w tej kolejności: `CLAUDE.md`, `docs/process/tematy.md`,
`docs/process/handoff.md`, `docs/spec/README.md`, `docs/spec/decisions.md`,
`docs/spec/scenarios.md`, `docs/spec/0X-mvpX.md`.)

**Nie zaczynaj** od starego handoffu, samego czatu, artefaktów zamkniętych tematów
ani od notatek decyzyjnych spoza rejestru. Notatki decyzyjne są historią rozważań,
nie routingiem.

### 2.2 Gdy zmieniasz sam proces

Przed dotknięciem plików samego procesu (definicja ról, punkt startowy, rejestry
procesu) przeczytaj tryb bezpiecznej zmiany procesu tego projektu, jeśli ma go
wydzielony osobno. Te ścieżki mają własny, ostrzejszy tryb — błąd w wytworze
wyłapie Evaluator, błąd w definicji samego Evaluatora nie wyłapie nikt.

(W nAgents: `.claude/skills/**`, `CLAUDE.md`, `docs/process/**` — tryb opisany
w `docs/process/zmiana-procesu.md`.)

### 2.3 Meldunek startowy

```text
Przeczytałem: <pliki startowe tego projektu, w ustalonej kolejności>.

Stan: <etap; tematy aktywne z pełnym ID i statusem>
Blokady: <lista, w tym decyzje otwarte>
Następna bramka: <co dokładnie>
Orkiestracja wieloagentowa: <wyłączona / włączona zgodą z dnia …>

Nie zaczynam zmian, dopóki nie potwierdzę ID, GOAL, allowlisty
({mapa-obszarow-dopuszczonych-zmian}) i decyzji wymaganych od właściciela.
Pracuję wyłącznie w bieżącym, czystym miejscu pracy w toku
({miejsce-pracy-w-toku}).
```

## 3. Role

```text
Operator → Evaluator → Final Control → włączenie do stanu obowiązującego
→ READY_FOR_DEPLOY → bramka przekazania na zewnątrz
```

### 3.1 Operator
Wykonuje **jeden** temat w izolacji, wyłącznie w allowliście.
**Nie ocenia własnej pracy, nie włącza jej do stanu obowiązującego, nie
przekazuje jej na zewnątrz.**

Kończy raportem terminalnym (§9). Gdy natrafi na decyzję produktową — zatrzymuje się
ze statusem `DECISION_REQUIRED`, nie rozstrzyga sam.

### 3.2 Evaluator — niezależny adwokat diabła
**Nie włącza wyniku do stanu obowiązującego, nie publikuje.** Sprawdza:

1. Czy treść zmiany do weryfikacji mieści się w allowliście — co do pozycji
2. Czy nie narusza żadnej z granic nienaruszalnych tej dziedziny (§7)
3. Czy scenariusze z kryteriów końca **faktycznie** przechodzą — nie czy raport tak twierdzi
4. Czy temat dotyka uprawnień choćby pośrednio; jeśli tak — czy scenariusze
   wskazane w kryteriach końca dla tego obszaru przechodzą
5. Czy w treści zmiany do weryfikacji nie ma wartości sekretów, także w
   przykładach i materiałach pomocniczych
6. Czy nie ma usunięć, których `GOAL` nie wymagał
7. Czy nie nakłada się z drugim aktywnym tematem
8. Czy dowód wykonania jest zielony na faktycznym stanie pracy, sprawdzony
   niezależnie od Operatora — nie tylko zgodnie z raportem
9. Czy `GOAL` w raporcie zgadza się z `GOAL` z zapisu dispatchu i czy numery
   scenariuszy w raporcie odpowiadają kryteriom końca — **rozbieżność jest sygnałem
   utraty kontekstu przez Operatora**, niezależnie od wyniku `PASS`/`FAIL`
10. Gdy temat był dzielony na węzły (§11.3.2) i choć jeden ma `FAIL` — wskazuje
    **dokładnie jeden** wadliwy węzeł i precyzyjną poprawkę wyłącznie dla niego.
    Węzły z `PASS` nie wracają razem z nim

### 3.3 Final Control
Zawsze **ktoś inny niż osoba, która wykonała temat, i inny niż zlecający** —
nigdy ten sam wykonawca, nigdy zlecający prowadzący rozmowę z właścicielem
(przy wykonawcy-programie: zawsze osobny subagent, nigdy główny agent).
**Nie wystawia `READY_FOR_DEPLOY`.**
Punktem odniesienia jest zawsze **wytwór na miejscu pracy w toku**, sprawdzony
bezpośrednio — nie tylko raporty Operatora i Evaluatora, które są deklaracją,
nie dowodem. Kontroluje kompletność śladu:

1. Czy istnieje zapis dispatchu i czy `GOAL` nie zmienił się po drodze
2. Czy ID jest to samo we wszystkich rundach
3. Czy werdykt Evaluatora opiera się na artefaktach, nie na deklaracjach
4. Czy `PASS-WITH-NOTES` nie ukrywa uwagi dotyczącej GOAL, dowodu wykonania,
   zakresu, granic nienaruszalnych, dowodu lub gotowości do włączenia do stanu
   obowiązującego
5. Czy licznik rund się zgadza i nie został po cichu zresetowany
6. Czy rejestr tematów odzwierciedla stan faktyczny
7. Przy temacie dzielonym na węzły — **ustala, który węzeł był najsłabszy**
   (dostał `FAIL` choć raz albo wymagał najwięcej rund) i przekazuje to
   orkiestratorowi do zapisu (§9.2)

### 3.4 Orkiestrator
Prowadzi rozmowę z właścicielem (przy wykonawcy-programie: główny czat).
Włącza do stanu obowiązującego **wyłącznie zatwierdzoną
allowlistę**, per pozycję, w razie potrzeby per fragment. Jako **jedyny** wystawia
`READY_FOR_DEPLOY` (§1.1) — i dopiero **po faktycznym włączeniu do stanu
obowiązującego**, nie po pozytywnym Final Control.

Przed włączeniem sprawdza rzeczywisty stan: co faktycznie leży w miejscu pracy
w toku, treść zmiany do weryfikacji, wynik dowodu wykonania, raporty, allowlistę.

### 3.5 Właściciel
Odpowiada na decyzje **wyłącznie w rozmowie z orkiestratorem** (przy
wykonawcy-programie: wyłącznie w głównym czacie orkiestratora).
Wykonawcy są kanałami technicznymi (przy wykonawcy-programie: subagenty) —
nie prowadź z nimi osobnych rozstrzygnięć produktowych i nie przyjmuj decyzji
za właściciela.

## 4. Pętla tematu

```text
dispatch → Operator → Evaluator → Final Control → włączenie do stanu
           obowiązującego → READY_FOR_DEPLOY
                 ↑          ↑             ↑
                 └──────────┴─────────────┘
      FAIL / BLOCK / TIMEOUT / INFRA / ZWIS / brak dowodu
```

### 4.1 Format ID

```
{prefiks-identyfikatora-tematu}-<ETAP>-<NNN>-<slug>
```

`{prefiks-identyfikatora-tematu}` i `ETAP` ∈ `{nazwy-etapow-projektu}` — prefiks
i zestaw etapów tego konkretnego projektu, ustalone raz przy starcie procesu,
nie przy każdym temacie, bez osobnego pytania kalibrującego.
`NNN` trzycyfrowy, rosnący w obrębie etapu, **nigdy nieużywany ponownie**.
ID jest niezmienne przez wszystkie rundy.

(W nAgents: prefiks `NAG`, etapy `MVP1 MVP2 MVP3 MVP4 PROC INFRA`.)

Pytanie decyzyjne: `<PREFIKS>-MVP1-003-uprawnienia-Q2` — **nigdy samo „Q2"**.
Nie numeruj pytań tak, by kolidowały z wcześniejszymi.

### 4.2 Zapis dispatchu — przed dispatchem, nie po

Plik tworzony **zanim** ruszy Operator, w miejscu i wedle szablonu ustalonego
przez ten projekt ({miejsce-i-szablon-zapisu-zlecenia}, bez osobnego pytania
kalibrującego — ustalane raz przy starcie procesu).

(W nAgents: `docs/process/dispatch/<PEŁNE-ID>.md`, szablon
`docs/process/dispatch/SZABLON.md`.)

Zapis niesie **trzy obowiązkowe składniki pętli**: wyzwalacz, zadanie i kryterium sukcesu.

| Składnik | Co odpowiada |
|---|---|
| **Wyzwalacz** | dlaczego ten temat startuje **teraz** i kto tak zdecydował |
| **Zadanie** | co ma być prawdą po zakończeniu |
| **Kryterium** | binarne `PRAWDA`/`FAŁSZ` plus numery scenariuszy (§6) |

Dopuszczalne wyzwalacze: decyzja właściciela (podaj ID ECHO), odblokowanie zależności
(podaj co się odblokowało), powrót po `FAIL` (podaj numer rundy), przegląd okresowy,
zdarzenie zewnętrzne. **„Bo była kolej" nie jest wyzwalaczem** — jeśli nie umiesz go
nazwać, temat prawdopodobnie nie powinien jeszcze startować.

**Dispatch bez tego pliku jest naruszeniem procesu** — bez niego nie da się później
sprawdzić, czy `GOAL` nie przesunął się w trakcie.

### 4.3 Przebieg

1. Zapis dispatchu → Operator
2. Terminalny raport Operatora → **natychmiast** zamknij przebieg i uruchom
   Evaluatora dla tego samego ID
3. `PASS` uruchamia Final Control **bez czekania na dodatkową zgodę**
4. Pozytywny Final Control → orkiestrator sprawdza faktyczny stan i włącza
   zmianę do stanu obowiązującego
5. Dopiero po **faktycznym włączeniu do stanu obowiązującego** orkiestrator
   zapisuje `READY_FOR_DEPLOY`
6. Każdy `FAIL`, `BLOCK`, `TIMEOUT`, `INFRA`, `ZWIS`, brak artefaktu lub błąd
   izolacji wraca do Operatora, potem Evaluatora i Final Control — z tym samym ID
   Przy temacie dzielonym na węzły (§11.3.2) wraca **wyłącznie węzeł wskazany
   przez Evaluatora** — reszta tematu stoi nietknięta

### 4.4 `PASS-WITH-NOTES`

**Nie kończy procesu**, jeśli uwagi dotyczą któregokolwiek z: kryterium `GOAL`,
dowodu wykonania, zakresu, granic nienaruszalnych, dowodu lub gotowości do
włączenia do stanu obowiązującego.
Wtedy temat wraca do Operatora jak przy `FAIL`.

Kończy proces tylko wtedy, gdy uwagi są kosmetyczne i zapisane jako osobny temat.

### 4.5 Limit rund: {liczba-podejsc-przed-eskalacja}

Domyślnie 3. Ustalane pytaniem kalibrującym §19.2 — zależy od tego, czy w tej
dziedzinie poprawka jest tania i szybka, czy każde nieudane podejście już samo
w sobie dużo kosztuje.

Po wyczerpaniu limitu orkiestrator **zatrzymuje temat i zgłasza właścicielowi**:
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

Każdy temat dostaje jawnie wypisaną mapę obszarów dopuszczonych zmian
({mapa-obszarow-dopuszczonych-zmian}) — nigdy dostęp do całego zasobu projektu
naraz. Dokładny kształt tych obszarów jest odwzorowaniem dziedziny: w projekcie
informatycznym to katalogi kodu, w księgowości — konta i zestawienia, w
kancelarii — konkretne sprawy i dokumenty (patrz tabela odwzorowań, „Lista
zasobów wolno dotknąć").

(W nAgents:)

| Obszar | Typowa allowlista |
|---|---|
| Uprząż — logika | `app/**`, `tests/**` |
| Rejestr agentów | `registry/agents.yaml`, `app/registry/**` |
| Migracje | `migrations/**`, `app/models/**` |
| Wiedza | `knowledge/**` |
| Dokumentacja | `docs/spec/**` (poza `decisions.md`) |
| Proces | `docs/process/**`, `.claude/skills/**`, `CLAUDE.md` |

**Nigdy w allowliście:** żaden plik z wartościami sekretów, dziennik decyzji
(zmienia go wyłącznie orkiestrator po ECHO), miejsca poza kontrolą samego
procesu, konfiguracja środowiska rzeczywistego bez jawnej zgody właściciela.

**Zmiana procesu nigdy nie jedzie w allowliście tematu produktowego** — nawet
jednolinijkowa. To osobny temat w domenie `PROCES`.

Zmiana elementu strukturalnego ({zmiana-elementu-strukturalnego} — w projekcie
informatycznym: migracja struktury bazy danych) jest zawsze osobnym obszarem w
allowliście, bo bywa trudna albo niemożliwa do cofnięcia — patrz §8.1, kiedy
wymaga formalnej decyzji właściciela.

### 5.2 Izolacja

Każdy temat pracuje w osobnej przestrzeni roboczej tematu — osobnym miejscu
pracy w toku, żeby wykonawcy nie nadpisywali sobie nawzajem pracy. Dokładna
postać tego miejsca i wzór jego nazwy ({miejsce-pracy-roboczej}, {wzor-nazwy-przestrzeni-roboczej})
są parametrem tego projektu — patrz pytanie kalibrujące §19.5. Pomyłka co do
tego miejsca bywa w niektórych dziedzinach trudna albo niemożliwa do cofnięcia.

```
{miejsce-pracy-roboczej}:  <wg wzoru nazwy tego projektu>
baza:                      wersja obowiązująca wskazana przez właściciela —
                           nigdy domyślna nazwa bez potwierdzenia
```

(W nAgents: worktree `../nagents-<ID>`, branch `auto/<ID>`, baza — gałąź
robocza wskazana przez właściciela, nie zakładaj `main`.)

Jeden temat = jedna przestrzeń robocza tematu = jeden aktywny przebieg Operatora.
Miejsce pracy usuwane po włączeniu do stanu obowiązującego albo po zamknięciu
tematu.

### 5.3 Plan sprawdzenia

```text
1. dowód wykonania w całości            # cały zakres, nie tylko obszar tematu
2. dowód wykonania dla obszaru tematu   # obszar dotknięty tym tematem
3. scenariusze z kryteriów końca        # ręcznie, w środowisku ćwiczebnym
4. przy zmianie uprawnień: scenariusze wskazane w kryteriach końca dla tego
   obszaru — obowiązkowo, bezwarunkowo
5. przy zmianie wiedzy/reguł trwałych: pełny zestaw sprawdzeń — od etapu
   ustalonego dla tego projektu
```

(W nAgents: `pytest -q` dla punktu 1, `pytest tests/<obszar> -v` dla punktu 2,
scenariusze A2 i A3 dla punktu 4, „od MVP3" dla punktu 5.)

Punkt 4 obowiązuje także wtedy, gdy temat dotyka uprawnień tylko pośrednio.

### 5.4 Rozpoznanie przed startem

Sprawdź, czy temat już nie istnieje, i przygotuj środowisko przedudostępnieniowe:
zanim zaczniesz, sprawdź stan pracy w toku i przeszukaj istniejący dorobek
projektu pod kątem pojęcia z `GOAL` — czy podobne rozwiązanie już nie powstało.

(W nAgents:)

```bash
git status && git branch --show-current
grep -rn "<pojęcie z GOAL>" app/ docs/spec/
```

Plus lektura: rejestr tematów (kolizje), dziennik decyzji (czy decyzja to przesądza),
scenariusze (czy scenariusz istnieje — jeśli nie, dopisz go **przed** startem tematu).

### 5.5 Przed włączeniem do stanu obowiązującego

Przejrzyj treść zmiany do weryfikacji **również pod kątem usunięć**, nakładania
się z drugim aktywnym tematem i regresji względem pracy równoległej. Usunięcie,
którego `GOAL` nie wymagał, jest sygnałem ostrzegawczym.

## 6. Kryteria końca

Każdy temat ma jednozdaniowy `GOAL` i kryteria końca **wskazujące numery scenariuszy**
z rejestru scenariuszy tego projektu.

Przykład (nAgents):

```text
GOAL: Pracownik spoza grupy nie dobija się do agenta ani przez interfejs,
      ani z jego pominięciem.
KONIEC: scenariusze A2 i A3 przechodzą; wpisy `deny` widoczne w audycie;
        dowód wykonania zielony na obszarze uprawnień
        (tests/test_permissions.py).
```

**Kryterium bez numeru scenariusza jest niekompletne.**

## 7. Granice nienaruszalne tej dziedziny

**Zasada jest uniwersalna: każdy projekt ma własną, jawnie spisaną listę granic,
których nie wolno przekroczyć, niezależnie od tego, jak dobra jest reszta pracy
— i skutek naruszenia którejkolwiek z nich jest zawsze ten sam: natychmiastowy
`FAIL`.** Treść tej listy jest odwzorowaniem dziedziny — inna w księgowości,
inna w kancelarii, inna w informatyce — i ustala się ją pytaniami rozpoznającymi
dziedzinę §17.10, §17.11, §17.12: czego nie wolno naruszyć nigdy choćby reszta
pracy była bez zarzutu; co narzuca prawo albo umowa z klientem; które dane są
wrażliwe i gdzie nie wolno ich wynosić.

**Osłabienie, usunięcie albo dodanie wyjątku do którejkolwiek granicy wymaga
ECHO** — patrz tryb bezpiecznej zmiany procesu tego projektu.

Wypełnienie dla nAgents — siedem granic wynikających z modelu bezpieczeństwa w
`docs/spec/00-architektura.md`:

1. **Żadnych wartości sekretów w zapisach projektu.** W rejestrze wyłącznie
   `vault_ref`. Sekret w treści zmiany do weryfikacji = `FAIL` i rotacja klucza.
2. **Agent rodzaju `stanowiskowy` nie ma własnych poświadczeń.** Niepusta lista
   `secrets` = `FAIL` na poziomie walidatora rejestru.
3. **Odpowiedź nieujawniająca istnienia zasobu: brak dostępu zwraca 404, nie
   403.** Zmiana tego zachowania wymaga ECHO.
4. **Domyślna odmowa.** Wykonanie dodające ścieżkę „wszyscy mogą, chyba że" = `FAIL`.
5. **Świadomy wybór elementów do włączenia: nigdy `git add -A` ani `git add .`**
   — włączanie do stanu obowiązującego wyłącznie wedle allowlisty, per plik, w
   razie potrzeby per hunk. Szczególnie niebezpieczne w miejscu pracy współdzielonym.
6. **Środowisko rzeczywiste a ćwiczebne: żadnych prawdziwych danych osobowych
   poza `prod`.**
7. **Przekazanie na zewnątrz wyłącznie tam, gdzie wskazał właściciel.**

## 8. Decyzje — ABC/ECHO

### 8.1 Kiedy formalna decyzja jest obowiązkowa

Zasada jest uniwersalna: **decyzje dotyczące kosztu, danych, dostępu lub
odwracalności trafiają do dziennika decyzji, zanim powstanie realizująca je
praca — nie po.** Co konkretnie się do tego kwalifikuje, jest odwzorowaniem
dziedziny i tego konkretnego projektu — w nAgents konkretnie:

- modelu uprawnień lub sposobu ich egzekwowania
- zakresu danych wysyłanych do dostawcy modelu
- budżetów, limitów i zachowania po przekroczeniu
- retencji, audytu lub realizacji praw osób
- decyzji trwałych tego projektu, otwartych na dziś (patrz dziennik decyzji
  projektu — w nAgents: **D-010**, topologia agentów, i **D-011**, rezydencja
  pamięci)
- wyboru silnika, bramy modeli lub bazy
- którejkolwiek z granic nienaruszalnych tej dziedziny (§7)

Dla drobnej implementacji **w ramach** przyjętej decyzji — nie jest wymagana.

#### Kto rozstrzyga — właściciel czy orkiestrator

**Nie każda otwarta kwestia jest pytaniem do właściciela.** Podział:

| Rodzaj | Kto | Przykłady |
|---|---|---|
| Pieniądze, prawo, ludzie, ryzyko, zakres | **Właściciel** — pytanie ABC | ile wolno wydać na jedno zlecenie, czy dane klientów idą do narzędzia spoza firmy, kto jest w pilocie, co robimy z monitoringiem pracowników |
| Technika bez konsekwencji dla powyższych | **Orkiestrator** — decyduje i **informuje**, nie pyta | jak nazywamy miejsca pracy roboczej, w jakim formacie trzymamy wersje robocze, gdzie leżą kopie |

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

ECHO zapisuje się **dopiero po jednoznacznej odpowiedzi literą**, w rejestrze
ECHO tego projektu, w formacie `<ID>-Q2 = B` z datą i autorem. Dopiero potem
kontynuuj ten sam ID.

| Artefakt | Gdzie | Kiedy |
|---|---|---|
| ECHO | rejestr ECHO projektu | zawsze po odpowiedzi literą |
| ADR (decyzja architektoniczna) | dziennik decyzji projektu | dodatkowo, gdy decyzja zmienia trwałe założenia projektu |

(W nAgents: ECHO w `docs/process/echo.md`, format `NAG-MVP1-003-Q2 = B`; ADR w
`docs/spec/decisions.md`.)

## 9. Kontrakt raportu

Zestaw statusów jest bezpiecznym, rozszerzalnym domyślnym — nie wymaga pytania
kalibrującego, chyba że projekt potrzebuje własnych stanów.

```text
STATUS: PASS | PASS-WITH-NOTES | FAIL | BLOCK | TIMEOUT | INFRA | DECISION_REQUIRED
DOMAIN: <nazwy domen raportu tego projektu>
TEMAT:  <prefiks>-<ETAP>-<NNN>-<slug>
GOAL:   <jedno zdanie>
ZMIANY: <pozycje z allowlisty + zapis punktu kontrolnego albo „brak zmian">
TESTY:  <wynik dowodu wykonania + numery scenariuszy + wynik sprawdzenia ręcznego>
BLOKADY: <lista albo „brak">
NASTĘPNY KROK: <kolejna bramka>
DEPLOY/PUSH: NIE WYKONANO
```

(W nAgents: `DOMAIN` ∈ `PRODUKT | PROCES | INFRA | INFORMACYJNY`; `ZMIANY` =
pliki z allowlisty + SHA commita; `TESTY` = wynik `pytest` + scenariusze.)

`DEPLOY/PUSH` domyślnie `NIE WYKONANO`. `WYKONANO` wpisuje wyłącznie orkiestrator,
po jawnym poleceniu właściciela i wyłącznie tam, gdzie on wskazał. Nazwa pola
zostaje `DEPLOY/PUSH` jako stały token kontraktu nawet tam, gdzie odpowiednikiem
jest publikacja, złożenie dokumentu albo zwolnienie partii, nie dosłowny „push".

**Status nie zmienia się** na podstawie nazwy miejsca pracy w toku, interfejsu,
deklaracji agenta ani nieistniejącego raportu.

### Domeny

Zestaw i treść domen są parametrem tego projektu; poniższy podział jest
wypełnieniem dla nAgents.

| Domena | Co obejmuje (nAgents) |
|---|---|
| `PRODUKT` | sam wytwór — kod uprzęży, rejestr, migracje, interfejs |
| `PROCES` | `.claude/skills/**`, `CLAUDE.md`, `docs/process/**` |
| `INFRA` | praca nad środowiskiem uruchomieniowym — wdrożenie, kontenery, baza, brama modeli, kopie zapasowe |
| `INFORMACYJNY` | analiza, rozpoznanie, dokumentacja bez zmiany zachowania |

### 9.1 Zasada czystości — co wraca do orkiestratora

Raport terminalny niesie **destylat, nie surowe dane**: ścieżki/pozycje i zapis
punktu kontrolnego zamiast wklejonej treści zmiany do weryfikacji, wynik
sprawdzenia zamiast pełnego logu, jedno zdanie cytatu zamiast strony źródła.
Surowe materiały zostają w miejscu pracy w toku Operatora — orkiestrator sięga
po nie sam, jeśli musi zweryfikować, ale domyślnie pracuje na destylacie z pól
`ZMIANY` i `TESTY`.

**Limit twardy: {limit-objetosci-raportu} na raport węzła.** Domyślnie 400 słów
— bezpieczny domyślny, kosmetyczny, bez osobnego pytania kalibrującego. Zasada
bez liczby jest apelem, nie regułą. Przekroczenie to `PASS-WITH-NOTES`, nie
`FAIL` — ale wraca do poprawy, bo raport, którego nikt nie przeczyta w całości,
nie pełni swojej funkcji.

### 9.2 Metryka wdrożenia przy dekompozycji

Gdy temat przeszedł przez bramę triage (§11.3.2), **Final Control ustala** przy zamknięciu,
który węzeł był najsłabszy, a **orkiestrator zapisuje** to jednym zdaniem w
rejestrze tematów tego projektu (w nAgents: `docs/process/tematy.md`). Rozdzielenie
jest celowe: Final Control ma dane z przeglądu rund, ale tylko orkiestrator
zamyka temat.

Żaden węzeł nie miał `FAIL` → wpisz „brak, wszystkie węzły `PASS` za pierwszym razem".
To nie jest osobny artefakt — jedno zdanie w istniejącym rejestrze. Po kilku tematach
widać, co się psuje najczęściej.

## 10. Watchdog i pojemność

„Watchdog" i `ZWIS` zostają jako stałe tokeny procesu wszędzie, tak samo jak
`READY_FOR_DEPLOY` (§1.1) i `DEPLOY/PUSH` (§9) — nazwa nie zmienia się między
projektami, zmienia się tylko to, po czym w danej dziedzinie poznaje się brak
ruchu.

| Parametr | Wartość |
|---|---|
| Jeden temat | **jeden aktywny przebieg Operatora** |
| Brak ruchu → `ZWIS` | **{czas-do-uznania-zawieszenia}** |
| Aktywna pula tematów | **{liczba-tematow-rownoleglych}** |

Oba parametry ustala się pytaniami kalibrującymi (§19.4 — próg zwisu, §19.3 —
pula tematów), zależnie od tego, ile czasu naturalnie trwa jeden etap pracy w
tej dziedzinie i ile jedna osoba jest w stanie rzetelnie przejrzeć naraz.

(W nAgents: zwis po 20 minutach ciszy, pula 2 tematy równolegle — bo wszystko
przegląda jedna osoba; trzeci równoległy temat przekracza pojemność przeglądu
i kończy się włączeniem do stanu obowiązującego bez realnej kontroli.)

**Obsadzanie slotów:** gdy istnieje niezablokowana praca, a slot jest wolny — obsadź go.
Zostawienie wolnego zasobu przez przeoczenie jest błędem tak samo jak przeciążenie.

**Po raporcie terminalnym** zwolnij slot i uruchom następny etap natychmiast.

**Przy `ZWIS`:** sprawdź przebieg pracy, stan miejsca pracy w toku i artefakty
**zamiast zgadywać**. Nie anuluj i nie restartuj w ciemno — orkiestrator przejmuje temat.

## 11. Delegowanie pracy i orkiestracja wieloagentowa

Ta sekcja ma dwie warstwy. **§11.1, §11.3.2, §11.3.3, §11.4 i §11.5 opisują
delegowanie pracy jako takie** — działają identycznie, niezależnie od tego,
czy wykonawcą jest osoba czy program; to samo dotyczy koleżanki z rozliczeń
przekazującej pracę koleżance. **§11.2, §11.3 i §11.3.1 stosują się wyłącznie,
gdy wykonawcą jest program** (patrz pytanie rozpoznające §17.1) — każda z nich
otwiera się tym zdaniem wprost.

### 11.1 Kto zleca, nie wykonuje sam

**Zasada jest uniwersalna:** osoba, która prowadzi rozmowę z właścicielem,
przygotowuje zlecenie i włącza zatwierdzony wynik do stanu obowiązującego
(rola zlecającego, §3.4), **nie wykonuje sama pracy merytorycznej zlecenia** —
tę wykonuje wykonawca (§3.1). Powód jest ten sam, co przy zakazie oceniania
własnej pracy (§3.2): kto zleca i integruje, traci zewnętrzny punkt odniesienia,
jeśli jest jednocześnie tym, kto wykonał. Rozdzielenie tych dwóch ról jest
warunkiem, żeby reszta pętli (Operator → Evaluator → Final Control) w ogóle
miała sens — zlecający sprawdzający sam siebie nie jest sprawdzeniem.

To, czy w danej organizacji zlecający i wykonawca są zawsze różnymi osobami,
czy czasem jedna osoba pełni obie role po kolei, w różnym czasie i z jasnym
przełączeniem kapelusza, jest odwzorowaniem dziedziny — rozstrzyga je pytanie
rozpoznające §17.1.

(W nAgents ta norma jest zapisana jako decyzja właściciela ECHO-001 — pełna
treść i uzasadnienie w §21.)

**Poniższe (§11.2, §11.3, §11.3.1) stosuje się, gdy wykonawcą jest program.**

### 11.2 Przydział modeli i poziomu wysiłku

**Stosuje się, gdy wykonawcą jest program.**

Każda rola ma przypisany model i poziom wysiłku myślenia —
{model-wykonawcy}, {model-sprawdzajacego}, {model-kontroli-koncowej},
{poziom-wysilku-mysleniowego}. Ustala się je pytaniem kalibrującym §19.1 —
zależnie od tego, ile w tej dziedzinie kosztuje błąd przeoczony na danym etapie,
nie od tego, co jest dziś technicznie dostępne jako najmocniejsze.

Przydział, raz ustalony, **nie wymaga potwierdzania przy każdym dispatchu.**
Zmiana wymaga ECHO.

(W nAgents: patrz §21.)

### 11.3 Zawsze przez to samo narzędzie zlecania — nigdy przez wywołanie ad hoc

**Stosuje się, gdy wykonawcą jest program.**

Gdy zlecenie idzie do programu, musi przejść przez narzędzie orkiestracji,
które pozwala jawnie przypisać do wywołania i model, i poziom wysiłku (§11.2)
— zwykłe, doraźne wywołanie tego zwykle nie umożliwia. Dispatch z pominięciem
tego narzędzia jest naruszeniem procesu, nawet dla pojedynczego, drobnego
zadania, bo oznacza, że przydział z §11.2 nie został zastosowany.

Przy takim narzędziu obowiązuje ta sama pętla: Operator → Evaluator → Final
Control. Etapy narzędzia **odwzorowują role, nie zastępują ich** — nazwa
etapu ma odpowiadać roli, a etykieta wywołania ma zawierać rolę i temat.

**Każde wywołanie musi mieć jawnie podane `model` i `effort`.** Pominięcie
któregokolwiek oznacza, że przydział z §11.2 nie został zastosowany.

(W nAgents: narzędzie workflow, decyzja właściciela ECHO-002 — pełna treść
w §21.)

### 11.3.1 Dobierz szerokość fan-outu do faktycznego limitu

**Stosuje się, gdy wykonawcą jest program.** Ten limit ({limit-rownoleglosci-wywolan})
nie jest odwzorowaniem dziedziny — wynika wyłącznie z zasobów kontenera
uruchomieniowego, nie z rodzaju pracy. Ten sam wzór obowiązuje niezależnie od
tego, czy temat jest informatyczny, księgowy czy kancelaryjny; różni się
wyłącznie od maszyny do maszyny. (Dla wykonawców-ludzi odpowiednikiem tego
ograniczenia jest pojemność przeglądu jednej osoby, §10 — nie moc obliczeniowa.)

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
z góry, niezależnie od tego, czy cokolwiek je zajmuje. Czekanie na „spokojniejszą
porę" niczego nie zmieni — zmienia to wyłącznie większy kontener.

Uwaga o proporcji: przy 4 rdzeniach rezerwa zjada połowę mocy, przy 8 już ćwierć.
Przeskok z 4 na 8 rdzeni **potraja** liczbę równoległych agentów.

**Reguła:** liczba równoległych wywołań w jednej fali powinna odpowiadać limitowi.
Gdy zadań jest więcej niż miejsc, **łącz je w grubsze paczki** zamiast mnożyć
cienkich agentów. Cztery paczki przy limicie dwóch kończą się szybciej niż
siedem drobnych.

(W nAgents: konkretny zmierzony limit, obserwacja w praktyce i uwaga o
zbieżności z pulą tematów z §10 — patrz §21.)

### 11.3.2 Kiedy dzielić zadanie na mniejsze, a kiedy nie

Przed dispatchem odpowiedz na dwa pytania. Progi podziału są parametrem tego
projektu ({progi-podzialu-tematu-na-wezly}) — poniższe wartości są wypełnieniem
dla nAgents, nie zasadą samą w sobie.

| Pytanie | Próg (nAgents) |
|---|---|
| Czy temat ma co najmniej dwa niezależne obszary z mapy obszarów dopuszczonych zmian, więcej scenariuszy w kryteriach końca niż próg tego projektu, albo więcej pozycji w allowliście niż próg tego projektu? | dwa obszary / >3 scenariusze / >6 pozycji w allowliście — dowolny z trzech |
| Czy przetworzenie w jednym ciągu grozi przepełnieniem tego, co jeden wykonawca potrafi rzetelnie utrzymać naraz w głowie (dla programu: kontekstu)? | surowe dane powyżej progu tego projektu (nAgents: ok. 3000 tokenów) |

**Choć jedno „tak" i kroki nie są sekwencyjnie zależne** → podziel na
mniejsze zadania. Kroki zależne (krok 2 potrzebuje wyniku kroku 1) **nie
dzielą się**, niezależnie od progów. Obie odpowiedzi „nie" → jeden wykonawca,
bez podziału.

Dla wykonawcy-programu szerokość fan-outu dobierz według §11.3.1. Dla
wykonawców-ludzi liczbę równocześnie prowadzonych węzłów ogranicza pojemność
przeglądu — patrz §10 i pytanie kalibrujące §19.3.

#### Najmniejszy skuteczny graf, nie największy możliwy

**Podział kosztuje, niezależnie od tego, kto wykonuje.** Każdy dodatkowy
węzeł powtarza wstęp/kontekst, wymaga własnej koordynacji i własnego
przekazania wyniku — więcej węzłów nie skraca pracy proporcjonalnie do ich
liczby, bo ten narzut się sumuje. Dziel wtedy, gdy progi z tabeli powyżej są
przekroczone — nie dlatego, że się da.

(Stosuje się dodatkowo, gdy wykonawcą jest program: architektura wieloagentowa
bywa szacowana na rząd wielkości więcej tokenów niż pojedyncze zapytanie —
patrz §21 po konkretną, niezweryfikowaną wielokrotność i źródło.)

**ID węzła:** ID rodzica z sufiksem litery — `NAG-MVP1-003-a`, `-b`, `-c`.
Węzeł nie dostaje osobnego wpisu w rejestrze. **Licznik rund (§4.5) liczy się
dla całego tematu**, nie osobno dla każdego węzła.

Każdy węzeł ma **binarne kryterium sukcesu** obok numeru scenariusza z §6 —
sprawdzalne `PRAWDA`/`FAŁSZ`. Brak formy binarnej jest niekompletnością tak
samo jak brak numeru scenariusza.

*Trzeciego kryterium podziału z protokołów zewnętrznych — „wynik krytyczny
wymaga zewnętrznej walidacji" — nie wprowadzamy jako osobnej bramki: u nas
obowiązuje bezwarunkowo dla każdego tematu przez Evaluatora (§3.2). Osobna
bramka sugerowałaby fałszywie, że tematy nieoznaczone jako krytyczne tej
walidacji nie przechodzą.*

### 11.3.3 Co wykonawca dostaje w zleceniu — cztery pola, wszystkie obowiązkowe

Zapis dispatchu dla każdego węzła zawiera:

| Pole | Co to jest |
|---|---|
| **Zadanie** | wąski zakres, jedno zdanie |
| **Reguła przeciw samooszukiwaniu** | konkretny sposób oszukania siebie, którego **zakazujemy** (przy wykonawcy-programie nazywana też regułą anty-halucynacyjną) |
| **Binarne kryterium** | sprawdzalne `PRAWDA`/`FAŁSZ` |
| **Procedura naprawcza** | co dokładnie robi sprawdzający przy `FAIL` — zapisane z góry, nie improwizowane |

Drugie pole jest tym, którego brakuje najczęściej. **Kryterium sukcesu
sprawdza, czy wynik jest kompletny. Reguła przeciw samooszukiwaniu zakazuje
sposobu, w jaki wykonawca oszuka sam siebie** — uzna niedokończoną albo błędną
pracę za gotową. To są dwie różne rzeczy.

Reguły przeciw samooszukiwaniu tego projektu **pochodzą z faktycznie
zaobserwowanych błędów, nie z teorii** — zbierz własne z pierwszych tematów,
zanim spiszesz listę na stałe. Nie kopiuj cudzych przykładów: błąd popełniony
w innym projekcie rzadko trafia w to, co faktycznie zawodzi w tym.

(Nasze obserwowane przypadki — patrz §21.)

#### Przykład: węzły dla tematu, w którym wytworem jest kod

Konkretny podział — dla tematu, w którym wytworem jest kod. Inna dziedzina
dzieli funkcjonalnie tak samo (wykonanie / sprawdzenie / zgodność albo
bezpieczeństwo), ale nazywa strumienie inaczej i definiuje własne reguły
przeciw samooszukiwaniu — patrz tabela odwzorowań, „Podział na równoległe
strumienie pracy z regułą przeciw samooszukiwaniu".

| Węzeł | Zakres | Reguła przeciw samooszukiwaniu |
|---|---|---|
| `-a` **Logika** | `app/**` | Zakaz `Any` bez uzasadnienia w komentarzu. Zakaz `except: pass` i łapania `Exception` bez ponownego rzucenia albo zalogowania. Zakaz `# TODO` w kodzie idącym do integracji |
| `-b` **Testy** | `tests/**`, pisane z `scenarios.md` | Zakaz `assert wynik` i `assert wynik is not None` jako jedynej asercji. Przy uprawnieniach obowiązkowo testy negatywne dla scenariuszy wskazanych w kryteriach końca tego obszaru |
| `-c` **Bezpieczeństwo** | siedem barier, sekrety, zapytania | **Zakaz twierdzenia „uprawnienia są sprawdzane" bez wskazania linii wywołania sprawdzenia dostępu.** Zakaz deklaracji o bezpieczeństwie bez nazwania wektora. Wymóg podania liczby zapytań do bazy na żądanie |

**Testy pisze inny węzeł niż kod, z `scenarios.md`, bez wglądu w implementację.**
Wykonawca, który napisał kod, pisze testy sprawdzające to, co kod robi — a nie
to, czego wymaga scenariusz. Testy przechodzą, wymaganie nie jest
zrealizowane, wszystko świeci na zielono i nikt tego nie łapie.

### 11.4 Co zlecający robi sam

Wyjątki od §11.1, bo z definicji nie da się ich delegować:

- rozmowa z właścicielem i przyjmowanie decyzji
- zapis ECHO i aktualizacja rejestrów procesu
- przygotowanie dispatchu i allowlisty
- włączenie zatwierdzonej pracy do stanu obowiązującego i wystawienie `READY_FOR_DEPLOY`
- przekazanie na zewnątrz po jawnym poleceniu

**Wszystko poza tą listą jest delegowane.**

### 11.5 Gdy zgoda na pracę wielu wykonawców naraz zostanie cofnięta

Właściciel może cofnąć zgodę na pracę wielu wykonawców naraz jednym zdaniem w
rozmowie. Wtedy jedna osoba (albo jeden ciągły wątek pracy z programem) pełni
po kolei wszystkie role, różnicowane wyłącznie treścią zlecenia, a §11.1
przestaje obowiązywać do odwołania.

## 12. Dyscyplina źródeł i korekt

### 12.1 Hierarchia źródeł

**Zasada jest uniwersalna: zanim fakt o zewnętrznym rozwiązaniu, dostawcy albo
przepisie trafi do dokumentu decyzyjnego, sprawdź go w źródle wyższego rzędu,
nie w plotce ani we własnym skojarzeniu.** Poza informatyką to samo pojęcie ma
inną postać — księgowy sprawdza saldo klienta względem wyciągu bankowego, nie
względem zapamiętanej rozmowy; prawnik sprawdza przepis w ustawie, nie w
przekonaniu kolegi z sąsiedniego pokoju. Który konkretnie dokument liczy się w
tej dziedzinie za rząd 1, a który za plotkę, jest odwzorowaniem dziedziny
({zrodlo-rozstrzygajace-faktu}, patrz tabela odwzorowań).

W nAgents projekt opiera się na ocenie cudzych narzędzi. Dwa razy zapisano w
dokumentach nieprawdę, opierając się na źródle niższego rzędu. Stąd twarda
kolejność dla oceny narzędzi:

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

(W nAgents: dwa przypadki obserwowane w tym projekcie.)

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

**Zasada jest uniwersalna:** budujemy pod listę wymagań właściciela, nie pod
to, co robi cudze gotowe rozwiązanie w tej samej przestrzeni — czy to
konkurencyjna platforma w informatyce, czy szerszy zakres usług sąsiedniego
biura rachunkowego albo kancelarii. **Gonienie parytetu funkcja po funkcji,
usługa po usłudze, zamienia projekt krótki w projekt wielokrotnie dłuższy.**

Funkcja albo usługa nierealizująca żadnego wymagania ani scenariusza
**nie wchodzi** — idzie do dziennika decyzji projektu jako rozważona i
odrzucona.

(W nAgents: projekt istnieje obok gotowych platform komercyjnych, które mają
więcej funkcji i zawsze będą miały — patrz §13.2, czego ten projekt świadomie
nie robi. Rozważone i odrzucone idą do `docs/spec/decisions.md`.)

### 13.2 Czego dany projekt świadomie nie robi

**Zasada jest uniwersalna:** każdy projekt ma spisaną wprost listę rzeczy,
których świadomie nie robi, choć mógłby — razem z krótkim uzasadnieniem, po co
ta granica. Lista chroni przed powolnym rozrostem opisanym w §13.1: bez niej
granica projektu żyje wyłącznie w pamięci jednej osoby i zaciera się przy
każdej kolejnej prośbie „skoro już przy tym jesteśmy". Dotyczy to biura
rachunkowego równie dobrze jak firmy informatycznej — biuro też może świadomie
nie prowadzić np. kadr klienta, mimo że umiałoby.

Lista powstaje przy rozpoznaniu zakresu na starcie i rośnie, gdy pojawi się
kolejna rzecz świadomie rozważona i odrzucona (§13.1) — nigdy jako spis z góry
wszystkiego, czego teoretycznie projekt mógłby nie robić.

Temat naruszający którykolwiek punkt tej listy wymaga pytania ABC, nie decyzji
Operatora.

(Wypełnienie dla nAgents — patrz §21.)

### 13.3 Rozjazd w trakcie tematu

Gdy pojawi się pomysł spoza `GOAL` — **zapisz go jako nowy temat w rejestrze
i wróć do swojego.** Nie poszerzaj allowlisty w biegu.

## 14. Pliki trwałej prawdy

### 14.1 Pliki

Przy jednej osobie technicznej to jedyna obrona przed tym, że cała wiedza
o projekcie mieszka w jednej głowie.

Każdy projekt wskazuje własny zestaw plików trwałej prawdy
({zestaw-plikow-trwalej-prawdy}) — niezależnie od nazw, zestaw pokrywa te
same funkcje:

| Funkcja pliku | Po co |
|---|---|
| opis, co budujemy i dlaczego tak | punkt odniesienia dla wyborów technicznych |
| dziennik decyzji | wybory dotyczące kosztu, danych, dostępu, odwracalności |
| rejestr scenariuszy | sytuacje do obsłużenia — źródło dowodu wykonania |
| bieżąca norma pracy | jak pracujemy dziś; krótkie i zmienne |

(W nAgents: `docs/spec/00-architektura.md`, `docs/spec/decisions.md`,
`docs/spec/scenarios.md`, `CLAUDE.md`.)

**Wybór trafia do dziennika decyzji zanim powstanie realizująca go praca.**
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
Deklaracja „mamy backup" (przećwiczone zabezpieczenie ciągłości, patrz tabela
odwzorowań §18.5) bez udokumentowanego odtworzenia to `FAIL`.

## 15. Konwencje repozytorium

### 15.1 Gdzie co trafia

Każdy projekt ustala własną mapę miejsc w archiwum trwałej prawdy
({mapa-miejsc-w-archiwum-projektu}). Osiem funkcji dokumentacyjnych, które ta
mapa musi pokrywać — punkt wejścia, tryb zmiany samego procesu, rejestr zadań,
przekazanie, decyzje właściciela, wybory trwałe, sytuacje, zapis zlecenia —
są opisane pełniej w §20; tu tylko to, czego §20 nie obejmuje, bo dotyczy
repozytorium jako całości,
nie samego procesu:

| Funkcja | Uwagi |
|---|---|
| materiały źródłowe cudzego pochodzenia | **nie modyfikujemy** |
| notatki decyzyjne, zamrożone | **historia rozważań, nie routing** |
| dokument do pokazania na zewnątrz | wersja do czytania + kopia trwała osobno |

(W nAgents: `docs/process/zrodla/`, `docs/nota-*.md`, artefakt + kopia w repo;
resztę mapy patrz §21.)

**Notatki decyzyjne, raz zapisane, są zamrożone.** Nie aktualizuj ich, gdy
ustalenie się zmieni — zmienia się dziennik decyzji i specyfikacja. Notatka
pokazuje, co wiedziano wtedy, i to jest jej wartość.

### 15.2 Język

Język dokumentacji, komentarzy w rejestrze i komunikatów dla użytkownika
({jezyk-dokumentacji-i-procesu}) ustala się pytaniem kalibrującym §19.6. Nazwy
techniczne, pola danych, ścieżki i identyfikatory zostają **po angielsku, bez
znaków diakrytycznych**,
niezależnie od wybranego języka dokumentacji — to konwencja techniczna, nie
wybór właściciela.

(W nAgents: dokumentacja, komentarze w rejestrze i komunikaty dla użytkownika
— po polsku, wszędzie.)

### 15.3 Zapis punktu kontrolnego

Postać zapisu punktu kontrolnego ({zapis-punktu-kontrolnego}) jest
odwzorowaniem dziedziny — w informatyce jest nim commit, w innej dziedzinie
numerowana wersja dokumentu albo wpis w rejestrze zmian z odtwarzalną
historią (patrz tabela odwzorowań, „Zapis zmian i możliwość cofnięcia").
Niezależnie od postaci, każdy zapis niesie:

- krótki opis w pierwszym wierszu/polu: `<obszar>: <co się zmienia>`
- w treści: **co i dlaczego**, nie jak
- język zgodny z ustaleniem tego projektu (§15.2)
- (przy wykonawcy-programie: **nigdy identyfikator modelu ani nazwę narzędzia
  wykonawcy** w zapisie trafiającym do archiwum trwałej prawdy)

(W nAgents: commit git; opis po polsku, bez znaków diakrytycznych w treści —
ograniczenie samego narzędzia, nie zasada procesu; pierwszy wiersz `process:`,
`docs:`, `app:`.)

### 15.4 Miejsce docelowe przekazania na zewnątrz

Przekazanie na zewnątrz wyłącznie tam, gdzie wskazał właściciel (granica
nienaruszalna, patrz §7). **Nie zakładaj domyślnego miejsca** — w informatyce
domyślnej gałęzi `main`, i odpowiednio w innych dziedzinach. Miejsce docelowe
jest zapisane w punkcie startowym projektu; jeśli go tam nie ma albo wygląda
na nieaktualne — zapytaj, nie zgaduj.

(W nAgents: gałąź robocza `claude/git-connection-9sz6dg`, zapisana w
`CLAUDE.md`.)

### 15.5 Koszt

**Zasada jest uniwersalna:** koszt pracy trzeba znać i pilnować. **Co dokładnie
jest jego głównym składnikiem, jest odwzorowaniem dziedziny** — bywa nim
rachunek za model językowy, czas ludzkiej pracy rozliczany godzinowo, materiał
zużyty w produkcji, albo opłata za dostęp do systemu zewnętrznego. Optymalizuj
ten składnik, który w tym projekcie faktycznie kosztuje najwięcej — nie ten,
który najłatwiej zmierzyć albo który kosztował najwięcej gdzie indziej.

(W nAgents: patrz §21.)

## 16. Wzorzec promptu dla subagenta

**Stosuje się, gdy wykonawcą jest program.** Dla wykonawcy-człowieka zlecenie
ma te same cztery pola treści (§11.3.3), ale nie przyjmuje postaci „promptu" —
koleżanka z rozliczeń dostaje zlecenie, nie prompt.

Gotowy szablon zlecenia. Kopiuj i wypełnij — pola odpowiadają matrycy z §11.3.3.
Miejsca oznaczone parametrem zależą od dziedziny i od tego konkretnego
projektu — wypełnij je wartościami ustalonymi dla projektu, nad którym
faktycznie pracujesz, nie przepisuj przykładu nAgents dosłownie.

```text
KONTEKST PROJEKTU
Przeczytaj obowiązkowo, w kolejności ustalonej dla tego projektu
({kolejnosc-plikow-startowych}): <pliki startowe tego projektu>,
<specyfikacja bieżącego etapu>
<jedno zdanie: czym ten projekt jest, a czym świadomie nie jest>

TWOJA ROLA: Operator | Evaluator | Final Control
TEMAT:      {prefiks-identyfikatora-tematu}-<ETAP>-<NNN>-<slug>[-<litera węzła>]

ZADANIE
<wąski zakres, jedno zdanie — co ma być prawdą po zakończeniu>

REGUŁA PRZECIW SAMOOSZUKIWANIU (ANTY-HALUCYNACYJNA, gdy wykonawcą jest program)
<konkretny sposób oszukania siebie, którego zakazujemy — patrz §11.3.3>

BINARNE KRYTERIUM SUKCESU
<sprawdzalne PRAWDA/FAŁSZ; twój wynik zostanie wobec niego zweryfikowany>
Dodatkowo: scenariusze <numery z rejestru scenariuszy> muszą przechodzić.

ALLOWLISTA
<pozycje mapy obszarów dopuszczonych zmian, per pozycję>
Zakazane bezwzględnie: pliki z wartościami sekretów, dziennik decyzji projektu,
wewnętrzne katalogi systemu zapisu wersji tego projektu.

OGRANICZENIA WYJŚCIA
- maksymalnie {limit-objetosci-raportu} w raporcie (§9.1)
- destylat, nie surowe dane: pozycje i zapis punktu kontrolnego zamiast treści
  zmiany do weryfikacji, wynik dowodu wykonania zamiast pełnego logu
- nie edytujesz pozycji spoza allowlisty
- przy decyzji produktowej zatrzymujesz się ze statusem DECISION_REQUIRED

FORMAT ODPOWIEDZI
STATUS / DOMAIN / TEMAT / GOAL / ZMIANY / TESTY / BLOKADY / NASTĘPNY KROK
DEPLOY/PUSH: NIE WYKONANO
```

(W nAgents: kontekst projektu = `CLAUDE.md, docs/process/tematy.md,
docs/spec/00-architektura.md, docs/spec/decisions.md, docs/spec/scenarios.md,
<specyfikacja etapu>`; zdanie o projekcie = „nAgents to warstwa zarządzania
nad flotą Hermesów. Nie jest silnikiem agenta."; TEMAT wg wzoru
`NAG-<ETAP>-<NNN>-<slug>`; zakazane bezwzględnie = `.env*`,
`docs/spec/decisions.md`, `.git/**`; limit raportu = 400 słów.)

**Wywołanie zawsze przez workflow, z jawnym `model` i `effort`** (§11.2, §11.3).

### 16.1 Czego w prompcie nie może zabraknąć

| Pole | Co się dzieje przy braku |
|---|---|
| Reguła przeciw samooszukiwaniu (anty-halucynacyjna, gdy wykonawcą jest program) | wykonawca wypełni lukę domysłem i nie zauważy, że zgaduje |
| Binarne kryterium | krytyk nie ma wobec czego orzekać, ocena robi się uznaniowa |
| Limit słów | do syntezy trafiają surowe dane i zatruwają kontekst orkiestratora |
| Allowlista | zmiana wychodzi poza zakres tematu, integracja staje się ryzykowna |
| Kolejność czytania | agent zaczyna od przypadkowego pliku i buduje na nieaktualnym stanie |

---

## 17. Rozpoznanie dziedziny — o co pytasz, zanim zapytasz o liczby

Te pytania idą pierwsze, przed pytaniami o liczby z §19 — nie da się ustalić,
ilu ma być sprawdzających, zanim wiadomo, na czym w tej firmie polega
sprawdzenie. Bez odpowiedzi na nie parametry z §19 nie mają się o co oprzeć:
liczba rund poprawek jest bez znaczenia, dopóki nie wiadomo, co w ogóle
oznacza „poprawka", a próg zawieszenia — dopóki nie wiadomo, co znaczy
„zrobione".

Zadaje się je wszystkie naraz, w jednej wiadomości. Odpowiedzi zapisuje się
dosłownie, tak jak padły — nie streszcza się ich i nie poprawia języka.
Odpowiedź niepełna — na przykład „tak jakoś to sprawdzamy" bez wskazania po
czym poznać, że coś jest sprawdzone — nie jest odpowiedzią i pytanie zostaje
otwarte.

### 17.1 Kto wykonuje pracę w tym projekcie: ludzie, programy, czy jedno i drugie

**Dlaczego o to pytam.** Od tej odpowiedzi zależy, czy w ogóle ma sens pytanie
o dobór modelu językowego i o to, jak dokładnie ma on „myśleć" (§19.1) — ta
grupa pytań dotyczy wyłącznie wykonawcy będącego programem. Gdy wykonawcą jest
osoba, reszta procesu działa identycznie, ale to jedno pytanie odpada.

**Pytanie.** Kto w tym projekcie faktycznie wykonuje zlecenia — osoby,
programy, czy jedno i drugie, zależnie od zadania?

**Przykładowe odpowiedzi z różnych branż.**
Księgowość: „zlecenia wykonują księgowe i księgowi, żaden program nie
prowadzi rozliczenia samodzielnie."
Firma informatyczna: „program (agent oparty na modelu językowym) wykonuje
zlecenie, osoba je zleca i sprawdza wynik."
Marketing: „zależnie od zadania — treść pisze czasem osoba, czasem program,
ale zawsze sprawdza to samo, druga osoba."

**Co się stanie, jeśli nie odpowiesz.** Agent założy domyślnie, że wykonawcą
jest program, i zada pytanie o model oraz poziom wysiłku myślenia tam, gdzie
ono nie ma zastosowania — co zdezorientuje osobę, która nigdy nie miała do
czynienia z takim wyborem.

**Co zapisujemy.** Pojęcie „rodzaj wykonawcy" — zapis w opisie projektu;
rozstrzyga, czy w ogóle zadaje się pytanie 19.1.

### 17.2 Ile trwa typowy krok pracy w tej dziedzinie

**Dlaczego o to pytam.** To jest podstawa do ustawienia czasu, po którym
proces uznaje wykonawcę za zawieszonego (§19.4) — bez tej wiedzy próg jest
zgadywany. W księgowości krok bywa dniem, w rozmowie z programem minutami.

**Pytanie.** Ile zwykle trwa jeden krok pracy w tej dziedzinie, zanim
wykonawca da znać o postępie albo skończy?

**Przykładowe odpowiedzi z różnych branż.**
Księgowość: „jedno zamknięcie miesiąca dla jednego klienta trwa zwykle cały
dzień roboczy, czasem dwa."
Kancelaria: „przygotowanie jednego pisma procesowego to zwykle pół dnia do
dnia."
Firma informatyczna: „jedno zlecenie dla programu trwa od kilku do
kilkudziesięciu minut."

**Co się stanie, jeśli nie odpowiesz.** Próg zawieszenia zostanie ustawiony na
wyczucie — może fałszywie alarmować przy zwykłym, długim kroku pracy, albo
przeciwnie, wykrywać realne zawieszenie dopiero po zbyt długim czasie.

**Co zapisujemy.** Pojęcie „typowy czas kroku pracy" — zapis obok progu
zawieszenia (§19.4).

### 17.3 Co powstaje w wyniku pracy

**Dlaczego o to pytam.** Bez wiedzy, jaki rodzaj rzeczy ma powstać na końcu
zlecenia — dokument, obliczenie, umowa, gotowa część programu, opublikowana
kampania — agent nie wie, czego pilnować i co właściwie ma przekazać do
sprawdzenia. Od tej odpowiedzi zależy, jak dalej nazywamy „wynik pracy" we
wszystkich kolejnych ustaleniach.

**Pytanie.** Co dokładnie ma Pan/Pani dostać do ręki na koniec pracy nad
jednym zleceniem — jaki to jest rodzaj rzeczy?

**Przykładowe odpowiedzi z różnych branż.**
Księgowość: „gotowe rozliczenie miesiąca z zestawieniem podatku, które można
od razu wysłać do urzędu."
Kancelaria: „podpisana umowa albo pismo gotowe do złożenia w sądzie."
Firma informatyczna: „działająca część programu, z której klient może od
razu korzystać."

**Co się stanie, jeśli nie odpowiesz.** Agent nie będzie wiedział, jak
wygląda „zrobione", i przy każdym zleceniu będzie zgadywał kształt wyniku
od nowa, co da niespójne rezultaty między zleceniami.

**Co zapisujemy.** Pojęcie „wytwór" — zapis w opisie projektu (odpowiednik
pliku wprowadzającego, jak `docs/spec/README.md` w tym projekcie).

### 17.4 Po czym poznajemy, że rzecz jest skończona i dobra

**Dlaczego o to pytam.** To jest pytanie najważniejsze w całym procesie —
każde zlecenie na końcu jest oceniane właśnie pod tym kątem. Bez jasnej
odpowiedzi agent nie ma czym mierzyć, czy praca jest naprawdę skończona, czy
tylko wygląda na skończoną.

**Pytanie.** Co konkretnie sprawdza Pan/Pani (albo każe komuś sprawdzić),
żeby uznać, że praca jest zrobiona dobrze, a nie tylko że wygląda gotowa?

**Przykładowe odpowiedzi z różnych branż.**
Księgowość: „przeliczenie zgadza się co do grosza z liczeniem ręcznym na
kalkulatorze."
Kancelaria: „pismo przeczytała druga osoba, która go nie pisała, i nie
znalazła błędu."
Firma informatyczna: „program przechodzi zestaw automatycznych sprawdzeń bez
żadnego błędu."

**Co się stanie, jeśli nie odpowiesz.** Przy braku odpowiedzi o dowód
wykonania agent nie ma jak stwierdzić skończenia i będzie wracał z tym
pytaniem przy każdym zadaniu.

**Co zapisujemy.** Pojęcie „dowód wykonania" — zapis w opisie zasad danej
dziedziny (odpowiednik `docs/spec/decisions.md` w tym projekcie).

### 17.5 Czy sprawdzenie jest niezależne od tego, kto pracę wykonał

**Dlaczego o to pytam.** Zasada mówi, że nikt nie ocenia własnej pracy, ale
to działa tylko wtedy, gdy istnieje sposób sprawdzenia, który nie polega na
słowie wykonawcy. Muszę wiedzieć, czy taki niezależny sposób w ogóle
istnieje i na czym dokładnie polega.

**Pytanie.** Czy wynik sprawdza ktoś inny niż osoba, która go wykonała, albo
sprawdza go coś, co nie zależy od jej słowa — a jeśli tak, to jak to wygląda?

**Przykładowe odpowiedzi z różnych branż.**
Księgowość: „drugi księgowy liczy tę samą pozycję niezależnie i porównujemy
wyniki."
Kancelaria: „wspólnik czyta pismo, zanim pójdzie do klienta, niezależnie od
zapewnień autora."
Firma informatyczna: „automatyczne sprawdzenia programu uruchamiają się same,
bez udziału osoby, która go pisała."

**Co się stanie, jeśli nie odpowiesz.** Agent założy, że jedynym dowodem jest
słowo wykonawcy, co łamie zasadę niezależnej oceny — sprawdzenie stanie się
czystą formalnością bez żadnej wartości.

**Jeśli odpowiedź brzmi „nikt tego nie sprawdza niezależnie".** To nie jest
odpowiedź zamykająca to pytanie — jest sygnałem, że proces musi dopiero
wskazać, kto obejmie tę rolę, zanim ruszy pierwsze zlecenie. Zasada „nikt nie
ocenia własnej pracy" jest niezmienna w każdej dziedzinie; brak niezależnego
sprawdzenia w danej firmie nie jest ustaleniem, które proces przyjmuje —
jest brakiem, który proces musi wypełnić, zanim zacznie działać.

**Co zapisujemy.** Pojęcie „sprawdzenie niezależne od wykonawcy" — zapis
razem z dowodem wykonania, w tym samym miejscu.

### 17.6 Co w tej firmie rozstrzyga spór o fakt

**Dlaczego o to pytam.** Zanim fakt o zewnętrznym rozwiązaniu, dostawcy albo
przepisie trafi do dokumentu decyzyjnego, trzeba wiedzieć, które źródło w tej
firmie liczy się jako rozstrzygające, a które jest tylko plotką albo
skojarzeniem (§12). Bez tej wiedzy agent nie odróżni ustalonego faktu od
domysłu.

**Pytanie.** Gdy w tej pracy pojawi się spór o fakt — na przykład czy dany
dostawca faktycznie coś oferuje, albo co dokładnie mówi przepis — co go
rozstrzyga: jaki dokument, jaka osoba, jakie źródło?

**Przykładowe odpowiedzi z różnych branż.**
Księgowość: „saldo rozstrzyga wyciąg bankowy, nie zapamiętana rozmowa z
klientem."
Kancelaria: „treść obowiązku rozstrzyga tekst przepisu, nie przekonanie
kolegi z sąsiedniego pokoju."
Firma informatyczna: „cechę narzędzia rozstrzyga jego oficjalna dokumentacja,
nie artykuł podsumowujący."

**Co się stanie, jeśli nie odpowiesz.** Agent będzie traktował artykuł,
wrażenie albo cudzą opinię jako rozstrzygający fakt i może oprzeć na tym
decyzję, która okaże się błędna.

**Co zapisujemy.** Pojęcie „źródło rozstrzygające faktu"
({zrodlo-rozstrzygajace-faktu}) — zapis obok hierarchii źródeł tej dziedziny
(§12).

### 17.7 Gdzie praca powstaje, zanim zacznie obowiązywać

**Dlaczego o to pytam.** Trzeba rozróżnić wersję roboczą, jeszcze do
poprawki, od wersji ostatecznej, na której firma faktycznie się opiera. Bez
tego rozróżnienia agent może pomylić szkic z gotową rzeczą i narazić firmę
na błąd widoczny na zewnątrz, u klienta albo w urzędzie.

**Pytanie.** Gdzie leży praca, dopóki jest jeszcze szkicem, i po czym poznać,
że przestała być szkicem i zaczęła obowiązywać naprawdę?

**Przykładowe odpowiedzi z różnych branż.**
Księgowość: „wersja robocza leży w osobnym pliku „do sprawdzenia", do
systemu księgowego trafia dopiero po podpisie."
Kancelaria: „projekt pisma leży w folderze „projekty", do akt sprawy trafia
dopiero podpisany egzemplarz."
Firma informatyczna: „nad kodem pracuje się w osobnej, prywatnej kopii, do
programu używanego przez klientów trafia dopiero po połączeniu z główną
wersją."

**Co się stanie, jeśli nie odpowiesz.** Agent nie odróżni szkicu od wersji
obowiązującej i może potraktować niedokończoną pracę jako gotową do użycia.

**Co zapisujemy.** Pojęcia „miejsce pracy roboczej" i „wersja obowiązująca" —
zapis w opisie sposobu pracy danej dziedziny.

### 17.8 Czy da się cofnąć zmianę i wrócić do stanu sprzed

**Dlaczego o to pytam.** Od odpowiedzi zależy, jak ostrożnie agent ma
podchodzić do danej zmiany. To, czego nie da się cofnąć, wymaga dodatkowej
zgody i większej ostrożności niż zmiana, którą łatwo odwrócić.

**Pytanie.** Jeśli praca okaże się błędna po tym, jak zacznie obowiązywać,
czy da się wrócić do poprzedniego stanu, a jeśli tak — jak i jak szybko?

**Przykładowe odpowiedzi z różnych branż.**
Księgowość: „zaksięgowaną pozycję można poprawić dokumentem korygującym w
tym samym miesiącu."
Kancelaria: „złożonego już pisma w sądzie nie da się cofnąć, trzeba składać
kolejne pismo prostujące."
Firma informatyczna: „poprzednią wersję programu można przywrócić w kilka
minut."

**Co się stanie, jeśli nie odpowiesz.** Agent nie będzie wiedział, które
zmiany traktować jako bezpieczne do szybkiego wprowadzenia, a które wymagają
dodatkowej ostrożności przed krokiem, którego nie da się cofnąć.

**Co zapisujemy.** Pojęcie „odwracalność zmiany" — zapis przy opisie ryzyka
danego rodzaju pracy.

### 17.9 Kto zatwierdza i bez czyjej zgody rzecz nie wchodzi w życie

**Dlaczego o to pytam.** Muszę wiedzieć, kto ma prawo powiedzieć ostatnie
słowo, zanim wynik zacznie obowiązywać na zewnątrz — trafi do klienta, do
urzędu albo zostanie wysłany dalej. Inaczej agent może uznać pracę za gotową
bez zgody osoby, która za nią faktycznie odpowiada.

**Pytanie.** Czyja zgoda jest konieczna, zanim wynik pracy faktycznie zacznie
obowiązywać — trafi do klienta, urzędu albo zostanie wysłany dalej?

**Przykładowe odpowiedzi z różnych branż.**
Księgowość: „rozliczenie idzie do urzędu dopiero po akceptacji głównej
księgowej."
Kancelaria: „pismo podpisuje wyłącznie prawnik prowadzący daną sprawę."
Firma informatyczna: „nowa wersja programu trafia do klientów dopiero po
zgodzie kierownika projektu."

**Co się stanie, jeśli nie odpowiesz.** Agent może przekazać wynik dalej bez
wymaganej zgody albo, przeciwnie, wstrzymywać każdą pracę w oczekiwaniu na
zgodę, której zakres nie jest jasny.

**Co zapisujemy.** Pojęcie „osoba zatwierdzająca" — zapis przy zasadach
zatwierdzania danego rodzaju pracy.

### 17.10 Czego nie wolno naruszyć nigdy, choćby reszta pracy była bez zarzutu

**Dlaczego o to pytam.** To są granice, których przekroczenie oznacza
odrzucenie pracy niezależnie od tego, jak dobra jest ona poza tym. Muszą być
spisane wprost, bo agent nie może się ich domyślać — inaczej sam ustali,
gdzie leży granica, i może się pomylić.

**Pytanie.** Jakie są granice, których przekroczenie oznacza, że praca jest
odrzucona bez względu na to, jak dobra jest poza tym?

**Przykładowe odpowiedzi z różnych branż.**
Księgowość: „nigdy nie wysyłamy rozliczenia do urzędu bez akceptacji
klienta, nawet gdy termin goni."
Kancelaria: „nigdy nie mówimy o sprawie klienta osobie spoza zespołu
prowadzącego, nawet jego rodzinie."
Firma informatyczna: „nigdy nie zapisujemy hasła klienta wprost w kodzie
programu, tylko w osobnym, chronionym miejscu."

**Co się stanie, jeśli nie odpowiesz.** Agent będzie działał tylko na
granicach ogólnych, wspólnych dla każdej dziedziny, i domyślnie uzna, że w
tej pracy nie ma dodatkowych granic — nawet jeśli w rzeczywistości są.

**Co zapisujemy.** Lista „granic nienaruszalnych tej dziedziny" — zapis obok
ogólnych barier, jako ich uzupełnienie (§7).

### 17.11 Co narzuca prawo albo umowa z klientem

**Dlaczego o to pytam.** Terminy ustawowe, obowiązek zachowania tajemnicy
albo wymóg uzyskania zgody klienta na coś mogą ograniczać to, co w tej pracy
w ogóle wolno zrobić — niezależnie od tego, co byłoby najwygodniejsze albo
najszybsze.

**Pytanie.** Jakie terminy, obowiązki zachowania tajemnicy albo wymagane
zgody wynikają w tej pracy z przepisów prawa albo z umowy z klientem?

**Przykładowe odpowiedzi z różnych branż.**
Księgowość: „deklarację podatkową trzeba złożyć do 25. dnia miesiąca,
inaczej są odsetki karne."
Kancelaria: „treść sprawy jest objęta tajemnicą zawodową, nie wolno jej
ujawnić nawet po zakończeniu współpracy."
Firma informatyczna: „umowa z klientem zabrania przekazywania kodu innym
firmom bez pisemnej zgody."

**Co się stanie, jeśli nie odpowiesz.** Agent nie będzie wiedział o
obowiązujących terminach i zakazach i może je złamać nieświadomie, mimo
dobrej woli.

**Co zapisujemy.** Pojęcie „wymogi prawne i umowne" — zapis obok granic
nienaruszalnych tej dziedziny.

### 17.12 Które dane są wrażliwe i gdzie nie wolno ich wynosić

**Dlaczego o to pytam.** Muszę wiedzieć, jakie informacje w tej pracy
wymagają szczególnej ostrożności — dane osobowe, finansowe, tajemnice
klienta — i do jakich miejsc, na przykład narzędzi spoza firmy, nie wolno
ich przekazywać.

**Pytanie.** Jakie dane w tej pracy są wrażliwe, i do jakich miejsc albo
narzędzi nie wolno ich przekazywać?

**Przykładowe odpowiedzi z różnych branż.**
Księgowość: „numery kont bankowych i numery PESEL pracowników nie mogą
trafić do żadnego narzędzia spoza systemu kadrowego firmy."
Kancelaria: „treść akt sprawy nie może być wklejana do żadnego ogólnie
dostępnego narzędzia do sprawdzania tekstu."
Firma informatyczna: „prawdziwe dane klientów nie mogą trafiać do wersji
programu używanej wyłącznie do prób i testów."

**Co się stanie, jeśli nie odpowiesz.** Agent nie będzie wiedział, które
informacje traktować jako szczególnie chronione, i może przypadkiem
przekazać je tam, gdzie nie powinny trafić.

**Co zapisujemy.** Pojęcie „dane wrażliwe i ich dozwolone miejsca" — zapis
obok granic nienaruszalnych, jako uzupełnienie zasady ochrony danych.

### 17.13 Co się dzieje, gdy praca jest błędna i wyjdzie to po czasie

**Dlaczego o to pytam.** To określa, jak poważnie traktować niepewność przy
podejmowaniu decyzji. Jeśli skutek błędu jest mały i łatwo naprawialny,
agent może działać śmielej; jeśli skutek jest poważny — kara finansowa,
utrata klienta, odpowiedzialność prawna — musi być dużo ostrożniejszy i
częściej pytać, zanim coś zrobi.

**Pytanie.** Jeśli w pracy okaże się błąd dopiero po jakimś czasie, jakie są
tego realne skutki i kto wtedy za nie odpowiada?

**Przykładowe odpowiedzi z różnych branż.**
Księgowość: „błąd w rozliczeniu odkryty po miesiącu to kara od urzędu
skarbowego, którą pokrywa biuro rachunkowe z własnej polisy."
Kancelaria: „błąd w piśmie złożonym po terminie może oznaczać przegraną
sprawę klienta i odpowiedzialność zawodową prawnika."
Firma informatyczna: „błąd w programie wykryty już po tym, jak klienci
zaczęli z niego korzystać, to naprawa poprawką, bez kosztów finansowych."

**Co się stanie, jeśli nie odpowiesz.** Agent nie będzie wiedział, jak wysoko
ustawić ostrożność przy niepewnych decyzjach — albo będzie nadmiernie
ostrożny tam, gdzie to niepotrzebne, albo zbyt lekkomyślny tam, gdzie skutek
jest poważny.

**Co zapisujemy.** Pojęcie „skutek błędu i odpowiedzialność" — zapis w
opisie ryzyka danej dziedziny.

---

## 18. Odwzorowanie pojęć na dziedzinę

Proces nazywa **funkcję** pojęcia; jego **postać** ustala się odpowiedziami z
sekcji 17. Tabele poniżej pokazują, jak ta sama funkcja wygląda fizycznie w
pięciu przykładowych dziedzinach. Ostatnia kolumna nazywa konkretną pomyłkę,
w którą łatwo wpaść, jeśli odwzorowanie zrobi się na skróty, zamiast wynikać
z odpowiedzi na pytania z sekcji 17.

### 18.1 Wytwór, dowód, sprawdzenie

**Wytwór pracy**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Zmiana w kodzie spełniająca wymaganie zamówienia | Wykonawca (deweloper) przypisany do tematu | Nie pomylić wytworu z relacją o nim — wytworem jest stan kodu, nie lista commitów czy opis w czacie |
| Księgowość i finanse | Gotowy dokument lub zestawienie: rozliczenie, deklaracja, uzgodniona pozycja | Księgowy/rozliczeniowiec prowadzący temat | Nie pomylić zamiaru zaksięgowania z wytworem — dopóki zapisu nie ma w systemie, wytworu nie ma |
| Obsługa prawna | Gotowe pismo lub dokument: umowa, opinia, pismo procesowe | Prawnik lub aplikant prowadzący sprawę | Nie pomylić projektu pisma z pismem finalnym — wersja sprzed drugiej lektury nie jest gotowym wytworem |
| Marketing i sprzedaż | Gotowy materiał lub kampania: treść, kreacja, oferta | Specjalista ds. kreacji/kampanii | Nie pomylić briefu ani pomysłu z gotowym materiałem — pomysł nie jest wytworem, dopóki nie ma formy do publikacji |
| Operacje i produkcja | Wykonane zadanie: przegląd, naprawa, partia produkcyjna | Operator/technik wykonujący zadanie | Nie pomylić wpisu w harmonogramie z faktycznym wykonaniem — zaplanowanie przeglądu nie jest przeglądem |

**Treść zmiany do weryfikacji**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Diff — dokładna różnica między stanem przed i po | Wykonawca generuje, sprawdzający czyta | Nie oceniać opisu zmiany zamiast samego diffu |
| Księgowość i finanse | Zestawienie „było / jest" dla zmienionych pozycji, z powodem zmiany | Wykonawca przygotowuje, sprawdzający porównuje liczby | Nie oceniać podsumowania słownego zamiast konkretnych liczb i pozycji |
| Obsługa prawna | Dokument w trybie śledzenia zmian (redline), nie sama wersja końcowa | Wykonawca włącza tryb zmian, drugi prawnik czyta redline | Nie oceniać streszczenia zmian zamiast redline'a — streszczenie może pominąć istotny szczegół |
| Marketing i sprzedaż | Konkretna wersja materiału (plik, makieta, treść), nie opis „co zrobiłem" | Wykonawca przedstawia wersję, oceniający porównuje z briefem | Nie oceniać briefu ani pomysłu zamiast finalnej kreacji |
| Operacje i produkcja | Protokół stanu przed/po ze szczegółami (odczyty, wymienione części) | Wykonawca sporządza protokół, kontroler weryfikuje | Nie oceniać deklaracji „zrobione" zamiast protokołu z pomiarami |

**Dowód wykonania**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Zielony zestaw testów automatycznych | Zestaw testów uruchamiany niezależnie od wykonawcy | Nie pomylić raportu „u mnie przechodzi" z faktycznym zielonym przebiegiem na aktualnym stanie kodu |
| Księgowość i finanse | Zgodność przeliczenia z niezależnym przeliczeniem tej samej pozycji, co do wymaganej precyzji | Druga osoba lub kontrola wewnętrzna wykonująca przeliczenie | Nie pomylić sprawdzenia z ponownym uruchomieniem tego samego wyliczenia tym samym sposobem — to nie jest niezależne sprawdzenie |
| Obsługa prawna | Pozytywny wynik drugiej lektury pod kątem zgodności z prawem i wzorem | Drugi prawnik wykonujący drugą lekturę | Nie pomylić czytania przez tę samą osobę, która pisała pismo, z drugą lekturą — musi być ktoś inny |
| Marketing i sprzedaż | Zgodność materiału z briefem i formalna akceptacja zamawiającego | Zamawiający/klient potwierdzający zgodność | Nie pomylić domysłu „klientowi się spodoba" z wyraźną akceptacją |
| Operacje i produkcja | Podpisany protokół odbioru z wynikiem pomiaru/kontroli jakości | Kontroler jakości podpisujący protokół | Nie pomylić wpisu „wykonano" w systemie z podpisanym protokołem kontrolera |

**Sprawdzenie niezależne**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Ponowne uruchomienie testów na faktycznym stanie repozytorium przez osobę inną niż autor | Recenzent (rola sprawdzająca), nie autor kodu | Nie pomylić przeglądu „na oko" bez uruchomienia testów z realnym sprawdzeniem dowodu |
| Księgowość i finanse | Przeliczenie przez drugą osobę, niezależnie od danych wejściowych użytych przez pierwszą | Druga osoba w dziale lub kontrola wewnętrzna | Nie pomylić ponownego odpalenia tego samego arkusza przez tę samą osobę z niezależnym przeliczeniem |
| Obsługa prawna | Ponowne przeczytanie pisma od zera przez osobę, która go nie pisała | Drugi prawnik niebędący autorem pisma | Nie pomylić akceptacji „bo autor ma doświadczenie" z faktyczną drugą lekturą |
| Marketing i sprzedaż | Ocena materiału względem briefu przez osobę spoza zespołu tworzącego | Osoba/dział spoza zespołu tworzącego, lub klient | Nie pomylić pochwały współpracownika z formalną, udokumentowaną oceną względem kryteriów |
| Operacje i produkcja | Powtórzenie pomiaru lub kontroli przez inną osobę niż wykonawca czynności | Inny inspektor niż wykonawca | Nie pomylić kontroli przez tę samą osobę w innym dniu z kontrolą przez inną osobę |

**Źródło rozstrzygające fakt**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Oficjalna dokumentacja narzędzia albo repozytorium i jego zgłoszenia błędów | Wykonawca sprawdza przed wpisaniem cechy narzędzia do dokumentu decyzyjnego | Nie pomylić artykułu podsumowującego albo materiału marketingowego z dokumentacją źródłową (§12.1) |
| Księgowość i finanse | Wyciąg bankowy albo dokument źródłowy klienta | Osoba uzgadniająca saldo | Nie pomylić zapamiętanej rozmowy z klientem z zapisem w wyciągu |
| Obsługa prawna | Treść przepisu albo orzeczenia | Prawnik ustalający stan prawny | Nie pomylić przekonania kolegi z sąsiedniego pokoju z brzmieniem ustawy |
| Marketing i sprzedaż | Brief zatwierdzony przez zamawiającego albo wytyczne marki | Osoba oceniająca zgodność materiału | Nie pomylić własnego wyobrażenia o marce z zapisanymi wytycznymi |
| Operacje i produkcja | Specyfikacja producenta albo protokół pomiaru | Technolog/kontroler ustalający parametr | Nie pomylić „tak się zawsze robiło" z aktualną specyfikacją |

### 18.2 Przestrzeń pracy, zapis, włączenie do stanu obowiązującego

**Miejsce pracy w toku**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Branch/worktree roboczy, oddzielony od głównej gałęzi | Wykonawca pracujący na swoim branchu | Nie pomylić brancha roboczego z gałęzią główną — praca w toku nie jest jeszcze obowiązująca |
| Księgowość i finanse | Robocza wersja arkusza/rejestru poza plikiem końcowym czy systemem produkcyjnym | Osoba przygotowująca rozliczenie na kopii roboczej | Nie pomylić arkusza roboczego z systemem księgowym — wpis w arkuszu nie jest zaksięgowaniem |
| Obsługa prawna | Wersja robocza dokumentu w edytorze, poza repozytorium umów czy systemem obiegu | Prawnik pracujący nad wersją roboczą | Nie pomylić wersji roboczej w edytorze z dokumentem złożonym lub podpisanym |
| Marketing i sprzedaż | Wersja robocza materiału w narzędziu projektowym, przed publikacją | Zespół kreacji w narzędziu roboczym | Nie pomylić makiety roboczej z materiałem już opublikowanym |
| Operacje i produkcja | Obszar/etap pracy w toku, oddzielony od gotowej partii lub wydania | Zespół/stanowisko realizujące zadanie przed odbiorem | Nie pomylić partii w trakcie produkcji z partią zwolnioną do wysyłki |

**Izolacja pracy od stanu obowiązującego**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Osobny worktree i branch na jeden temat, żeby wykonawcy nie nadpisywali swojej pracy | Wykonawca zakłada branch, wymuszone konwencją procesu | Nie pomylić pracy na osobnym branchu z pracą bezpośrednio na głównej gałęzi „bo szybciej" |
| Księgowość i finanse | Osobna kopia robocza pliku/okresu na czas korekty, zanim trafi do zatwierdzonego rejestru | Osoba prowadząca temat, przy zasadach wewnętrznych | Nie pomylić poprawiania danych bezpośrednio w już wysłanym rozliczeniu z pracą na kopii przed zatwierdzeniem |
| Obsługa prawna | Kopia robocza dokumentu w systemie obiegu, nie edycja bezpośrednio na wersji obowiązującej | Prawnik prowadzący sprawę, przy zasadach systemu obiegu | Nie pomylić edycji dokumentu już podpisanego lub złożonego z edycją jego roboczej kopii |
| Marketing i sprzedaż | Osobna wersja kampanii/materiału w toku przygotowania, nie nadpisywanie materiału aktywnego | Zespół kreacji, przy zasadach publikacji | Nie pomylić edycji materiału już opublikowanego na żywo z edycją wersji roboczej |
| Operacje i produkcja | Fizyczne lub proceduralne odgrodzenie partii/etapu w toku od tego, co już zwolniono | Zespół realizujący zadanie, przy procedurze zakładu | Nie pomylić wprowadzania zmian na partii już zwolnionej do klienta z pracą na partii w toku |

**Zapis zmian i możliwość cofnięcia**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Commit jako punkt kontrolny zapisujący stan — nie jest jeszcze integracją; historia pozwala wrócić do poprzedniej wersji | Wykonawca commituje kolejne kroki | Nie pomylić commitu (zapis) z merge'em do głównej gałęzi (integracja) — to dwa różne momenty |
| Księgowość i finanse | Zapis korekty z zachowaniem poprzedniej wersji pozycji (np. storno, ślad zmiany) | Osoba wprowadzająca korektę, zgodnie z zasadą śladu rewizyjnego | Nie pomylić poprawienia liczby „na miejscu" bez śladu z prawidłową korektą zostawiającą ślad |
| Obsługa prawna | Kolejne numerowane wersje dokumentu w systemie obiegu, z historią zmian | System obiegu dokumentów / osoba zapisująca kolejną wersję | Nie pomylić nadpisania pliku dokumentu z zapisaniem nowej wersji przy zachowaniu poprzedniej |
| Marketing i sprzedaż | Zapis kolejnych wersji materiału z możliwością przywrócenia poprzedniej | Osoba zapisująca kolejne wersje w narzędziu/systemie plików | Nie pomylić nadpisania pliku źródłowego z zapisaniem wersji — bez wersjonowania nie da się wrócić do poprzedniej kreacji |
| Operacje i produkcja | Zapis w dzienniku/rejestrze czynności z możliwością odtworzenia stanu sprzed zmiany | Osoba prowadząca dziennik/rejestr zmian procesu | Nie pomylić ustnej informacji o zmianie z zapisanym, odtwarzalnym śladem tej zmiany |

**Świadomy wybór elementów do włączenia**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Włączanie do commitu tylko jawnie wskazanych plików, nigdy hurtowo całego katalogu roboczego | Wykonawca/integrujący, jawnie wskazując pliki | Nie pomylić wygody hurtowego dodania wszystkiego z bezpieczeństwem — może wciągnąć plik, którego nikt nie sprawdził |
| Księgowość i finanse | Przeniesienie do zatwierdzonego zestawienia tylko jawnie sprawdzonych pozycji, nie całego roboczego arkusza | Osoba zatwierdzająca, jawnie wskazując pozycje | Nie pomylić przekopiowania całego arkusza z przeniesieniem wyłącznie sprawdzonych pozycji |
| Obsługa prawna | Włączenie do dokumentu finalnego tylko fragmentów jawnie zaakceptowanych podczas drugiej lektury | Prawnik zatwierdzający, jawnie wskazując fragmenty | Nie pomylić zaakceptowania jednego fragmentu z automatycznym zaakceptowaniem całego dokumentu |
| Marketing i sprzedaż | Publikacja tylko jawnie zatwierdzonych elementów materiału, nie całego folderu roboczego | Osoba zatwierdzająca publikację, jawnie wskazując elementy | Nie pomylić zatwierdzenia jednego elementu kampanii z automatycznym zatwierdzeniem całego pakietu |
| Operacje i produkcja | Przekazanie do wydania tylko jawnie odebranych partii/elementów, nie całej linii en bloc | Kontroler odbierający, jawnie wskazując odebrane partie | Nie pomylić odbioru jednej próbki z partii z odbiorem całej partii bez kontroli pozostałych sztuk |

**Uznanie pracy za obowiązującą**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Faktyczny merge do wspólnej gałęzi | Rola integrująca, nie sprawdzający | Nie pomylić pozytywnej oceny sprawdzającego z integracją — status końcowy wystawia integrujący dopiero po faktycznym merge'u |
| Księgowość i finanse | Faktyczne zaksięgowanie/zatwierdzenie w systemie księgowym | Osoba z uprawnieniem do zaksięgowania | Nie pomylić ustnej zgody księgowego z faktycznym zaksięgowaniem w systemie |
| Obsługa prawna | Faktyczne złożenie, podpisanie lub wysłanie dokumentu | Osoba uprawniona do złożenia/podpisania | Nie pomylić parafowania projektu z jego formalnym złożeniem lub podpisaniem |
| Marketing i sprzedaż | Faktyczna publikacja materiału lub uruchomienie kampanii | Osoba uprawniona do publikacji | Nie pomylić akceptacji makiety z faktem opublikowania na żywo |
| Operacje i produkcja | Faktyczne zwolnienie partii/wydania do użytku po odbiorze | Osoba uprawniona do zwolnienia partii | Nie pomylić wyniku kontroli jakości z decyzją o zwolnieniu partii — to bywa osobny krok i osoba |

**Archiwum trwałej prawdy projektu**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Repozytorium z plikami architektury, decyzji i scenariuszy — jedno miejsce prawdy, opisane krótkim komentarzem przy każdym zapisie | Zespół/proces utrzymujący repozytorium dokumentacji | Nie pomylić notatki w czacie z zapisanym plikiem w repozytorium — czat nie jest archiwum |
| Księgowość i finanse | Rejestr zasad przeliczeń i decyzji o sposobie księgowania, prowadzony w jednym miejscu | Osoba odpowiedzialna za politykę rachunkowości | Nie pomylić maila z ustaleniem zapisanym w rejestrze polityk |
| Obsługa prawna | Rejestr wzorów, obowiązujących stanowisk i decyzji interpretacyjnych | Osoba odpowiedzialna za rejestr wzorów kancelarii | Nie pomylić rozmowy telefonicznej z klientem z zapisanym stanowiskiem w aktach sprawy |
| Marketing i sprzedaż | Repozytorium briefów, wytycznych marki i decyzji strategicznych | Osoba odpowiedzialna za wytyczne marki | Nie pomylić ustnej decyzji ze spotkania z zapisanym briefem lub decyzją w dokumentacji |
| Operacje i produkcja | Dokumentacja procedur i decyzji dotyczących sposobu prowadzenia produkcji | Osoba odpowiedzialna za dokumentację procedur zakładu | Nie pomylić wiedzy „w głowie kierownika zmiany" z zapisaną instrukcją lub procedurą |

### 18.3 Zakres, granice, bariery

**Lista zasobów wolno dotknąć**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Allowlista katalogów kodu, poza którą zmiana jest naruszeniem | Właściciel/prowadzący temat ustala listę przy starcie | Nie pomylić „mogę to naprawić po drodze" z zakresem allowlisty — poprawka poza listą jest naruszeniem, nawet jeśli słuszna |
| Księgowość i finanse | Lista kont/zestawień, które dany temat wolno korygować | Przełożony/właściciel ustala zakres przy zleceniu | Nie pomylić dostępu do całego systemu księgowego z dostępem do konkretnego, przydzielonego zakresu |
| Obsługa prawna | Lista spraw/dokumentów objętych danym tematem | Partner/opiekun sprawy ustala zakres przy zleceniu | Nie pomylić dostępu do repozytorium kancelarii z dostępem do konkretnej, przydzielonej sprawy |
| Marketing i sprzedaż | Lista materiałów/kanałów objętych daną kampanią | Kierownik kampanii ustala zakres przy briefie | Nie pomylić dostępu do wszystkich kanałów marki z dostępem do kanałów objętych daną kampanią |
| Operacje i produkcja | Lista stanowisk/linii/partii objętych zadaniem | Kierownik produkcji ustala zakres przy zleceniu | Nie pomylić uprawnienia do jednej linii z uprawnieniem do całego zakładu |

**Zmiana elementu strukturalnego**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Migracja struktury bazy danych — zmiana trudna do cofnięcia | Wykonawca, z dodatkową akceptacją ze względu na trudność cofnięcia | Nie pomylić migracji z bieżącą zmianą w kodzie — migracja bywa nieodwracalna po uruchomieniu na produkcji |
| Księgowość i finanse | Zmiana planu kont lub metody przeliczania stosowanej wstecz | Główny księgowy lub osoba z odpowiednim uprawnieniem | Nie pomylić jednorazowej korekty pozycji ze zmianą metody obowiązującej na przyszłość |
| Obsługa prawna | Zmiana wzoru umowy lub klauzuli standardowej używanej we wszystkich przyszłych dokumentach | Partner odpowiedzialny za wzory dokumentów | Nie pomylić zmiany w jednym piśmie ze zmianą wzoru używanego przez całą kancelarię |
| Marketing i sprzedaż | Zmiana elementu identyfikacji marki (np. kluczowy komunikat) używanego we wszystkich materiałach | Osoba odpowiedzialna za markę, nie pojedynczy wykonawca kampanii | Nie pomylić zmiany treści jednego materiału ze zmianą elementu identyfikacji marki |
| Operacje i produkcja | Zmiana receptury/parametru procesu stosowanego do wszystkich kolejnych partii | Kierownik techniczny/technolog odpowiedzialny za proces | Nie pomylić jednorazowego odstępstwa od procedury ze zmianą samej procedury lub receptury |

**Twarda bariera**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Np. zakaz umieszczania wartości sekretów w repozytorium | Dotyczy każdego wykonawcy i sprawdzającego; egzekwuje rola kontrolna | Nie pomylić dobrej reszty pracy z usprawiedliwieniem naruszenia — naruszenie to porażka niezależnie od jakości reszty |
| Księgowość i finanse | Np. zakaz przesyłania danych klienta poza zatwierdzony, zabezpieczony system | Dotyczy każdego w dziale; egzekwuje kontrola wewnętrzna | Nie pomylić presji terminu z powodem do obejścia zabezpieczenia danych |
| Obsługa prawna | Np. zakaz ujawniania treści objętych tajemnicą zawodową poza uprawniony krąg osób | Dotyczy każdego w kancelarii; egzekwuje partner nadzorujący | Nie pomylić dobrej relacji z klientem z powodem do złagodzenia zasady poufności |
| Marketing i sprzedaż | Np. zakaz publikacji materiału bez zgody na wykorzystanie wizerunku lub danych osobowych | Dotyczy każdego w zespole; egzekwuje kierownik/dział prawny | Nie pomylić presji terminu kampanii z powodem do publikacji bez wymaganej zgody |
| Operacje i produkcja | Np. zakaz zwolnienia partii bez podpisanego protokołu kontroli jakości | Dotyczy każdego na linii; egzekwuje kierownik zmiany | Nie pomylić presji „linia stoi, trzeba wysłać" z powodem do zwolnienia partii bez protokołu |

**Odpowiedź nieujawniająca istnienia zasobu**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Brak dostępu do zasobu zwraca „nie znaleziono", nie „odmowa dostępu" | System/API obsługujące żądanie | Nie pomylić „odmowy dostępu" (ujawnia istnienie) z „nie znaleziono" (nie ujawnia) |
| Księgowość i finanse | Osoba bez uprawnień pytająca o dane klienta dostaje „nie znajduję takiej pozycji", nie potwierdzenie istnienia klienta/konta | Osoba/system obsługujący zapytania o dostęp do danych | Nie pomylić grzecznej odmowy „nie mam dostępu do tych danych" (potwierdza istnienie) z odpowiedzią neutralną |
| Obsługa prawna | Osoba spoza kręgu uprawnionego pytająca o sprawę nie otrzymuje potwierdzenia, że kancelaria taką sprawę prowadzi | Osoba przyjmująca zapytania o sprawy z zewnątrz | Nie pomylić odmowy informacji o sprawie (potwierdza jej istnienie) z odpowiedzią, która nie potwierdza ani nie zaprzecza |
| Marketing i sprzedaż | Osoba spoza zespołu pytająca o niepubliczną kampanię nie otrzymuje potwierdzenia jej istnienia | Osoba/system obsługujący zapytania o niepubliczne materiały | Nie pomylić „nie mogę Ci pokazać tej kampanii" (potwierdza istnienie) z odpowiedzią neutralną |
| Operacje i produkcja | Osoba bez uprawnień pytająca o wynik przeglądu/partii nie otrzymuje potwierdzenia, że taki przegląd istnieje | Osoba/system obsługujący zapytania o wyniki | Nie pomylić „ten wynik jest dla Ciebie niedostępny" z odpowiedzią, która nie potwierdza istnienia wyniku |

Ten wzorzec (404 zamiast 403) jest twardą konwencją techniczną w informatyce.
W dziedzinach spoza IT rzadko istnieje jako sformalizowana procedura — pytanie
17.12 sprawdza, czy w danej firmie w ogóle ma zastosowanie, zanim ktokolwiek
wpisze go jako granicę nienaruszalną.

### 18.4 Przygotowanie i środowisko

**Specyfikacja zamówienia / bieżącego etapu**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Dokument specyfikacji etapu — co ma powstać i wedle jakich reguł to oceniać | Właściciel produktu/projektu | Nie pomylić ogólnego pomysłu projektu z konkretną specyfikacją bieżącego etapu |
| Księgowość i finanse | Zakres zlecenia na dany okres rozliczeniowy — jakie pozycje, jaki termin, jakie standardy | Klient lub przełożony zlecający zakres | Nie pomylić ogólnej umowy o współpracy z konkretnym zakresem zlecenia na dany miesiąc |
| Obsługa prawna | Zakres zlecenia sprawy lub pisma — czego dotyczy, jaki termin, jakie wymogi formalne | Klient/mocodawca zlecający sprawę | Nie pomylić ogólnego pełnomocnictwa z konkretnym zakresem danego zlecenia |
| Marketing i sprzedaż | Brief kampanii — cel, grupa docelowa, kanały, termin, kryteria akceptacji | Zamawiający kampanię (klient/marka) | Nie pomylić długoterminowej strategii marki z briefem konkretnej kampanii |
| Operacje i produkcja | Zlecenie produkcyjne lub serwisowe — co, w jakiej ilości, wedle jakiej normy, w jakim terminie | Zlecający produkcję/przegląd | Nie pomylić harmonogramu rocznego z konkretnym zleceniem na dany dzień lub partię |

**Sprawdzenie, czy temat już istnieje, i środowisko przedudostępnieniowe**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Przeszukanie kodu/dokumentacji pod kątem istniejącego rozwiązania, plus test na środowisku dev przed produkcją | Wykonawca przed startem tematu | Nie pomylić testowania na produkcji z testowaniem na środowisku deweloperskim |
| Księgowość i finanse | Sprawdzenie, czy korekta nie została już wprowadzona, plus przeliczenie na kopii roboczej przed wpisem do rejestru | Wykonawca przed wprowadzeniem korekty | Nie pomylić testowej korekty w kopii z wpisem bezpośrednio do już wysłanego rozliczenia |
| Obsługa prawna | Sprawdzenie rejestru spraw pod kątem podobnego zagadnienia, plus wewnętrzny przegląd projektu dokumentu przed wysłaniem | Prawnik przed podjęciem sprawy | Nie pomylić wysłania projektu pisma do sądu lub strony z jego wewnętrznym przeglądem |
| Marketing i sprzedaż | Sprawdzenie, czy podobny materiał już nie powstał, plus test na małej grupie przed pełną publikacją | Zespół przed uruchomieniem materiału/kampanii | Nie pomylić testu na wąskiej grupie z pełnym uruchomieniem kampanii do całej bazy |
| Operacje i produkcja | Sprawdzenie, czy zadanie nie zostało już wykonane, plus próba na partii testowej przed pełną produkcją | Zespół przed uruchomieniem pełnej produkcji | Nie pomylić próby na partii testowej z uruchomieniem pełnej linii produkcyjnej |

**Środowisko rzeczywiste a ćwiczebne**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Środowisko produkcyjne z prawdziwymi danymi a środowisko deweloperskie/testowe | Zespół utrzymujący oba środowiska | Nie pomylić prawdziwych danych osobowych użytkowników z danymi testowymi w środowisku deweloperskim |
| Księgowość i finanse | Dane rzeczywistego klienta w systemie produkcyjnym a dane testowe/anonimizowane do ćwiczeń | Administrator systemu księgowego | Nie pomylić danych rzeczywistego klienta użytych do przećwiczenia procedury z danymi anonimizowanymi |
| Obsługa prawna | Akta rzeczywistej sprawy a materiały szkoleniowe niezawierające danych rzeczywistego klienta | Administrator systemu obiegu dokumentów kancelarii | Nie pomylić danych rzeczywistej sprawy użytych jako przykład szkoleniowy z materiałem wzorcowym |
| Marketing i sprzedaż | Kampania skierowana do rzeczywistych odbiorców a wersja testowa/wewnętrzna | Administrator platformy wysyłkowej/publikacyjnej | Nie pomylić wysyłki testowej z wysyłką do rzeczywistej bazy odbiorców |
| Operacje i produkcja | Rzeczywista partia dla klienta a partia testowa | Kierownik zakładu zarządzający liniami testowymi i produkcyjnymi | Nie pomylić partii testowej z partią przeznaczoną dla rzeczywistego klienta |

**Praca nad środowiskiem uruchomieniowym**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Praca nad wdrożeniem, kontenerami, bazą, kopiami zapasowymi — odróżniona od pracy nad samą funkcją | Rola infrastrukturalna, odróżniona od roli produktowej | Nie pomylić zmiany w konfiguracji środowiska z merytoryczną zmianą w produkcie |
| Księgowość i finanse | Praca nad samym systemem/narzędziem księgowym, odróżniona od pracy nad konkretnym rozliczeniem | Administrator/dostawca systemu księgowego | Nie pomylić konfiguracji programu księgowego z samym rozliczeniem klienta |
| Obsługa prawna | Praca nad systemem obiegu dokumentów, odróżniona od pracy nad konkretną sprawą | Administrator systemu obiegu dokumentów | Nie pomylić administracji systemem z merytoryczną pracą nad sprawą |
| Marketing i sprzedaż | Praca nad narzędziem/platformą do wysyłki czy publikacji, odróżniona od pracy nad konkretną kreacją | Administrator narzędzi/platformy marketingowej | Nie pomylić konfiguracji narzędzia do wysyłki z treścią samej kampanii |
| Operacje i produkcja | Praca nad samą linią/maszyną/infrastrukturą zakładu, odróżniona od pracy nad konkretnym zleceniem | Dział utrzymania ruchu/technolog | Nie pomylić konserwacji maszyny z wykonaniem konkretnego zlecenia na tej maszynie |

### 18.5 Przebieg pracy i ludzie

**Podział na równoległe strumienie pracy z regułą przeciw samooszukiwaniu**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Podział tematu na logikę / testy / bezpieczeństwo, z regułą przeciw samooszukiwaniu dla każdego strumienia | Prowadzący temat dzieli pracę na węzły | Nie pomylić podziału na strumienie z pozwoleniem, by jeden wykonawca robił wszystko po kolei bez niezależności |
| Księgowość i finanse | Podział zamknięcia okresu na przychody / koszty / rozrachunki, z regułą, że różnicy nie wolno „wyzerować" bez wyjaśnienia | Kierownik zamknięcia dzieli zadania między osoby | Nie pomylić podziału odpowiedzialności z sytuacją, gdy jedna osoba liczy i sama siebie akceptuje w każdym strumieniu |
| Obsługa prawna | Podział pracy nad sprawą na stan faktyczny / podstawę prawną / redakcję pisma, z regułą, że nie wolno pominąć niewygodnego przepisu | Partner prowadzący sprawę dzieli pracę | Nie pomylić podziału pracy z pominięciem etapu, który akurat jest niewygodny lub czasochłonny |
| Marketing i sprzedaż | Podział kampanii na treść / kanały / budżet, z regułą, że nie wolno zawyżać prognozy wyniku bez podstawy | Kierownik kampanii dzieli pracę między role | Nie pomylić podziału ról z pozwoleniem, by twórca sam oceniał zgodność własnej pracy z briefem |
| Operacje i produkcja | Podział zlecenia na przygotowanie / wykonanie / kontrolę, z regułą, że nie wolno pomijać etapu kontroli dla przyspieszenia | Kierownik produkcji dzieli zlecenie na etapy | Nie pomylić podziału etapów z łączeniem wykonania i kontroli w jednej osobie |

**Awaria i jej skutek**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Temat bez ruchu przez ustalony czas — eskalacja do właściciela zamiast cichego przeczekania | Watchdog procesu/rola prowadząca zgłasza brak ruchu | Nie pomylić braku ruchu z normalnym, długim, ale aktywnym przetwarzaniem — próg musi być jasno ustalony |
| Księgowość i finanse | Zlecenie zawieszone bez postępu (brak dokumentów, błąd systemu) — eskalacja zamiast cichego przekroczenia terminu | Osoba monitorująca terminy zgłasza przełożonemu | Nie pomylić oczekiwania na dokumenty od klienta (informowanego) z cichym zawieszeniem bez śladu |
| Obsługa prawna | Sprawa utknięta bez odpowiedzi lub decyzji przez ustalony czas — eskalacja, nie ciche przekroczenie terminu | Osoba monitorująca terminy zgłasza partnerowi | Nie pomylić uzasadnionego oczekiwania na sąd/urząd z zawieszeniem sprawy bez monitorowania terminu |
| Marketing i sprzedaż | Kampania zablokowana (brak akceptacji, awaria narzędzia) — eskalacja zamiast ciszy do dnia publikacji | Kierownik kampanii zgłasza zamawiającemu | Nie pomylić oczekiwania na akceptację klienta (z przypomnieniem) z cichym porzuceniem tematu |
| Operacje i produkcja | Zlecenie/linia zatrzymane bez postępu (awaria, brak materiału) — eskalacja, nie ciche opóźnienie bez zgłoszenia | Kierownik zmiany zgłasza przełożonemu | Nie pomylić planowego przestoju z awarią, o której nikt nie zgłosił eskalacji |

**Przekazanie pracy dalej**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Raport terminalny z destylatem stanu, nie surowymi logami | Wykonawca/sprawdzający sporządza raport | Nie pomylić zrzutu surowych logów z destylatem — nadmiar danych zalewa odbiorcę i ukrywa sygnał |
| Księgowość i finanse | Podsumowanie zamkniętego okresu dla kolejnego etapu — co zrobiono, co pozostało | Osoba zamykająca okres sporządza podsumowanie | Nie pomylić przekazania całego surowego arkusza roboczego z jasnym podsumowaniem stanu i wyjątków |
| Obsługa prawna | Notatka o stanie sprawy i otwartych kwestiach przy przekazaniu między osobami | Prawnik przekazujący sprawę sporządza notatkę | Nie pomylić przekazania całych akt bez komentarza z notatką wskazującą, co jest istotne i otwarte |
| Marketing i sprzedaż | Podsumowanie ustaleń i statusu akceptacji przy przekazaniu między etapami (np. kreacja → media) | Zespół kreacji przekazuje materiał z podsumowaniem | Nie pomylić przekazania folderu ze wszystkimi wersjami roboczymi z jasnym wskazaniem wersji aktualnej |
| Operacje i produkcja | Protokół stanu i otwartych problemów przy przekazaniu zmiany | Kierownik zmiany przekazuje protokół następnej zmianie | Nie pomylić ustnego przekazania zmiany „na szybko" z zapisanym protokołem stanu |

**Zatwierdzenie przez człowieka**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Decyzje o koszcie, zakresie, ryzyku wymagają jednoznacznej odpowiedzi właściciela, nie rekomendacji agenta | Właściciel projektu | Nie pomylić odpowiedzi „chyba tak" z jednoznaczną zgodą — niejednoznaczna odpowiedź nie jest decyzją |
| Księgowość i finanse | Decyzje wpływające na wynik finansowy klienta wymagają zgody klienta/przełożonego, nie tylko oceny wykonawcy | Klient lub przełożony z uprawnieniem decyzyjnym | Nie pomylić braku sprzeciwu klienta wobec propozycji z jego wyraźną zgodą |
| Obsługa prawna | Decyzje strategiczne w sprawie (np. ugoda, kierunek pisma) wymagają zgody klienta/mocodawcy | Klient/mocodawca | Nie pomylić milczenia klienta z akceptacją strategii procesowej |
| Marketing i sprzedaż | Decyzje dotyczące budżetu i wizerunku marki wymagają akceptacji zamawiającego, nie tylko zespołu wykonawczego | Zamawiający (klient/marka) | Nie pomylić braku odpowiedzi na wiadomość z akceptacją kreacji |
| Operacje i produkcja | Decyzje dotyczące bezpieczeństwa lub jakości wymagają zatwierdzenia przez uprawnioną osobę, nie tylko wykonawcy | Osoba z uprawnieniem decyzyjnym (np. kierownik zakładu) | Nie pomylić „pewnie OK" brygadzisty z formalnym zatwierdzeniem zwolnienia partii |

**Przećwiczone zabezpieczenie ciągłości**

| DZIEDZINA | POSTAĆ W TEJ DZIEDZINIE | KTO TO ROBI | CZEGO NIE WOLNO POMYLIĆ |
|---|---|---|---|
| Wytwarzanie oprogramowania | Kopia zapasowa, którą faktycznie przetestowano przez odtworzenie | Zespół odpowiedzialny za infrastrukturę | Nie pomylić istnienia backupu z jego sprawdzonym odtworzeniem — kopia nieprzetestowana nie jest zabezpieczeniem |
| Księgowość i finanse | Kopia danych, z której faktycznie odtworzono próbny zestaw, nie tylko „kopia istnieje na dysku" | Administrator systemu księgowego | Nie pomylić rutynowego wykonywania kopii z faktycznym sprawdzeniem, że dane dają się odtworzyć |
| Obsługa prawna | Kopia akt, z której faktycznie sprawdzono odtworzenie dostępu do sprawy | Administrator systemu obiegu dokumentów/archiwum | Nie pomylić archiwizacji akt z potwierdzonym dostępem do nich po odtworzeniu |
| Marketing i sprzedaż | Kopia materiałów, z której faktycznie przywrócono wersję na próbę | Administrator repozytorium materiałów | Nie pomylić przechowywania plików źródłowych z potwierdzoną możliwością ich przywrócenia i użycia |
| Operacje i produkcja | Plan awaryjny lub zapasowa linia, faktycznie przetestowane w praktyce, nie tylko spisane na papierze | Kierownik zakładu odpowiedzialny za plan awaryjny | Nie pomylić spisanej procedury awaryjnej z faktycznie przećwiczonym scenariuszem awarii |

### 18.6 Dziedziny, których tu nie ma

Powyższa piątka jest przykładowa, nie wyczerpująca — w kolejnej dziedzinie,
której tu nie ma, każde pojęcie odwzorowuje się od nowa pytaniem, nie
domysłem. Agent wchodzący w nieznaną dziedzinę zadaje sobie kolejno:

1. Co jest tu wytworem, a co tylko relacją o wytworze — po czym poznam, że
   praca faktycznie powstała, a nie że ktoś o niej opowiedział?
2. Kto tu sprawdza czyjąś pracę i czym różni się to sprawdzenie od
   powtórzenia tej samej czynności tym samym sposobem przez tę samą osobę?
3. Co w tej dziedzinie jest nieodwracalne albo trudne do cofnięcia — i kto
   ma prawo to naruszyć, jeśli w ogóle ktokolwiek ma?

---

## 19. Kalibracja liczb — o co pytasz właściciela na starcie

Te pytania zadaje się **po** pytaniach rozpoznających dziedzinę (§17) —
dopiero gdy wiadomo, co w tej pracy jest wytworem i po czym poznać, że jest
dobrze zrobiony, ma sens pytać o liczby i ustawienia procesu, który to
sprawdza.

Wszystkie pytania zadaje się **naraz**, w jednej wiadomości. Właściciel
odpowiada krótkim kodem przy każdym numerze, na przykład: `1B, 2A, 3C, 4B,
5A, 6C`. Odpowiedź niepełna — brak litery przy którymś numerze, albo „chyba
tak", „raczej B", „coś pomiędzy" — nie jest decyzją, a pytanie zostaje
otwarte do czasu jednoznacznej odpowiedzi. Po odpowiedziach agent zapisuje
wypełnioną tabelę parametrów (na końcu tej sekcji) i dopiero wtedy zaczyna
pracę — nie wcześniej.

Krotności kosztu w tym dokumencie to rząd wielkości, nie kwoty — cen nie
znamy. Punktem odniesienia (1×) jest zawsze wariant najtańszy w danym
pytaniu.

### 19.1 Który model do wykonania, który do sprawdzenia, który do kontroli końcowej — i jak dokładnie mają myśleć

**To pytanie zadaje się tylko wtedy, gdy z odpowiedzi na pytanie rozpoznające
§17.1 wynika, że wykonawcą jest — choćby częściowo — program.** Gdy w tym
projekcie wykonują wyłącznie ludzie, pytanie 19.1 odpada w całości, a tabela
w §19.7 nie ma tych czterech wierszy.

**Dlaczego o to pytam.** W obecnym projekcie nAgents wszystkie trzy role —
ten, kto wykonuje zlecenie (Operator), ten, kto sprawdza wynik (Evaluator), i
ten, kto robi ostatnią kontrolę przed zamknięciem sprawy (Final Control) —
używają tego samego, najmocniejszego dostępnego programu językowego
(„modelu") i każą mu myśleć najdokładniej, jak potrafi („poziom wysiłku" —
ile czasu i uwagi model poświęca jednemu zadaniu, zanim odpowie). To
najdroższe możliwe ustawienie. W innej dziedzinie może nie być potrzebne —
albo odwrotnie, może być potrzebne bardziej niż tutaj, jeśli błąd jest
kosztowny. Nie mogę tego rozstrzygnąć sam, bo to bezpośrednio przekłada się
na koszt każdego zlecenia.

**Pytanie.** Jakiego programu (którego „modelu") ma używać wykonawca, jakiego
sprawdzający, jakiego kontrola końcowa, i jak dokładnie każdy z nich ma się
zastanawiać nad swoją częścią pracy?

**Wariant A — jeden, tańszy i prostszy model dla wszystkich trzech ról, myślący płyciej.**
Co to znaczy w praktyce: wykonawca, sprawdzający i kontrola końcowa korzystają z tego samego, najtańszego dostępnego modelu, nastawionego na szybką odpowiedź, nie na dogłębne rozważanie.
Co zyskujesz: najniższy koszt każdego zlecenia, najszybszy czas odpowiedzi.
Co tracisz: większe ryzyko przeoczenia subtelnego błędu — model płyciej „przemyśli" trudny przypadek.
Koszt: 1× (punkt odniesienia).
Ryzyko: przy złożonym albo nietypowym zleceniu tańszy model może uznać błędną pracę za poprawną, bo nie dokopał się do sedna.

**Wariant B — jeden, najmocniejszy model dla wszystkich trzech ról, myślący najdokładniej (ustawienie dzisiejsze w nAgents).**
Co to znaczy w praktyce: wykonawca, sprawdzający i kontrola końcowa korzystają z tego samego, najmocniejszego dostępnego modelu, z nakazem dokładnego rozważenia każdej odpowiedzi.
Co zyskujesz: najwyższą staranność na każdym etapie, najmniejsze ryzyko przeoczenia błędu.
Co tracisz: najwyższy koszt, niezależnie od tego, czy dane zlecenie było proste, czy trudne.
Koszt: rzędu kilkukrotnie do dziesięciokrotnie droższy niż wariant A, i to dla każdej z trzech ról naraz — koszt całego tematu rośnie odpowiednio bardziej.
Ryzyko: przepłacanie przy rutynowych, prostych zleceniach, które nie wymagały takiej staranności.

**Wariant C — zróżnicowane: tańszy model dla wykonawcy, mocniejszy dla sprawdzającego i kontroli końcowej.**
Co to znaczy w praktyce: wykonawca pracuje tańszym modelem, ale wynik ocenia sprawdzający i kontrola końcowa mocniejszym modelem, myślącym dokładnie.
Co zyskujesz: niezależna, staranna ocena przy niższym koszcie samego wykonania — sprawdzanie zwykle wymaga przeczytania krótszego materiału niż jego napisanie od zera, więc bywa tańsze niż wykonanie nawet przy droższym modelu.
Co tracisz: samo wykonanie jest mniej staranne od startu, więc więcej błędów trafia dopiero na etap sprawdzania zamiast być uniknięte wcześniej.
Koszt: rzędu 2–4× wariantu A — więcej niż A, wyraźnie mniej niż B, bo drogi model pracuje tylko na dwóch z trzech etapów i na krótszym materiale.
Ryzyko: jeśli wykonawca generuje więcej błędów niż zakładano, rośnie liczba rund poprawek (patrz pytanie 19.2), co zjada część oszczędności.

**Jeśli nie odpowiesz.** Wartość domyślna: wariant A (najtańszy model, jedna,
płytsza warstwa myślenia dla wszystkich ról). Skutek: praca rusza tanio, ale
bez sprawdzenia, czy w tej dziedzinie taka staranność wystarcza — właściciel
może podnieść poziom w każdej chwili.

**Co zapisujemy.** {model-wykonawcy}, {model-sprawdzajacego},
{model-kontroli-koncowej}, {poziom-wysilku-mysleniowego} — w opisie procesu
tej dziedziny, odpowiednik §11.2 SKILL.md.

### 19.2 Ile razy próbować to samo zlecenie, zanim sprawa wraca do Pana/Pani

**Dlaczego o to pytam.** Gdy sprawdzenie wykaże błąd, wykonawca dostaje
szansę na poprawkę. Liczba takich podejść („rund") ma granicę — po jej
wyczerpaniu temat musi trafić z powrotem do Pana/Pani, nie krążyć w kółko bez
końca. W nAgents ta granica wynosi dziś 3. Nie wiem, czy w tej dziedzinie
poprawka jest tania i szybka (wtedy więcej podejść ma sens), czy każde
nieudane podejście już samo w sobie kosztuje wiele (wtedy lepiej eskalować
szybciej).

**Pytanie.** Po ilu nieudanych podejściach do tego samego zlecenia sprawa ma
automatycznie wrócić do Pana/Pani, zamiast być poprawianą dalej?

**Wariant A — 1 podejście.**
Co to znaczy w praktyce: pierwsza nieudana ocena od razu kończy automatyczną pracę nad tematem i przekazuje go Panu/Pani.
Co zyskujesz: żadna drobna pomyłka nie zdąży się namnożyć, zanim ją zobaczysz.
Co tracisz: nawet oczywiste do poprawienia usterki, które model mógłby sam skorygować, trafiają na Pana/Pani biurko.
Koszt: 1× (punkt odniesienia — najmniej pracy automatycznej na temat).
Ryzyko: zasypanie Pana/Pani drobnymi sprawami, które nie wymagały interwencji człowieka.

**Wariant B — 3 podejścia (ustawienie dzisiejsze w nAgents).**
Co to znaczy w praktyce: wykonawca dostaje do trzech szans na poprawienie tego samego zlecenia, dopiero czwarta nieudana ocena eskaluje temat.
Co zyskujesz: rozsądny margines na samodzielną korektę bez angażowania Pana/Pani przy każdej drobnostce.
Co tracisz: w najgorszym razie temat przechodzi przez pełny cykl wykonanie-plus-sprawdzenie trzy razy, zanim ktokolwiek się zorientuje, że utknął.
Koszt: do 3× kosztu jednego podejścia, w najgorszym przypadku.
Ryzyko: przy naprawdę trudnym zleceniu trzy podejścia mogą się nie różnić jakością — temat traci czas, zanim i tak trafi do Pana/Pani.

**Wariant C — 5–6 podejść.**
Co to znaczy w praktyce: wykonawca ma znacznie więcej prób samodzielnej poprawy, zanim sprawa wraca do Pana/Pani.
Co zyskujesz: większa szansa, że nawet zlecenie wymagające kilku iteracji zostanie domknięte bez Pana/Pani udziału.
Co tracisz: dłuższy czas, zanim naprawdę zablokowany temat w ogóle do Pana/Pani dotrze.
Koszt: do 5–6× kosztu jednego podejścia, w najgorszym przypadku.
Ryzyko: temat może „kręcić się" długo bez realnego postępu, zanim limit w końcu zadziała — a licznik nie wolno w tym czasie po cichu zerować (to zasada, nie parametr).

**Jeśli nie odpowiesz.** Wartość domyślna: 3 podejścia (jak dziś w nAgents).
Skutek: praca rusza z rozsądnym, ale niesprawdzonym pod kątem tej dziedziny
marginesem — może być za ciasny albo za luźny względem tego, ile faktycznie
kosztuje tu jedna nieudana próba.

**Co zapisujemy.** {liczba-podejsc-przed-eskalacja} — w opisie procesu tej
dziedziny, odpowiednik §4.5 SKILL.md.

### 19.3 Ile zleceń może być w pracy jednocześnie

**Dlaczego o to pytam.** „Wykonawca" to osoba albo program realizujący jedno
zlecenie; kilku wykonawców naraz oznacza, że kilka zleceń idzie do przodu w
tym samym czasie, a nie jedno po drugim w kolejce. W nAgents dziś w pracy
mogą być jednocześnie 2 tematy. Więcej — szybszy łączny czas realizacji, ale
więcej wyników trafia do sprawdzenia naraz, co obciąża osobę sprawdzającą i
Pana/Pani przegląd. Mniej — wolniej, ale nic nie umyka uwadze.

**Pytanie.** Ile zleceń może być jednocześnie w pracy, zanim kolejne musi
poczekać, aż któreś z obecnych się skończy?

**Wariant A — 1 zlecenie naraz.**
Co to znaczy w praktyce: kolejne zlecenie czeka, aż poprzednie zostanie w pełni zamknięte.
Co zyskujesz: pełna uwaga na jednym temacie, zero ryzyka pomylenia wyników dwóch zleceń.
Co tracisz: czas kalendarzowy — praca stoi w kolejce, nawet gdy zasoby na więcej są dostępne.
Koszt: 1× (punkt odniesienia).
Ryzyko: przy wielu drobnych zleceniach kolejka rośnie i całość trwa wyraźnie dłużej niż musiałaby.

**Wariant B — 2 zlecenia naraz (ustawienie dzisiejsze w nAgents).**
Co to znaczy w praktyce: dwa tematy mogą iść do przodu równocześnie, trzeci czeka.
Co zyskujesz: krótszy łączny czas oczekiwania niż przy jednym naraz.
Co tracisz: sprawdzający i Pan/Pani może dostać do przejrzenia dwa wyniki blisko siebie w czasie.
Koszt: rzędu 2× zużycia zasobów w danym momencie względem wariantu A.
Ryzyko: przy pobieżnym przeglądzie dwóch wyników naraz łatwiej coś przeoczyć niż przy jednym.

**Wariant C — 4–5 zleceń naraz.**
Co to znaczy w praktyce: kilka tematów idzie do przodu równolegle, kolejka niemal nie powstaje.
Co zyskujesz: najkrótszy łączny czas realizacji całej puli zleceń.
Co tracisz: znacznie większe obciążenie strony sprawdzającej i Pana/Pani przeglądu — kilka wyników naraz do oceny.
Koszt: rzędu 4–5× zużycia zasobów w danym momencie względem wariantu A.
Ryzyko: przy natłoku wyników do przejrzenia rośnie szansa, że któryś zostanie zaakceptowany pobieżnie, bez pełnej uwagi.

**Jeśli nie odpowiesz.** Wartość domyślna: 1 zlecenie naraz (ostrożniej niż
dzisiejsze 2 w nAgents, bo dla nowej dziedziny nie wiadomo jeszcze, ile
uwagi wymaga jeden przegląd). Skutek: praca idzie wolniej, ale nic nie
mnoży się bez Pana/Pani wiedzy, dopóki nie padnie jednoznaczna odpowiedź.
Przypominam też, że praca wielu wykonawców naraz sama w sobie wymaga
osobnej, jawnej zgody na daną sesję pracy — to już ustalona zasada, nie
przedmiot tego pytania.

**Co zapisujemy.** {liczba-tematow-rownoleglych} — w opisie procesu tej
dziedziny, odpowiednik §10 SKILL.md.

### 19.4 Po jakim czasie milczenia uznajemy wykonawcę za zawieszonego

**Dlaczego o to pytam.** Czasem wykonawca przestaje dawać znać o postępie —
nie wiadomo, czy nadal pracuje, czy utknął. Zbyt krótki czas do uznania
takiego stanu daje fałszywe alarmy przy zwykłych, dłuższych etapach pracy
(np. długim sprawdzaniu). Zbyt długi czas opóźnia wykrycie, że coś naprawdę
stoi w miejscu. Punktem wyjścia jest odpowiedź na pytanie rozpoznające §17.2 —
ile trwa typowy krok pracy w tej dziedzinie; próg zawieszenia ustawia się
wyraźnie poniżej tego czasu, nie w oderwaniu od niego. W nAgents ten czas
wynosi dziś 20 minut.

**Pytanie.** Po ilu minutach ciszy ze strony wykonawcy uznajemy, że utknął, i
trzeba to zgłosić?

**Wariant A — 10 minut.**
Co to znaczy w praktyce: brak jakiegokolwiek śladu postępu przez 10 minut uruchamia alarm o zawieszeniu.
Co zyskujesz: najszybsze wykrycie realnego problemu.
Co tracisz: normalne, dłuższe etapy pracy (np. dokładne sprawdzanie) mogą fałszywie wyglądać jak zawieszenie.
Koszt: rzędu 2–3× częstsze fałszywe alarmy niż przy wariancie B, każdy wymaga chwili Pana/Pani uwagi na sprawdzenie, czy to prawdziwy problem.
Ryzyko: przyzwyczajenie się do częstych fałszywych alarmów i ich ignorowanie — a wtedy prawdziwe zawieszenie też może zostać zignorowane.

**Wariant B — 20 minut (ustawienie dzisiejsze w nAgents).**
Co to znaczy w praktyce: cisza dłuższa niż 20 minut uruchamia alarm.
Co zyskujesz: rozsądny balans między szybkością wykrycia a liczbą fałszywych alarmów.
Co tracisz: przy naprawdę długich, złożonych etapach pracy wciąż możliwy jest pojedynczy fałszywy alarm.
Koszt: 1× (punkt odniesienia).
Ryzyko: przy dziedzinie, w której pojedynczy krok pracy trwa naturalnie dłużej niż 20 minut, alarmy i tak będą fałszywe.

**Wariant C — 40–60 minut.**
Co to znaczy w praktyce: dopiero cisza dłuższa niż 40–60 minut uruchamia alarm.
Co zyskujesz: prawie zero fałszywych alarmów nawet przy długich etapach pracy.
Co tracisz: prawdziwe zawieszenie wykrywane jest znacznie później.
Koszt: nie mnoży pracy, ale wydłuża czas reakcji na realny problem o tyle samo, o ile wydłużono próg — czyli około 2–3× dłużej niż przy wariancie B, zanim ktokolwiek się zorientuje.
Ryzyko: temat może stać martwy nawet godzinę, zanim ktokolwiek to zauważy.

**Jeśli nie odpowiesz.** Wartość domyślna: 20 minut (jak dziś w nAgents).
Skutek: próg startuje nieskalibrowany pod długość typowego etapu pracy w tej
dziedzinie — może dawać fałszywe alarmy albo, przeciwnie, zbyt późno
wykrywać realne zawieszenie.

**Co zapisujemy.** {czas-do-uznania-zawieszenia} — w opisie procesu tej
dziedziny, odpowiednik §10 SKILL.md.

### 19.5 Gdzie dokładnie ma powstawać wersja robocza każdego zlecenia

**Dlaczego o to pytam.** Z pytania rozpoznającego dziedzinę (§17.7) wiadomo
już, że praca ma powstawać osobno od wersji obowiązującej, zanim zostanie
zatwierdzona. Zostaje wskazać konkretne, nazwane miejsce — na przykład osobny
system śledzenia wersji z historią zmian, wspólny folder roboczy, albo
numerowaną wersję dokumentu — żeby wykonawca zawsze wiedział, gdzie zaczynać,
a sprawdzający wiedział, co dokładnie ocenia. Pomyłka co do tego miejsca bywa
w niektórych dziedzinach trudna albo niemożliwa do cofnięcia.

**Pytanie.** Gdzie dokładnie (jakie miejsce, jaka nazwa albo wzór nazwy) ma
powstawać wersja robocza każdego zlecenia, i dokąd trafia wynik po
zatwierdzeniu?

**Wariant A — jedno wspólne miejsce robocze dla wszystkich zleceń po kolei.**
Co to znaczy w praktyce: wszystkie zlecenia korzystają z tego samego miejsca roboczego, jedno na raz, po kolei.
Co zyskujesz: prostotę — jedna nazwa do zapamiętania, brak księgowości miejsc roboczych.
Co tracisz: możliwość prowadzenia dwóch zleceń naraz bez ryzyka wymieszania ich pracy.
Koszt: 1× (punkt odniesienia, najprostsze do utrzymania).
Ryzyko: jeśli mimo wszystko dwa zlecenia zaczną się równolegle, ich praca może się nadpisać albo wymieszać.

**Wariant B — osobne miejsce robocze dla każdego zlecenia z osobna, nazwane według stałego wzoru (jak dziś w nAgents: ustalona konwencja nazw miejsc roboczych, jedna na temat).**
Co to znaczy w praktyce: każde zlecenie dostaje własną, jednoznacznie nazwaną przestrzeń roboczą, niezależną od innych.
Co zyskujesz: zlecenia nie wchodzą sobie w drogę, jedno można cofnąć bez ruszania pozostałych.
Co tracisz: dodatkowy porządek do pilnowania — trzeba wiedzieć, które miejsce odpowiada któremu zleceniu.
Koszt: rzędu 1,5–2× wariantu A, i to wyłącznie wtedy, gdy zlecenia faktycznie idą równolegle — przy pracy pojedynczej różnicy praktycznie nie ma.
Ryzyko: przy niedbałym nazewnictwie można pomylić, które miejsce robocze odpowiada któremu zleceniu.

**Wariant C — praca powstaje wprost w miejscu obowiązującym, bez osobnej wersji roboczej.**
Co to znaczy w praktyce: to, co wykonawca zapisze, od razu jest wersją używaną na zewnątrz — nie ma etapu szkicu.
Co zyskujesz: najkrótszy czas od wykonania do obowiązywania.
Co tracisz: bufor na sprawdzenie przed wejściem w życie — błąd trafia od razu tam, gdzie może zaszkodzić.
Koszt: 1× w nakładzie pracy, ale koszt naprawy błędu po fakcie rośnie rzędu kilkukrotnie do kilkunastokrotnie względem kosztu poprawki wykrytej przed wejściem w życie, bo nie ma etapu, w którym dałoby się go zatrzymać przed wyjściem na zewnątrz.
Ryzyko: największe ze wszystkich trzech — praktycznie w każdej dziedzinie zakłada się, że sprawdzenie odbywa się przed uznaniem pracy za obowiązującą, nie po.

**Jeśli nie odpowiesz.** Praca nie rusza — bez nazwanego miejsca roboczego
wykonawca nie ma gdzie zacząć, a sprawdzający nie wie, co dokładnie ocenia.
To jedyne pytanie w tym zestawie bez bezpiecznej wartości domyślnej.

**Co zapisujemy.** {miejsce-pracy-roboczej}, {wzor-nazwy-przestrzeni-roboczej},
{miejsce-i-warunek-wlaczenia-do-wersji-obowiazujacej} — w opisie sposobu
pracy tej dziedziny, odpowiednik §15.4 i §5.2 SKILL.md.

### 19.6 W jakim języku mają powstawać dokumenty tej pracy

**Dlaczego o to pytam.** Język, w którym powstają zlecenia, sprawozdania i
zapisy decyzji, decyduje o tym, kto jest w stanie je przeczytać i sprawdzić —
Pan/Pani, zespół, ewentualny audytor zewnętrzny. W nAgents dziś jest to
polski, konsekwentnie w całym procesie. Nie zakładam, że w każdej dziedzinie
to będzie ten sam wybór — np. przy pracy z klientem zagranicznym może być
potrzebny inny język dla części dokumentów.

**Pytanie.** W jakim języku mają powstawać dokumenty tej pracy — zlecenia,
sprawozdania, zapisy decyzji?

**Wariant A — polski wszędzie (ustawienie dzisiejsze w nAgents).**
Co to znaczy w praktyce: każdy dokument procesu, bez wyjątku, powstaje po polsku.
Co zyskujesz: pełną precyzję i spójność dla polskojęzycznego właściciela i zespołu.
Co tracisz: ograniczoną dostępność dla kogokolwiek nieznającego polskiego — np. przy ewentualnym audycie zewnętrznym albo współpracy z partnerem zagranicznym.
Koszt: 1× (punkt odniesienia).
Ryzyko: przy nagłej potrzebie pokazania dokumentu osobie nieznającej polskiego trzeba tłumaczyć pod presją czasu.

**Wariant B — angielski wszędzie.**
Co to znaczy w praktyce: każdy dokument procesu, bez wyjątku, powstaje po angielsku.
Co zyskujesz: szerszą dostępność, łatwiejsze pokazanie pracy stronie zagranicznej.
Co tracisz: precyzję niuansów dla polskojęzycznego właściciela — subtelności decyzji łatwiej umykają w języku, który nie jest pierwszym językiem czytającego.
Koszt: 1× (punkt odniesienia, ta sama krotność co A — sam wybór języka nie zmienia nakładu pracy).
Ryzyko: przy odpowiedziach właściciela po polsku rośnie ryzyko drobnych nieporozumień przy tłumaczeniu ustaleń na dokument.

**Wariant C — zależnie od typu dokumentu: wewnętrzne po polsku, dokumenty trafiające na zewnątrz po angielsku (albo odwrotnie).**
Co to znaczy w praktyce: dwa równoległe tory językowe, przypisane do rodzaju dokumentu, nie do całego procesu naraz.
Co zyskujesz: każdy odbiorca dostaje dokument w języku, który mu służy najlepiej.
Co tracisz: jednolitość — trzeba pilnować, który dokument w którym języku, i dbać, żeby wersje się nie rozjechały treściowo.
Koszt: rzędu 1,3–1,5× wariantu A — dodatkowy nakład na rozróżnianie i pilnowanie spójności dwóch wersji językowych.
Ryzyko: rozjazd między wersjami — dokument wewnętrzny mówi coś innego niż jego odpowiednik na zewnątrz, i nikt tego nie zauważa, dopóki nie jest za późno.

**Jeśli nie odpowiesz.** Wartość domyślna: polski wszędzie (jak dziś w
nAgents). Skutek: dokumenty powstają spójnie, ale ewentualna potrzeba innego
języka dla odbiorcy zewnętrznego pozostaje nierozwiązana do czasu
jednoznacznej odpowiedzi.

**Co zapisujemy.** {jezyk-dokumentacji-i-procesu} — w opisie procesu tej
dziedziny, odpowiednik §15.2 SKILL.md.

### 19.7 Tabela do wypełnienia

Pusta na starcie każdego nowego projektu. Wypełnia się ją dopiero po
jednoznacznych odpowiedziach właściciela na pytania 19.1–19.6 (oraz, gdy
dotyczy, na dodatkowe parametry ujawnione w rozmowie kalibrującej z §17).

| Parametr | Wartość | Skąd (numer pytania albo decyzja agenta) |
|---|---|---|
| {model-wykonawcy} | | |
| {model-sprawdzajacego} | | |
| {model-kontroli-koncowej} | | |
| {poziom-wysilku-mysleniowego} | | |
| {liczba-podejsc-przed-eskalacja} | | |
| {liczba-tematow-rownoleglych} | | |
| {czas-do-uznania-zawieszenia} | | |
| {miejsce-pracy-roboczej} | | |
| {wzor-nazwy-przestrzeni-roboczej} | | |
| {miejsce-i-warunek-wlaczenia-do-wersji-obowiazujacej} | | |
| {jezyk-dokumentacji-i-procesu} | | |

---

## 20. Dokumenty projektu — co założyć i jak to wygląda

Poniższe jest niezależne od dziedziny — te same osiem funkcji obsługuje
zamknięcie miesiąca w biurze rachunkowym, rejestr umów w kancelarii i
wdrożenie w projekcie informatycznym. Zmienia się tylko to, co trzy warstwy
tego procesu każą zmieniać: nazwa i lokalizacja pliku to **parametr**,
ustalany pytaniem na starcie danego projektu; to, co wpisuje się w pole
„dowód wykonania" jest **odwzorowaniem dziedziny**, ustalanym pytaniami z
§17 (patrz też §18); a sam fakt, że taki dokument musi istnieć i pełnić tę
funkcję, jest **zasadą** i nie podlega negocjacji.

### 20.1 Osiem funkcji dokumentacyjnych

Każdy projekt, niezależnie od dziedziny, potrzebuje ośmiu dokumentów
pełniących te funkcje. Brak którejś nie jest oszczędnością — jest dziurą,
która ujawni się dopiero wtedy, gdy będzie kosztować najwięcej.

**Funkcja: punkt wejścia i kolejność czytania**

- *Po co istnieje:* żeby każdy, kto siada do pracy — osoba albo agent —
  zaczynał od tego samego miejsca i w tej samej kolejności, zamiast składać
  obraz projektu z przypadkowych fragmentów rozmowy albo z pamięci.
- *Kto aktualizuje:* osoba prowadząca proces, przy każdej zmianie samego
  zestawu dokumentów albo kolejności ich czytania — rzadko.
- *W jakim momencie:* na starcie projektu, jako pierwszy plik jaki w ogóle
  powstaje; potem przy każdej zmianie struktury.
- *Co się psuje, gdy jej brak:* każdy uczestnik buduje własny, inny obraz
  stanu sprawy. Dwie osoby — albo agent i właściciel — rozmawiają o dwóch
  różnych projektach, nie wiedząc o tym, dopóki się nie zderzą.

**Funkcja: tryb bezpiecznej zmiany samego procesu**

- *Po co istnieje:* pliki opisujące sam proces — definicje ról, punkt
  startowy, rejestry procesu — potrzebują ostrzejszego trybu zmiany niż
  zwykły wytwór: błąd w wytworze wyłapie sprawdzający, błąd w definicji
  samego sprawdzającego nie wyłapie nikt (§2.2). Ten dokument nazywa, co wolno
  zmienić od ręki, a co wymaga osobnej decyzji, żeby niewygodna reguła nie
  zniknęła po cichu przy pierwszej okazji.
- *Kto aktualizuje:* osoba prowadząca proces, w porozumieniu z właścicielem,
  przy każdej zmianie samego trybu zmiany — rzadko.
- *W jakim momencie:* zakładany na starcie, zaraz po punkcie wejścia — zanim
  ktokolwiek dotknie plików samego procesu.
- *Co się psuje, gdy jej brak:* pierwsza niewygodna reguła zostaje po cichu
  osłabiona albo usunięta w trakcie zwykłej pracy, bo nic nie wymusza
  osobnego trybu i osobnej zgody na zmianę samych zasad.

**Funkcja: rejestr zadań ze stanem**

- *Po co istnieje:* jedno miejsce z odpowiedzią „co się dzieje teraz, co
  czeka, co jest zablokowane, co jest zamknięte" — bez przeszukiwania
  historii rozmów, maili czy skrzynki roboczej.
- *Kto aktualizuje:* osoba prowadząca proces, przy każdej zmianie stanu
  zadania.
- *W jakim momencie:* natychmiast po zmianie stanu — otwarciu, zablokowaniu,
  zamknięciu — nigdy zbiorczo, na koniec dnia czy tygodnia.
- *Co się psuje, gdy jej brak:* zadania dublują się, bo nikt nie wie, że ktoś
  już to robi. Zadanie zablokowane wygląda jak porzucone. Nie da się
  odpowiedzieć na pytanie „ile jest w toku" bez przeglądania wszystkiego od
  nowa.

**Funkcja: zdjęcie bieżącej sytuacji do przekazania**

- *Po co istnieje:* pozwala nowej osobie — albo tej samej po przerwie — wejść
  w sprawę bez czytania całej historii. To zdjęcie chwili, nie dziennik
  zdarzeń.
- *Kto aktualizuje:* osoba kończąca sesję pracy albo przekazująca sprawę
  dalej. Zastępuje dokument w całości, nie dopisuje kolejnego wpisu pod
  poprzednim.
- *W jakim momencie:* przy każdym przekazaniu — zmianie osoby, zmianie zmiany,
  końcu dnia roboczego, dłuższej przerwie w pracy nad sprawą.
- *Co się psuje, gdy jej brak:* każde przejęcie zaczyna się od odtwarzania
  kontekstu z rozproszonych źródeł. Drobne, ale istotne ustalenia — dlaczego
  coś stoi, na co się czeka — giną między sesjami i trzeba je ustalać
  ponownie, często u tej samej osoby, która już raz o tym mówiła.

**Funkcja: dosłowny zapis decyzji właściciela**

- *Po co istnieje:* odróżnia to, co właściciel faktycznie postanowił, od
  tego, co agent zinterpretował, zarekomendował albo dopowiedział sobie
  z milczenia.
- *Kto aktualizuje:* osoba prowadząca proces, wyłącznie po jednoznacznej
  odpowiedzi właściciela — nigdy prewencyjnie, „bo pewnie się zgodzi".
- *W jakim momencie:* bezpośrednio po otrzymaniu jednoznacznej odpowiedzi na
  pytanie decyzyjne, zanim ruszy dalej praca, której odpowiedź dotyczyła.
- *Co się psuje, gdy jej brak:* rekomendacja agenta z czasem zaczyna uchodzić
  za decyzję właściciela. Przy sporze nie ma jak sprawdzić, co naprawdę
  zostało ustalone, a co dopowiedziane później przez kogoś, kto był pewien,
  że tak było.

**Funkcja: dziennik wyborów trwałych z uzasadnieniem**

- *Po co istnieje:* zapisuje wybory trudne do odwrócenia — koszt, dane,
  dostęp, sposób działania — razem z powodem, zanim ktokolwiek zacznie je
  realizować, żeby za pół roku dało się odtworzyć, dlaczego jest tak, a nie
  inaczej.
- *Kto aktualizuje:* osoba prowadząca proces, w porozumieniu z właścicielem,
  w chwili podjęcia wyboru.
- *W jakim momencie:* przed rozpoczęciem pracy, która ten wybór realizuje —
  nigdy jako opis napisany po fakcie, żeby „było udokumentowane".
- *Co się psuje, gdy jej brak:* wybory zapadają milcząco w toku roboty. Nikt
  nowy nie wie, że dana droga była rozważana i świadomie odrzucona, więc
  wraca się do niej co jakiś czas od nowa, tracąc czas na ponowne odkrywanie
  tego samego argumentu.

**Funkcja: opis sytuacji, które rzecz ma obsłużyć**

- *Po co istnieje:* nazywa, po czym poznać, że praca działa — konkretne
  przypadki z życia, nie ogólny opis funkcji. Jest źródłem sprawdzenia przed
  uznaniem czegokolwiek za skończone.
- *Kto aktualizuje:* osoba prowadząca proces razem z właścicielem, przy
  rozpoznawaniu zakresu na starcie — rozszerzana w miarę odkrywania nowych
  przypadków w trakcie pracy.
- *W jakim momencie:* przed napisaniem kryteriów końca dla jakiegokolwiek
  zadania; uzupełniana, gdy pojawi się przypadek, którego lista nie
  obejmowała.
- *Co się psuje, gdy jej brak:* „zrobione" staje się deklaracją bez
  odniesienia. Nie ma z czym porównać wyniku, więc sprawdzenie sprowadza się
  do wiary w słowo wykonawcy — a deklaracja wykonawcy nie jest dowodem
  wykonania.

**Funkcja: zapis pojedynczego zlecenia sprzed startu wykonawcy**

- *Po co istnieje:* ustala, co dokładnie ma zrobić wykonawca, zanim zacznie
  — żeby po fakcie dało się sprawdzić, czy zakres nie przesunął się w
  trakcie pracy.
- *Kto aktualizuje:* osoba zlecająca; nowy plik dla każdego zlecenia, nigdy
  nadpisanie poprzedniego.
- *W jakim momencie:* zanim wykonawca zacznie pracę — nigdy jako podsumowanie
  napisane po fakcie.
- *Co się psuje, gdy jej brak:* zakres staje się tym, co wykonawca akurat
  zrobił, a nie tym, co zamówiono. Nie ma jak ocenić pracy względem punktu
  wyjścia, bo punkt wyjścia nigdy nie został zapisany — a zapis zlecenia
  powstający po starcie wykonawcy to nie zapis zlecenia, to jego uzasadnienie
  wsteczne.

### 20.2 Drzewo plików

Nazwy i lokalizacje poniżej są wzorcem, nie receptą. Projekt ustala rzeczywiste
brzmienie jako parametr — pytaniem na starcie: gdzie u was mieszkają dokumenty
procesu i jak nazywacie plik, od którego zaczyna się każda sesja pracy.

```text
<punkt-wejścia>.md                     punkt wejścia i kolejność czytania — funkcja 1
<katalog-procesu>/
  zmiana-procesu.md                    tryb bezpiecznej zmiany samego procesu — funkcja 2
  zadania.md                           rejestr zadań ze stanem — funkcja 3
  przekazanie.md                       zdjęcie bieżącej sytuacji do przekazania — funkcja 4
  decyzje-właściciela.md               dosłowny zapis decyzji właściciela — funkcja 5
  wybory-trwałe.md                     dziennik wyborów trwałych z uzasadnieniem — funkcja 6
  sytuacje.md                          opis sytuacji, które proces ma obsłużyć — funkcja 7
  zlecenia/
    SZABLON.md                         wzorzec zapisu pojedynczego zlecenia — funkcja 8
    <ID>.md                            zapis pojedynczego zlecenia, jeden plik na zadanie — funkcja 8
```

### 20.3 Oznaczenie plików

| Plik | Oznaczenie |
|---|---|
| `<punkt-wejścia>.md` | OBOWIĄZKOWY |
| `<katalog-procesu>/zmiana-procesu.md` | OBOWIĄZKOWY |
| `<katalog-procesu>/zadania.md` | ROSNĄCY |
| `<katalog-procesu>/przekazanie.md` | ROSNĄCY |
| `<katalog-procesu>/decyzje-właściciela.md` | ROSNĄCY |
| `<katalog-procesu>/wybory-trwałe.md` | ROSNĄCY |
| `<katalog-procesu>/sytuacje.md` | ROSNĄCY |
| `<katalog-procesu>/zlecenia/SZABLON.md` | OBOWIĄZKOWY |
| `<katalog-procesu>/zlecenia/<ID>.md` | HISTORYCZNY |

OBOWIĄZKOWY — musi istnieć z realną treścią od pierwszego dnia, bo bez niego
nic innego nie da się ani znaleźć, ani zlecić. ROSNĄCY — zakładany na starcie
jako pusty szkielet z nagłówkami i strukturą, wypełnia się treścią w miarę
pracy. HISTORYCZNY — powstaje wyłącznie jako skutek konkretnej pracy, nigdy
nie zakłada się go na zapas.

### 20.4 Kolejność zakładania

1. **`<punkt-wejścia>.md`** — musi powstać pierwszy, bo to jedyne miejsce
   wskazujące, gdzie szukać reszty; bez niego kolejne pliki nie mają się skąd
   wziąć w polu widzenia kogokolwiek, kto dołącza do pracy.
2. **`zmiana-procesu.md`** — zakładany zaraz potem, zanim ktokolwiek zacznie
   dotykać plików samego procesu; bez niego pierwsza zmiana zasad — nawet ta
   z rozmowy kalibrującej — nie ma ostrzejszego trybu, do którego mogłaby się
   odwołać.
3. **`zadania.md`** — zanim cokolwiek się zacznie, musi być gdzie zapisać, że
   się zaczęło; pusty rejestr w chwili pierwszego zadania jest lepszy niż jego
   tworzenie pod presją, gdy zadania już się piętrzą.
4. **`wybory-trwałe.md`** — zakładany zaraz potem, bo pierwsze trwałe wybory
   — nawet o samym kształcie tej struktury dokumentów — zapadają w rozmowie
   kalibrującej, zanim powstanie pierwsze zadanie robocze.
5. **`sytuacje.md`** — musi istnieć, zanim powstanie pierwsze zlecenie, bo
   zlecenie odwołuje się do sytuacji jako do kryterium końca, a nie da się
   odwołać do czegoś, co jeszcze nie istnieje.
6. **`zlecenia/SZABLON.md`** — zakładany przed pierwszym zleceniem, bo
   zlecenie pisane bez wzorca ryzykuje pominięcie pola, które później okaże
   się rozstrzygające.
7. **`decyzje-właściciela.md`** — zakładany jako pusty rejestr gotowy przyjąć
   pierwszy wpis, który zwykle pojawia się jeszcze podczas rozmowy
   kalibrującej proces do dziedziny.
8. **`przekazanie.md`** — zakładany jako ostatni z ośmiu, bo dopiero po
   istnieniu poprzednich siedmiu jest co streszczać w zdjęciu sytuacji; pisany
   od razu po rozmowie kalibrującej, przed pierwszym realnym zleceniem.

Plik pojedynczego zlecenia (`zlecenia/<ID>.md`) nie ma numeru w tej
kolejności — powstaje dopiero w chwili konkretnego dispatchu, nigdy wcześniej.
Wyjaśnienie w punkcie 6.

### 20.5 Szkielety do skopiowania

Treść przykładowa poniżej pochodzi z biura rachunkowego i z kancelarii —
żeby było widać, że szkielet nie jest przywiązany do informatyki. Pola
w nawiasach ostrych są parametrem albo miejscem na treść konkretnego projektu.

**`<punkt-wejścia>.md`**

```text
# <NAZWA PROJEKTU> — punkt startowy

Proces obsługi zamknięć miesięcznych klientów biura rachunkowego ProFinanse.
Ten plik jest źródłem prawdy dla procesu i pierwszą rzeczą do przeczytania
w każdej sesji pracy.

## Kolejność czytania — obowiązkowa

| # | Plik | Po co |
|---|---|---|
| 1 | ten plik | mapa źródeł i norma procesu |
| 2 | `<katalog-procesu>/zadania.md` | co aktywne, co zablokowane, co zamknięte |
| 3 | `<katalog-procesu>/przekazanie.md` | zdjęcie bieżącej sytuacji |
| 4 | `<katalog-procesu>/wybory-trwałe.md` | wybory trwałe, w tym otwarte |
| 5 | `<katalog-procesu>/sytuacje.md` | sytuacje, które proces ma obsłużyć |

**Nie zaczynaj** od samej rozmowy, starego przekazania ani od luźnych notatek
sprzed wprowadzenia tego procesu.

## Stan na dziś

Trwają zamknięcia lipca. Trzech klientów w toku, jeden zablokowany brakiem
dokumentu źródłowego.

## Zasady pracy z właścicielem

Decyzje dotyczące kosztu, terminu, danych klienta albo tego, co nieodwracalne,
zapadają wyłącznie w rozmowie z właścicielką biura i są zapisywane w
`decyzje-właściciela.md` dopiero po jednoznacznej odpowiedzi.
```

**`<katalog-procesu>/zmiana-procesu.md`**

```text
# Tryb bezpiecznej zmiany samego procesu

Ten dokument, definicje ról i punkt startowy mają ostrzejszy tryb zmiany niż
zwykłe zlecenie klienta: błąd w rozliczeniu wyłapie druga księgowa, błąd w
opisie tego, jak ma wyglądać sprawdzenie, nie wyłapie nikt.

## Co wymaga tego trybu

Zmiana definicji ról, kolejności czytania, listy granic nienaruszalnych albo
samego tego pliku.

## Tryb

1. Zmiana idzie jako osobny temat w rejestrze zadań, nigdy w allowliście
   zlecenia klienckiego — nawet jednolinijkowa.
2. Właścicielka biura potwierdza zmianę jednoznaczną odpowiedzią, zanim
   powstanie realizująca ją treść.
3. Zapis zmiany trafia do dziennika wyborów trwałych z uzasadnieniem i datą.

## Co się psuje bez tego trybu

Niewygodna reguła — na przykład wymóg drugiej lektury przed wysyłką do
urzędu — znika po cichu przy pierwszym napiętym terminie, bo nic nie wymusza
osobnej zgody na zmianę samych zasad.
```

**`<katalog-procesu>/zadania.md`**

```text
# Rejestr zadań

Źródło prawdy o tym, co jest aktywne, zablokowane i zamknięte.
Aktualizowany przy każdej zmianie stanu zadania.

Format ID: `<PREFIKS>-<NNN>-<slug>`. ID jest niezmienne i nigdy nieużywane
ponownie.

## Aktywne

| ID | CEL | Zlecenie |
|---|---|---|
| `PF-014-zamkniecie-abc-lipiec` | Zamknięcie lipca dla ABC Sp. z o.o. zgadza się z wyciągiem bankowym co do grosza | `zlecenia/PF-014-zamkniecie-abc-lipiec.md` |

## Zablokowane

| ID | CEL | Blokada | Odblokuje |
|---|---|---|---|
| `PF-009-vat-marza-kwiecien` | Rozliczenie VAT-marża klienta X za czerwiec | brak faktury źródłowej od klienta | dostarczenie faktury przez klienta |

## Do rozpoczęcia

| ID | CEL | Zależy od |
|---|---|---|
| `PF-015-zamkniecie-def-lipiec` | Zamknięcie lipca dla DEF S.A. | `PF-014` — ten sam księgowy prowadzący |

## Zamknięte

| ID | CEL | Zamknięty | Wynik |
|---|---|---|---|
| `PF-013-zamkniecie-abc-czerwiec` | Zamknięcie czerwca dla ABC Sp. z o.o. | 2026-07-05 | zaakceptowane przez klienta, bez korekt |
```

**`<katalog-procesu>/przekazanie.md`**

```text
# Przekazanie

Stan na teraz. Zastępowany w całości przy każdym przekazaniu — to nie jest
log, tylko zdjęcie bieżącej sytuacji.

**Ostatnia aktualizacja:** 2026-07-08 (koniec dnia, zmiana księgowej prowadzącej)

## Gdzie jesteśmy

Trwa zamknięcie lipca dla trzech klientów. ABC Sp. z o.o. zamknięte i
zaakceptowane. DEF S.A. w toku. Klient X czeka na dostarczenie faktury
źródłowej sprzed rozpoczęcia rozliczenia VAT-marża.

## Co zostało ustalone

- Korekty wykryte po 10. dniu miesiąca nanosi biuro samodzielnie i informuje
  klienta mailem — `wybory-trwałe.md`, wybór W-004.

## Co blokuje

| Co | Skutek | Kto odblokuje | Zegar |
|---|---|---|---|
| Brak faktury źródłowej od klienta X | rozliczenie VAT-marża stoi | klient X | nieznany |

## Decyzje czekające na właściciela

| ID | Pytanie | Termin |
|---|---|---|
| `PF-Q-003` | Czy przyjmować dokumenty korygujące po 10. dniu miesiąca następnego, czy przesuwać korektę na kolejny okres | przed zamknięciem sierpnia |

## Następny krok

Dokończyć zamknięcie DEF S.A., potem wrócić do klienta X po dostarczeniu
faktury.

## Czego nie robić

- Nie księgować korekty klienta X „na szacunek" przed otrzymaniem dokumentu.
```

**`<katalog-procesu>/decyzje-właściciela.md`**

```text
# Rejestr decyzji właściciela

Dosłowny zapis decyzji właściciela. Wpis powstaje dopiero po jednoznacznej
odpowiedzi — nie po „chyba tak", „raczej", ani po braku sprzeciwu.

## Format wpisu

ID pytania:   <pełne ID pytania>
data:         <RRRR-MM-DD>
kto:          <właściciel>
pytanie:      <jedno zdanie, o co było pytane>
odpowiedź:    <litera albo jednoznaczna treść>
skutek:       <gdzie wpisano konsekwencję, jeśli decyzja jest wyborem trwałym>

## Wpisy

---

### PF-Q-001 — korekty po terminie: biuro nanosi samo czy odsyła do klienta

ID pytania:   PF-Q-001
data:         2026-03-03
kto:          właścicielka biura rachunkowego
pytanie:      Czy korektę wykrytą po 10. dniu miesiąca nanosi biuro samo, czy odsyła do klienta i czeka?
odpowiedź:    B — biuro nanosi samodzielnie i informuje klienta mailem tego samego dnia
skutek:       wybory-trwałe.md, wybór W-004
```

**`<katalog-procesu>/wybory-trwałe.md`**

```text
# Dziennik wyborów trwałych

Każdy wybór dotyczący kosztu, danych, dostępu, terminu lub odwracalności
trafia tutaj z rozważanymi opcjami i uzasadnieniem — zanim zacznie się praca,
która go realizuje.

Format: numer, data, stan, decyzja, rozważane opcje, uzasadnienie, konsekwencje.

---

## W-004 · Korekty po terminie nanosi biuro, nie klient
**2026-03-03 · przyjęty**

**Wybór:** korektę wykrytą po 10. dniu miesiąca biuro nanosi samodzielnie i
informuje klienta mailem tego samego dnia.

**Opcje:** (a) biuro nanosi samo, (b) odsyła do klienta i czeka na jego
poprawkę, (c) nanosi warunkowo, zależnie od kwoty korekty.

**Uzasadnienie:** klienci w praktyce nie odsyłają poprawek na czas; czekanie
opóźniało zamknięcia o tydzień i więcej. Mailowe powiadomienie zastępuje zgodę
uprzednią, którą i tak trudno było uzyskać w terminie.

**Konsekwencje:** biuro bierze na siebie odpowiedzialność za poprawność
korekty bez podpisu klienta pod nią. Wymaga zapisu potwierdzającego, że klient
został poinformowany — nie samego faktu poinformowania.
```

**`<katalog-procesu>/sytuacje.md`**

```text
# Sytuacje

Realne sytuacje, które proces ma obsłużyć. Powstają przed rozpoczęciem pracy
i są źródłem sprawdzenia — po nich poznaje się, że coś działa, a nie tylko,
że ktoś tak twierdzi.

---

## Zamknięcia miesięczne

| # | Sytuacja | Kiedy musi działać |
|---|---|---|
| Z1 | Klient dostarcza komplet dokumentów przed 5. dniem miesiąca — zamknięcie gotowe przed 10. | od startu |
| Z2 | Klient dostarcza dokument po terminie — korekta nanoszona zgodnie z wyborem W-004, nie wstrzymuje pozostałych klientów | od startu |
| Z3 | Faktura z błędnym numerem NIP — odrzucona z jasnym powodem, nie księgowana „na później" | od startu |

## Kontakt z klientem

| # | Sytuacja | Kiedy musi działać |
|---|---|---|
| K1 | Klient pyta o kwotę zobowiązania spoza bieżącego miesiąca — odpowiedź wyłącznie na podstawie policzonych danych, nigdy z szacunku | od startu |
```

**`<katalog-procesu>/zlecenia/SZABLON.md`**

```text
# Szablon zapisu zlecenia

Kopiuj do `zlecenia/<PEŁNE-ID>.md`, zanim zacznie pracę wykonawca. Zlecenie
bez tego pliku jest naruszeniem procesu — bez niego nie da się później
sprawdzić, czy zakres nie przesunął się w trakcie.

---

ZLECENIE:  <PEŁNE ID>                        np. PF-014-zamkniecie-abc-lipiec
DATA:      <RRRR-MM-DD>

WYZWALACZ
<Dlaczego to zlecenie startuje teraz i kto tak zdecydował. „Bo była kolej"
nie jest wyzwalaczem. Przykład: klient ABC Sp. z o.o. dostarczył komplet
dokumentów za lipiec trzeciego dnia miesiąca — zamknięcie może ruszyć.>

CEL
<Jedno zdanie. Przykład: zamknięcie lipca dla ABC Sp. z o.o. zgadza się z
wyciągiem bankowym co do grosza.>

KRYTERIA KOŃCA
- sytuacja Z1 sprawdzona: wszystkie dokumenty ujęte
- sytuacja Z3 sprawdzona: żadna faktura z błędnym NIP nie została zaksięgowana
- saldo końcowe zgodne z wyciągiem bankowym co do grosza

ZAKRES
W zakresie:     księgowanie dokumentów lipca, wyliczenie zaliczki na podatek
Poza zakresem:  korekty za czerwiec — osobne zlecenie

DOWÓD
<Co dokładnie wykonawca przedstawi jako dowód wykonania. Postać dowodu jest
odwzorowaniem dziedziny — ustala się ją pytaniem do właściciela (§17.4), nie
zakłada z góry. Przykład dla biura rachunkowego: zestawienie księgowań plus
zgodność z wyciągiem bankowym, sprawdzona ręcznie przez drugą osobę, nie
tylko wygenerowana.>

MIEJSCE PRACY
<patrz format „nazwa miejsca pracy w toku" w punkcie 20.6 — gdzie fizycznie
powstaje robocza wersja, zanim trafi do rejestru głównego>

ZALEŻNOŚCI
Zależy od:   brak
Blokuje:     PF-015-zamkniecie-def-lipiec — ten sam księgowy prowadzący
```

### 20.6 Formaty przekrojowe

**Identyfikator zadania**

```text
<PREFIKS>-<NNN>-<slug>
```
`PREFIKS` — ustalony przez projekt, zwykle skrót obszaru albo klienta (np. `PF`
dla biura rachunkowego, `UM` dla rejestru umów w kancelarii). `NNN` — trzycyfrowy,
rosnący, nigdy nieużywany ponownie. `slug` — kilka słów opisujących treść zadania,
bez spacji. ID jest niezmienne przez cały czas trwania zadania.

**Wiersz rejestru zadań**

```text
| ID | CEL | Stan | Zależy od / Blokada |
|---|---|---|---|
| `UM-021-aneks-najmu-biuro-c` | Aneks do umowy najmu biura C podpisany przez obie strony | aktywne | — |
```

**Zapis decyzji właściciela**

```text
ID pytania:   UM-021-aneks-najmu-biuro-c-Q1
data:         2026-05-14
kto:          wspólnik zarządzający
pytanie:      Czy aneks przedłużamy o rok, czy renegocjujemy stawkę od podstaw?
odpowiedź:    A — przedłużenie o rok bez renegocjacji stawki
skutek:       wybory-trwałe.md, wybór W-011
```

**Zapis wyboru trwałego**

```text
## W-011 · Aneksy najmu przedłużane bez renegocjacji stawki poniżej pięciu lat
**2026-05-14 · przyjęty**

**Wybór:** aneksy przedłużające umowy najmu poniżej pięciu lat trwania nie
otwierają renegocjacji stawki, chyba że klient sam o to wnosi.

**Opcje:** (a) zawsze renegocjować, (b) nigdy nie renegocjować, (c) nie
otwierać z inicjatywy kancelarii, ale odpowiadać na wniosek klienta.

**Uzasadnienie:** renegocjacja przy każdym aneksie wydłużała proces o tygodnie
bez proporcjonalnej korzyści przy krótkich przedłużeniach.

**Konsekwencje:** przy aneksach na dłużej niż pięć lat renegocjacja wraca jako
pytanie osobne, nieobjęte tym wyborem.
```

**Opis sytuacji do obsłużenia**

```text
| # | Sytuacja | Kiedy musi działać |
|---|---|---|
| U3 | Kontrahent żąda zmiany terminu płatności w trakcie trwania umowy — zmiana wymaga pisemnego aneksu, nie ustnej zgody | od startu |
```

**Opis pojedynczego zlecenia**

```text
ZLECENIE:  UM-021-aneks-najmu-biuro-c
WYZWALACZ: najemca wystąpił pisemnie o przedłużenie 14 maja
CEL:       aneks przedłużający najem biura C o rok podpisany przez obie strony
KRYTERIA KOŃCA: sytuacja U3 sprawdzona — zmiana terminu tylko pisemnie;
                oba podpisy złożone
ZAKRES:    przygotowanie i podpisanie aneksu; poza zakresem — renegocjacja stawki (W-011)
DOWÓD:     podpisany aneks w rejestrze umów, potwierdzenie doręczenia obu stronom
```

**Nazwa miejsca pracy w toku**

```text
robocze/<PEŁNE-ID>/
```
Miejsce, w którym powstaje robocza wersja rezultatu, oddzielona od rejestru
głównego do chwili zatwierdzenia — odpowiednik izolacji roboczej w dowolnej
dziedzinie. W kancelarii bywa to katalog z projektem pisma i korespondencją
roboczą; w biurze rachunkowym — robocza kopia arkusza rozliczeniowego, jeszcze
nieopublikowana klientowi. Przykład: `robocze/UM-021-aneks-najmu-biuro-c/` —
projekt aneksu i notatki z negocjacji, do chwili podpisania nieobecne
w rejestrze głównym.

**Opis zamknięcia zadania**

```text
ID:            UM-021-aneks-najmu-biuro-c
STAN:          ZAMKNIĘTE
DOWÓD:         aneks podpisany 2026-05-28, obie strony potwierdziły odbiór
DATA:          2026-05-28
NASTĘPNY KROK: brak
```

### 20.7 Czego nie zakładać na starcie

- **Plików pojedynczych zleceń na zapas.** `zlecenia/<ID>.md` powstaje
  wyłącznie w chwili konkretnego dispatchu. Założony wcześniej sugeruje, że
  zadanie zostało zamówione, zanim faktycznie zostało — myli plan z faktem.
- **Kategorii sytuacji, których nikt jeszcze nie zaobserwował.** Wymyślona z
  góry lista sytuacji podszywa się pod rozpoznanie, którego nie było. Fałszywa
  struktura jest gorsza niż jej brak, bo trzeba ją najpierw obalić, zanim
  wejdzie prawdziwe rozpoznanie.
- **Wpisów „przykładowych" albo „do wypełnienia później" w rejestrach.** Wiersz
  w tabeli, nawet oznaczony jako przykład, bywa później czytany jako wpis
  prawdziwy — zwłaszcza przez kogoś, kto dołącza do pracy bez pełnego
  kontekstu.
- **Dodatkowych warstw procesu ponad osiem funkcji, zanim brak którejś z nich
  faktycznie zaboli.** Dokument bez funkcji, którą ktoś potrafi nazwać, jest
  balastem — kimś musi być utrzymywany, nikt go nie czyta.
- **Treści, której nie ma pokrycia w rozmowie z właścicielem.** Dotyczy to
  wypełnienia dokumentów tak samo, jak samego procesu: czego nie ustalono,
  tego się nie wpisuje jako ustalone.

Puste pliki założone na zapas szkodzą z jednego powodu: tworzą złudzenie, że
struktura jest kompletna, podczas gdy nikt jej nie wypełnił treścią. Różnica
między „plik istnieje" a „informacja jest znana" znika z pola widzenia — a to
właśnie ta różnica ma być widoczna przez cały czas trwania projektu.

### 20.8 Lista kontrolna

- [ ] Plik wejściowy istnieje, ma realną treść i wskazuje kolejność czytania
      pozostałych siedmiu dokumentów.
- [ ] Tryb bezpiecznej zmiany samego procesu istnieje i jest założony przed
      dotknięciem jakiegokolwiek pliku samego procesu.
- [ ] Rejestr zadań istnieje, choćby pusty, z sekcjami stanu odpowiednimi dla
      tego projektu.
- [ ] Dziennik wyborów trwałych istnieje i zawiera przynajmniej te wybory,
      które zapadły podczas rozmowy kalibrującej proces do tej dziedziny.
- [ ] Opis sytuacji istnieje z przynajmniej jedną sytuacją na każdy obszar
      pracy zidentyfikowany w rozmowie kalibrującej — nie z listą wymyśloną
      bez tej rozmowy.
- [ ] Szablon zlecenia istnieje i zawiera każde pole z formatu „opis
      pojedynczego zlecenia" w punkcie 20.6.
- [ ] Rejestr decyzji właściciela istnieje, choćby pusty, gotowy przyjąć
      pierwszy wpis.
- [ ] Zdjęcie bieżącej sytuacji albo jeszcze nie istnieje — co jest poprawnym
      stanem przed pierwszym przekazaniem — albo istnieje i jest aktualne na
      dziś, nie sprzed tygodnia.
- [ ] Żaden plik pojedynczego zlecenia nie został założony na zapas.
- [ ] Nazwy i lokalizacje wszystkich ośmiu dokumentów są zapisane w pliku
      wejściowym jako ustalony parametr — nie istnieją wyłącznie w pamięci
      agenta prowadzącego pierwszą sesję.

---

## 21. Dodatek: wypełnienie dla projektu nAgents

To jest wypełnienie jednego projektu, nie norma dla innych. Projekt
przejmujący ten dokument wymienia wyłącznie tę sekcję na swoją — sekcje 0
do 20 zostają bez zmian, bo są niezależne od dziedziny i od konkretnego
projektu.

### 21.1 Odwzorowanie pojęć w naszej dziedzinie

| Pojęcie procesu | Postać w nAgents |
|---|---|
| Wytwór | Zmiana w kodzie/rejestrze/dokumentacji spełniająca `GOAL` zlecenia, faktycznie włączona do stanu obowiązującego — nie diff, nie raport, nie deklaracja Operatora (§1.2) |
| Dowód wykonania | Zielony zestaw testów automatycznych (`pytest`), sprawdzony na aktualnym stanie repozytorium niezależnie od deklaracji wykonawcy (§1.1, §5.3, §6) |
| Miejsce pracy w toku | Gałąź/worktree robocze, oddzielone od gałęzi bazowej wskazanej przez właściciela — „stan" w rozumieniu §1.2 |
| Izolacja | Osobny worktree i branch na jeden temat (`../nagents-<ID>`, `auto/<ID>`), żeby wykonawcy nie nadpisywali sobie nawzajem pracy (§5.2) |

### 21.2 Parametry i ich wartości

| Parametr | Wartość w nAgents | Sekcja źródłowa |
|---|---|---|
| {model-wykonawcy} | Sonnet 5 | §11.2 |
| {model-sprawdzajacego} | Sonnet 5 | §11.2 |
| {model-kontroli-koncowej} | Sonnet 5 | §11.2 |
| {poziom-wysilku-mysleniowego} | wysoki, jednakowo dla wszystkich trzech ról | §11.2 |
| model orkiestratora | model sesji głównego czatu, bez osobnego przydziału — rola nieujęta w §11.2, bo prowadzi rozmowę, nie wykonuje zlecenia | §3.4 |
| {liczba-podejsc-przed-eskalacja} | 3 | §4.5 |
| {liczba-tematow-rownoleglych} | 2 | §10 |
| {czas-do-uznania-zawieszenia} | 20 minut | §10 |
| {limit-objetosci-raportu} | 400 słów | §9.1 |
| {prefiks-identyfikatora-tematu} | `NAG` | §4.1 |
| {nazwy-etapow-projektu} | `MVP1 MVP2 MVP3 MVP4 PROC INFRA` | §4.1 |
| {lista-twardych-barier} | siedem granic (sekrety, brak poświadczeń agenta stanowiskowego, 404 zamiast 403, domyślna odmowa, zakaz `git add -A`/`git add .`, brak PII poza `prod`, przekazanie na zewnątrz tylko tam, gdzie wskazał właściciel) | §7 |
| {nazwy-domen-raportu} | `PRODUKT PROCES INFRA INFORMACYJNY` | §9 |
| {zestaw-statusow-raportu} | `PASS PASS-WITH-NOTES FAIL BLOCK TIMEOUT INFRA DECISION_REQUIRED` | §9 |
| {jezyk-dokumentacji-i-procesu} | polski, wszędzie | §15.2 |
| {miejsce-i-warunek-wlaczenia-do-wersji-obowiazujacej} / miejsce docelowe przekazania na zewnątrz | gałąź robocza `claude/git-connection-9sz6dg`, zapisana w `CLAUDE.md` | §15.4 |
| {miejsce-pracy-roboczej}, {wzor-nazwy-przestrzeni-roboczej} | worktree `../nagents-<ID>`, branch `auto/<ID>` | §5.2 |
| {limit-rownoleglosci-wywolan} | `min(16, liczba_CPU − 2)` | §11.3.1 |
| {progi-podzialu-tematu-na-wezly} | 2 obszary / >3 scenariusze / >6 plików / ~3000 tokenów | §11.3.2 |
| {kolejnosc-plikow-startowych} | `CLAUDE.md` → `docs/process/tematy.md` → `docs/process/handoff.md` → `docs/spec/README.md` → `docs/spec/decisions.md` → `docs/spec/scenarios.md` → `docs/spec/0X-mvpX.md` | §2.1, `CLAUDE.md` |
| {zestaw-plikow-trwalej-prawdy} | `docs/spec/00-architektura.md`, `docs/spec/decisions.md`, `docs/spec/scenarios.md`, `CLAUDE.md` | §14.1 |
| {miejsce-i-szablon-zapisu-zlecenia} | `docs/process/dispatch/<PEŁNE-ID>.md`, szablon `docs/process/dispatch/SZABLON.md` | §4.2 |
| {konwencja-numeracji-wezlow} | sufiks litery: `-a`, `-b`, `-c` | §11.3.2 |
| {mapa-miejsc-w-archiwum-projektu} | `docs/spec/`, `docs/process/`, `docs/process/zrodla/`, `docs/nota-*.md`, artefakt + kopia w repo | §15.1 |
| {zmiana-elementu-strukturalnego} | migracja struktury bazy danych (`migrations/**`, `app/models/**`) | §5.1 |
| {zrodlo-rozstrzygajace-faktu} | hierarchia pięciu rzędów źródeł dla oceny narzędzi i dostawców | §12.1 |

Skąd wzięły się wartości nietypowe: przydział modeli (§11.2) i zasada, że
cała praca wykonawcza idzie wyłącznie przez workflow (§11.3), pochodzą z
zapisów decyzji właściciela **ECHO-001** i **ECHO-002** — pełna treść w §21.5
— nie z domyślnego ustawienia szkieletu. Limit szerokości fan-outu
({limit-rownoleglosci-wywolan}) nie pochodzi z rozmowy z właścicielem —
wynika z liczby rdzeni maszyny, na której działa orkiestrator, i przelicza się
automatycznie, bez osobnego pytania kalibrującego (§11.3.1).

### 21.3 Czego nAgents świadomie nie robi

Wypełnienie reguły uniwersalnej z §13.2:

- **nie jest silnikiem agenta** — tym jest Hermes
- **nie jest komunikatorem** — tym jest Teams
- **nie przechowuje pamięci agenta** — robi to Hermes i dostawca pamięci
- **nie hostuje modeli** — te są po API, wymienne
- **nie zawiera niczego specyficznego dla NASTER w kodzie** — konfiguracja i
  wiedza, nigdy kod

### 21.4 Koszt w nAgents

Wypełnienie reguły uniwersalnej z §15.5: w nAgents głównym składnikiem kosztu
jest rachunek za modele językowe, i przewyższa on koszt infrastruktury o rząd
wielkości. Stąd optymalizujemy dobór modeli i wielkość kontekstu, nie rozmiar
serwera — warstwa rozmowy chodzi na modelu tanim, analiza nocna może być
wolna i dokładna.

### 21.5 Decyzje ECHO-001 i ECHO-002, przykłady obserwowane w praktyce

**ECHO-001** — decyzja właściciela z 2026-08-22, wypełnienie normy z §11.1:

> W tym projekcie **cała praca wykonawcza idzie do subagentów.** Orkiestrator
> prowadzi rozmowę z właścicielem, przygotowuje dispatch, integruje i wystawia
> `READY_FOR_DEPLOY` — ale nie pisze kodu ani nie prowadzi analizy samodzielnie.

To jest odwrócenie domyślnego ustawienia ze szkieletu, gdzie orkiestracja
była opcjonalna. W nAgents jest normą.

**ECHO-002** — decyzja właściciela z 2026-08-22, wypełnienie normy z §11.3:

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

Nie ma wiersza „bez workflow". Jeden agent to nadal workflow — po prostu z
jednym wywołaniem.

**Fan-out, obserwacje 2026-08-22 (§11.3.1):** przy `loadavg` 0.08 (maszyna
praktycznie bezczynna) i braku dławienia cgroup limit nadal wynosił 2 —
czekanie na „spokojniejszą porę" niczego nie zmienia, zmienia to wyłącznie
większy kontener. W workflow `nagents-pytania-abc` siedmiu Operatorów przy
limicie 2 wykonywało się falami po dwóch — trzeci startował dokładnie w
chwili zakończenia pierwszego, czyli cztery fale po dwóch zamiast
siedmiokrotnego przyspieszenia, plus koszt przełączania i siedem razy
powtórzony wstęp do promptu. Uwaga o zbieżności: limit techniczny (2) zgadza
się z pulą tematów z §10 (2), ustaloną z zupełnie innego powodu — pojemności
przeglądu jednej osoby. Przy zmianie któregokolwiek sprawdź, czy drugi nadal
ma sens.

**Koszt podziału na węzły (§11.3.2):** architektura wieloagentowa zużywa
rząd wielkości więcej tokenów niż pojedyncze zapytanie — spotykana szacunkowa
wielokrotność to około piętnastu. Liczba pochodzi ze źródła rzędu czwartego
(§12.1) i nie została zweryfikowana u źródła — traktuj jako rząd wielkości,
nie jako pomiar.

**Nasze tryby samooszukiwania — obserwowane, nie hipotetyczne (§11.3.3):**

| Tryb | Przypadek z tego projektu | Reguła zakazująca |
|---|---|---|
| Cecha narzędzia z podsumowania | „Hermes ma panel administracyjny" | Zakaz opisywania cechy narzędzia bez odwołania do źródła rzędu 1 lub 2 (§12.1) |
| Wniosek z opisu zamiast z dokumentacji | „Eve nie ma kanału Teams" — zmieniło wynik porównania | jak wyżej |
| Deklaracja zamiast artefaktu | „zleciłem uzupełnienie" — nie zlecono | Zakaz raportowania czynności bez identyfikatora zadania albo SHA |
| Pytanie o rzecz rozstrzygniętą | wariant sprzeczny z D-006 | Zakaz proponowania wariantu bez sprawdzenia dziennika decyzji |

Żaden z nich nie został złapany przez regułę procesu, bo takiej reguły nie było.

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
