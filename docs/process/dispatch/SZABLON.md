# Szablon zapisu dispatchu

Kopiuj do `docs/process/dispatch/<PEŁNE-ID>.md` **zanim** ruszy Operator.
Dispatch bez tego pliku jest naruszeniem procesu — bez niego nie da się później
sprawdzić, czy GOAL nie przesunął się w trakcie.

---

```text
TEMAT:   NAG-<ETAP>-<NNN>-<slug>
DOMENA:  PRODUKT | PROCES | INFRA | INFORMACYJNY
DATA:    <RRRR-MM-DD>
RUNDA:   1 z 3

WYZWALACZ
<Dlaczego ten temat startuje teraz i kto tak zdecydował. Jedno z:
 decyzja właściciela (podaj ID ECHO) | odblokowanie zależności (co się odblokowało) |
 powrót po FAIL (numer rundy) | przegląd okresowy | zdarzenie zewnętrzne.
 „Bo była kolej" nie jest wyzwalaczem.>

GOAL
<Jedno zdanie. Co ma być prawdą po zakończeniu, a nie jest teraz.>

KRYTERIA KOŃCA
- scenariusz <numer z docs/spec/scenarios.md> przechodzi, sprawdzony ręcznie
- scenariusz <numer> przechodzi
- pytest zielony w całości
- <inne mierzalne, jeśli są>

ZAKRES
W zakresie:      <co robimy>
Poza zakresem:   <co świadomie zostawiamy — im dokładniej, tym mniejszy rozjazd>

ALLOWLISTA
- <ścieżka/plik>
- <ścieżka/plik>
Zakazane bezwzględnie: .env*, docs/spec/decisions.md, .git/**

IZOLACJA
worktree: ../nagents-<ID>
branch:   auto/<ID>
baza:     <gałąź wskazana przez właściciela>

PLAN TESTÓW
1. pytest -q
2. pytest tests/<obszar> -v
3. scenariusze z kryteriów końca — ręcznie, na dev
4. jeśli temat dotyka uprawnień choćby pośrednio: A2 i A3 obowiązkowo

ZALEŻNOŚCI
Zależy od:    <ID tematów, które muszą być zamknięte>
Blokuje:      <ID tematów czekających na ten>
Decyzje:      <ID decyzji, które muszą być rozstrzygnięte, albo „brak">

BARIERY DOTKNIĘTE PRZEZ TEN TEMAT
<Wymień, które z siedmiu barier temat może naruszyć. Evaluator sprawdzi je
szczególnie. „Żadna" jest dopuszczalną odpowiedzią, ale przemyśl ją.>
```
