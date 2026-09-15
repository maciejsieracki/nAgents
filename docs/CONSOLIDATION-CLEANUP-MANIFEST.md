# 8gent — manifest sprzątania dokumentacji

Status: audyt wykonany; lista usunięć: **pusta**.

## Zakres

Sprzątanie oceniono na podstawie:

- P4 dla 833 rekordów i 254 rodzin ścieżek;
- macierzy `NAGENTS-CONSOLIDATION-PLAN.md`;
- raportów Operatora, Evaluatora, Defense, Final Control i P7;
- aktualnego katalogu checkoutu 8gent;
- rozdzielenia źródeł kanonicznych, historii, evidence i projektów towarzyszących.

## Dlaczego niczego nie usunięto

P4 oznaczał 561 nadmiarowych rekordów w 105 rodzinach identycznego SHA, ale
rekord snapshotu nie jest automatycznie plikiem do usunięcia. Kopie pochodzą z
różnych checkoutów, worktree, refów i handoffów. Nie ma bezpiecznej operacji
„usuń wszystkie duplikaty”.

Każdy kandydat oceniany jako konsolidacja nadal ma jedną z tych funkcji:

- źródło kanoniczne (`CLAUDE.md`, `docs/spec/`, `docs/process/`, skill);
- źródło pierwotne (`docs/process/zrodla/`);
- historia uzasadnienia lub korekt (`docs/nota-*`, stare handoffy);
- evidence fazy, runu albo audytu (`docs/process/dispatch/`, `audit/`, staging);
- materiał wymagający decyzji właściciela (`NAG-INTEGRATIONS`);
- osobny projekt lub zewnętrzne repozytorium (AutoBot Monitor, The-Game).

Nowe księgi są zweryfikowanymi kopiami stagingu, ale nie zastępują jeszcze
źródeł pierwszeństwa. Dlatego usunięcie źródła po samym podobieństwie lub
nazwie mogłoby zniszczyć proweniencję, historię albo aktualną normę.

## Allowlista usunięć

```text
DELETE_ALLOWLIST: []
```

Nie wykonano `rm`, `git clean`, przenoszenia ani usuwania z repozytorium.
Nie zmieniono też dokumentów w `/home/ubuntu/handoffs/`, repozytorium AutoBot
Monitor ani projektu The-Game.

## Co zostało uporządkowane zamiast kasowania

- utworzono dziewięć jawnych ksiąg tematycznych;
- dodano `NAGENTS-PROJECT.md` jako wejście dla nowego agenta;
- dodano ten manifest i manifest paczki publikacyjnej;
- zachowano staging oraz raporty jako audytowalny ślad;
- wyraźnie oznaczono integracje jako `OWNER_HOLD` po decyzji A.

## Warunek przyszłego usunięcia

Plik może trafić do przyszłej allowlisty usunięć dopiero, gdy dla konkretnej
ścieżki istnieje jednocześnie:

1. exact mapping `stary plik → nowa księga → sekcja → zachowana treść`;
2. niezależny dowód, że nie pozostała żadna unikalna treść;
3. brak funkcji źródła kanonicznego, pierwotnego, historycznego lub evidence;
4. sprawdzenie wszystkich odnośników i aktywnych tematów;
5. jawna zgoda właściciela na tę konkretną ścieżkę;
6. readback po usunięciu i kontrolowany commit.

Na obecnym etapie żaden plik nie spełnia wszystkich warunków.
