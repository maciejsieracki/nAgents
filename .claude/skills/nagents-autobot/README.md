# nAgents AutoBot — co to jest i jak to wdrożyć gdzie indziej

Plik `SKILL.md` to kompletny proces pracy agentowej. **Jest samowystarczalny** —
nie wymaga innych skilli procesowych. 2913 linii, dwadzieścia dwie sekcje
(0–21).

**Dokument jest niezależny od dziedziny.** Nie zakłada, że projekt, w którym
działa, jest projektem informatycznym — sekcja 17 to rozpoznanie dziedziny (o
co dopytać, zanim zapytasz o liczby), sekcja 19 to kalibracja liczb (parametry
ustalane pytaniem do właściciela), a sekcja 21 to nasze wypełnienie —
konkretne wartości i odwzorowania obowiązujące w projekcie nAgents. Projekt
przejmujący ten dokument wymienia wyłącznie sekcję 21 na swoją; sekcje 0–20
zostają bez zmian.

---

## Pochodzenie

Wywodzi się z uniwersalnego szkieletu AutoBot (role Operator → Evaluator →
Final Control → integracja, dyscyplina ABC/ECHO, watchdog, kontrakt raportu),
napisanego pierwotnie dla innego projektu. Kopia źródłowa szkieletu leży
w `docs/process/zrodla/` i **nie jest modyfikowana**.

Do tego doszły dwie warstwy:

1. **Wiązania projektowe** — wartości, które szkielet zostawiał jako
   „ustal per projekt": limit rund, pula tematów, próg zwisu, domeny raportu,
   przydział modeli, format identyfikatorów, allowlisty, bariery.
2. **Praktyki wypracowane w trakcie pracy** — w tym reguły wyprowadzone
   z faktycznie popełnionych błędów, nie z teorii.

---

## Co dodaliśmy ponad szkielet

### Dyscyplina wykonania

| Element | Sekcja | Po co |
|---|---|---|
| Lista rzeczy, które **nie są** dowodem zakończenia | §1.2 | raport, gałąź, commit i deklaracja agenta to nie jest wynik |
| Zapis dispatchu jako plik, przed dispatchem | §4.2 | inaczej nie da się sprawdzić, czy cel nie przesunął się w trakcie |
| Checklisty ról — 10 punktów Evaluatora, 7 Final Control | §3.2, §3.3 | rola bez checklisty jest deklaracją, nie kontrolą |
| Zasada czystości z **twardym limitem 400 słów** | §9.1 | „zwróć destylat" bez liczby jest apelem, nie regułą |
| Wykrywanie dryfu celu i numerów scenariuszy | §3.2 pkt 9 | mechaniczny sygnał utraty kontekstu, sprawdzalny bez czytania transkryptu |

### Dekompozycja i pętla — z protokołu Graph Engineering

| Element | Sekcja |
|---|---|
| Brama „kiedy dzielić na węzły" z progami liczbowymi: 2 obszary / 3 scenariusze / 6 plików / 3000 tokenów — uniwersalna, progi są parametrem | §11.3.2 |
| Co wykonawca dostaje w zleceniu: zadanie, **reguła przeciw samooszukiwaniu**, binarne kryterium, procedura naprawcza — uniwersalne | §11.3.3 |
| Wskazanie **dokładnie jednego** wadliwego węzła zamiast zwrotu całego tematu | §3.2 pkt 10, §4.3 |
| Metryka wdrożenia: który węzeł był najsłabszy | §9.2 |
| Dobór szerokości fan-outu do faktycznego limitu współbieżności | §11.3.1 |

**Reguła przeciw samooszukiwaniu to najważniejszy pojedynczy dodatek** (przy
wykonawcy-programie nazywana też anty-halucynacyjną). Binarne kryterium
sprawdza, czy wynik jest kompletny. Reguła przeciw samooszukiwaniu zakazuje
konkretnego sposobu, w jaki wykonawca oszuka sam siebie. To dwie różne rzeczy
i większość procesów ma tylko pierwszą.

### Reguły z popełnionych błędów

| Reguła | Sekcja | Błąd, który ją wywołał |
|---|---|---|
| Hierarchia źródeł, pięć rzędów wiarygodności | §12.1 | dwa razy wpisano do dokumentu decyzyjnego nieprawdziwą cechę narzędzia, wziętą z podsumowania zamiast z dokumentacji |
| Zakaz raportowania czynności bez identyfikatora zadania albo SHA | §21.5 | „zleciłem uzupełnienie" — nie zlecono |
| Zakaz proponowania wariantu bez sprawdzenia dziennika decyzji | §21.5 | zadano pytanie o rzecz już rozstrzygniętą |
| Procedura korygowania własnego błędu | §12.3 | poprawka musi trafić tam, gdzie mieszka ustalenie, nie tylko do rozmowy |

### Komunikacja z właścicielem

| Element | Sekcja | Po co |
|---|---|---|
| Podział decyzji: właściciel kontra orkiestrator | §8.1 | pytanie techniczne postawione właścicielowi to przerzucenie decyzji, do której nie ma podstaw |
| Test zrozumiałości pytania — trzy warunki | §8.1 | zestaw 27 pytań przeszedł kontrolę formy, a adresat ich nie zrozumiał |
| Rejestr języka w tekstach dla właściciela | §14.2 | sprawdzian: usuń nazwy narzędzi; jeśli zdanie traci sens, było o mechanizmie |
| Szablon pytania ABC z wymogiem 2 argumentów za i 2 przeciw **każdego** wariantu | §8.2 | wariant z jednym argumentem to pozorny wybór |

### Pozostałe

Wzorzec promptu dla subagenta gotowy do skopiowania (§16), dyscyplina zakresu
i zakaz gonienia parytetu funkcji (§13), cztery pliki trwałej prawdy (§14),
konwencje repozytorium (§15), szybki start dla nowego agenta (§0).

---

## Co jest uniwersalne, a co trzeba przepisać

### Przenosi się bez zmian

- Struktura ról i pętli, w tym reguła jednego wadliwego węzła
- Lista rzeczy, które nie są dowodem zakończenia (§1.2)
- Zasada czystości i limit słów (§9.1)
- Co wykonawca dostaje w zleceniu i **idea** reguły przeciw samooszukiwaniu (§11.3.3)
- Hierarchia źródeł (§12.1)
- Podział decyzji, test zrozumiałości, rejestr języka (§8.1, §14.2)
- Dyscyplina zakresu (§13.1, §13.3)
- Wzorzec promptu (§16)

### Wymaga przepisania pod własny projekt

| Co | Gdzie | Uwaga |
|---|---|---|
| **Siedem twardych barier** | §7 | wynikają z modelu bezpieczeństwa nAgents; wypisz własne |
| **Konkretne reguły przeciw samooszukiwaniu** | §21.5 | nasze dotyczą oceny narzędzi i dokumentów; przy innym rodzaju pracy będą inne |
| Format identyfikatora tematu | §4.1 | prefiks i etapy |
| Allowlisty i ścieżki | §5.1 | struktura repozytorium |
| Kolejność czytania i punkt startowy | §2.1 | nazwy plików procesu |
| Limity: 3 rundy, pula 2, zwis 20 min | §4.5, §10 | wynikają z tego, że wszystko przegląda jedna osoba |
| Przydział modeli i zgoda na orkiestrację (stosuje się tylko, gdy wykonawcą jest program — §17.1) | §11.2, §11.3 | u nas zapisane jako ECHO-001 i ECHO-002, pełna treść w §21.5 |
| Węzły dla tematu kodującego | §11.3.3 | nasze są pod Pythona i nasz model uprawnień |

---

## Jak wdrożyć w innym projekcie

1. Skopiuj `SKILL.md` do `.claude/skills/<nazwa>/SKILL.md`, zmień `name` w nagłówku.
2. Przepisz sekcje z tabeli „wymaga przepisania". **Zacznij od §7 — bariery.**
   Bez własnych barier reszta procesu nie ma czego pilnować.
3. Utwórz punkt startowy (`CLAUDE.md`) z kolejnością czytania i rejestry procesu:
   `tematy.md`, `handoff.md`, `echo.md`, katalog `dispatch/`.
4. Utwórz `zmiana-procesu.md` — tryb bezpiecznej zmiany samego procesu.
   Bez niego pierwsza niewygodna reguła zostanie po cichu usunięta.
5. Wypisz **własne reguły przeciw samooszukiwaniu** (mechanizm w §11.3.3, nasze
   przykłady w §21.5). Nie kopiuj naszych — zbierz je z rzeczywistych błędów w
   swoim projekcie. Do tego czasu zostaw tabelę pustą z adnotacją „do
   uzupełnienia po pierwszych tematach".

**Nie zaczynaj od kopiowania limitów.** Liczby w §4.5 i §10 wynikają z pojemności
przeglądu jednej osoby. Przy zespole trzyosobowym będą inne.

---

## Rejestry, których skill wymaga

```
CLAUDE.md                        punkt startowy, kolejność czytania
docs/process/tematy.md           rejestr tematów: aktywne, zablokowane, zamknięte
docs/process/handoff.md          bieżący stan, zastępowany w całości
docs/process/echo.md             literalne decyzje właściciela
docs/process/dispatch/           zapis dispatchu per temat + SZABLON.md
docs/process/zmiana-procesu.md   tryb zmiany samego procesu
docs/spec/decisions.md           decyzje architektoniczne z uzasadnieniem
docs/spec/scenarios.md           scenariusze — źródło kryteriów końca
```

Bez nich skill odwołuje się do plików, których nie ma, i traci wykonalność.
