# Dispatch — NAG-DEC-001-wybory-otwarte

```text
TEMAT:   NAG-DEC-001-wybory-otwarte
DOMENA:  INFORMACYJNY
DATA:    2026-08-25
RUNDA:   1 z 3

WYZWALACZ
Stwierdzenie właściciela, 2026-08-25: „tego wiedzieć nigdy nie będziemy
i musimy sami znaleźć rozwiązania" — o wykazie podprocesorów appto.

To zamyka drogę odtwarzania cudzych wyborów i otwiera obowiązek rozstrzygnięcia
własnych. Sprawdzenie stanu rejestru pokazało, że stos techniczny jest już
rozstrzygnięty dziewięcioma decyzjami, ale **warstwa integracji nie ma ani
jednego wpisu w dzienniku decyzji**, mimo że specyfikacja zakłada sięganie
przez agentów po dane z systemów firmowych.

GOAL
Właściciel ma przed sobą komplet otwartych wyborów technicznych, każdy
w postaci pytania z wariantami, ceną i ryzykiem, sformułowanego językiem,
którym posługuje się prowadzący firmę, a nie programista.

CO JEST OTWARTE — ustalone przed dispatchem, do zweryfikowania przez wykonawcę
1. Warstwa integracji — brak decyzji w rejestrze. Luka największa.
2. D-011 rezydencja wspólnej pamięci — otwarta, blokuje MVP3.
3. D-010 topologia agentów — odroczona świadomie do danych z MVP1.
4. Luki z noty 07 — do przejrzenia, które wymagają decyzji, a które nie.

CZEGO NIE OTWIERAMY PONOWNIE
D-001 do D-009 są przyjęte. Wykonawca ich nie podważa i nie proponuje zmian,
chyba że znajdzie sprzeczność wewnętrzną — wtedy zgłasza, nie rozstrzyga.
Wyjątek: uzasadnienie D-001 czeka na poprawkę właściciela, temat osobny.

KRYTERIA KOŃCA
- Każdy otwarty wybór ma pytanie w formacie: dlaczego pytam, pytanie,
  warianty A/B/C z ceną i ryzykiem każdego, co będzie przy braku odpowiedzi,
  gdzie zapisujemy decyzję.
- Koszt podany jako krotność albo rząd wielkości w złotówkach tam, gdzie da się
  go oszacować, i jawnie oznaczony jako szacunek.
- Test zrozumiałości: osoba prowadząca firmę odpowiada literą bez dopytywania.
  Żadnego terminu technicznego bez wyjaśnienia w tym samym zdaniu.
- Przy warstwie integracji: co najmniej trzy warianty, a przy każdym odpowiedź
  na pytanie, co się stanie, gdy dostawca zniknie albo zmieni warunki.
  To jest ten sam rodzaj ryzyka, przez który odrzucono gotową platformę —
  ma być nazwany wprost przy każdym wariancie, także wtedy, gdy wariant
  jest zalecany.
- Rekomendacja przy każdym pytaniu, z uzasadnieniem. Rekomendacja nie jest
  decyzją i ma to być napisane.

ZAKRES
W zakresie:      docs/nota-08-wybory-otwarte.md — nowy plik
                 docs/process/pytania/2026-08-25-wybory.md — zestaw pytań
                 docs/process/tematy.md
Poza zakresem:   docs/spec/decisions.md — decyzje wpisuje właściciel po
                 odpowiedzi. Zmiana specyfikacji etapów. Pisanie kodu.

ALLOWLISTA
- docs/nota-08-wybory-otwarte.md
- docs/process/pytania/2026-08-25-wybory.md
- docs/process/tematy.md
- docs/process/dispatch/NAG-DEC-001-wybory-otwarte.md
Zakazane bezwzględnie: .env*, docs/spec/**, docs/process/zrodla/**, .git/**

IZOLACJA
Temat informacyjny. Gałąź `claude/git-connection-9sz6dg`.

PLAN TESTÓW
1. Test kompletności: przejść docs/spec/decisions.md i cztery etapy, sprawdzić,
   czy każdy wybór wymagający decyzji ma pytanie, i czy nie ma pytania
   o rzecz już rozstrzygniętą.
2. Test zrozumiałości z pozycji osoby nietechnicznej, pytanie po pytaniu.
3. Test uczciwości: czy przy każdym wariancie podano, co się traci, a nie
   tylko co się zyskuje.
4. Test granicy: czy dokument nigdzie nie rozstrzyga za właściciela.

ZALEŻNOŚCI
Zależy od:    nota 07 — katalog luk
Blokuje:      start kodowania; MVP3 przez D-011
Decyzje:      D-010, D-011 oraz nowa decyzja o warstwie integracji

BARIERY DOTKNIĘTE PRZEZ TEN TEMAT
Bariera 4 — domyślna odmowa. Wariant integracji dopuszczający szerokie
uprawnienia do systemów firmowych musi to ujawnić jako koszt, nie przemilczeć.
Bariera 2 — agent stanowiskowy bez własnych poświadczeń. Żaden wariant
integracji nie może tego naruszać; jeśli któryś naruszałby, ma to być
napisane wprost jako powód odrzucenia.
Bariera 7 — push wyłącznie na `claude/git-connection-9sz6dg`.
```
