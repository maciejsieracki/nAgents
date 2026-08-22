# Dziennik decyzji

Każdy wybór dotyczący **kosztu, danych, dostępu, wdrożenia lub odwracalności** trafia tutaj
z opcjami i uzasadnieniem — zanim powstanie kod, który go realizuje.

Format: numer, data, stan, decyzja, rozważane opcje, uzasadnienie, konsekwencje.

---

## D-001 · Budujemy, nie kupujemy
**2026-08-22 · przyjęta**

**Decyzja:** budujemy własną warstwę zarządzania na Hermesie. Gotowa platforma
(appto) pozostaje **wzorcem projektowym**, nie zakupem.

**Opcje:** (a) zakup gotowej platformy, (b) budowa własna, (c) hybryda.

**Uzasadnienie:** rozliczenie za tokeny u dostawcy odbiera swobodę wyboru modelu,
co jest wymaganiem numer trzy. Wymóg podłączania dowolnych modeli przeważył nad
oszczędnością czasu.

**Konsekwencje:** bierzemy na siebie utrzymanie i ryzyko jednej osoby technicznej.
Nie kupujemy wsparcia wdrożeniowego, które byłoby odpowiedzią na to ryzyko.
Wzorce funkcjonalne kopiujemy świadomie — bez gonienia parytetu.

---

## D-002 · Brama modeli jest nasza
**2026-08-22 · przyjęta**

**Decyzja:** wszystkie wywołania modeli przechodzą przez LiteLLM, z naszymi kluczami.
Żaden inny komponent nie zna poświadczeń dostawców.

**Opcje:** (a) klucze w konfiguracji każdego profilu, (b) wspólna brama, (c) rozliczenie u dostawcy platformy.

**Uzasadnienie:** budżet egzekwowalny w jednym punkcie, koszt przypisywalny do agenta
i człowieka, zmiana dostawcy bez dotykania agentów.

**Konsekwencje:** brama jest pojedynczym punktem awarii — wymaga monitorowania
i wariantu awaryjnego od MVP2.

---

## D-003 · Agenci stanowiskowi bez własnych kluczy
**2026-08-22 · przyjęta**

**Decyzja:** agent rodzaju `stanowiskowy` nie posiada żadnych poświadczeń do systemów
firmowych. Dane pobiera przez agenta projektowego swojej domeny.
Walidator rejestru odrzuca konfigurację naruszającą tę regułę.

**Uzasadnienie:** dwadzieścia profili z kluczami to dwadzieścia miejsc wycieku.
Jeden agent domenowy jest pilnowany i audytowany.

**Konsekwencje:** każde pytanie wymagające danych przechodzi przez dodatkowy skok.
Akceptujemy koszt opóźnienia w zamian za zawężenie powierzchni ataku.

---

## D-004 · Brak dostępu zwraca 404, nie 403
**2026-08-22 · przyjęta**

**Decyzja:** żądanie do agenta bez uprawnień zwraca „nie znaleziono".

**Uzasadnienie:** komunikat o braku dostępu ujawnia istnienie zasobu. Marketing nie ma
dowiadywać się, że istnieje agent kadrowy.

**Konsekwencje:** diagnostyka trudniejsza — dlatego prawdziwy powód zawsze trafia
do audytu, nawet gdy użytkownik go nie widzi.

---

## D-005 · Baza wiedzy w repozytorium, nie w bazie danych
**2026-08-22 · przyjęta**

**Decyzja:** wiedza firmowa i zespołowa jako pliki tekstowe w gicie.

**Opcje:** (a) tabele w bazie z własnym wersjonowaniem, (b) repozytorium git, (c) zewnętrzny system dokumentów.

**Uzasadnienie:** historia, autor, data i cofnięcie powstają same. Przegląd zmiany przed
wdrożeniem jest naturalny. Skoro jedna zmiana wpływa na wszystkich agentów, cofnięcie
musi być operacją standardową, nie funkcją do napisania.

**Konsekwencje:** edycja wiedzy wymaga narzędzia obsługującego repozytorium.
Panel z MVP2 zapisuje zmiany jako commity, a nie omija wersjonowania.

---

## D-006 · Pamięć prywatna odcinana od agentów procesowych
**2026-08-22 · przyjęta**

**Decyzja:** agent wykonujący proces krytyczny nie czyta poziomu kontekstu prywatnego.

**Uzasadnienie:** rozliczenie ma przebiegać identycznie niezależnie od tego, kto pyta.
Prywatne preferencje są w tym miejscu zakłóceniem. Rozwiązuje też problem dziedziczenia
przy przejęciu stanowiska.

**Konsekwencje:** agent procesowy sprawia wrażenie mniej „dopasowanego".
To jest cecha, nie usterka.

---

## D-007 · Wielonajemność przygotowana od pierwszego dnia
**2026-08-22 · przyjęta**

**Decyzja:** `tenant_id` w każdej tabeli od MVP1, mimo że najemca jest jeden.
Nic specyficznego dla NASTER w kodzie.

**Uzasadnienie:** dołożenie izolacji do działającego systemu jest przepisaniem,
a nie rozszerzeniem. Koszt teraz: kilka godzin.

**Konsekwencje:** nieco więcej ceremonii w zapytaniach od początku.

---

## D-008 · Python i szablony serwerowe zamiast osobnego frontendu
**2026-08-22 · przyjęta**

**Decyzja:** FastAPI + Jinja2 + HTMX. Bez SPA.

**Uzasadnienie:** pięć ekranów w MVP1. Osobny frontend to drugi proces budowania,
drugi zestaw zależności i drugi obszar do utrzymania przez jedną osobę.
Python zgadza się z bramą modeli.

**Konsekwencje:** przy bardzo rozbudowanym panelu w MVP4 może okazać się ciasno.
Wtedy wydzielamy sam panel, nie całość.

---

## D-009 · Tokenizacja nie zwalnia z umowy powierzenia
**2026-08-22 · przyjęta**

**Decyzja:** wdrażamy tokenizację danych wrażliwych (MVP4), ale **nie traktujemy jej
jako podstawy prawnej** do przetwarzania bez umowy powierzenia.

**Uzasadnienie:** to jest pseudonimizacja, nie anonimizacja. Dane pseudonimizowane
pozostają danymi osobowymi. Anonimizacja przy rozliczeniach jest niemożliwa, bo wynik
musi wrócić do konkretnej umowy.

**Konsekwencje:** umowa powierzenia z dostawcą modelu jest wymagana niezależnie
od tokenizacji. Tokenizacja ogranicza szkodę przy wycieku, nie obowiązki.

---

## D-010 · Topologia agentów — decyzja odroczona
**2026-08-22 · otwarta**

**Pytanie:** 27 osobnych profili czy sześć agentów projektowych plus poziomy kontekstu
z pamięcią prywatną?

**Stan:** trzy niezależne źródła (Rauch, Negacz, konstrukcja Eve) układają to jako
poziomy kontekstu. Topologia 27 profili jest po części obejściem otwartego błędu
wspólnej pamięci w Hermesie.

**Decyzja:** rejestr projektujemy tak, żeby obsłużył **oba warianty** — pole `rodzaj`
i `parent_agent_id` istnieją od MVP1. Wybór zapada po MVP1, na danych z realnego użycia.

**Termin:** przed rozpoczęciem MVP3.

---

## D-011 · Rezydencja wspólnej pamięci — blokada
**2026-08-22 · otwarta, blokująca**

**Pytanie:** gdzie fizycznie przechowywana jest pamięć współdzielona między agentami?

**Stan:** nierozstrzygnięte. Oczekiwanie na informacje o bazie po stronie firmy.

**Skutek:** MVP1 i MVP2 działają bez pamięci współdzielonej i **nie są zablokowane**.
MVP3 nie startuje bez odpowiedzi.

**Warianty:** (a) własny serwer, (b) dostawca zewnętrzny z umową powierzenia,
(c) rezygnacja ze wspólnej pamięci na rzecz wyłącznie wiedzy w repozytorium.

**Wariant (c) jest realny** i wart rozważenia — usuwa ryzyko i część złożoności,
kosztem tego, że agenci nie uczą się z rozmów.
