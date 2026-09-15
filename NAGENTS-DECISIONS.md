# NAGENTS-DECISIONS.md — pakiet stagingowy

STATUS: `STAGING_ONLY`
DOMAIN: `INFORMACYJNY`
TEMAT: `NAG-CONSOLIDATE-DECISIONS-Q1`
P4_STATUS: `PASS_WITH_EXPLICIT_OWNER_GATES`
GENERATED_AT_UTC: `2026-09-14T17:22:58Z`

Ten plik jest addytywną ekstrakcją do niezależnego przeglądu. Nie jest nowym
źródłem prawdy, nie zastępuje `docs/spec/decisions.md`, `docs/process/echo.md`,
`docs/process/pytania/2026-08-25-wybory.md`, `NAGENTS-CONSOLIDATION-PLAN.md`,
`NAGENTS-PROJECT.md` ani maszynowego `P4-classification.json`.

Usunięcie całego katalogu stagingowego odtwarza stan źródeł. W tej fazie nie
zmieniono, nie przeniesiono i nie usunięto żadnego źródła; nie wykonano
`git add`, commit, push, merge, deployu ani restartu. Pakiet nie nadaje zgody
na publikację ani nie rozstrzyga otwartych pytań właścicielskich.

Hierarchia treści w tym pakiecie:

1. `DEC-01`: literalne decyzje z aktualnego dziennika `docs/spec/decisions.md`.
2. `DEC-02`: literalne wpisy właścicielskie z `docs/process/echo.md`.
3. `DEC-03`: pytania i ich statusy z aktualnego pakietu pytań; rekomendacja nie jest decyzją.
4. `DEC-04`: historia i korekty z noty `HISTORY`; nie sterują normą.
5. P6, P4 i indeks projektu dostarczają macierzy, statusu i proweniencji, nie nowych wyborów.

Każdy blok treści ma `SOURCE_PATH`, SHA-256, status źródła i locator. Bloki
oznaczone `BEGIN_LITERAL_SOURCE` są kopiami zakresów bajtów źródła, bez
parafrazowania. Szczegółowy ledger maszynowy znajduje się w `coverage.json`.

## 0A. Aktualna decyzja platformowa

Bieżąca decyzja właściciela jest zapisana jako **D-014** w
`docs/spec/decisions.md` i rozwinięta w `docs/OPENCLAW-STRATEGY.md`:

- runtime i control plane 8gent: **OpenClaw**;
- osobny OpenRouter: **nie budujemy**;
- osobny OpenMonitor: **nie budujemy**;
- AutoBot Monitor: **kandydat na opcjonalny plugin OpenClaw**;
- Hermes/LiteLLM: wcześniejszy wariant, zachowany w literalnych blokach jako
  `HISTORY/EVIDENCE`, nie jako bieżąca architektura.

`DEC-01` poniżej pozostaje snapshotem zakresu D-001…D-013 z czasu konsolidacji.
Nie wolno używać jego starych twierdzeń o Hermesie lub LiteLLM ponad D-014.

## 0. Zakres, statusy i bramy

P6 wyznacza cztery sekcje tego pakietu (P6 §4.2, linie 110–117):

| Sekcja | Zakres | Ranga wejścia | Bramka |
|---|---|---|---|
| `DEC-01` | D-001…D-013: opcje, wybór/stan, uzasadnienie, konsekwencje | `CANONICAL` | 13/13 ID literalnie zgodne; nic z noty nie staje się wyborem |
| `DEC-02` | literalne ECHO i daty | `CANONICAL` | odpowiedź, autor, data i locator dla każdego wpisu; rekomendacje wykluczone |
| `DEC-03` | pytania otwarte i kolejność | `CANONICAL` | statusy zrekoncyliowane; D-010/D-011 i bramy pozostają jawne |
| `DEC-04` | warianty, korekty i historia | `HISTORY` | historia nie zmienia bieżącego statusu; hash i skutek korekty pozostają jawne |

P6 `OWNER-01`–`OWNER-05` nadal ograniczają wybór zakresu, integracje, usuwanie,
przenoszenie i publikację. Ten pakiet korzysta wyłącznie z odwracalnego stagingu.

## 1. `DEC-01` — rejestr D-001…D-013

SOURCE_PATH: `docs/spec/decisions.md`
SOURCE_SHA256: `57264a14bcf38b38811d42b025aba31359470db6c4e33dbc7abe9cad03681d49`
SOURCE_STATUS: `CANONICAL` (P4 `NAG-SPEC`, rodzina `PF-0241`)
SOURCE_OF_TRUTH: `true` dla literalnego dziennika decyzji
SOURCE_LOCATOR: nagłówki `D-001`…`D-013`, linie źródła zapisane w `coverage.json`
SOURCE_LINK: [`docs/spec/decisions.md`](../../../spec/decisions.md)

Statusy w poniższej tabeli są indeksem literalnych linii statusu. `D-010` i
`D-011` nie są przyjętymi wyborami; nie wolno ich traktować jako zgody.

| ID | Data | Status literalny | Rekoncylacja | Locator |
|---|---|---|---|---|
| `D-001` | `2026-08-22` | `przyjęta` | przyjęta; obowiązuje zgodnie ze źródłem | `docs/spec/decisions.md:10-27` |
| `D-002` | `2026-08-22` | `przyjęta` | przyjęta; obowiązuje zgodnie ze źródłem | `docs/spec/decisions.md:28-43` |
| `D-003` | `2026-08-22` | `przyjęta` | przyjęta; obowiązuje zgodnie ze źródłem | `docs/spec/decisions.md:44-58` |
| `D-004` | `2026-08-22` | `przyjęta` | przyjęta; obowiązuje zgodnie ze źródłem | `docs/spec/decisions.md:59-71` |
| `D-005` | `2026-08-22` | `przyjęta` | przyjęta; obowiązuje zgodnie ze źródłem | `docs/spec/decisions.md:72-87` |
| `D-006` | `2026-08-22` | `przyjęta` | przyjęta; obowiązuje zgodnie ze źródłem | `docs/spec/decisions.md:88-101` |
| `D-007` | `2026-08-22` | `przyjęta` | przyjęta; obowiązuje zgodnie ze źródłem | `docs/spec/decisions.md:102-114` |
| `D-008` | `2026-08-22` | `przyjęta` | przyjęta; obowiązuje zgodnie ze źródłem | `docs/spec/decisions.md:115-128` |
| `D-009` | `2026-08-22` | `przyjęta` | przyjęta; obowiązuje zgodnie ze źródłem | `docs/spec/decisions.md:129-143` |
| `D-010` | `2026-08-22` | `otwarta` | otwarta; odroczona do danych z MVP1 / przed MVP3 | `docs/spec/decisions.md:144-160` |
| `D-011` | `2026-08-22` | `otwarta, blokująca` | otwarta, blokująca MVP3; MVP1/MVP2 nie są zablokowane | `docs/spec/decisions.md:161-178` |
| `D-012` | `2026-09-13` | `przyjęta dyrektywą właściciela` | przyjęta; obowiązuje zgodnie ze źródłem | `docs/spec/decisions.md:179-220` |
| `D-013` | `2026-09-13` | `przyjęta wyborem właściciela wariantu A` | przyjęta; obowiązuje zgodnie ze źródłem | `docs/spec/decisions.md:221-264` |

Każdy poniższy zakres jest literalną kopią z `SRC-01`. Tabela powyżej
nie zastępuje tych zapisów.

### Literalny zakres `D-001` — zapis źródłowy

SOURCE_REF: `SRC-01`
SOURCE_PATH: `docs/spec/decisions.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/spec/decisions.md:10-27` (## D-001 · Budujemy, nie kupujemy)
LITERAL_SHA256: `b41d6c9a8c7ceb851d1341c914adfbc9cf3568a211edaa58e377aa86f2adc0b5`
BEGIN_LITERAL_SOURCE:D-001
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
END_LITERAL_SOURCE:D-001

### Literalny zakres `D-002` — zapis źródłowy

SOURCE_REF: `SRC-01`
SOURCE_PATH: `docs/spec/decisions.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/spec/decisions.md:28-43` (## D-002 · Brama modeli jest nasza)
LITERAL_SHA256: `eaabbf94b5ef8d840a1456a0134904fa004b0858977bd1ea0b4bc4844c253c1d`
BEGIN_LITERAL_SOURCE:D-002
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
END_LITERAL_SOURCE:D-002

### Literalny zakres `D-003` — zapis źródłowy

SOURCE_REF: `SRC-01`
SOURCE_PATH: `docs/spec/decisions.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/spec/decisions.md:44-58` (## D-003 · Agenci stanowiskowi bez własnych kluczy)
LITERAL_SHA256: `7d16c896eae274dd32f5c7066b943c7fc0fc62a20939503ec5989e620c72c77c`
BEGIN_LITERAL_SOURCE:D-003
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
END_LITERAL_SOURCE:D-003

### Literalny zakres `D-004` — zapis źródłowy

SOURCE_REF: `SRC-01`
SOURCE_PATH: `docs/spec/decisions.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/spec/decisions.md:59-71` (## D-004 · Brak dostępu zwraca 404, nie 403)
LITERAL_SHA256: `77a01bac0f739aee9dba17f46172848d8b4161b9dbfed074319f72b43b0b3aaa`
BEGIN_LITERAL_SOURCE:D-004
## D-004 · Brak dostępu zwraca 404, nie 403
**2026-08-22 · przyjęta**

**Decyzja:** żądanie do agenta bez uprawnień zwraca „nie znaleziono".

**Uzasadnienie:** komunikat o braku dostępu ujawnia istnienie zasobu. Marketing nie ma
dowiadywać się, że istnieje agent kadrowy.

**Konsekwencje:** diagnostyka trudniejsza — dlatego prawdziwy powód zawsze trafia
do audytu, nawet gdy użytkownik go nie widzi.

---
END_LITERAL_SOURCE:D-004

### Literalny zakres `D-005` — zapis źródłowy

SOURCE_REF: `SRC-01`
SOURCE_PATH: `docs/spec/decisions.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/spec/decisions.md:72-87` (## D-005 · Baza wiedzy w repozytorium, nie w bazie danych)
LITERAL_SHA256: `d0c265213cfb4364bd0ee0fde64880d12a034641a1fba8b36de12418f8e640b9`
BEGIN_LITERAL_SOURCE:D-005
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
END_LITERAL_SOURCE:D-005

### Literalny zakres `D-006` — zapis źródłowy

SOURCE_REF: `SRC-01`
SOURCE_PATH: `docs/spec/decisions.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/spec/decisions.md:88-101` (## D-006 · Pamięć prywatna odcinana od agentów procesowych)
LITERAL_SHA256: `c25d43a885e1f1050c626be747965c2388740c7b6a90a6ec6a9b952a394e5488`
BEGIN_LITERAL_SOURCE:D-006
## D-006 · Pamięć prywatna odcinana od agentów procesowych
**2026-08-22 · przyjęta**

**Decyzja:** agent wykonujący proces krytyczny nie czyta poziomu kontekstu prywatnego.

**Uzasadnienie:** rozliczenie ma przebiegać identycznie niezależnie od tego, kto pyta.
Prywatne preferencje są w tym miejscu zakłóceniem. Rozwiązuje też problem dziedziczenia
przy przejęciu stanowiska.

**Konsekwencje:** agent procesowy sprawia wrażenie mniej „dopasowanego".
To jest cecha, nie usterka.

---
END_LITERAL_SOURCE:D-006

### Literalny zakres `D-007` — zapis źródłowy

SOURCE_REF: `SRC-01`
SOURCE_PATH: `docs/spec/decisions.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/spec/decisions.md:102-114` (## D-007 · Wielonajemność przygotowana od pierwszego dnia)
LITERAL_SHA256: `8b3e4eceeea0d4bc43442de9ec60e3e145c72ad53c2c9d156da89ce106a3f10d`
BEGIN_LITERAL_SOURCE:D-007
## D-007 · Wielonajemność przygotowana od pierwszego dnia
**2026-08-22 · przyjęta**

**Decyzja:** `tenant_id` w każdej tabeli od MVP1, mimo że najemca jest jeden.
Nic specyficznego dla NASTER w kodzie.

**Uzasadnienie:** dołożenie izolacji do działającego systemu jest przepisaniem,
a nie rozszerzeniem. Koszt teraz: kilka godzin.

**Konsekwencje:** nieco więcej ceremonii w zapytaniach od początku.

---
END_LITERAL_SOURCE:D-007

### Literalny zakres `D-008` — zapis źródłowy

SOURCE_REF: `SRC-01`
SOURCE_PATH: `docs/spec/decisions.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/spec/decisions.md:115-128` (## D-008 · Python i szablony serwerowe zamiast osobnego frontendu)
LITERAL_SHA256: `d010ce58daf24d83d55d0518ee97a4312bf8b2738c5026d0e52c6973a7711eb7`
BEGIN_LITERAL_SOURCE:D-008
## D-008 · Python i szablony serwerowe zamiast osobnego frontendu
**2026-08-22 · przyjęta**

**Decyzja:** FastAPI + Jinja2 + HTMX. Bez SPA.

**Uzasadnienie:** pięć ekranów w MVP1. Osobny frontend to drugi proces budowania,
drugi zestaw zależności i drugi obszar do utrzymania przez jedną osobę.
Python zgadza się z bramą modeli.

**Konsekwencje:** przy bardzo rozbudowanym panelu w MVP4 może okazać się ciasno.
Wtedy wydzielamy sam panel, nie całość.

---
END_LITERAL_SOURCE:D-008

### Literalny zakres `D-009` — zapis źródłowy

SOURCE_REF: `SRC-01`
SOURCE_PATH: `docs/spec/decisions.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/spec/decisions.md:129-143` (## D-009 · Tokenizacja nie zwalnia z umowy powierzenia)
LITERAL_SHA256: `07bf2bbb16fe5bb7efa38fa073aa35a628d53794caf48ebda8a4b28c3361e3cd`
BEGIN_LITERAL_SOURCE:D-009
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
END_LITERAL_SOURCE:D-009

### Literalny zakres `D-010` — zapis źródłowy

SOURCE_REF: `SRC-01`
SOURCE_PATH: `docs/spec/decisions.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/spec/decisions.md:144-160` (## D-010 · Topologia agentów — decyzja odroczona)
LITERAL_SHA256: `1bf484cdfdf623dc8258e0e93f933cd59cd4f385ad31482b1c703778e9de56c2`
BEGIN_LITERAL_SOURCE:D-010
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
END_LITERAL_SOURCE:D-010

### Literalny zakres `D-011` — zapis źródłowy

SOURCE_REF: `SRC-01`
SOURCE_PATH: `docs/spec/decisions.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/spec/decisions.md:161-178` (## D-011 · Rezydencja wspólnej pamięci — blokada)
LITERAL_SHA256: `cc2b640cba5f7ae2b2b3f1fac9f0e59e33fa761a2db1b72a9b9d91950005d96e`
BEGIN_LITERAL_SOURCE:D-011
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

---
END_LITERAL_SOURCE:D-011

### Literalny zakres `D-012` — zapis źródłowy

SOURCE_REF: `SRC-01`
SOURCE_PATH: `docs/spec/decisions.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/spec/decisions.md:179-220` (## D-012 · Web-first i serwerowa własność pracy)
LITERAL_SHA256: `2a3dd97dd3819b1df276cbfefa8108d07f15729aa1fa775325ee4dcda1fe5440`
BEGIN_LITERAL_SOURCE:D-012
## D-012 · Web-first i serwerowa własność pracy
**2026-09-13 · przyjęta dyrektywą właściciela**

**Decyzja:** 8gent dostarcza najpierw bezpieczny, prosty dostęp webowy do
gotowych profili i czatów. Pracownik nie konfiguruje gatewaya, serwera, profilu
technicznego, modeli ani poświadczeń. Serwer jest właścicielem sesji, kolejki,
workerów, pamięci i audytu; przeglądarka oraz Desktop są klientami.

Nakładka na Desktop albo dostosowanie Desktopu wchodzi dopiero jako drugi etap,
po potwierdzeniu scenariusza webowego. AutoBot Router i AutoBot Monitor pozostają
modułami 8gent, a nie osobnymi projektami nadrzędnymi.

**Rozważane opcje:**

- **(a) Desktop-first** — pracownik korzysta z Desktopu, a ciągłość zależy od
  jego połączenia i lokalnego cyklu życia.
- **(b) Web-first** — pracownik korzysta z gotowego webowego profilu/czatu,
  a Desktop jest późniejszym klientem dodatkowym.
- **(c) Gateway-first dla pracownika** — każdy pracownik sam wybiera i
  konfiguruje gateway oraz profil techniczny.

**Wybór:** **(b)**.

**Uzasadnienie:** wariant (a) narusza podstawowy cel pracy serwerowej, a wariant
(c) przenosi złożoność administracyjną na pracowników. Wariant (b) pozwala
oddzielić użyteczność pracownika od obsługi infrastruktury i zachować jedną
serwerową ścieżkę wykonania.

**Konsekwencje:**

1. Pierwszym zadaniem jest potwierdzenie bezpiecznego, łatwego dostępu przez
   istniejącą webową powierzchnię Hermesa albo web 8gent.
2. Zamknięcie przeglądarki musi zostać sprawdzone jako scenariusz ciągłości;
   sam status „połączono" nie jest dowodem.
3. Ustawienia zaawansowane trafiają do powierzchni administratora 8gent/Hermesa
   albo terminala; pracownik widzi przydzielony profil i czat.
4. Desktop nie może być wymagany do działania 8gent ani do utrzymania workerów.
5. Dokładny wybór hosta, uwierzytelniania, TLS i rezydencji danych pozostaje
   osobnymi decyzjami, jeśli zmieni koszt, dostęp, dane lub odwracalność.

---
END_LITERAL_SOURCE:D-012

### Literalny zakres `D-013` — zapis źródłowy

SOURCE_REF: `SRC-01`
SOURCE_PATH: `docs/spec/decisions.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/spec/decisions.md:221-264` (## D-013 · Serwerowy pomocnik procesu dla autonomicznej pętli)
LITERAL_SHA256: `59ac5f4dc5932ba06fe6b1653c1b28af4b39c18234a69e50e6d5e8fb556b88a6`
BEGIN_LITERAL_SOURCE:D-013
## D-013 · Serwerowy pomocnik procesu dla autonomicznej pętli
**2026-09-13 · przyjęta wyborem właściciela wariantu A**

**Decyzja:** 8gent używa serwerowego pomocnika procesu z trwałą instrukcją,
który odbiera dyspozycje Crona i prowadzi wyłącznie kwalifikowane przejścia
istniejącego grafu Kanbana. Pomocnik nie jest dzieckiem bieżącego czatu ani
Desktopu. Działa jako niezależny profil/sesja procesu pod nadzorem serwera.

Cron pozostaje read-only generatorem dyspozycji. Pomocnik wykonuje świeży
readback i może uruchomić następny wcześniej zatwierdzony etap:

```text
Operator → Evaluator → Obrona tylko przy konkretnych zarzutach
→ Final Control → INTEGRATION_REQUIRED
```

Pomocnik nie tworzy nowego zakresu, nie zmienia `GOAL`, allowlisty ani decyzji
właściciela. Nie uznaje raportu, statusu UI ani `queued` za dowód. Nie wykonuje
integracji, pushu, merge'u ani deployu. Niejasność, brak dowodu, obcy profil,
brak zależności, konflikt projektu lub błąd receiptu oznacza zatrzymanie
strumienia i eskalację.

**Rozważane warianty:**

- **(A) Pomocnik serwerowy z instrukcją procesu** — trwały proces wykonuje
  rutynowe przejścia i eskaluje wyjątki.
- **(B) Mechaniczny dispatcher** — działa bez modelu, ale nie ocenia dobrze
  zarzutów, obrony i nietypowych raportów.
- **(C) Obieg zależny od Desktopu** — pozostawia właściciela procesu po stronie
  klienta i nie zapewnia ciągłości po jego zamknięciu.

**Wybór:** **(A)**.

**Konsekwencje:**

1. Kanban, eventy, rodzice, runy i durable receipts pozostają źródłem prawdy.
2. Reguły 8gent i AutoBot są ładowane z wersjonowanych dokumentów, a krytyczne
   przejścia są dodatkowo wymuszane przez kod; sama instrukcja językowa nie jest
   kontrolą bezpieczeństwa.
3. Pomocnik może działać bez obecności właściciela przy przejściach
   jednoznacznie opisanych w grafie.
4. Pytania produktowe, decyzje o danych, kosztach, dostępie, prawie,
   odwracalności oraz wszystkie niejednoznaczności trafiają do eskalacji.
5. Zamykanie Desktopu jest wymaganym scenariuszem testowym, nie założeniem.
END_LITERAL_SOURCE:D-013

## 2. `DEC-02` — literalny rejestr ECHO

SOURCE_PATH: `docs/process/echo.md`
SOURCE_SHA256: `e631fb90812066690a1c1a927abb885472157c2607f8c0ba4f2e6ad56996bbde`
SOURCE_STATUS: `CANONICAL` (P4 `NAG-PROCESS`, rodzina `PF-0219`)
SOURCE_OF_TRUTH: `true` dla literalnych wpisów ECHO; zasady procesu pozostają w skillu/CLAUDE
SOURCE_LOCATOR: `§Wpisy` oraz pięć nagłówków `###` w zakresie linii 32–146
SOURCE_LINK: [`docs/process/echo.md`](../../echo.md)

Zidentyfikowano 5 wpisów ECHO. Każdy ma poniżej literalną odpowiedź,
datę, autora, status źródła i locator. `MOJA REKOMENDACJA` z noty lub pytań
nie jest ECHO i nie jest kopiowana do tego rejestru jako decyzja.

| ECHO | Odpowiedź literalna | Data | Autor | Powiązanie | Status źródła | Locator |
|---|---|---|---|---|---|---|
| `NAG-MVP1-008-audyt-Q1` | `B` | `2026-09-11` | `właściciel` | brak D-xxx; źródłowy skutek mówi wprost: bez zmiany decisions.md | `CANONICAL` | `docs/process/echo.md:32-44` |
| `ECHO-001` | `przyjęte` | `2026-08-22` | `właściciel` | zasady procesu; źródło nie przypisuje wpisu do D-xxx | `CANONICAL` | `docs/process/echo.md:45-72` |
| `ECHO-002` | `przyjęte` | `2026-08-22` | `właściciel` | zasady procesu; źródło nie przypisuje wpisu do D-xxx | `CANONICAL` | `docs/process/echo.md:73-99` |
| `ECHO-003` | `A` | `2026-08-23` | `właściciel` | potwierdza ECHO-001; nie tworzy nowego D-xxx | `CANONICAL` | `docs/process/echo.md:100-136` |
| `NAG-INFRA-002-pomocnik-serwerowy-Q1` | `A` | `2026-09-13` | `właściciel` | D-013 | `CANONICAL` | `docs/process/echo.md:137-146` |

Każdy wpis poniżej jest literalnym zakresem z `SRC-02`. Zachowano także
objaśnienia źródłowe, ale ich status nie wykracza poza status ECHO zapisany
w samym źródle.

### Literalny wpis `NAG-MVP1-008-audyt-Q1` — zapis źródłowy

SOURCE_REF: `SRC-02`
SOURCE_PATH: `docs/process/echo.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/process/echo.md:32-44` (### NAG-MVP1-008-audyt-Q1 — trwałość i dostęp do audytu)
LITERAL_SHA256: `a3dc2d9915e41d4bdf830c6b9ccd9479c21888d72e15dfeda4b55a6c5e0fb9ab`
BEGIN_LITERAL_SOURCE:NAG-MVP1-008-audyt-Q1
### NAG-MVP1-008-audyt-Q1 — trwałość i dostęp do audytu

```text
NAG-MVP1-008-audyt-Q1 = B
data: 2026-09-11
kto: właściciel
pytanie: Czy audyt ma być trwale zapisywany w PostgreSQL i dostępny właścicielowi oraz roli administracyjnej?
wariant: PostgreSQL `audit_event` z migracją oraz `GET /admin/audit` dla właściciela i roli administracyjnej `admin`.
skutek: wznowienie tematu NAG-MVP1-008-audyt w rundzie 2; bez zmiany docs/spec/decisions.md.
```

---
END_LITERAL_SOURCE:NAG-MVP1-008-audyt-Q1

### Literalny wpis `ECHO-001` — zapis źródłowy

SOURCE_REF: `SRC-02`
SOURCE_PATH: `docs/process/echo.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/process/echo.md:45-72` (### ECHO-001 — delegowanie i przydział modeli)
LITERAL_SHA256: `446a57288beee647e50ee822ab37836278ffc55be1ee6c4a9210859fde7be58e`
BEGIN_LITERAL_SOURCE:ECHO-001
### ECHO-001 — delegowanie i przydział modeli

```text
ECHO-001 = przyjęte
data: 2026-08-22
kto:  właściciel
```

**Treść decyzji, zapisana literalnie:**

> „Zgodnie z zasadami Autobot w tym czacie zlecasz wszystkie prace zawsze do
> wykonania. Sonet 5 jako operator i działasz dalej za zasadą Autobot. (…)
> W tym czacie nie pracujesz nad niczym sam. Wszystko zlecasz do subagentów
> zgodnie z zasadą Autobot. Jeżeli jest więcej zadań zebranych, uruchamiasz
> jeszcze Agendic Workflow. Ustaliliśmy, że jest operator, jest walidator
> i finalna weryfikacja. Wszystkie lecą na Sonnet 5 High."

**Skutek:**

1. Orkiestracja wieloagentowa **włączona** — zgoda jawna, bezterminowa do odwołania.
2. Przydział: Operator, Evaluator i Final Control — **Sonnet 5, effort wysoki.**
3. Orkiestrator nie wykonuje pracy sam; wyjątki wyliczone w skillu §11.4.
4. Przy kilku zebranych tematach — workflow z fan-outem, nie kolejka wywołań.

**Zapisane w:** `.claude/skills/nagents-autobot/SKILL.md` §11.

---
END_LITERAL_SOURCE:ECHO-001

### Literalny wpis `ECHO-002` — zapis źródłowy

SOURCE_REF: `SRC-02`
SOURCE_PATH: `docs/process/echo.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/process/echo.md:73-99` (### ECHO-002 — dispatch wyłącznie przez workflow)
LITERAL_SHA256: `14c94e1a6e88774dbca956199fd2d6ea4913117ae2c3e6167fa23ce6d3d8ed50`
BEGIN_LITERAL_SOURCE:ECHO-002
### ECHO-002 — dispatch wyłącznie przez workflow

```text
ECHO-002 = przyjęte
data: 2026-08-22
kto:  właściciel
```

**Treść decyzji, zapisana literalnie:**

> „Jeżeli przydzielasz subagentów do pracy, dawaj zawsze w agencie workflow,
> ponieważ wtedy możesz wyznaczyć afort. Jeśli przydzielasz zwykłych subagentów
> bez workflow, nie możesz tego zrobić."

**Skutek:**

1. **Każde** zlecenie pracy subagentowi idzie przez narzędzie workflow — także
   pojedyncze, drobne zadanie.
2. Powód techniczny: zwykłe wywołanie subagenta przyjmuje wyłącznie model;
   **effort jest ustawialny tylko w workflow**. Bez workflow przydział
   „Sonnet 5, effort wysoki" z ECHO-001 jest niewykonalny.
3. Każde wywołanie `agent()` musi mieć **jawnie** podane `model` i `effort`.

**Zapisane w:** `.claude/skills/nagents-autobot/SKILL.md` §11.3.

---
END_LITERAL_SOURCE:ECHO-002

### Literalny wpis `ECHO-003` — zapis źródłowy

SOURCE_REF: `SRC-02`
SOURCE_PATH: `docs/process/echo.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/process/echo.md:100-136` (### ECHO-003 — potwierdzenie ECHO-001 przy sprzeczności zapisów)
LITERAL_SHA256: `460e00eadfa2f1d6fa643210ecddf07b25de4f6e0f23c3702ed86846e36c8076`
BEGIN_LITERAL_SOURCE:ECHO-003
### ECHO-003 — potwierdzenie ECHO-001 przy sprzeczności zapisów

```text
ECHO-003 = A
data: 2026-08-23
kto:  właściciel
```

**Skąd wzięło się pytanie.** Właściciel zapisał poza tym repozytorium: „ten
proces obowiązuje w projekcie 8gent, orkiestracja wieloagentowa domyślnie OFF
do jawnej zgody". To stało w sprzeczności z **ECHO-001**, gdzie zgoda została
udzielona bezterminowo, oraz z całą pracą wykonaną 2026-08-23, która na tej
zgodzie się opierała. Orkiestrator nie rozstrzygnął sprzeczności sam — zadał
pytanie w głównym wątku, zgodnie z zasadą, że przy konflikcie zapisów decyzję
podejmuje właściciel.

**Pytanie:** ECHO-001 dał bezterminową zgodę na pracę przez subagentów, a
nowszy zapis mówi o zgodzie na daną sesję. Co obowiązuje?

**Wybrany wariant A:** ECHO-001 zostaje w mocy. Zgoda jest bezterminowa.
Zlecający nie pyta o nią przy każdym zadaniu ani na starcie sesji.

**Skutek:**

1. **ECHO-001 obowiązuje bez zmian.** Zgoda na pracę wielu wykonawców naraz
   jest bezterminowa, do jawnego odwołania.
2. Reguła „domyślnie wyłączona, wymaga zgody na daną sesję" z `CLAUDE.md`
   opisuje stan wyjściowy projektu, w którym takie ECHO nie zapadło. Tutaj
   zapadło. `CLAUDE.md` uzupełniony o to zastrzeżenie, żeby następny agent nie
   trafił na tę samą sprzeczność.
3. Sposób odwołania zgody nie zmienia się — opisuje go §11.5 skilla.

**Zapisane w:** `CLAUDE.md` (zasady pracy z właścicielem),
`.claude/skills/nagents-autobot/SKILL.md` §21.

---
END_LITERAL_SOURCE:ECHO-003

### Literalny wpis `NAG-INFRA-002-pomocnik-serwerowy-Q1` — zapis źródłowy

SOURCE_REF: `SRC-02`
SOURCE_PATH: `docs/process/echo.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/process/echo.md:137-146` (### NAG-INFRA-002-pomocnik-serwerowy-Q1 — wybór trybu pomocnika)
LITERAL_SHA256: `c7c0a874c160cceaa2ef58280ea0de9f43c5d821a4d7a70acf93477f97890460`
BEGIN_LITERAL_SOURCE:NAG-INFRA-002-pomocnik-serwerowy-Q1
### NAG-INFRA-002-pomocnik-serwerowy-Q1 — wybór trybu pomocnika

```text
NAG-INFRA-002-pomocnik-serwerowy-Q1 = A
data: 2026-09-13
kto: właściciel
pytanie: Czy pętla procesu ma być prowadzona przez serwerowego pomocnika z trwałą instrukcją, a nie przez Desktop?
wariant: Wariant A — pomocnik serwerowy odbiera dyspozycje Crona, prowadzi kwalifikowane przejścia Kanbana i eskaluje niejednoznaczności.
skutek: D-013; uruchomienie implementacji i canary bez zależności od Desktopu.
```
END_LITERAL_SOURCE:NAG-INFRA-002-pomocnik-serwerowy-Q1

## 3. `DEC-03` — pytania otwarte oddzielone od decyzji

SOURCE_PATH: `docs/process/pytania/2026-08-25-wybory.md`
SOURCE_SHA256: `3e0b66a6157ed159fc5c95a8a65e8b3ec006ad465b69c164573249286d9398e7`
SOURCE_STATUS: `CANONICAL` (P4 `NAG-DECISIONS`, rodzina `PF-0225`)
SOURCE_OF_TRUTH: `true` dla aktualnego pakietu pytań; decyzje pozostają w DEC-01/ECHO
SOURCE_LOCATOR: nagłówki `Pytanie 1`…`Pytanie 8`, linie źródła zapisane w `coverage.json`
SOURCE_LINK: [`docs/process/pytania/2026-08-25-wybory.md`](../../pytania/2026-08-25-wybory.md)

Plik źródłowy mówi, że każde z ośmiu pytań wymaga decyzji właściciela.
`MOJA REKOMENDACJA` jest jawnie podpowiedzią, nie decyzją. Poniższa tabela
rejestruje status pytania osobno od D-001…D-013 i od ECHO.

| ID | Tytuł | Pilność literalna | Status | Powiązanie z decyzją | Rekomendacja | Locator |
|---|---|---|---|---|---|---|
| `Q-01` | Gdzie ma fizycznie stać komputer, na którym działa cały system | `BLOKUJE TERAZ` | `OPEN_OWNER_DECISION` | brak | `NOT_A_DECISION` | `docs/process/pytania/2026-08-25-wybory.md:34-99` |
| `Q-02` | Czy potrzebujemy prawnika, zanim system zacznie pracować na prawdziwych danych | `BLOKUJE TERAZ` | `OPEN_OWNER_DECISION` | brak | `NOT_A_DECISION` | `docs/process/pytania/2026-08-25-wybory.md:100-170` |
| `Q-03` | Jak długo trzymamy zapisy rozmów pracowników z agentami | `BLOKUJE TERAZ` | `OPEN_OWNER_DECISION` | brak | `NOT_A_DECISION` | `docs/process/pytania/2026-08-25-wybory.md:171-234` |
| `Q-04` | W jaki sposób agent ma się łączyć z programami firmy | `POTRZEBNE PRZED ETAPEM MVP2` | `OPEN_OWNER_DECISION` | brak | `NOT_A_DECISION` | `docs/process/pytania/2026-08-25-wybory.md:235-394` |
| `Q-05` | Ile programów firmowych planujemy naraz, a ile na zapas | `POTRZEBNE PRZED ETAPEM MVP2` | `OPEN_OWNER_DECISION` | brak | `NOT_A_DECISION` | `docs/process/pytania/2026-08-25-wybory.md:395-470` |
| `Q-06` | Kto formalnie potwierdza, że dany etap projektu jest skończony | `POTRZEBNE PRZED ETAPEM MVP2` | `OPEN_OWNER_DECISION` | brak | `NOT_A_DECISION` | `docs/process/pytania/2026-08-25-wybory.md:471-539` |
| `Q-07` | Co się dzieje, gdy jedyna osoba techniczna w firmie jest niedostępna | `POTRZEBNE PRZED ETAPEM MVP2` | `OPEN_OWNER_DECISION` | brak | `NOT_A_DECISION` | `docs/process/pytania/2026-08-25-wybory.md:540-603` |
| `Q-08` | Gdzie ma być przechowywana wspólna wiedza, którą agenci zapamiętują z rozmów | `POTRZEBNE PRZED ETAPEM MVP3` | `OPEN_OWNER_DECISION` | `D-011` | `NOT_A_DECISION` | `docs/process/pytania/2026-08-25-wybory.md:604-676` |

D-010 jest osobną otwartą decyzją z DEC-01. Źródłowy pakiet pytań opisuje
ją jako informację wcześniej odłożoną, a nie jako dziewiąte pytanie z tego
zestawu. D-011 jest istniejącą otwartą i blokującą decyzją, do której odnosi
się Q-08. Żadne z tych statusów nie jest zgodą na wybór wariantu.

Każdy poniższy zakres jest literalną kopią odpowiedniej sekcji pytań. Wszelkie
warianty i rekomendacje wewnątrz zakresu zachowują status `NOT_A_DECISION`.

### Literalne pytanie `Q-01` — zakres źródłowy

SOURCE_REF: `SRC-03`
SOURCE_PATH: `docs/process/pytania/2026-08-25-wybory.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/process/pytania/2026-08-25-wybory.md:34-99` (## Pytanie 1 — Gdzie ma fizycznie stać komputer, na którym działa cały system)
CONTENT_STATUS: `OPEN_QUESTION; RECOMMENDATIONS_NOT_DECISIONS`
LITERAL_SHA256: `d49a6bf58077d48e1dffd80f938bfdbd896282c2ebcd903d1b3f12741a0527c0`
BEGIN_LITERAL_SOURCE:Q-01
## Pytanie 1 — Gdzie ma fizycznie stać komputer, na którym działa cały system

**Pilność: BLOKUJE TERAZ**

### SYTUACJA
Dokumentacja mówi tylko, że na start wystarczy jeden komputer o określonej
mocy. Nie zapisano, czy ma to być nasz własny sprzęt, czy wynajęty
u zewnętrznej firmy, ani w jakim kraju ma fizycznie stać.

### PROBLEM
Bez tej decyzji nie da się w ogóle uruchomić działającego systemu. To
dosłownie miejsce, w którym cała reszta pracy ma zacząć działać — uderza to
od pierwszego dnia pierwszego etapu. Dodatkowo, jeśli na tym komputerze
będą dane osobowe pracowników i klientów, kraj, w którym stoi, ma znaczenie
prawne. Przepisy o ochronie danych w Unii Europejskiej są surowsze niż poza
nią.

### MOŻLIWE ROZWIĄZANIA

**WARIANT A — własny sprzęt w firmie**
Na czym polega: kupujemy albo wykorzystujemy istniejący komputer w biurze,
system działa lokalnie.
Co zyskujesz: pełną kontrolę, brak miesięcznej opłaty za wynajem.
Co tracisz: brak zapasowego prądu i internetu klasy profesjonalnej
serwerowni — awaria prądu w biurze wyłącza system dla wszystkich naraz.
Ile to kosztuje: **szacunek** — jednorazowo kilka tysięcy złotych za sprzęt,
plus prąd i internet, który i tak już opłacamy.
Ryzyko: awaria sprzętu bez kopii w innym miejscu może oznaczać utratę danych.
Co wtedy zrobimy: trzymać kopię zapasową także poza biurem.

**WARIANT B — wynajęty serwer w Unii Europejskiej**
Na czym polega: wynajmujemy jeden komputer u zewnętrznego dostawcy,
wskazując wyraźnie miejsce jego pracy w Unii Europejskiej.
Co zyskujesz: zapasowe zasilanie i łącze klasy profesjonalnej, łatwość
powiększenia mocy, gdy firma urośnie.
Co tracisz: comiesięczna opłata, zależność od dostawcy.
Ile to kosztuje: **szacunek** — kilkaset złotych miesięcznie na start,
rosnące z użyciem.
Ryzyko: dostawca ma awarię swojej serwerowni — system stoi, dopóki jej nie
naprawi. Co wtedy zrobimy: wybrać dostawcę z gwarancją dostępności zapisaną
w umowie.

**WARIANT C — najtańszy wynajęty serwer, bez względu na kraj**
Na czym polega: bierzemy najtańszą ofertę niezależnie od tego, w jakim kraju
fizycznie stoi.
Co zyskujesz: najniższy koszt miesięczny.
Co tracisz: potencjalny problem prawny z ochroną danych pracowników
i klientów, jeśli trafiają poza Unię Europejską bez dodatkowych
zabezpieczeń.
Ile to kosztuje: **szacunek** — najtańsza opcja, ale z dodatkowym kosztem
prawnym, który może zjeść oszczędność.
Ryzyko: kontrola albo skarga pracownika ujawnia nieprawidłowy transfer
danych za granicę Unii — kara finansowa i utrata zaufania. Co wtedy zrobimy:
ten wariant odradzamy bez zgody prawnika.

### PYTANIE
Stawiamy system na własnym sprzęcie w firmie, czy wynajmujemy serwer
u zewnętrznego dostawcy z siedzibą w Unii Europejskiej, czy bierzemy
najtańszą ofertę bez względu na kraj?

### MOJA REKOMENDACJA
Wariant B — daje przewidywalność i mniej obowiązków dla jedynej osoby
technicznej, przy rozsądnym koszcie. To rekomendacja, nie decyzja.

---
END_LITERAL_SOURCE:Q-01

### Literalne pytanie `Q-02` — zakres źródłowy

SOURCE_REF: `SRC-03`
SOURCE_PATH: `docs/process/pytania/2026-08-25-wybory.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/process/pytania/2026-08-25-wybory.md:100-170` (## Pytanie 2 — Czy potrzebujemy prawnika, zanim system zacznie pracować na prawdziwych danych)
CONTENT_STATUS: `OPEN_QUESTION; RECOMMENDATIONS_NOT_DECISIONS`
LITERAL_SHA256: `fbd6dce5a67fc44519f60b09ff19c4bf8ed7ed421e5e3c5ca3614902270823a0`
BEGIN_LITERAL_SOURCE:Q-02
## Pytanie 2 — Czy potrzebujemy prawnika, zanim system zacznie pracować na prawdziwych danych

**Pilność: BLOKUJE TERAZ**

### SYTUACJA
System od pierwszego dnia będzie wysyłał treść rozmów do zewnętrznych
dostawców modeli językowych, którzy fizycznie przetwarzają te wiadomości.
Dotyczy to danych pracowników, a przy pierwszym prawdziwym teście — także
danych klientów. Dziś nie mamy z tymi dostawcami podpisanej żadnej umowy
o ochronie tych danych, ani potwierdzenia, że pracownicy zostali
poinformowani o takim przetwarzaniu ich rozmów.

### PROBLEM
Pierwszy prawdziwy test systemu ma polegać na przeliczeniu prawdziwych
rozliczeń klientów, pod koniec pierwszego etapu. Zrobienie tego bez
wymaganych prawem zgód i umów naraża firmę na odpowiedzialność prawną
i karę. Naprawa po fakcie oznacza zmianę dostawcy i powtórzenie testu, nie
tylko poprawkę dokumentu. Procesy prawne bywają czasochłonne i nie da się
ich przyspieszyć w ostatniej chwili — dlatego to pytanie ma znaczenie już
teraz, nie dopiero przy samym teście.

### MOŻLIWE ROZWIĄZANIA

**WARIANT A — pełny przegląd prawny przed uruchomieniem na prawdziwych
danych**
Na czym polega: zlecamy prawnikowi sprawdzenie całości — umowę z dostawcą
modeli, informację dla pracowników o przetwarzaniu ich rozmów i podstawę
prawną wysyłania danych klientów.
Co zyskujesz: pewność, że pierwszy prawdziwy test jest legalny.
Co tracisz: czas — prawnik potrzebuje typowo od kilku dni do kilku tygodni.
Ile to kosztuje: **szacunek** — koszt godzin prawnika, rząd wielkości kilku
tysięcy złotych za komplet dokumentów. Dokładnej kwoty nie znamy bez
wyceny.
Ryzyko: prawnik znajdzie problem trudny do szybkiego rozwiązania, np.
dostawca nie chce podpisać takiej umowy. Co wtedy zrobimy: zlecić przegląd
jak najwcześniej, równolegle z pisaniem kodu.

**WARIANT B — pierwszy test na danych sztucznych, prawdziwe dane później**
Na czym polega: pierwszy test etapu robimy na wymyślonych danych
rozliczeniowych, a przejście na prawdziwe dane klientów odkładamy do czasu,
aż dokumenty prawne będą gotowe.
Co zyskujesz: brak blokady na starcie, można zamknąć pierwszy etap bez
czekania na prawnika.
Co tracisz: słabszy dowód, że system dobrze radzi sobie z prawdziwymi,
różnorodnymi przypadkami — dane sztuczne są zwykle „czystsze".
Ile to kosztuje: brak dodatkowego kosztu teraz, koszt prawny i tak trzeba
ponieść później.
Ryzyko: odkładanie dokumentów prawnych „na później" ma tendencję do
przeciągania się bez terminu. Co wtedy zrobimy: zapisać twardy termin.

**WARIANT C — uruchomienie na prawdziwych danych bez czekania na prawnika**
Na czym polega: nie czekamy, testujemy od razu na prawdziwych danych
rozliczeniowych klientów.
Co zyskujesz: najszybszy możliwy termin zamknięcia pierwszego etapu.
Co tracisz: realne ryzyko naruszenia przepisów o ochronie danych osobowych.
Ile to kosztuje: brak kosztu z góry, potencjalnie wysoki koszt kary
i naprawy.
Ryzyko: kontrola albo skarga ujawnia brak wymaganych zgód — kara finansowa
i utrata zaufania klientów. Co wtedy zrobimy: ten wariant odradzamy.

### PYTANIE
Zlecamy prawnikowi przegląd przed pierwszym testem na prawdziwych danych,
czy najpierw testujemy na danych sztucznych i przechodzimy na prawdziwe
później, czy ruszamy od razu na prawdziwych danych bez czekania?

### MOJA REKOMENDACJA
Wariant A, uruchomiony równolegle z pisaniem kodu, żeby czas pracy prawnika
nie opóźniał harmonogramu. To rekomendacja, nie decyzja.

---
END_LITERAL_SOURCE:Q-02

### Literalne pytanie `Q-03` — zakres źródłowy

SOURCE_REF: `SRC-03`
SOURCE_PATH: `docs/process/pytania/2026-08-25-wybory.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/process/pytania/2026-08-25-wybory.md:171-234` (## Pytanie 3 — Jak długo trzymamy zapisy rozmów pracowników z agentami)
CONTENT_STATUS: `OPEN_QUESTION; RECOMMENDATIONS_NOT_DECISIONS`
LITERAL_SHA256: `c7d54f520909a2ebfab6926afe082536734c173407b1134fedc282d0870d5f9f`
BEGIN_LITERAL_SOURCE:Q-03
## Pytanie 3 — Jak długo trzymamy zapisy rozmów pracowników z agentami

**Pilność: BLOKUJE TERAZ**

### SYTUACJA
System zapisuje każdą rozmowę pracownika z agentem oraz osobny dziennik
„kto, kiedy, do czego miał albo nie miał dostępu". Nie ustalono dziś, jak
długo te zapisy mają być przechowywane ani kto dokładnie może je czytać.

### PROBLEM
Bez jasnej zasady każdy zapis rozmowy zostaje „na zawsze" z automatu. To
jest trudne do obrony prawnie i buduje nieufność pracowników, jeśli nie
wiedzą, kto czyta ich rozmowy z agentem. Ryzyko rośnie z każdym kolejnym
miesiącem działania systemu — im więcej rozmów się zbierze bez ustalonej
zasady, tym trudniej to później uporządkować. Ta decyzja wiąże się wprost
z pytaniem 2 (prawnik) i powinna zapaść w tym samym czasie, przed pierwszym
testem na prawdziwych danych klientów.

### MOŻLIWE ROZWIĄZANIA

**WARIANT A — krótsza, jawna retencja rozmów (np. 12 miesięcy), dziennik
zdarzeń dłużej**
Na czym polega: treść rozmów kasujemy automatycznie po ustalonym czasie,
a osobny dziennik „kto z czego korzystał" trzymamy dłużej, bo to dowód,
nie treść prywatna.
Co zyskujesz: jasna, obronialna zasada, większe zaufanie pracowników.
Co tracisz: po skasowaniu rozmowy nie da się już wrócić do jej treści,
jeśli okaże się potrzebna.
Ile to kosztuje: **szacunek** — kilka godzin pracy na wdrożenie
automatycznego kasowania.
Ryzyko: zbyt krótki okres utrudni wyjaśnienie sporu sprzed kilkunastu
miesięcy. Co wtedy zrobimy: dobrać okres razem z prawnikiem.

**WARIANT B — przechowywanie bezterminowe**
Na czym polega: nic nie kasujemy automatycznie, wszystko zostaje na zawsze.
Co zyskujesz: zawsze można wrócić do dowolnej rozmowy z przeszłości.
Co tracisz: rosnące ryzyko prawne i rosnący koszt miejsca na dane w czasie.
Ile to kosztuje: brak kosztu teraz, rosnący koszt miejsca w miarę upływu
lat.
Ryzyko: kontrola prawna uzna to za naruszenie zasady ograniczonego
przechowywania danych. Co wtedy zrobimy: ten wariant odradzamy.

**WARIANT C — bardzo krótka retencja (np. 30 dni)**
Na czym polega: rozmowy kasujemy niemal natychmiast po zakończeniu.
Co zyskujesz: najmniejsze możliwe ryzyko prawne.
Co tracisz: brak możliwości sprawdzenia, co agent odpowiedział miesiąc
wcześniej — utrudnia to też uczenie się agentów na przeszłych rozmowach
w trzecim etapie.
Ile to kosztuje: brak dodatkowego kosztu.
Ryzyko: zbyt krótki czas może uniemożliwić wyjaśnienie świeżej reklamacji.
Co wtedy zrobimy: dopasować długość do realnych potrzeb, nie tylko
bezpieczeństwa.

### PYTANIE
Jak długo mają być przechowywane zapisy rozmów pracowników z agentami,
zanim zostaną automatycznie skasowane?

### MOJA REKOMENDACJA
Wariant A, 12 miesięcy dla treści rozmów i dłużej dla samego dziennika
zdarzeń — to zresztą już założenie w czwartym etapie projektu, warto
ustalić je świadomie już teraz. To rekomendacja, nie decyzja.

---
END_LITERAL_SOURCE:Q-03

### Literalne pytanie `Q-04` — zakres źródłowy

SOURCE_REF: `SRC-03`
SOURCE_PATH: `docs/process/pytania/2026-08-25-wybory.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/process/pytania/2026-08-25-wybory.md:235-394` (## Pytanie 4 — W jaki sposób agent ma się łączyć z programami firmy)
CONTENT_STATUS: `OPEN_QUESTION; RECOMMENDATIONS_NOT_DECISIONS`
LITERAL_SHA256: `1d9a152322cc3070b983f4c309685f6688c30c289d703fed625fc48acfe79001`
BEGIN_LITERAL_SOURCE:Q-04
## Pytanie 4 — W jaki sposób agent ma się łączyć z programami firmy

**Pilność: POTRZEBNE PRZED ETAPEM MVP2**

Zanim to pytanie: samo łączenie się z programem firmowym i wykonywanie w nim
czynności (np. zapisanie wyniku) **nie jest naszą pracą** — robi to Hermes,
nasz program silnikowy. Nasza praca jest węższa: gdzie trzymamy hasła, które
programy podłączamy, i kto sprawdza uprawnienia. Pełne ustalenie z odesłaniami
do dokumentacji — w nocie 08, sekcja „Co jest pracą Hermesa, a co naszą".

### SYTUACJA
W dzienniku decyzji projektu nie ma dziś ani jednego zapisu o tym, w jaki
sposób agent ma sięgać po dane z programów firmowych. Dziś program do
rozliczeń prowizji to plik Excel. W planie technicznym od pierwszego etapu
zarezerwowano już jednak miejsce na hasło dostępu do niego — na wypadek,
gdyby okazało się potrzebne wcześniej, niż zakładamy.

### PROBLEM
Test kończący pierwszy etap polega na przeliczeniu prawdziwych rozliczeń
z pliku Excel, więc sam ten test jeszcze nie wymaga tej decyzji. Staje się
ona potrzebna przy drugim etapie — plan zakłada, że agent ma już zapisywać
wynik wprost do systemu rozliczeń, a nie tylko czytać plik. Jeśli wybierzemy
źle, ryzykujemy dokładnie to, przed czym właściciel już raz uciekł.
Uzależnienie całej firmy od jednej zewnętrznej firmy, która może zniknąć,
podnieść cenę albo zmienić zasady bez pytania nas o zdanie.

### MOŻLIWE ROZWIĄZANIA

**WARIANT A — piszemy połączenia sami, tylko do programów, których naprawdę
używamy**
Na czym polega: nasza osoba techniczna pisze osobny kawałek programu
łączący z każdym konkretnym programem firmowym — na start tylko z programem
rozliczeniowym.
Co zyskujesz: pełną kontrolę nad tym, jak połączenie działa, i zero opłat
dla kogokolwiek z zewnątrz.
Co tracisz: każdy nowy program firmowy to kilka dni pracy naszej jednej
osoby technicznej. Nikt nas nie ostrzeże, gdy dostawca programu zmieni
zasady — sami musimy to zauważyć i naprawić.
Ile to kosztuje: **szacunek** — kilka dni pracy na jedno połączenie, potem
utrzymanie w razie zmian po stronie programu firmowego, nieregularnie,
kilka godzin miesięcznie.
Co się stanie, gdy coś pójdzie nie tak: żaden dostawca zewnętrzny nie może
tu zniknąć ani podnieść ceny — nie ma go w tym wariancie. Jedyne ryzyko to
czas naszej jednej osoby technicznej.
Jak szerokie uprawnienia w programach firmowych to wymaga: dokładnie tyle,
ile potrzeba do jednej, konkretnej czynności w jednym, konkretnym programie
— nic więcej, bo sami decydujemy, co program robi.
Czy łamie zasadę „agent bez własnych haseł": nie. Hasło dostępu trzyma
program rozliczeniowy jako całość, nie pojedynczy pracownik.
Ryzyko: przy większej liczbie programów jedna osoba staje się wąskim
gardłem. Jeśli zachoruje albo odejdzie, nikt inny nie zna od razu
wszystkich tych połączeń.

**WARIANT B — kupujemy dostęp do gotowego cudzego pośrednika
integracyjnego**
Na czym polega: płacimy zewnętrznej firmie miesięczny abonament za to, że
ma już gotowe połączenia do setek programów, i korzystamy z jej gotowej
pracy zamiast pisać własną.
Co zyskujesz: natychmiastowy dostęp do bardzo szerokiej listy programów,
bez pisania własnego kodu do każdego z nich.
Co tracisz: rosnący rachunek miesięczny, zależny od tego, ile z tego
korzystamy. To dokładnie ten sam rodzaj uzależnienia od jednej zewnętrznej
firmy, z powodu którego właściciel już raz odrzucił zakup gotowej platformy
appto.
Ile to kosztuje: **nie umiem tego oszacować w złotówkach** — ceny takich
pośredników zależą od konkretnego dostawcy i liczby operacji, a nie mamy
dziś żadnej oferty do porównania. Rząd wielkości to zwykle stały abonament
miesięczny plus opłata za każde użycie, rosnąca z ruchem.
Co się stanie, gdy coś pójdzie nie tak: jeśli ten pośrednik zniknie, zostanie
kupiony przez inną firmę albo podniesie cenę — to wszystko, czym łączymy
się z programami firmowymi, staje się zakładnikiem jego decyzji, nie
naszej.
Jak szerokie uprawnienia w programach firmowych to wymaga: bardzo szerokie.
Pośrednik zwykle prosi o dostęp ogólny do konta w danym programie, nie
tylko do jednej czynności, bo sam nie wie z góry, czego dokładnie agent
będzie chciał.
Czy łamie zasadę „agent bez własnych haseł": nie wprost, ale osłabia ją —
hasła dostępu trafiają do systemu firmy trzeciej, nie tylko do naszego
magazynu haseł.
Ryzyko: dokładnie to ryzyko uzależnienia, którego właściciel chciał
uniknąć, odrzucając zakup gotowej platformy.

**WARIANT C — korzystamy z otwartego, wspólnego sposobu podłączania
narzędzi**
Na czym polega: istnieje dziś ustalony, otwarty sposób, w jaki program
firmowy może „przedstawić się" naszemu agentowi samodzielnie, bez pisania
osobnego połączenia po naszej stronie. To trochę jak gniazdko elektryczne
o jednym standardowym kształcie — każde urządzenie po prostu się w nie
wpina. Nikt konkretny nie jest właścicielem tego standardu, więc nikt nie
może nam go zabrać ani podnieść za niego ceny. Nasz program silnikowy,
Hermes, już dziś potrafi w ten sposób rozmawiać z narzędziami — to nie jest
coś, co musielibyśmy dopiero zbudować.
Co zyskujesz: dla każdego programu firmowego, który już mówi tym otwartym
językiem, podłączenie jest praktycznie darmowe. Nie płacimy nikomu
abonamentu i nie piszemy własnego kodu.
Co tracisz: nie każdy program firmowy mówi tym językiem. Starsze, mniej
popularne albo bardzo specyficzne programy — być może nasz własny program
do rozliczeń — mogą go nie obsługiwać wcale, i wtedy ten wariant nic nie
daje.
Ile to kosztuje: **szacunek** — zero złotych za sam sposób podłączania, bo
już go mamy. Nie da się z góry oszacować, ile pracy zajmie sprawdzenie, czy
i jak dobrze konkretny program firmowy go obsługuje — trzeba to zweryfikować
program po programie.
Co się stanie, gdy coś pójdzie nie tak: gdyby ten otwarty standard przestał
być rozwijany przez społeczność, każde nasze pojedyncze połączenie zostaje
osobnym, wymiennym elementem. Możemy je po kolei zastąpić czymś innym, bez
przepisywania wszystkiego naraz. To ryzyko dużo mniejsze niż przy
pośredniku z wariantu B, bo nie ma tu jednej firmy, która może nas
„wyłączyć" jednym pismem.
Jak szerokie uprawnienia w programach firmowych to wymaga: zależy od
konkretnego programu. Zwykle da się ograniczyć do jednej czynności, ale to
program firmowy decyduje, jak dokładnie to ograniczenie wygląda, nie my.
Czy łamie zasadę „agent bez własnych haseł": nie — to wyłącznie sposób
rozmowy między programami. Hasła dostępu nadal trzymamy tak samo jak
w wariancie A.
Ryzyko: rozczarowanie, jeśli program, na którym nam najbardziej zależy —
program rozliczeniowy — akurat tego języka nie obsługuje. Wtedy i tak
wracamy do wariantu A dla tego jednego przypadku.

**WARIANT D — mieszanka: A dla programów wrażliwych, C dla reszty**
Na czym polega: program rozliczeniowy i inne wrażliwe programy (np.
przyszłe kadry) łączymy sami, ręcznie, pod pełną kontrolą. Dla mniej
wrażliwych programów, które mówią otwartym językiem z wariantu C,
korzystamy z tego gotowego sposobu.
Co zyskujesz: najwyższą kontrolę tam, gdzie błąd kosztowałby najwięcej —
rozliczenia, dane osobowe — i najniższy koszt tam, gdzie kontrola nie jest
aż tak krytyczna.
Co tracisz: musimy pilnować dwóch różnych sposobów jednocześnie zamiast
jednego. To więcej rzeczy do utrzymania przez jedną osobę techniczną.
Ile to kosztuje: **szacunek** — suma kosztów wariantu A dla programów
wrażliwych (kilka dni na program) plus czas weryfikacji z wariantu C dla
reszty. Bez opłaty za pośrednika z wariantu B.
Co się stanie, gdy coś pójdzie nie tak: nie ma tu jednego dostawcy, od
którego zależymy całościowo. Ryzyko jest rozłożone tak samo jak w wariancie
A i C osobno.
Jak szerokie uprawnienia w programach firmowych to wymaga: wąskie wszędzie.
To właśnie zaleta tego wariantu — żadna jego część nie wymaga szerokiego
dostępu ponad potrzebę.
Czy łamie zasadę „agent bez własnych haseł": nie, w żadnej z dwóch części.
Ryzyko: przy jednej osobie technicznej dwa różne sposoby pracy to więcej do
ogarnięcia niż jeden. Łatwiej o pomyłkę, który program jest obsługiwany
którą metodą.

### PYTANIE
Czy programy firmowe łączymy sami ręcznie, kupujemy dostęp do gotowego
cudzego pośrednika, korzystamy z otwartego wspólnego sposobu podłączania,
czy stosujemy mieszankę zależną od tego, jak wrażliwy jest dany program?

### MOJA REKOMENDACJA
Wariant D — dla programu rozliczeniowego, który jest najbardziej wrażliwy
(dotyczy pieniędzy i danych osobowych), piszemy połączenie sami, pod pełną
kontrolą. Dla każdego kolejnego programu firmowego najpierw sprawdzamy, czy
mówi otwartym, wspólnym językiem, którym już posługuje się nasz program
silnikowy — jeśli tak, korzystamy z tego bez dodatkowego kosztu. Wariant B
(płatny pośrednik) odradzam, bo odtwarza dokładnie to uzależnienie od
jednej zewnętrznej firmy, którego właściciel już raz się pozbył, rezygnując
z zakupu appto. To rekomendacja, nie decyzja.

---
END_LITERAL_SOURCE:Q-04

### Literalne pytanie `Q-05` — zakres źródłowy

SOURCE_REF: `SRC-03`
SOURCE_PATH: `docs/process/pytania/2026-08-25-wybory.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/process/pytania/2026-08-25-wybory.md:395-470` (## Pytanie 5 — Ile programów firmowych planujemy naraz, a ile na zapas)
CONTENT_STATUS: `OPEN_QUESTION; RECOMMENDATIONS_NOT_DECISIONS`
LITERAL_SHA256: `c2c40b71935b3cf376ff503a47f50a8680055f3846f9eabec3b8c59b8879a731`
BEGIN_LITERAL_SOURCE:Q-05
## Pytanie 5 — Ile programów firmowych planujemy naraz, a ile na zapas

**Pilność: POTRZEBNE PRZED ETAPEM MVP2**

To pytanie idzie w parze z poprzednim — odpowiedź tutaj wpływa na to, jak
szeroko warto zbudować rozwiązanie z pytania 4.

### SYTUACJA
Dziś nasza dokumentacja z pewnością wymienia z nazwy jeden program firmowy,
do którego ma sięgać agent — wewnętrzny program do rozliczeń prowizji.
Ostatni etap budowy planu, za kilkanaście miesięcy, mówi ogólnie o kolejnych
trzech kategoriach programów, bez podania nazw.

### PROBLEM
Jeśli zaplanujemy rozwiązanie od razu pod dziesiątki programów, zapłacimy
teraz za możliwość, z której skorzystamy może za rok, może wcale. Jeśli
zaplanujemy je tylko pod jeden program, a za pół roku okaże się potrzebny
piąty, będziemy to przerabiać od nowa, tracąc czas pracy zamiast go
oszczędzać. Nierozstrzygnięcie tego teraz oznacza, że osoba budująca system
i tak musi zgadywać. Zgadnie wtedy najbezpieczniej dla siebie, niekoniecznie
najtaniej dla firmy.

### MOŻLIWE ROZWIĄZANIA

**WARIANT A — planujemy wyłącznie to, co dziś naprawdę używamy**
Na czym polega: budujemy połączenie tylko do programu rozliczeniowego,
który już mamy w planie. Nic ponad to, dopóki nie pojawi się konkretna
potrzeba.
Co zyskujesz: najniższy koszt teraz, zero pracy zmarnowanej na programy,
z których nigdy nie skorzystamy.
Co tracisz: przy każdym nowym programie firmowym trzeba będzie wrócić do
tego tematu i zapłacić za niego osobno, zamiast mieć to już gotowe.
Ile to kosztuje: **szacunek** — kilka dni pracy jednej osoby technicznej na
jedno połączenie, powtarzane przy każdym kolejnym programie.
Ryzyko: jeśli programów przybędzie szybciej niż zakładamy, koszt pracy
zacznie rosnąć liniowo — każdy nowy program to kolejne kilka dni, bez
żadnej zniżki za doświadczenie z poprzednich.

**WARIANT B — planujemy od razu z zapasem na 3–5 programów w ciągu roku**
Na czym polega: budujemy pierwsze połączenie tak, żeby dało się je łatwo
powielić dla kolejnych programów podobnego typu, nawet jeśli dziś ich
jeszcze nie mamy.
Co zyskujesz: mniej pracy przy każdym kolejnym programie, bo szkielet już
istnieje.
Co tracisz: trochę więcej czasu teraz na zaprojektowanie czegoś bardziej
uniwersalnego niż potrzeba na start.
Ile to kosztuje: **szacunek** — o około 30–50% więcej czasu na pierwsze
połączenie, w zamian za mniej pracy przy drugim i trzecim.
Ryzyko: zaprojektujemy zapas pod programy, które i tak nigdy się nie
pojawią — wtedy dodatkowy czas poszedł na nic.

**WARIANT C — czekamy z jakąkolwiek decyzją, aż pojawi się drugi realny
program**
Na czym polega: teraz kończymy tylko program rozliczeniowy, a o skali
w ogóle nie decydujemy, dopóki nie będzie konkretnej, nazwanej potrzeby.
Co zyskujesz: żadnej pracy wykonanej na wyrost, decyzja zapada dopiero, gdy
mamy realne dane, nie zgadywanie.
Co tracisz: przy pojawieniu się drugiego programu decyzja i tak musi
zapaść — odkładamy moment wyboru, nie samą potrzebę wyboru.
Ile to kosztuje: brak dodatkowego kosztu teraz.
Ryzyko: żadne — to najbezpieczniejszy wariant kosztowo, choć nie przyspiesza
niczego w przyszłości.

### PYTANIE
Czy planujemy dziś rozwiązanie tylko pod program rozliczeniowy, czy
z zapasem na kilka kolejnych programów w ciągu roku, czy odkładamy tę
decyzję do pierwszej realnej potrzeby?

### MOJA REKOMENDACJA
Wariant C — nie mamy dziś żadnego nazwanego, pewnego drugiego programu do
podłączenia, więc projektowanie zapasu byłoby zgadywaniem. Do decyzji
wracamy, gdy pojawi się konkretna nazwa i konkretna potrzeba. To
rekomendacja, nie decyzja.

---
END_LITERAL_SOURCE:Q-05

### Literalne pytanie `Q-06` — zakres źródłowy

SOURCE_REF: `SRC-03`
SOURCE_PATH: `docs/process/pytania/2026-08-25-wybory.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/process/pytania/2026-08-25-wybory.md:471-539` (## Pytanie 6 — Kto formalnie potwierdza, że dany etap projektu jest skończony)
CONTENT_STATUS: `OPEN_QUESTION; RECOMMENDATIONS_NOT_DECISIONS`
LITERAL_SHA256: `4f91446328a689d5efa9ddd72cd03f739d73eeaf648b9024f2c454d5e500c86d`
BEGIN_LITERAL_SOURCE:Q-06
## Pytanie 6 — Kto formalnie potwierdza, że dany etap projektu jest skończony

**Pilność: POTRZEBNE PRZED ETAPEM MVP2**

### SYTUACJA
Każdy etap kończy się testem na prawdziwych danych, który ma zdecydować,
czy etap jest gotowy. Nie zapisano, czyja to ostateczna zgoda — czy
wystarczy, że osoba techniczna zobaczy dobry wynik, czy musi to Pan/Pani
osobiście zobaczyć i potwierdzić.

### PROBLEM
Bez jasnej zasady etap może zostać uznany za „gotowy" bez faktycznej
akceptacji osoby, która za niego płaci i odpowiada biznesowo. Albo
odwrotnie — każdy drobny krok będzie czekał na Pana/Pani osobisty przegląd,
co spowolni pracę bardziej, niż potrzeba. Pierwsze takie zamknięcie nastąpi
już pod koniec pierwszego etapu, więc zasadę warto ustalić, zanim to
nastąpi.

### MOŻLIWE ROZWIĄZANIA

**WARIANT A — Pan/Pani osobiście potwierdza zamknięcie każdego etapu**
Na czym polega: przed przejściem do kolejnego etapu widzi Pan/Pani wynik
kluczowego testu biznesowego i wyraźnie potwierdza, że etap jest zamknięty.
Co zyskujesz: pewność, że żaden etap nie zostanie uznany za gotowy bez
Pana/Pani wiedzy.
Co tracisz: zamknięcie etapu czeka na Pana/Pani dostępność.
Ile to kosztuje: brak kosztu pieniężnego, koszt to czas Pana/Pani uwagi raz
na kilka tygodni.
Ryzyko: przy napiętym harmonogramie oczekiwanie na przegląd może stać się
wąskim gardłem. Co wtedy zrobimy: umówić z góry orientacyjny termin przy
każdym etapie.

**WARIANT B — osoba techniczna zamyka etap samodzielnie, na podstawie
testów**
Na czym polega: etap uznaje się za zamknięty, gdy wszystkie zaplanowane
testy przechodzą, bez osobnego potwierdzenia Pana/Pani.
Co zyskujesz: najszybsze możliwe tempo przechodzenia między etapami.
Co tracisz: brak Pana/Pani bezpośredniej weryfikacji najważniejszego,
biznesowego kryterium.
Ile to kosztuje: brak dodatkowego kosztu.
Ryzyko: etap zostaje uznany za gotowy, mimo że wynik nie satysfakcjonuje
faktycznie Pana/Pani jako odbiorcy pracy. Co wtedy zrobimy: ten wariant
odradzamy dla testu kluczowego dla biznesu.

**WARIANT C — rozwiązanie pośrednie: zamyka technika, Pan/Pani dostaje
krótkie podsumowanie z prawem sprzeciwu**
Na czym polega: osoba techniczna zamyka etap od razu, ale Pan/Pani dostaje
w ciągu 48 godzin krótkie podsumowanie wyniku kluczowego testu i może w tym
czasie zgłosić sprzeciw.
Co zyskujesz: szybkość wariantu B połączoną z realnym wglądem z wariantu A.
Co tracisz: teoretycznie etap mógł już zacząć „żyć" przez te 48 godzin,
zanim Pan/Pani zdąży zareagować.
Ile to kosztuje: brak dodatkowego kosztu poza krótkim czasem na
przeczytanie podsumowania.
Ryzyko: 48 godzin okaże się za krótkie przy napiętym kalendarzu. Co wtedy
zrobimy: wydłużyć okno sprzeciwu, jeśli w praktyce się nie sprawdzi.

### PYTANIE
Czy etap zamyka się dopiero po Pana/Pani osobistym potwierdzeniu kluczowego
testu? Czy wystarczy, że osoba techniczna zobaczy dobry wynik i poinformuje
Pana/Panią później? Czy stosujemy rozwiązanie pośrednie z prawem sprzeciwu?

### MOJA REKOMENDACJA
Wariant A dla testu kluczowego dla biznesu (np. zgodność rozliczenia) —
reszta testów technicznych może zamykać się bez Pana/Pani udziału. To
rekomendacja, nie decyzja.

---
END_LITERAL_SOURCE:Q-06

### Literalne pytanie `Q-07` — zakres źródłowy

SOURCE_REF: `SRC-03`
SOURCE_PATH: `docs/process/pytania/2026-08-25-wybory.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/process/pytania/2026-08-25-wybory.md:540-603` (## Pytanie 7 — Co się dzieje, gdy jedyna osoba techniczna w firmie jest niedostępna)
CONTENT_STATUS: `OPEN_QUESTION; RECOMMENDATIONS_NOT_DECISIONS`
LITERAL_SHA256: `1321a87143ddceaff31e86c23189b7c68e55556877fe979725f30877305e699d`
BEGIN_LITERAL_SOURCE:Q-07
## Pytanie 7 — Co się dzieje, gdy jedyna osoba techniczna w firmie jest niedostępna

**Pilność: POTRZEBNE PRZED ETAPEM MVP2**

### SYTUACJA
Cały system — hasła dostępowe, klucze do systemów firmowych, prawo do
natychmiastowego odcięcia komuś dostępu — dziś opiera się na jednej osobie
technicznej. Nie ma zapisanej żadnej zapasowej drogi na wypadek jej
nieobecności.

### PROBLEM
Jeśli ta osoba zachoruje, wyjedzie na urlop albo odejdzie z firmy w trudnych
okolicznościach — nikt inny nie odetnie od razu dostępu zwalnianemu
pracownikowi. Nikt też nie zatwierdzi pilnej operacji, która wymaga zgody
człowieka. To ryzyko rośnie z każdym kolejnym etapem, bo system obsługuje
coraz więcej realnych spraw firmowych.

### MOŻLIWE ROZWIĄZANIA

**WARIANT A — druga osoba z zapasowym dostępem**
Na czym polega: wskazujemy drugą osobę (np. Pana/Panią albo inną zaufaną
osobę), która ma zapisane, zabezpieczone hasła na wypadek awarii, w
bezpiecznym miejscu.
Co zyskujesz: ciągłość działania firmy nawet przy nagłej nieobecności
jedynej osoby technicznej.
Co tracisz: dodatkowa osoba ma dostęp do wrażliwych danych — większe grono
do pilnowania.
Ile to kosztuje: **szacunek** — kilka godzin pracy na przygotowanie
procedury, bez kosztu bieżącego.
Ryzyko: zapasowy dostęp używany rzadko może się zdezaktualizować. Co wtedy
zrobimy: coroczne sprawdzenie, czy faktycznie działa.

**WARIANT B — świadome przyjęcie ryzyka, nic nie zmieniamy**
Na czym polega: przy nieobecności jedynej osoby technicznej system po
prostu stoi, dopóki ona nie wróci.
Co zyskujesz: brak dodatkowej pracy teraz.
Co tracisz: brak jakiejkolwiek ochrony przed dłuższą, nieplanowaną
nieobecnością.
Ile to kosztuje: brak kosztu teraz.
Ryzyko: nagła choroba albo odejście zatrzymuje firmę w krytycznym momencie.
Co wtedy zrobimy: nic — to jest właśnie ryzyko tego wariantu.

**WARIANT C — zewnętrzna firma informatyczna na wezwanie**
Na czym polega: podpisujemy umowę z zewnętrzną firmą, która w nagłym
wypadku przejmie dostęp awaryjny.
Co zyskujesz: profesjonalne wsparcie bez angażowania osób prywatnych.
Co tracisz: koszt stałej gotowości takiej firmy, czas na wdrożenie jej
w specyfikę naszego systemu.
Ile to kosztuje: **szacunek** — kilkaset do kilku tysięcy złotych
miesięcznie za samą gotowość.
Ryzyko: firma zewnętrzna nie zna specyfiki systemu na tyle, by szybko
zareagować. Co wtedy zrobimy: umowa z jasną procedurą i okresowym
przeglądem.

### PYTANIE
Kto ma mieć zapasowy dostęp do systemu, gdy jedyna osoba techniczna jest
niedostępna — druga zaufana osoba, nikt, czy zewnętrzna firma na wezwanie?

### MOJA REKOMENDACJA
Wariant A — najniższy koszt przy realnej ochronie przed najbardziej
prawdopodobnym scenariuszem. To rekomendacja, nie decyzja.

---
END_LITERAL_SOURCE:Q-07

### Literalne pytanie `Q-08` — zakres źródłowy

SOURCE_REF: `SRC-03`
SOURCE_PATH: `docs/process/pytania/2026-08-25-wybory.md`
SOURCE_STATUS: `CANONICAL`
SOURCE_LOCATOR: `docs/process/pytania/2026-08-25-wybory.md:604-676` (## Pytanie 8 — Gdzie ma być przechowywana wspólna wiedza, którą agenci zapamiętują z rozmów)
CONTENT_STATUS: `OPEN_QUESTION; RECOMMENDATIONS_NOT_DECISIONS`
LITERAL_SHA256: `5a7447af9a3cf5105d75a68682665f61f5f15d14a2177d82e8a67ddbb0408c59`
BEGIN_LITERAL_SOURCE:Q-08
## Pytanie 8 — Gdzie ma być przechowywana wspólna wiedza, którą agenci zapamiętują z rozmów

**Pilność: POTRZEBNE PRZED ETAPEM MVP3**

### SYTUACJA
Dziś każdy agent (program obsługujący jedną dziedzinę firmy, np.
księgowość) pamięta wyłącznie to, co ktoś na stałe zapisał mu w plikach.
Nie ma jeszcze miejsca, w którym agenci mogliby zapamiętywać wnioski
z bieżących rozmów i dzielić się nimi między sobą.

### PROBLEM
Trzeci etap projektu ma dać agentom taką wspólną pamięć — żeby np. agent
księgowości zapamiętał ustalenie z jednej rozmowy i wykorzystał je
w kolejnej, także z inną osobą. Bez wskazania, gdzie fizycznie ta pamięć ma
leżeć, trzeci etap **w ogóle nie może wystartować** — to jedyna twarda
blokada w całym projekcie. Pierwsze dwa etapy działają bez tej decyzji,
więc nie pali się to dziś ani jutro. Ale bez odpowiedzi, zanim dojdzie do
trzeciego etapu, praca stanie w miejscu na wiele dni.

### MOŻLIWE ROZWIĄZANIA

**WARIANT A — własny serwer firmy**
Na czym polega: pamięć agentów trzymamy na komputerze, który należy do
NASTER i stoi fizycznie u nas albo w wynajętej przez nas serwerowni.
Co zyskujesz: pełną kontrolę nad danymi, nikt z zewnątrz nie ma do nich
dostępu bez naszej zgody.
Co tracisz: to my odpowiadamy za awarie, kopie zapasowe i bezpieczeństwo
tego sprzętu — dodatkowy obowiązek dla jedynej osoby technicznej.
Ile to kosztuje: **szacunek** — kilka tysięcy złotych rocznie za sprzęt
i miejsce, plus czas pracy osoby technicznej na utrzymanie. Dokładnej kwoty
nie znamy.
Ryzyko: awaria bez zapasowej kopii w innym miejscu może oznaczać utratę
danych. Co wtedy zrobimy: trzymać kopię zapasową także poza tym samym
miejscem.

**WARIANT B — zewnętrzny dostawca z podpisaną umową o ochronie danych**
Na czym polega: pamięć trzymamy u zewnętrznej firmy specjalizującej się
w takich usługach, z podpisaną umową zobowiązującą ją do ochrony danych
zgodnie z prawem.
Co zyskujesz: mniej pracy utrzymaniowej dla naszej jedynej osoby
technicznej, rzadsze awarie.
Co tracisz: dane firmowe leżą fizycznie poza naszymi ścianami, zależymy od
warunków i cen tego dostawcy.
Ile to kosztuje: **szacunek** — opłata miesięczna zależna od ilości danych,
prawdopodobnie kilkaset złotych miesięcznie na start.
Ryzyko: dostawca zmienia warunki, podnosi cenę albo znika z rynku. Co
wtedy zrobimy: wybrać dostawcę z dobrą renomą i zapisać w umowie prawo do
zabrania danych w każdej chwili.

**WARIANT C — rezygnacja ze wspólnej pamięci, tylko wiedza zapisana ręcznie**
Na czym polega: agenci nie zapamiętują nic sami z rozmów — cała ich wiedza
to wyłącznie to, co ktoś ręcznie zapisze w dokumentach firmowych.
Co zyskujesz: zero nowego ryzyka związanego z przechowywaniem danych, brak
nowego kosztu utrzymania.
Co tracisz: agenci nie uczą się z bieżącej pracy — każde ustalenie trzeba
ręcznie dopisać do dokumentu, żeby agent je „znał" następnym razem.
Ile to kosztuje: brak kosztu technicznego, ale realny koszt czasu ludzkiego
na ręczne aktualizacje.
Ryzyko: agenci sprawiają wrażenie „zapominających", co może zniechęcić
pracowników. Co wtedy zrobimy: przy tym wariancie trzeci etap trzeba
częściowo przeprojektować.

### PYTANIE
Gdzie ma fizycznie leżeć pamięć, którą agenci będą dzielić między sobą — u
nas, u zewnętrznego dostawcy, czy rezygnujemy z niej na razie?

### MOJA REKOMENDACJA
Wariant B jako rozwiązanie startowe — najmniej obciąża jedyną osobę
techniczną, a ryzyko zmiany dostawcy da się ograniczyć umową. To jednak
rekomendacja, nie decyzja.

---
END_LITERAL_SOURCE:Q-08

### Zakres źródłowy poza Q-01…Q-08 — statusy, nie nowe decyzje

SOURCE_REF: `SRC-03`
SOURCE_PATH: `docs/process/pytania/2026-08-25-wybory.md`
SOURCE_STATUS: `CANONICAL_SOURCE_BUT_NOT_DECISION`
SOURCE_LOCATOR: `docs/process/pytania/2026-08-25-wybory.md:677-743`
CONTENT_STATUS: `DEFERRED_OR_REJECTED_OR_TECHNICAL_CONTEXT; NOT_A_NEW_OWNER_DECISION`
LITERAL_SHA256: `3f1d0982016ea1d79c0f77ae36086a3f276c1eb4caaf1971bb7d15dac428b780`
BEGIN_LITERAL_SOURCE:NON_QUESTION_STATUS
## Sprawy, które nie trafiają do Pana/Pani jako pytanie

Poniżej to, czego Pan/Pani nie musi rozstrzygać teraz — i dlaczego.

### Informacyjnie, bez pytania — już wcześniej odłożone, nadal aktualne
Wcześniej odłożono świadomie jedno pytanie. Czy budujemy 27 osobnych,
niezależnych programów-agentów, czy sześć głównych agentów domenowych?
Każdy z tych sześciu korzystałby z trzech osobnych zasobów wiedzy — firmy,
zespołu i pojedynczej osoby. Postanowiono poczekać z odpowiedzią na dane z pierwszego etapu — jak
agenci faktycznie są używani w praktyce. To odłożenie nadal ma sens:
pierwszy etap jeszcze się nie zaczął, więc danych z realnego użycia po
prostu jeszcze nie ma. Warto tylko wiedzieć, że opis drugiego i trzeciego
etapu już dziś po cichu zakłada wariant „sześć agentów". To nie jest błąd —
to coś, co formalna decyzja powinna w swoim czasie potwierdzić albo
zmienić.

### Odkładamy na później — realne wybory, ale nic dziś nie blokują
- **Czy niewykorzystany miesięczny budżet agenta przepada, czy się
  kumuluje.** Realna decyzja o pieniądzach, ale dotyczy dopiero drugiego
  etapu. Startujemy z założeniem „przepada co miesiąc" i wracamy do tego po
  pierwszym miesiącu realnych danych o zużyciu.
- **Czy wystarczą dwa poziomy uprawnień (zwykły pracownik / właściciel), czy
  potrzebny trzeci, pośredni poziom (np. kierownik zespołu).** Nic dziś tego
  nie wymaga przy jednym agencie i garstce osób w pilocie. Wracamy do tego,
  gdy liczba agentów i zespołów faktycznie urośnie.

### Odrzucamy z uzasadnieniem
- **Panel z dokumentem do zatwierdzenia obok rozmowy z agentem.** Konkurent
  appto ma taki ekran, my dziś nie. Odrzucamy budowę takiego ekranu jako
  osobnej rzeczy. Drugi etap projektu ma już mechanizm „operacja
  nieodwracalna czeka na zgodę człowieka" — załatwia ten sam problem bez
  kopiowania cudzego wyglądu. Jeśli w praktyce zabraknie tego ekranu, wróci
  jako konkretna, nazwana potrzeba, nie jako gonienie funkcji konkurenta.
- **Obecność w komunikatorze Slack obok Teams.** Już wcześniej świadomie
  odrzucone — firma pracuje w ekosystemie Microsoft (logowanie, komunikator
  Teams), drugi komunikator obok byłby zbędnym, drugim narzędziem do
  utrzymania. Nie ma dziś sygnału, żeby to podważyć.
- **Poprawka uzasadnienia jednej z już przyjętych decyzji** (o budowie
  własnej platformy zamiast zakupu gotowej). To zadanie wprost zastrzeżone
  jako osobny temat, poza tym zestawieniem — nie ruszamy przyjętych decyzji
  w tym dokumencie.

### Rozstrzygamy sami — techniczne, bez odczuwalnego skutku dla firmy
- **Czy pracownicy rozmawiają z agentem przez przeglądarkę, czy przez
  Teams.** To już jest ustalone w planie: najpierw przeglądarka (pierwszy
  etap), Teams dochodzi w trzecim etapie. Chodzi o to, żeby najpierw
  sprawdzić, czy agenci w ogóle dobrze działają, zanim dołoży się drugi
  sposób rozmowy.
- **Czy logowanie firmowym kontem ma włączone potwierdzenie drugim
  urządzeniem (tzw. dwuskładnikowe logowanie).** To sprawdzenie ustawienia
  już istniejącego systemu firmowego, nie nowa decyzja — potwierdzimy stan
  faktyczny i zaproponujemy włączenie, jeśli jest wyłączone. To nie wymaga
  osobnego pytania, ale zostanie zapisane jako drobna decyzja po fakcie.
- **Czy potrzebna osobna aplikacja na telefon, czy wystarczy przeglądarka
  dostosowana do telefonu.** Zaczynamy od przeglądarki — tańsze, szybsze,
  bez utraty niczego istotnego na start. Wracamy do tematu, jeśli pracownicy
  faktycznie zgłoszą taką potrzebę.
- **Szczegóły techniczne wyboru z pytania 1** (konkretny rodzaj wynajmu
  serwera) **i z pytań 4–5** (konkretny sposób trzymania haseł dostępowych,
  dobór biblioteki łączącej) — po Pana/Pani decyzji resztę dobiera osoba
  techniczna, bo to już nie zmienia kosztu ani ryzyka dla firmy, tylko
  sposób wykonania.

---

*Pełne uzasadnienia, wyliczenia i odesłania do dokumentacji — w
`docs/nota-08-wybory-otwarte.md`.*
END_LITERAL_SOURCE:NON_QUESTION_STATUS

## 4. `DEC-04` — historia wariantów i korekt

SOURCE_PATH: `docs/nota-08-wybory-otwarte.md`
SOURCE_SHA256: `b05ce062506e6ac5d50309c504a6bf0a32f5257e925c1b78ad27d3ccb86625d9`
SOURCE_STATUS: `HISTORY` (P4 `NAG-RESEARCH`, rodzina `PF-0206`)
SOURCE_OF_TRUTH: `false` dla decyzji i bieżącego routingu
SOURCE_LOCATOR: wszystkie sekcje `##` wyszczególnione w `coverage.json`
SOURCE_LINK: [`docs/nota-08-wybory-otwarte.md`](../../../nota-08-wybory-otwarte.md)

Nota zachowuje tok rozumowania, warianty, ceny/szacunkowe koszty, ryzyka,
korekty redakcyjne i wyjaśnienia statusu pytań. Nie wybiera wariantu za
właściciela. Szczególnie: rekomendacje pozostają rekomendacjami, informacja
o D-010 nie zamyka D-010, a korekta pilności Q-04 nie tworzy decyzji.

| Zakres historii | Status | Traktowanie | Locator |
|---|---|---|---|
| `H-01` ## Co jest pracą Hermesa, a co naszą — ustalenie dla pytań 4 i 5 | `HISTORY` | `HISTORY_ONLY; nie norma` | `docs/nota-08-wybory-otwarte.md:15-106` |
| `H-02` ## Pytanie 1 — Gdzie ma fizycznie stać komputer, na którym działa system | `HISTORY` | `HISTORY_ONLY; nie norma` | `docs/nota-08-wybory-otwarte.md:107-126` |
| `H-03` ## Pytanie 2 — Czy potrzebujemy prawnika przed testem na prawdziwych danych | `HISTORY` | `HISTORY_ONLY; nie norma` | `docs/nota-08-wybory-otwarte.md:127-152` |
| `H-04` ## Pytanie 3 — Retencja rozmów pracowników z agentami | `HISTORY` | `HISTORY_ONLY; nie norma` | `docs/nota-08-wybory-otwarte.md:153-175` |
| `H-05` ## Pytanie 4 — Sposób łączenia agenta z programami firmy | `HISTORY` | `HISTORY_ONLY; nie norma` | `docs/nota-08-wybory-otwarte.md:176-211` |
| `H-06` ## Pytanie 5 — Skala: ile programów planujemy naraz | `HISTORY` | `HISTORY_ONLY; nie norma` | `docs/nota-08-wybory-otwarte.md:212-226` |
| `H-07` ## Pytanie 6 — Kto potwierdza zamknięcie etapu | `HISTORY` | `HISTORY_ONLY; nie norma` | `docs/nota-08-wybory-otwarte.md:227-242` |
| `H-08` ## Pytanie 7 — Ciągłość działania bez jedynej osoby technicznej | `HISTORY` | `HISTORY_ONLY; nie norma` | `docs/nota-08-wybory-otwarte.md:243-258` |
| `H-09` ## Pytanie 8 — Rezydencja wspólnej pamięci (D-011) | `HISTORY` | `HISTORY_ONLY; nie norma` | `docs/nota-08-wybory-otwarte.md:259-277` |
| `H-10` ## Informacja (nie pytanie) — D-010, topologia agentów | `HISTORY` | `HISTORY_ONLY; nie norma` | `docs/nota-08-wybory-otwarte.md:278-298` |
| `H-11` ## Sprawy odłożone, odrzucone, rozstrzygane samodzielnie | `HISTORY` | `HISTORY_ONLY; nie norma` | `docs/nota-08-wybory-otwarte.md:299-329` |
| `H-12` ## Uwagi redakcyjne — co poprawiono względem wersji roboczej | `HISTORY` | `HISTORY_ONLY; nie norma` | `docs/nota-08-wybory-otwarte.md:330-347` |

Poniższy archiwalny zakres zachowuje pełny tekst noty. Każde zdanie w tym
zakresie dziedziczy `SOURCE_STATUS: HISTORY`; nie może nadpisać DEC-01, DEC-02
ani statusów DEC-03.

SOURCE_REF: `SRC-04`
SOURCE_PATH: `docs/nota-08-wybory-otwarte.md`
SOURCE_STATUS: `HISTORY`
SOURCE_SHA256: `b05ce062506e6ac5d50309c504a6bf0a32f5257e925c1b78ad27d3ccb86625d9`
LITERAL_SHA256: `d6d9d05241d6babdef65739514830a41538b77c4a4de4de70cfd40105e2c546b` (zakres bez końcowego znaku nowej linii)
BEGIN_LITERAL_SOURCE:HISTORY_NOTE_FULL
# Nota 08 — wybory otwarte: zaplecze merytoryczne

NASTER · projekt 8gent · temat `NAG-DEC-001-wybory-otwarte` · 25 sierpnia 2026

Ten dokument jest zapleczem dla `docs/process/pytania/2026-08-25-wybory.md`
— zestawu ośmiu pytań, które trafiają do właściciela. Tu jest to samo, ale
rozwinięte: pełne wyliczenia, przesłanki, odesłania do miejsc w dokumentacji,
i jedna dodatkowa rzecz, której w dokumencie dla właściciela nie ma — analiza
podziału pracy między Hermesem a nami w sprawie integracji.

Numeracja pytań poniżej odpowiada 1:1 numeracji w dokumencie dla właściciela.

---

## Co jest pracą Hermesa, a co naszą — ustalenie dla pytań 4 i 5

To ustalenie stoi za pytaniami 4 i 5 (integracje) i nie jest samo w sobie
pytaniem do właściciela — jest granicą, którą trzeba było najpierw wytyczyć,
żeby wiedzieć, o co w ogóle pytać.

**Źródła sprawdzone:** `docs/spec/00-architektura.md` §1 (Cel systemu) i §4
(Warstwy); `docs/nota-07-katalog-funkcji.md` §3 pkt 6 i §7 (Integracje —
warianty podejścia do 956 narzędzi); `docs/spec/01-mvp1.md` §7 (rejestr
agentów, pole `secrets`/`vault_ref`); `docs/spec/04-mvp4.md` §1 (konektory);
`docs/spec/scenarios.md` (scenariusze R1–R7).

**Ustalenie:** `00-architektura.md` §1 mówi wprost — „8gent to warstwa
zarządzania nad flotą instancji Hermesa. Nie jest silnikiem agenta — Hermes
nim jest (…) wykonuje całą pracę: rozmowę, narzędzia, piaskownicę, pamięć,
kanały, wybór modelu." Samo łączenie się z programem firmowym i wykonywanie
w nim czynności (np. zapisanie wyniku, wysłanie maila) jest więc **pracą
Hermesa**, nie naszą. `nota-07` §7 to potwierdza od strony konkurenta:
appto rozwiązuje ten sam problem, kupując dostęp do gotowego pośrednika —
czyli traktuje to jako osobną warstwę wykonawczą, nie jako część własnej
warstwy zarządzania.

**Nasza praca jest węższa, ale realna i dziś nierozstrzygnięta.** Cztery
rzeczy, wszystkie mieszczące się w czterech pytaniach 8gent z
`00-architektura.md` §1 („kto to jest", „do czego ma prawo", „ile mu wolno
wydać", „co po sobie zostawił"):

1. Gdzie trzymamy hasła i klucze dostępu do programów firmowych, żeby agent
   mógł z nich skorzystać, nie mając ich fizycznie u siebie. Dokumentacja
   przewiduje samo miejsce na to — pole `vault_ref` w `01-mvp1.md` §7 — ale
   nie mówi, co konkretnie ma tam siedzieć naprawdę. To pytanie 4.
2. Które programy firmowe w ogóle podłączamy, i w jakiej kolejności. To
   pytanie 5.
3. Co się dzieje, gdy program firmowy nie mówi otwartym, powszechnym
   językiem integracji (patrz wariant C niżej), tylko wymaga osobnego,
   ręcznie napisanego połączenia — kto to pisze i utrzymuje. Rozstrzygane
   pytaniem 4 jako efekt uboczny wyboru wariantu.
4. Kto sprawdza, zanim agent coś zrobi w programie firmowym, czy w ogóle
   wolno mu to zrobić, i czy zostaje z tego ślad w dzienniku zdarzeń. To już
   jest rozstrzygnięte niezależnie od integracji — mechanizm nadań i audytu
   z `00-architektura.md` §6 (model bezpieczeństwa) obejmuje każdą operację
   agenta, niezależnie od tego, którym wariantem z pytania 4 się połączył.

Pytania 4 i 5 dotyczą wyłącznie tych czterech spraw — nie proszą o decyzję
w rzeczach, które i tak wykonuje Hermes.

**Skala problemu jest mniejsza, niż sugeruje porównanie z appto.** appto
reklamuje się liczbą 956 programów, do których potrafi się podłączyć.
Sprawdzenie `docs/spec/scenarios.md` i czterech etapów budowy pokazuje, ile
programów firmowych NASTER faktycznie wymienia z nazwy: **jeden**, z
pewnością — wewnętrzny program do rozliczeń prowizji. `docs/nota-07-katalog-
funkcji.md` wspomina dodatkowo nazwę jednego konkretnego programu księgowego
jako przykład możliwego kolejnego podłączenia, zaznaczoną wprost jako
niepewną („może"). Ostatni etap budowy (MVP4, za kilkanaście miesięcy)
wspomina ogólnie o kategoriach — program do rozliczeń, system ERP, poczta,
dysk firmowy — bez podania żadnej konkretnej nazwy produktu. Różnica skali
między appto a nami nie jest różnicą w wykonaniu, tylko w samym zamówieniu:
nikt w tej firmie nie potrzebuje dziś 956 połączeń, tylko jednego, może
dwóch–trzech w ciągu najbliższego roku.

### Do zweryfikowania — status „programu rozliczeniowego" dziś

Jedna rzecz wymaga jawnego odnotowania, bo inaczej pytanie 4 mogłoby wywołać
konsternację u właściciela („o jakim programie mowa?").

`docs/spec/scenarios.md`, sekcja „Rozliczenia — proces pilotażowy":
scenariusze R1–R4 (kryterium biznesowe MVP1, `NAG-MVP1-010`) opisują pracę na
pliku Excel — „podkładka pod fakturę", „usunięte pozycje trafiają do
osobnego arkusza", „trzy zamknięte miesiące przeliczone (…) zgadzają się z
liczeniem ręcznym". Skille już obecne w projekcie (`podkladka-audyt`,
`weryfikacja-korekt-rozliczenia`) potwierdzają to samo: operują na plikach
`.xlsx`, nie na koncie w zewnętrznym systemie.

Jednocześnie `docs/spec/01-mvp1.md` §7 deklaruje w rejestrze agenta
księgowości pole `secrets` z wpisem `SYSTEM_ROZLICZEN_TOKEN` już od
pierwszego etapu — ale nigdzie w opisie przepływu MVP1 (§8) ten sekret nie
jest faktycznie wywoływany. To jest zarezerwowane miejsce na przyszłość, nie
opis dzisiejszego stanu.

Dowód, że przyszłość jest jednak bliska: `scenarios.md` R6 („Zapis wyniku do
systemu wstrzymany do zatwierdzenia przez człowieka", MVP2) i R7 („Nowa
korekta w systemie wyzwala weryfikację automatycznie", MVP3) opisują
faktyczny zapis do systemu i sygnał przychodzący z systemu — czyli żywe
połączenie, nie plik. **Wniosek:** dziś (MVP1) program rozliczeniowy to plik
Excel; pytanie 4 dotyczy decyzji potrzebnej, zanim w MVP2 trzeba będzie
zapisywać wynik wprost do systemu, a nie tylko czytać plik. Dlatego pytanie
ma pilność „potrzebne przed etapem MVP2", nie „blokuje teraz" — w wersji
roboczej tego dokumentu było to sformułowane zbyt ostro, jakby dotyczyło już
samego testu kończącego MVP1. Poprawione w obu dokumentach.

---

## Pytanie 1 — Gdzie ma fizycznie stać komputer, na którym działa system

Źródła: `docs/spec/01-mvp1.md` (wymagania sprzętowe startowe), `docs/spec/
00-architektura.md` §7 (stos technologiczny) i §8 (środowiska). Specyfikacja
nie rozstrzyga lokalizacji ani formy własności serwera — to luka
potwierdzona bezpośrednim przeglądem, nie domysł.

Prawna waga lokalizacji: dane osobowe pracowników (od MVP1 — logowanie przez
Entra ID) i klientów (test na prawdziwym rozliczeniu, koniec MVP1) trafiają
na ten komputer. RODO różnicuje wymagania w zależności od tego, czy dane
zostają w Unii Europejskiej, czy są przekazywane poza nią — stąd wariant C
(najtańsza oferta bez względu na kraj) niesie realne ryzyko prawne, nie tylko
wizerunkowe.

Warianty, ceny i ryzyka — jak w dokumencie dla właściciela, bez zmian
merytorycznych względem wersji roboczej. Rekomendacja: wariant B (wynajem w
UE) — równowaga między obciążeniem jedynej osoby technicznej a kosztem.

---

## Pytanie 2 — Czy potrzebujemy prawnika przed testem na prawdziwych danych

Źródła: `docs/nota-07-katalog-funkcji.md` §8 (czego dokumenty prawne appto
uczą nas o naszej własnej budowie), w szczególności §8b — „Obowiązki, które
mamy już dziś — przed drugim najemcą": obowiązek informacyjny wobec
pracowników (art. 13 RODO, **wymaga potwierdzenia prawnika**), podstawa
prawna przetwarzania logów, umowa powierzenia z dostawcą modelu (D-009 już
nazywa ten obowiązek, ale nie mówi „kiedy" ma być podpisany). §8d proponuje
wprost: „Umowa powierzenia z dostawcą modelu — przed albo równolegle z MVP1,
nie czekać do MVP4, skoro bramka modeli działa od pierwszego etapu."

`docs/spec/scenarios.md` R8 (nazwane w nocie 07 jako „Scenariusz 8") i
harmonogram MVP1 (`docs/process/tematy.md`, `NAG-MVP1-010`) umieszczają test
na prawdziwym rozliczeniu pod koniec pierwszego etapu — tygodnie 11–12 w
tabeli etapów `01-mvp1.md`. Procesy prawne (przegląd umowy z dostawcą
modeli, przygotowanie informacji dla pracowników) mają zwyczajowo tygodnie
realizacji, więc zlecenie ich teraz, równolegle z pisaniem kodu, jest jedyną
drogą, która nie opóźnia zamknięcia etapu.

Nie otwieramy tu ponownie D-009 — pytanie 2 dotyczy **terminu i zakresu
operacyjnego** wykonania obowiązku, który D-009 już nazwał, nie samej zasady.

Rekomendacja: wariant A, równolegle z kodem.

---

## Pytanie 3 — Retencja rozmów pracowników z agentami

Źródła: `docs/nota-07-katalog-funkcji.md` §8b — „retencja audytu — dziś
«bezterminowo» w MVP1, «konfigurowalna per najemca» docelowo. Pytanie do
prawnika, nie ustalenie: czy retencja bezterminowa daje się uzasadnić zasadą
minimalizacji RODO, czy wymaga rewizji przed MVP2/3." `docs/spec/04-mvp4.md`
§7 pkt 6 („Retencja i eksport") zakłada już mechanizm retencji, ale dopiero
w czwartym etapie — pytanie 3 pyta, czy zasadę **ustalamy świadomie już
teraz**, zamiast dryfować do MVP4 z faktycznym „bezterminowo" w tle.

Powiązanie z pytaniem 2: to samo źródło ryzyka (dane pracowników i klientów
w rozmowach z agentem), ten sam adresat rozstrzygnięcia (prawnik), zasadne
połączenie terminu. W wersji roboczej tego pytania (plik roboczy
`wybory-pozostale.md`) problem nie wskazywał wprost momentu, w którym brak
decyzji zaczyna szkodzić — poprawione: ryzyko rośnie z każdym miesiącem
zbierania danych bez ustalonej zasady, a decyzja powinna zapaść razem
z pytaniem 2, przed testem na prawdziwych danych klientów.

Rekomendacja: wariant A, 12 miesięcy dla treści, dłużej dla dziennika
zdarzeń — zgodne z tym, co MVP4 już zakłada.

---

## Pytanie 4 — Sposób łączenia agenta z programami firmy

Pełna analiza w sekcji „Co jest pracą Hermesa, a co naszą" powyżej. Warianty
A–D, ceny, ryzyka — jak w dokumencie dla właściciela. Dwie bariery
architektury sprawdzone jawnie przy każdym wariancie, zgodnie z wymogiem
dispatchu:

- **Bariera 2** (agent stanowiskowy bez własnych poświadczeń do systemów
  firmowych) — żaden wariant jej nie łamie wprost. Wariant B ją **osłabia**
  (hasła trafiają też do systemu firmy trzeciej) — nazwane wprost jako powód
  do ostrożności, nie jako złamanie bariery, bo hasło nadal nie jest
  przypisane pojedynczemu pracownikowi.
- **Bariera 4** (domyślna odmowa, nie „wszyscy mogą, chyba że") — pole „jak
  szerokie uprawnienia to wymaga" przy każdym wariancie odpowiada wprost:
  wariant A i C dają się ograniczyć do jednej czynności; wariant B strukturalnie
  wymaga szerokiego dostępu, bo pośrednik nie wie z góry, czego agent
  zażąda — to jest koszt wariantu B ujawniony, nie przemilczany.

Poprawki językowe względem wersji roboczej (`wybor-integracje.md`): słowo
„warstwa" zastąpione opisowo tam, gdzie padało bez wyjaśnienia (Wariant B:
„cała nasza warstwa łączenia się z programami" → „to wszystko, czym łączymy
się z programami firmowymi"); zdanie otwierające ustalenie wstępne rozbite z
jednego 38-słowowego na trzy krótsze; zdanie w PROBLEM („jeśli wybierzemy
źle, ryzykujemy…") rozbite na dwa; analogiczna poprawka w Wariancie B
(„rosnący rachunek miesięczny… i to jest dokładnie ten sam rodzaj
uzależnienia…") oraz w Wariancie C (opis standardu otwartego, opis ryzyka
odejścia społeczności) — wszystkie rozbite na krótsze zdania, jedna myśl na
zdanie, zgodnie z wymogiem zlecenia.

Rekomendacja: wariant D (mieszanka). Wariant B odradzany jawnie — odtwarza
mechanizm ryzyka z D-001 (odrzucenie zakupu appto), jeden poziom niżej; to
samo ryzyko nazwane wprost w `nota-07-katalog-funkcji.md` §7, wariant B tejże
noty.

---

## Pytanie 5 — Skala: ile programów planujemy naraz

Patrz sekcja „Co jest pracą Hermesa, a co naszą" wyżej — ustalenie skali
(jeden program pewny, jeden „może", reszta bez nazwy) stoi za tym pytaniem.
Warianty A–C bez zmian merytorycznych względem wersji roboczej. Rekomendacja:
wariant C — czekamy na nazwaną potrzebę.

Nota dla porządku: pytania 4 i 5 są ze sobą powiązane — wybór skali (5)
ogranicza sensowność budowania „zapasu" w wariancie 4. Jeśli właściciel
wybierze w pytaniu 5 wariant inny niż C (np. B — zapas na 3–5 programów),
warto przy zapisie odpowiedzi w `docs/spec/decisions.md` odnotować obie
odpowiedzi razem, bo mogą się wzajemnie ograniczać.

---

## Pytanie 6 — Kto potwierdza zamknięcie etapu

Źródła: `docs/process/tematy.md` (`NAG-MVP1-010` jako „kryterium biznesowe
całego etapu"), `.claude/skills/nagents-autobot/SKILL.md` §6 (Kryteria
końca) — proces AutoBot rozróżnia zamknięcie techniczne (testy) od
zamknięcia biznesowego, ale nie mówi, kto reprezentuje stronę biznesową przy
tym konkretnym projekcie z jedną osobą decyzyjną. To pytanie wypełnia tę
lukę dla 8gent.

Warianty A–C, ceny, ryzyka — jak w dokumencie dla właściciela. Rekomendacja:
wariant A dla testu kluczowego (R1–R4/R8), reszta testów technicznych
zamyka się bez udziału właściciela — spójne z rozróżnieniem w skilli
procesowej.

---

## Pytanie 7 — Ciągłość działania bez jedynej osoby technicznej

Źródła: `docs/spec/00-architektura.md` §6.5 (operacja nieodwracalna wymaga
człowieka) i §6.3 (klucze idą w dół hierarchii, nie w bok) — oba mechanizmy
zakładają, że **ktoś** jest dostępny do zatwierdzenia albo odcięcia dostępu.
Żaden zapis w architekturze nie wskazuje zastępcy, gdy tym „kimś" jest
jedyna osoba techniczna w firmie dwudziestoosobowej. Ryzyko rośnie wprost
proporcjonalnie do liczby realnych spraw firmowych obsługiwanych przez
system — a ta rośnie z każdym kolejnym etapem (MVP1 → MVP4).

Warianty A–C, ceny, ryzyka — jak w dokumencie dla właściciela. Rekomendacja:
wariant A (druga osoba z zapasowym dostępem) — najniższy koszt przy realnej
ochronie.

---

## Pytanie 8 — Rezydencja wspólnej pamięci (D-011)

Źródła: `docs/spec/decisions.md` (D-011, zapisana jako otwarta i blokująca
MVP3), `docs/process/tematy.md` (`NAG-MVP3-001-pamiec-wspolna`, w sekcji
Zablokowane — „Blokada: D-011 — nierozstrzygnięta rezydencja danych").
`docs/spec/03-mvp3.md` opisuje mechanizm wspólnej pamięci (wiedza firmy,
zespołu, osoby, z dziedziczeniem) zakładając, że rezydencja jest już
rozstrzygnięta — nie rozstrzyga jej sama.

To jedyne z ośmiu pytań, które formalnie odpowiada już istniejącej,
zarejestrowanej decyzji otwartej (D-011), nie nowo odkrytej luce. Pozostałe
siedem nie miało wcześniej wpisu w `decisions.md` w ogóle.

Warianty A–C, ceny, ryzyka — jak w dokumencie dla właściciela. Rekomendacja:
wariant B (zewnętrzny dostawca z umową) — najmniej obciąża jedyną osobę
techniczną.

---

## Informacja (nie pytanie) — D-010, topologia agentów

Wcześniej odłożono świadomie: 27 osobnych agentów-programów, czy sześć
agentów domenowych z trzema oddzielnymi zasobami wiedzy (firma, zespół,
osoba), z których każdy agent czerpie. `docs/spec/decisions.md` zapisuje to
jako D-010, odroczoną świadomie do danych z MVP1 — to odłożenie nadal ma
sens, bo MVP1 się jeszcze nie zaczęło.

Odnotowania warte: `docs/spec/02-mvp2.md` i `03-mvp3.md` już dziś po cichu
zakładają wariant „sześć agentów" — opisują agentów stanowiskowych jako
podpięte do agenta domenowego, korzystające z trzech zasobów wiedzy z
dziedziczeniem. To nie jest sprzeczność z odroczeniem D-010 — po prostu
część pracy w drugim i trzecim etapie idzie już w jednym konkretnym
kierunku, zanim decyzja formalnie zapadnie. Poprawka językowa względem
wersji roboczej: słowo „warstwy" (wiedzy) zastąpione opisowo — „trzy osobne
zasoby wiedzy: firmy, zespołu i pojedynczej osoby" — bo padało bez
wyjaśnienia przy drugim użyciu w tym samym akapicie, mimo że pierwsze użycie
miało wyjaśnienie w nawiasie.

---

## Sprawy odłożone, odrzucone, rozstrzygane samodzielnie

Pełne uzasadnienia identyczne z sekcją zamykającą dokument dla właściciela.
Tu tylko odesłania źródłowe:

- **Budżet miesięczny agenta — przepada czy się kumuluje:** decyzja
  dotyczy MVP2 (`docs/spec/02-mvp2.md`), nic w MVP1 tego nie wymusza.
- **Trzeci poziom uprawnień:** `00-architektura.md` §5 (model danych)
  definiuje dziś dwa poziomy; trzeci nie ma dziś nazwanego przypadku użycia.
- **Panel zatwierdzania dokumentu:** `nota-07-katalog-funkcji.md` §4 (luki),
  pozycja o ekranie zatwierdzania — zestawiona z mechanizmem „operacja
  nieodwracalna czeka na zgodę człowieka" z `00-architektura.md` §6.5, który
  pokrywa tę samą potrzebę.
- **Slack obok Teams:** decyzja już przyjęta wcześniej (poza zakresem D-001
  do D-009, ale nieotwierana ponownie tu), `00-architektura.md` §7 (stos)
  zakłada ekosystem Microsoft.
- **Poprawka uzasadnienia D-001:** zastrzeżona jako osobny temat w dispatchu
  `NAG-DEC-001-wybory-otwarte` („Wyjątek: uzasadnienie D-001 czeka na
  poprawkę właściciela, temat osobny") — nieotwierana tutaj.
- **Kanał rozmowy (przeglądarka/Teams), logowanie dwuskładnikowe, aplikacja
  mobilna, szczegóły techniczne serwera i integracji:** decyzje już ujęte w
  planie albo niezmieniające kosztu/ryzyka dla firmy — pełne uzasadnienie w
  dokumencie dla właściciela. Poprawka względem wersji roboczej: pozycja o
  logowaniu dwuskładnikowym wcześniej zapowiadała „włączymy je" z góry, co
  dotyka dostępu i powinno przejść przez zapis decyzji, choćby drobnej —
  poprawiona formuła: „potwierdzimy stan faktyczny i zaproponujemy
  włączenie (…), a to zostanie zapisane jako drobna decyzja po fakcie",
  zamiast zapowiadać wykonanie zmiany z góry.

---

## Uwagi redakcyjne — co poprawiono względem wersji roboczej

Zgodnie z oceną Evaluatora (`PASS-WITH-NOTES`, runda 1), zastosowano:

1. Rozbicie zdań przekraczających limit „dwadzieścia kilka słów" na krótsze,
   po jednej myśli na zdanie — w obu dokumentach źródłowych, nie tylko w
   trzech przykładach wskazanych wprost przez Evaluatora.
2. Usunięcie słowa „warstwa" bez tłumaczenia w trzech miejscach (Wariant B
   pytania 4, informacja o D-010, opis sprawy odrzuconej dot. integracji) —
   zastąpione zwykłym językiem.
3. Poprawkę testu granicy przy logowaniu dwuskładnikowym — z zapowiedzi
   działania z góry na propozycję plus zapis jako drobna decyzja po fakcie.
4. Weryfikację statusu „programu rozliczeniowego" wobec `scenarios.md` —
   opisaną wyżej w sekcji „Do zweryfikowania" — ze zmianą pilności pytania 4
   z nadmiernie ostrej na „potrzebne przed etapem MVP2".

Drobiazg Evaluatora o elemencie czasowym w pytaniu 3 (retencja) również
zastosowany — dodane zdanie o narastaniu ryzyka z każdym miesiącem.
END_LITERAL_SOURCE:HISTORY_NOTE_FULL

## 5. P6, P4 i indeks projektu — proweniencja, nie nowe decyzje

### 5.1 P6 — macierz i bramy

SOURCE_PATH: `NAGENTS-CONSOLIDATION-PLAN.md`
SOURCE_SHA256: `b282d9e49bf77994a290fbfab71803217d02c9565ab8809e6fbf33501ca4b15c`
SOURCE_STATUS: `PLAN_ONLY / OWNER_HOLD_REQUIRED`
SOURCE_LOCATOR: §3 wiersz `NAGENTS-DECISIONS` (linia 82); §4.2 (linie 110–117); §6.4 (linie 264–277); §8–§9 (linie 310–336)
SOURCE_LINK: [`NAGENTS-CONSOLIDATION-PLAN.md`](../../../../NAGENTS-CONSOLIDATION-PLAN.md)

P6 jest podstawą układu `DEC-01`…`DEC-04`, ale nie jest źródłem decyzji
właściciela. Jego owner gates nadal obowiązują. P6 nie nadaje zgody na
scalanie, usuwanie, publikację ani zmianę rangi źródeł.

### 5.2 NAGENTS-PROJECT — indeks kandydujący

SOURCE_PATH: `NAGENTS-PROJECT.md`
SOURCE_SHA256: `84cc8a89c0e7bd53268fc78826938bec062f9223340aa2e0934d64719764a2fb` (bieżący readback)
SOURCE_SIZE_BYTES: `53421`; SOURCE_LINE_COUNT: `819`
SOURCE_SNAPSHOT_SHA256: `bab1666d8528622b055b1a3f0b9e21a918be5efabc5be0af2ac06064ae00236a`
SOURCE_SNAPSHOT_SIZE_BYTES: `53403`; SOURCE_SNAPSHOT_LINE_COUNT: `819`
SOURCE_STATUS: `CONSOLIDATION_CANDIDATE` (P4 `NAG-INDEX`, rodzina `PF-0184`)
SOURCE_OF_TRUTH: `false` dla decyzji; indeks wskazuje, gdzie czytać normę
SOURCE_LOCATOR: §1 (linie 63–80); §2.1 wiersze decyzji (linie 122–123); §4.4 (linie 248–302); §11–§12 (linie 748–819); dokładny drift: `NAGENTS-PROJECT.md:165`
PROVENANCE_STATUS: `DRIFT_RECONCILED / CURRENT_READBACK`
DRIFT_DELTA_BYTES: `+18`
DRIFT_BYTE_LOCATOR: offset `11051` zero-based; bieżący zakres `11051–11212` zero-based inclusive (`11052–11213` one-based inclusive)
DRIFT_SNAPSHOT_LINE: "| 8gent project anchor | `nagents-docs / p_cb0f9def` | `hermes project show nagents-docs`; używać jako `project_id` na wspólnym boardzie |"
DRIFT_CURRENT_LINE: "| 8gent project anchor | `nagents-docs / p_e90c30bc` | `hermes --profile default project show nagents-docs`; używać jako `project_id` na wspólnym boardzie |"
DRIFT_EFFECT: Anchor przechodzi z `p_cb0f9def` i niekwalifikowanego odczytu na `p_e90c30bc` odczytywany jawnie w profilu `default`; treść decyzji, status indeksu i liczniki pakietu nie zmieniają się.
DRIFT_RECONSTRUCTION: Zastąpienie bieżącej linii 165 linią snapshotu odtwarza dokładnie SHA-256 `bab1666d8528622b055b1a3f0b9e21a918be5efabc5be0af2ac06064ae00236a`, rozmiar `53403` B i `819` linii.
SOURCE_LINK: [`NAGENTS-PROJECT.md`](../../../../NAGENTS-PROJECT.md)

Indeks kieruje do `docs/spec/decisions.md` i `docs/process/echo.md` jako
źródeł decyzji oraz wskazuje przyszły pakiet `NAGENTS-DECISIONS.md` jako
kandydata P6. Nie nadaje temu stagingowi rangi normy. P4/P5 odnotowały
wariant snapshotu indeksu; bieżący readback i snapshot są rozdzielone w
ledgerze. Rekonstrukcja linii 165 dowodzi, że różnica ma dokładnie `+18` bajtów
i dotyczy wyłącznie anchoru oraz kwalifikacji profilu odczytu.

### 5.3 P4 — maszynowa proweniencja

SOURCE_PATH: `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json`
SOURCE_SHA256: `95d2a06fc80e4cf1c27d59959d84b8631e70acbb3e0b879b83df95c38bf382b8`
SOURCE_STATUS: `PASS_WITH_EXPLICIT_OWNER_GATES`
SOURCE_OF_TRUTH: `false` dla decyzji właściciela; `true` jako audyt statusów/proweniencji P4
SOURCE_LOCATOR: `logical_group_rollups[group_id=NAG-DECISIONS]`; `path_family_classifications[path_family_id=PF-0241, PF-0219, PF-0225, PF-0206, PF-0184]`
SOURCE_LINK: [`P4-classification.json`](../../../process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json)

P4 zachowuje grupy, rodziny, rekordy, warianty hash i rekomendacje bram.
Nie wybiera zwycięzcy przez podobieństwo ani nie usuwa kopii. `coverage.json`
przechowuje wyłącznie bezpieczny, wybrany zakres proweniencji; pełna
klasyfikacja pozostaje w P4.

## 6. Granice bezpieczeństwa i rollback

- Brak wartości sekretów, tokenów, kluczy, danych uwierzytelniających, PII, runtime, sesji i surowych logów.
- Nazwy pól/sekretów w źródłowej dokumentacji nie są wartościami sekretów; pakiet nie dopisuje żadnej wartości.
- Historia i rekomendacje są oznaczone `HISTORY` / `NOT_A_DECISION`; nie nadają uprawnień i nie zmieniają decyzji.
- Pytania pozostają pytaniami, dopóki właściciel nie zapisze jednoznacznego ECHO/ADR w źródle.
- Rollback to usunięcie tego nowego katalogu stagingowego po osobnej kontroli; źródła pozostają bez zmian.
- `READY_FOR_DEPLOY`, publikacja, push, merge i deploy nie wynikają z tego artefaktu.

## 7. Ledger źródeł (skrót; pełne pola w `coverage.json`)

### SRC-01 — `docs/spec/decisions.md`

PATH: `docs/spec/decisions.md`
SHA256: `57264a14bcf38b38811d42b025aba31359470db6c4e33dbc7abe9cad03681d49`
SIZE_BYTES: `10789`; LINE_COUNT: `264`
STATUS: `CANONICAL`
P4: P4 `PF-0241` / `NAG-SPEC` / `CANONICAL`; rekordów: 12
SOURCE_OF_TRUTH: `true`
LOCATOR: D-001…D-013; §D-010 i §D-011 pozostają otwarte.
LINK: [docs/spec/decisions.md](../../../spec/decisions.md)

### SRC-02 — `docs/process/echo.md`

PATH: `docs/process/echo.md`
SHA256: `e631fb90812066690a1c1a927abb885472157c2607f8c0ba4f2e6ad56996bbde`
SIZE_BYTES: `5334`; LINE_COUNT: `146`
STATUS: `CANONICAL`
P4: P4 `PF-0219` / `NAG-PROCESS` / `CANONICAL`; rekordów: 12
SOURCE_OF_TRUTH: `true`
LOCATOR: §Wpisy; pięć wpisów ###.
LINK: [docs/process/echo.md](../../echo.md)

### SRC-03 — `docs/process/pytania/2026-08-25-wybory.md`

PATH: `docs/process/pytania/2026-08-25-wybory.md`
SHA256: `3e0b66a6157ed159fc5c95a8a65e8b3ec006ad465b69c164573249286d9398e7`
SIZE_BYTES: `38007`; LINE_COUNT: `743`
STATUS: `CANONICAL`
P4: P4 `PF-0225` / `NAG-DECISIONS` / `CANONICAL`; rekordów: 12
SOURCE_OF_TRUTH: `true`
LOCATOR: §Pytanie 1–8; §Sprawy, które nie trafiają…
LINK: [docs/process/pytania/2026-08-25-wybory.md](../../pytania/2026-08-25-wybory.md)

### SRC-04 — `docs/nota-08-wybory-otwarte.md`

PATH: `docs/nota-08-wybory-otwarte.md`
SHA256: `b05ce062506e6ac5d50309c504a6bf0a32f5257e925c1b78ad27d3ccb86625d9`
SIZE_BYTES: `19004`; LINE_COUNT: `347`
STATUS: `HISTORY`
P4: P4 `PF-0206` / `NAG-RESEARCH` / `HISTORY`; rekordów: 12
SOURCE_OF_TRUTH: `false`
LOCATOR: §Co jest pracą Hermesa…; §Pytanie 1–8; §Informacja…D-010; §Uwagi redakcyjne.
LINK: [docs/nota-08-wybory-otwarte.md](../../../nota-08-wybory-otwarte.md)

### SRC-05 — `NAGENTS-CONSOLIDATION-PLAN.md`

PATH: `NAGENTS-CONSOLIDATION-PLAN.md`
SHA256: `b282d9e49bf77994a290fbfab71803217d02c9565ab8809e6fbf33501ca4b15c`
SIZE_BYTES: `130539`; LINE_COUNT: `665`
STATUS: `PLAN_ONLY / OWNER_HOLD_REQUIRED`
P4: brak ścieżki P4 (input plan/maszyna)
SOURCE_OF_TRUTH: `false`
LOCATOR: §3 wiersz NAGENTS-DECISIONS; §4.2 DEC-01…DEC-04; §6.4; §8–§9.
LINK: [NAGENTS-CONSOLIDATION-PLAN.md](../../../../NAGENTS-CONSOLIDATION-PLAN.md)

### SRC-06 — `NAGENTS-PROJECT.md`

PATH: `NAGENTS-PROJECT.md`
SHA256: `84cc8a89c0e7bd53268fc78826938bec062f9223340aa2e0934d64719764a2fb` (bieżący readback)
SIZE_BYTES: `53421`; LINE_COUNT: `819`
SNAPSHOT_SHA256: `bab1666d8528622b055b1a3f0b9e21a918be5efabc5be0af2ac06064ae00236a`
SNAPSHOT_SIZE_BYTES: `53403`; SNAPSHOT_LINE_COUNT: `819`
STATUS: `CONSOLIDATION_CANDIDATE`
P4: P4 `PF-0184` / `NAG-INDEX` / `CONSOLIDATION_CANDIDATE`; rekordów: 2
SOURCE_OF_TRUTH: `false`
LOCATOR: §1; §2.1 wiersze o decyzjach; §4.4; §11; §12; drift dokładnie `NAGENTS-PROJECT.md:165`, offset `11051` zero-based.
DRIFT: snapshot → current, `+18` B; stara linia ma `p_cb0f9def` i `hermes project show`, bieżąca ma `p_e90c30bc` i `hermes --profile default project show`.
DRIFT_EFFECT: Bieżący indeks jawnie czyta anchor 8gent w profilu `default`; decyzje, status `CONSOLIDATION_CANDIDATE` i liczniki stagingu pozostają bez zmian.
LINK: [NAGENTS-PROJECT.md](../../../../NAGENTS-PROJECT.md)

### SRC-07 — `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json`

PATH: `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json`
SHA256: `95d2a06fc80e4cf1c27d59959d84b8631e70acbb3e0b879b83df95c38bf382b8`
SIZE_BYTES: `2655394`; LINE_COUNT: `54556`
STATUS: `PASS_WITH_EXPLICIT_OWNER_GATES`
P4: brak ścieżki P4 (input plan/maszyna)
SOURCE_OF_TRUTH: `false`
LOCATOR: logical_group_rollups[NAG-DECISIONS]; path_family_classifications[PF-0241, PF-0219, PF-0225, PF-0206, PF-0184].
LINK: [docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json](../../../process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json)
