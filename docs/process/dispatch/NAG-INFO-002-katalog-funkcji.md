# Dispatch — NAG-INFO-002-katalog-funkcji

```text
TEMAT:   NAG-INFO-002-katalog-funkcji
DOMENA:  INFORMACYJNY
DATA:    2026-08-25
RUNDA:   1 z 3

WYZWALACZ
Doprecyzowanie właściciela, 2026-08-25: „naszym celem jest osiągnąć to samo
i te same funkcjonalności, ale jako własna platforma."

To zamyka pytanie, które NAG-INFO-001 miało otworzyć. Nie badamy już, czy kupić.
Badamy, co dokładnie trzeba odtworzyć i gdzie to wchodzi w nasze etapy.
Właściciel dostarczył cztery strony appto jako źródła pierwotne, bo bramka
sieciowa środowiska blokuje tę domenę.

GOAL
Mamy zamknięty katalog funkcji appto, każda przypisana do etapu MVP1–MVP4 albo
jawnie odrzucona, oraz listę tych, które wymagają decyzji właściciela, bo
zmieniają koszt, dane, dostęp albo odwracalność.

ŹRÓDŁA — wyłącznie te, nic spoza nich
docs/process/zrodla/appto-strona-glowna-pl.md
docs/process/zrodla/appto-cennik-pl.md
docs/process/zrodla/appto-wdrozenie-kohortowe-pl.md
docs/process/zrodla/appto-integracje-pl.md

Materiał sprzedażowy jest źródłem faktu o tym, CO PRODUCENT TWIERDZI. Funkcja
opisana na stronie może nie istnieć albo działać inaczej. Katalog opisuje więc
„co trzeba umieć", a nie „jak oni to zrobili" — tego drugiego nie wiemy.

KRYTERIA KOŃCA
- Każda funkcja z czterech źródeł znajduje się w katalogu albo jest jawnie
  odrzucona z uzasadnieniem. Pominięcie milczeniem jest błędem.
- Każda pozycja katalogu ma: nazwę, opis w jednym zdaniu, etap docelowy,
  trudność, zależność techniczną i status wobec obecnej specyfikacji
  (JEST W SPECYFIKACJI / BRAKUJE / ODRZUCAMY).
- Rozstrzygnięta osobno kwestia integracji: appto deklaruje 956 narzędzi,
  czego kilkuosobowy zespół nie zbuduje ręcznie. Musi paść jawna propozycja,
  jak do tego podejść, z wariantami i ceną każdego.
- Wskazane funkcje wymagające nowej decyzji właściciela — jako wnioski,
  nie jako rozstrzygnięcia. Dziennik decyzji zmienia wyłącznie właściciel.
- Nota 06 przepisana na źródłach pierwotnych zamiast na domysłach.

KOREKTA, KTÓRA MUSI PAŚĆ WPROST
Decyzja D-001 została podjęta z uzasadnieniem: „rozliczenie za tokeny u dostawcy
odbiera swobodę wyboru modelu". Cennik appto tego nie potwierdza — jednostką
jest kredyt, a wszystkie modele są dostępne bez dopłat, przełączane przez
użytkownika. Sama decyzja o budowie własnej platformy pozostaje w mocy z woli
właściciela, ale jej zapisane uzasadnienie jest częściowo nieprawdziwe.
Raport ma to powiedzieć wprost i zaproponować poprawione uzasadnienie
do zatwierdzenia przez właściciela.

ZAKRES
W zakresie:      docs/nota-06-appto-research.md — przepisanie w całości
                 docs/nota-07-katalog-funkcji.md — nowy plik
                 docs/process/tematy.md
Poza zakresem:   zmiana docs/spec/** — specyfikację i dziennik decyzji zmienia
                 właściciel. Raport formułuje wnioski, nie wprowadza ich.
                 Kontakt z appto. Zakładanie konta.

ALLOWLISTA
- docs/nota-06-appto-research.md
- docs/nota-07-katalog-funkcji.md
- docs/process/tematy.md
- docs/process/dispatch/NAG-INFO-002-katalog-funkcji.md
Zakazane bezwzględnie: .env*, docs/spec/**, docs/process/zrodla/**, .git/**

IZOLACJA
Temat informacyjny. Gałąź `claude/git-connection-9sz6dg`.

PLAN TESTÓW
1. Test pokrycia: przejść cztery źródła i sprawdzić, czy każda wymieniona
   funkcja ma pozycję w katalogu albo jawne odrzucenie.
2. Test przypisania: sprawdzić w docs/spec/01-mvp1.md do 04-mvp4.md, czy
   „JEST W SPECYFIKACJI" jest prawdą przy każdej takiej pozycji.
3. Test integracji: czy warianty podejścia do 956 narzędzi mają podaną cenę
   i ryzyko, a nie tylko nazwę.
4. Test granicy: czy raport nigdzie nie zmienia decyzji ani specyfikacji,
   tylko proponuje.

ZALEŻNOŚCI
Zależy od:    NAG-INFO-001 — źródła pierwotne, zebrane
Blokuje:      rewizję zakresu etapów, jeśli właściciel przyjmie wnioski
Decyzje:      D-001 — uzasadnienie do poprawienia przez właściciela

BARIERY DOTKNIĘTE PRZEZ TEN TEMAT
Bariera 6 — żadnych prawdziwych danych klientów w przykładach.
Bariera 7 — push wyłącznie na `claude/git-connection-9sz6dg`.
Zakaz cichej zmiany decyzji właściciela — raport proponuje, nie rozstrzyga.
```
