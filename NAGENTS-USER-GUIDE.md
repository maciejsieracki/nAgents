# NAGENTS-USER-GUIDE.md — staging przewodnika pracownika

STATUS: `STAGING_ONLY`
DOMAIN: `INFORMACYJNY`
TEMAT: `NAG-CONSOLIDATE-USER-GUIDE-Q1`
P4_STATUS: `PASS_WITH_EXPLICIT_OWNER_GATES`
P6_STATUS: `PLAN_ONLY / OWNER_HOLD_REQUIRED`

Ten plik jest addytywnym materiałem do przeglądu. Nie jest nowym źródłem prawdy,
regulaminem, instrukcją administratora ani dowodem działania systemu. Nie nadaje
uprawnień i nie zastępuje `CLAUDE.md`, `docs/spec/` ani źródeł procesu. Usunięcie
tego katalogu stagingowego nie zmienia żadnego źródła kanonicznego.

## Dla pracownika

Ten przewodnik opisuje prostą drogę do pracy z przydzielonym profilem i czatem
oraz sposób przekazywania i odbierania zadań. Mówi, jaki skutek ma mieć ta
organizacja pracy. Nie wymaga wiedzy technicznej.

## USER-01 — Otwórz swój profil i czat

STATUS WEJŚCIA: `CONSOLIDATION_CANDIDATE` (`NAG-USER`) + `CANONICAL` (`NAG-ENTRY`)
ŹRÓDŁA: `docs/proces-dla-pracownikow.md`; `CLAUDE.md`; `NAGENTS-PROJECT.md`; decyzja `D-012`
LOCATOR P6: `NAGENTS-CONSOLIDATION-PLAN.md` §4.7, wiersz 164
LOKATOR W PAKIECIE: `NAGENTS-USER-GUIDE.md` §USER-01
SCENARIUSZE: `U1`, `U2`

Docelowa ścieżka pracownika jest krótka:

1. Otwierasz webową stronę NAgents/Hermesa.
2. Logujesz się firmowym kontem.
3. Widzisz gotowy profil i wyłącznie czaty przydzielone Tobie.
4. Otwierasz swój czat i pracujesz.

Nie konfigurujesz gatewaya, serwera, profilu technicznego, modelu, poświadczeń
ani routingu. Nie musisz znać adresu zaplecza ani sposobu, w jaki odpowiedź
powstaje. Jeśli nie widzisz potrzebnego profilu lub czatu, nie próbuj otwierać
cudzego — zgłoś to przełożonemu albo osobie odpowiedzialnej za dostęp.

Web jest podstawowym miejscem pracy. Desktop może być później wygodnym dodatkiem,
ale nie powinien być warunkiem rozpoczęcia ani kontynuowania pracy.

## USER-02 — Pięć sytuacji, które znasz

STATUS WEJŚCIA: `CONSOLIDATION_CANDIDATE` (`NAG-USER`)
ŹRÓDŁO: `docs/proces-dla-pracownikow.md`
LOCATOR P6: `NAGENTS-CONSOLIDATION-PLAN.md` §4.7, wiersz 165
LOKATOR W PAKIECIE: `NAGENTS-USER-GUIDE.md` §USER-02

To nie są wpadki pojedynczych osób. To sytuacje, w których wynik, ustalenie,
zakres albo powód decyzji nie zostały zabezpieczone.

### 1. Rozliczenia

Zestawienie wygląda na gotowe, ale jedna pozycja została policzona według starej
stawki. Dlatego liczy się sprawdzenie liczb i wyniku przez kogoś innego, a nie
sam dopisek „gotowe". Dzięki temu problem ma szansę zostać znaleziony, zanim
zestawienie pójdzie dalej.

### 2. Handel — oferta

Handlowiec przygotowuje ofertę, sam ją przegląda i wysyła. Błąd w warunkach
płatności może przejść niezauważony, bo własnych błędów nie widzi się tak łatwo
jak cudzych. Przed wysłaniem ofertę ogląda druga osoba.

### 3. Obsługa klienta

Na korytarzu pada szybkie ustalenie, że pewne zgłoszenia będziemy odtąd
załatwiać inaczej. Jeśli nikt go nie zapisze, po dwóch tygodniach trzy osoby
pamiętają trzy wersje. Ustalenie zapisujemy od razu w miejscu dostępnym dla
zainteresowanych.

### 4. Administracja

Zlecenie miało dotyczyć jednego dokumentu, ale po drodze dochodzą kolejne
uzupełnienia i poprawki. Bez zapisu początkowy zakres znika. Na starcie zapisujemy,
co ma powstać, po czym poznamy dobrą realizację i czego przy tym zadaniu nie wolno
zrobić. Każde rozszerzenie jest wtedy widoczne.

### 5. Handel — pamięć o kliencie

Osoba, która od lat prowadzi klienta w szczególny sposób, odchodzi z firmy.
W dokumentach zostaje sposób działania, ale nie zostaje powód. Następna osoba
albo powtarza go bez zrozumienia, albo wraca do tego samego problemu. Jeśli coś
robimy nietypowo, zapisujemy obok jednym zdaniem dlaczego.

## USER-03 — Odbiór pracy, ciągłość i zgłoszenie problemu

STATUS WEJŚCIA: `CANONICAL` (`NAG-SPEC`, scenariusze) + `CANONICAL` (`D-012`, `D-013`)
ŹRÓDŁA: `docs/spec/scenarios.md`; `CLAUDE.md`; `NAGENTS-PROJECT.md`; `docs/spec/decisions.md`
LOCATOR P6: `NAGENTS-CONSOLIDATION-PLAN.md` §4.7, wiersz 166
LOKATOR W PAKIECIE: `NAGENTS-USER-GUIDE.md` §USER-03
SCENARIUSZE: `U3`, `U7`, `U8`, `U9`, `U10`, `U11`

### Jak przekazywać i odbierać pracę

Przed rozpoczęciem zapisz dwa lub trzy zdania:

- co ma powstać;
- po czym poznasz, że jest zrobione dobrze;
- czego przy tym zadaniu nie wolno zrobić.

Przed przekazaniem wyniku poproś o spojrzenie kogoś innego niż wykonawca.
Nie musi to być przełożony — wystarczy osoba z zespołu, która nie przygotowywała
tego wyniku. Sprawdzamy to, co faktycznie powstało, a nie tylko informację
„gotowe".

Ustalenie zapisuj od razu, jednym zdaniem, w miejscu, gdzie zobaczą je osoby,
których dotyczy. Jeśli robisz coś nietypowo, dopisz obok, dlaczego. W ten sposób
wynik, zakres, ustalenie i powód pozostają w firmie także wtedy, gdy ktoś zmieni
stanowisko albo odejdzie.

### Gdy zamkniesz przeglądarkę

Przeglądarka pokazuje pracę, ale nie powinna być jej właścicielem. Zamykanie
przeglądarki odłącza widok; nie powinno kończyć poprawnie rozpoczętej pracy na
serwerze. Po ponownym wejściu powinno dać się odczytać ten sam stan. Jeśli tak
nie jest, zgłoś problem zamiast rozpoczynać drugą wersję tej samej pracy.

### Co dzieje się w tle

Założona organizacja pracy pozwala, żeby serwerowy pomocnik wykonywał tylko
wcześniej zatwierdzone przejścia. Nie rozszerza zadania, nie zmienia ustalonego
celu, nie rozstrzyga za właściciela i nie tworzy pustego sprawdzania.

Jeśli brakuje danych lub dowodu, wynik jest niejasny albo pojawia się konflikt,
sprawa zatrzymuje się i zostaje zgłoszona. Zatrzymana zostaje tylko ta sprawa,
a nie cała praca innych osób. Ponowienie tej samej dyspozycji po awarii nie
powinno tworzyć drugiej kopii pracy, drugiego następcy ani drugiego dostarczenia.

### Gdy coś jest nie tak

Nie obchodź ograniczenia i nie zgaduj. Zapisz krótko, co robiłeś, kiedy to było
oraz jaki wynik lub komunikat zobaczyłeś. Następnie zgłoś sprawę przełożonemu albo
administratorowi. Brak danych, brak potwierdzenia lub niejasna odpowiedź są
powodem do zatrzymania i wyjaśnienia, nie do dopisywania brakującej informacji
z pamięci.

### Wersja minimalna — do zastosowania od jutra

1. Przed startem zapisz zakres, oznakę dobrego wyniku i rzecz zakazaną.
2. Przed przekazaniem poproś inną osobę o sprawdzenie wyniku.
3. Zapisz ustalenie od razu w jednym wspólnym miejscu.
4. Przy nietypowym działaniu dopisz obok jedno zdanie z powodem.

Typowe zadanie biurowe wymaga na te czynności około 20–30 dodatkowych minut,
w tym 10–15 minut drugiej osoby. To ma sens, gdy błąd kosztowałby więcej niż
pół godziny naprawy albo gdy ktoś będzie tę pracę kontynuował, powtarzał lub się
do niej odwoływał.

Nie stosuj pełnego zestawu przy zadaniu na trzy minuty, przy czymś jednorazowym,
czego nikt po Tobie nie przejmie, ani w sytuacji awaryjnej, w której liczy się
czas reakcji, a nie dokumentacja. Nakładanie całej procedury na drobiazgi osłabia
nawyk tam, gdzie naprawdę jest potrzebny.

## USER-04 — Granica między pracownikiem a administratorem

STATUS WEJŚCIA: `CONSOLIDATION_CANDIDATE` (`NAG-RBAC`) + `CANONICAL` (bieżąca architektura/MVP2)
ŹRÓDŁA: `ROLE-I-UPRAWNIENIA-nAgents.md` jako metadana P4; `docs/spec/00-architektura.md`; `docs/spec/02-mvp2.md`; `CLAUDE.md`
LOCATOR P6: `NAGENTS-CONSOLIDATION-PLAN.md` §4.7, wiersz 167
LOKATOR W PAKIECIE: `NAGENTS-USER-GUIDE.md` §USER-04
SCENARIUSZ: `U4`

Poniższy podział objaśnia ścieżkę odbiorcy. Nie jest tabelą nadającą role ani
dostęp. Ostateczny dostęp wynika z aktualnych zasad i nadań firmy, a nie z tego
przewodnika.

| Rola | Co robi |
|---|---|
| Pracownik | Korzysta wyłącznie z przydzielonego profilu i czatu, wysyła wiadomości, odbiera wyniki i zgłasza niejasności. Nie zarządza dostępem, limitami, poświadczeniami ani ustawieniami zaawansowanymi. |
| Administrator lub inna osoba upoważniona | Przydziela i odbiera dostęp, przygotowuje profile, zarządza ustawieniami, limitami i zgodami oraz rozwiązuje problemy z dostępem. Robi to w przeznaczonej do tego powierzchni administracyjnej albo innym zatwierdzonym miejscu. |
| Serwerowa część systemu | Utrzymuje sesję, kolejkę, pamięć i ślad działania. Przeglądarka i Desktop są sposobami dostępu do pracy, nie jej właścicielami. |

Widzisz tylko to, do czego zostałeś przydzielony. Nie próbuj omijać tego podziału
ani szukać profilu innej osoby. Jeśli uważasz, że powinieneś mieć dostęp, poproś
o jego sprawdzenie administratora. Jeśli dostęp został odebrany, nie zakładaj
nowego profilu samodzielnie.

Czynności mogące wysłać korespondencję, zmienić rozliczenie albo zmienić dane
w innym systemie wymagają zatwierdzenia upoważnionej osoby. Sama rozmowa nie
oznacza, że taka zmiana została wykonana.

Nie otrzymujesz technicznych haseł ani kluczy. Zaawansowane ustawienia, budżety,
profile, nadania, zgody i ślad działania należą do administratora. Ten przewodnik
nie przenosi tych obowiązków na pracownika.

## Krótka kontrola przed użyciem

- Czy logujesz się firmowym kontem?
- Czy widzisz tylko przydzielony profil i czat?
- Czy zakres zadania, dobry wynik i rzecz zakazana są zapisane?
- Czy wynik obejrzała inna osoba, zanim poszedł dalej?
- Czy ustalenie i powód nietypowego działania są zapisane?
- Czy przy niejasności zatrzymałeś sprawę i zgłosiłeś ją zamiast zgadywać?

## Ślad redakcyjny — nie jest częścią instrukcji dla pracownika

Poniższe wpisy służą do niezależnego przeglądu proweniencji. Status `STAGING_ONLY`
nie oznacza publikacji ani potwierdzenia działania w środowisku rzeczywistym.

| Sekcja | Status źródła | Źródła i locatory | Kryterium pokrycia |
|---|---|---|---|
| `USER-01` | `CONSOLIDATION_CANDIDATE` + `CANONICAL` | `docs/proces-dla-pracownikow.md` §Czym jest ta zasada; `CLAUDE.md` §Główny kierunek produktu: web-first i §Podział ról użytkowników; `docs/spec/decisions.md` §D-012 | Pracownik ma opisaną ścieżkę firmowe logowanie → gotowy profil → przydzielony czat; przewodnik nie zleca mu konfiguracji zaplecza. |
| `USER-02` | `CONSOLIDATION_CANDIDATE` | `docs/proces-dla-pracownikow.md` §Pięć sytuacji, które znasz; §Co to daje; §Wersja minimalna | Wszystkie pięć sytuacji i cztery minimalne czynności są zachowane w języku skutków; opis nie ustanawia technicznej normy. |
| `USER-03` | `CANONICAL` + `CANONICAL_DECISION` | `docs/spec/scenarios.md` U1–U4, U7–U11; `docs/spec/decisions.md` §D-012 i §D-013; `CLAUDE.md` §Pomocnik procesu podczas nieobecności właściciela | Zachowane są ciągłość po zamknięciu klienta, ponowne odczytanie stanu, zatrzymanie niejasności, brak nowego zakresu/pustej obrony i brak duplikatu po replayu. |
| `USER-04` | `CONSOLIDATION_CANDIDATE` + `CANONICAL` | P4 `PF-0191` (`NAG-RBAC`) jako metadana; `docs/spec/00-architektura.md` §6.1–§6.5; `docs/spec/02-mvp2.md` §1, §5, §7; `CLAUDE.md` §Podział ról użytkowników | Opis rozdziela użycie przydzielonego czatu od administracji; nie nadaje ról, nie kopiuje starej macierzy i wskazuje bieżące źródła. |

### Granice pakietu

- Przewodnik pozostaje informacyjny i przeznaczony dla pracownika.
- Pełne bariery bezpieczeństwa, statusy techniczne i readbacki pozostają w
  kanonicznej normie procesu i specyfikacji; nie są tu ustanawiane na nowo.
- P4 `NAG-RBAC` został użyty jako bezpieczna metadana proweniencji. Zewnętrznego
  pliku `ROLE-I-UPRAWNIENIA-nAgents.md` nie odczytywano ani nie kopiowano.
- Decyzje `D-010` i `D-011`, publikacja, integracja, usunięcia, merge, push i
  deploy pozostają poza tym pakietem.

## Źródła użyte do redakcji

- [`docs/proces-dla-pracownikow.md`](../../../proces-dla-pracownikow.md) — kandydat `NAG-USER`.
- [`docs/spec/scenarios.md`](../../../spec/scenarios.md) — scenariusze odbioru, w tym `U1`–`U11`.
- [`CLAUDE.md`](../../../../CLAUDE.md) — punkt startowy, web-first i granice ról.
- [`NAGENTS-PROJECT.md`](../../../../NAGENTS-PROJECT.md) — indeks i rozdział pracownik–administrator.
- [`NAGENTS-CONSOLIDATION-PLAN.md`](../../../../NAGENTS-CONSOLIDATION-PLAN.md) — macierz P6 `USER-01`–`USER-04`.
- `docs/spec/decisions.md`, `docs/spec/00-architektura.md` i `docs/spec/02-mvp2.md` — bieżące źródła wspierające granice opisane w P6.
- `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json` — indeks proweniencji i metadana `NAG-RBAC`; nie jest źródłem instrukcji dla pracownika.

DEPLOY/PUSH: `NIE WYKONANO`
