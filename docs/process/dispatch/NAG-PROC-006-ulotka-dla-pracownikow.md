# Dispatch — NAG-PROC-006-ulotka-dla-pracownikow

```text
TEMAT:   NAG-PROC-006-ulotka-dla-pracownikow
DOMENA:  INFORMACYJNY
DATA:    2026-08-23
RUNDA:   1 z 3

WYZWALACZ
Polecenie właściciela, 2026-08-23: „Przygotuj jeszcze potem krótki opis dla
pracowników, czym jest nasza zasada AutoBot, jakie problemy rozwiązuje, jaki ma
cel i jakie są korzyści z jej stosowania. Zróbmy z tego mały dokument
informacyjny, aby pracownicy mogli wdrożyć ją w swoich projektach."
Słowo „potem" jest wiążące: dokument powstaje po zamknięciu NAG-PROC-005,
bo opisuje proces w wersji niezależnej od dziedziny.

GOAL
Pracownik bez wykształcenia technicznego czyta dokument w kwadrans, rozumie,
po co ta zasada istnieje, i potrafi zastosować jej minimalną wersję w swoim
własnym projekcie następnego dnia, bez pytania kogokolwiek o pomoc.

ADRESAT — ustalenie wiążące dla całej treści
Około dwudziestu pracowników firmy z branży energetycznej. Jedna osoba
techniczna. Pozostali pracują w handlu, rozliczeniach, obsłudze klienta
i administracji. Nikt z nich nie pisze programów. Większość nie zleca pracy
sztucznej inteligencji, ale każdy komuś coś zleca albo od kogoś coś odbiera.

Z tego wynika kadrowanie całego dokumentu: AutoBot jest sposobem przekazywania
i odbierania pracy. To, że wykonawcą bywa program, jest szczegółem, nie tematem.
Dokument, który da się zastosować wyłącznie przy pracy ze sztuczną inteligencją,
jest chybiony i wraca do poprawy.

KRYTERIA KOŃCA
- Długość: do trzech stron. Czas czytania do piętnastu minut.
- Zero żargonu procesowego i technicznego. Słowa „operator", „ewaluator",
  „dispatch", „workflow", „agent", „iteracja" nie padają ani razu, chyba że
  wyjaśnione w tym samym zdaniu i konieczne.
- Problem opisany przed rozwiązaniem, na konkretnych sytuacjach z życia biura,
  nie w abstrakcji.
- Jest wersja minimalna do zastosowania od jutra — najwyżej cztery czynności.
- Jest napisane, ile to kosztuje czasu i kiedy NIE warto tego stosować.
  Dokument, który samych zalet, jest ulotką reklamową i nie zostanie przyjęty.
- Nie zawiera nazwisk, danych klientów ani przykładów z prawdziwych spraw.

ZAKRES
W zakresie:      docs/proces-dla-pracownikow.md — nowy plik
                 docs/process/tematy.md — wpis o temacie
Poza zakresem:   zmiana SKILL.md; zmiana docs/spec/**; szkolenie, regulamin,
                 zarządzenie wewnętrzne — to nie jest dokument kadrowy

ALLOWLISTA
- docs/proces-dla-pracownikow.md
- docs/process/tematy.md
- docs/process/dispatch/NAG-PROC-006-ulotka-dla-pracownikow.md
Zakazane bezwzględnie: .env*, docs/spec/**, .claude/**, .git/**

IZOLACJA
Dokument informacyjny. Praca na gałęzi `claude/git-connection-9sz6dg`.

PLAN TESTÓW
1. Test czytelnika: przejść tekst z pozycji osoby z rozliczeń, która nigdy nie
   słyszała o tej zasadzie. Każde zdanie wymagające dopytania jest błędem.
2. Test zastosowania: czy po lekturze da się wskazać cztery czynności do
   wykonania jutro, bez wracania do tekstu.
3. Test uczciwości: czy podano koszt i przypadki, w których nie warto.
4. Test długości: do trzech stron.

ZALEŻNOŚCI
Zależy od:    NAG-PROC-005-skill-uniwersalny — dokument opisuje proces
              w wersji niezależnej od dziedziny
Blokuje:      brak
Decyzje:      brak

BARIERY DOTKNIĘTE PRZEZ TEN TEMAT
Bariera 6 — żadnych prawdziwych danych osobowych ani danych klientów
w przykładach. Przykłady zmyślone, wyraźnie neutralne.
Bariera 7 — push wyłącznie na `claude/git-connection-9sz6dg`.
```
