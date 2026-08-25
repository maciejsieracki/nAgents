# Nota 07 — katalog funkcji appto i mapowanie na nAgents

**NASTER · projekt nAgents · temat `NAG-INFO-002-katalog-funkcji` · 25 sierpnia 2026**

Ta nota jest przeznaczona dla właściciela firmy, nie dla programisty. Terminy
techniczne są wyjaśnione w tym samym zdaniu, w którym się pojawiają.

---

## 1. Po co ta nota i skąd wzięliśmy dane

Właściciel zdecydował: appto.ai jest wzorcem, a nie towarem do kupienia — budujemy
własną platformę o tych samych funkcjach. Ta nota zbiera, czego appto uczy nas
o zakresie do zbudowania, i przypisuje każdą funkcję do jednego z naszych czterech
etapów (MVP1–MVP4) albo jawnie ją odrzuca.

**Źródła — wyłącznie cztery pliki, nic spoza nich:**

- `docs/process/zrodla/appto-strona-glowna-pl.md`
- `docs/process/zrodla/appto-cennik-pl.md`
- `docs/process/zrodla/appto-wdrozenie-kohortowe-pl.md`
- `docs/process/zrodla/appto-integracje-pl.md`

Wszystkie cztery to treść wklejona przez właściciela, bo bramka sieciowa środowiska
blokuje domenę appto.ai. Nikt w tym zwiadzie nie otworzył ani jednej strony appto
samodzielnie.

**Ważne zastrzeżenie, które trzeba mieć w pamięci przy każdej pozycji tej noty:**
to jest materiał sprzedażowy producenta. Jest dowodem na to, **co appto o sobie
twierdzi**, nie na to, jak appto naprawdę działa pod spodem. Funkcja opisana na
stronie może nie istnieć albo działać inaczej, niż sugeruje opis marketingowy.
W hierarchii wiarygodności źródeł, którą stosujemy w tym projekcie, materiał
sprzedażowy konkurenta stoi na samym dole — **nigdy nie jest podstawą decyzji
samą w sobie**, tylko materiałem poglądowym do zestawienia z naszą specyfikacją.
Cała ta nota operuje wyłącznie na takim materiale — czytelnik powinien o tym
pamiętać przy każdej tabeli poniżej.

---

## 2. Co appto pokazuje — katalog funkcji

Poniższe tabele wymieniają każdą funkcję appto znalezioną w czterech źródłach,
pogrupowaną tematycznie. Kolumna „CO ROBI" opisuje funkcję własnymi słowami;
krótki cytat obok pokazuje, skąd to wiemy.

**Uwaga metodologiczna o tym, co świadomie pominięto:** ze strony „wdrożenie
kohortowe" pominięto elementy samego **płatnego programu doradczego** (lekcje
wideo, webinary, cotygodniowa „godzina AI", społeczność na platformie Circle,
ścieżki ról, harmonogram 5 tygodni + 8 tygodni + 180 dni follow-up) — to usługa
obok platformy appto, nie funkcja samego produktu. Z tej samej przyczyny pominięto
pozycję **„Wsparcie we wdrożeniu"** z cennika (sekcja „Każdy plan to pełne appto") —
to również usługa towarzysząca sprzedaży, nie mechanizm platformy, więc traktujemy
ją tak samo jak treść strony wdrożenia kohortowego. Pominięto też **kalkulator
zwrotu z inwestycji** jako narzędzie strony sprzedażowej (interaktywny suwak
licząca zwrot z „14 minut tygodniowo") — to element marketingu cennika, nie
funkcja produktu; sama teza o czasie zwrotu jest i tak niesprawdzalna (patrz
część B, pozycja 5). Pominięto też **pakiet „wdrożenie enterprise" dla firm
powyżej ok. 100 osób** (cennik, sekcja konfiguratora i pytanie 6 FAQ: „SLA,
własne integracje i modele, priorytetowe wsparcie", wycena wyłącznie
indywidualna) — to próg komercyjny bez podanej treści funkcjonalnej (appto nie
mówi, na czym te elementy technicznie polegają), a przy jednym najemcy wielkości
NASTER (D-007) sam próg wielkości zespołu jest nieadekwatny niezależnie od
treści. Wspomniane dla porządku testu pokrycia, nie rozwijane w osobny wiersz
katalogu.

### a) rozmowa i interfejs

| NAZWA | CO ROBI | ŹRÓDŁO | PODSTAWA |
|---|---|---|---|
| Czat / kokpit | Rozmowa z asystentem w naturalnym języku jako główny interfejs | strona główna | „Kokpit / czat — Pytasz normalnym językiem" |
| Artefakty w osobnym panelu | Dokumenty, tabele i oferty generowane obok czatu, w osobnym panelu | strona główna | „Artefakty obok czatu — dokumenty, tabele i oferty w osobnym panelu" |
| Wyszukiwanie skrótem Cmd+K | Jeden skrót wyszukuje jednocześnie wątki, pliki i asystentów | strona główna | „Cmd+K — wyszukiwanie wątków, plików i asystentów pod jednym skrótem" |
| Historia wątków | Zapisana historia rozmów dostępna do przeglądania | strona główna | pozycja „Historia wątków" |
| Dostęp web i mobile | Aplikacja dostępna przez przeglądarkę i telefon | strona główna | „Web i mobile" |
| Wyszukiwanie w sieci | Pobranie aktualnych danych z internetu, gdy wiedza firmowa nie wystarcza | strona główna | „aktualne dane z internetu, gdy kontekst to za mało" |
| Szablony i skróty | Gotowe szablony i skróty przyspieszające rozpoczęcie zadania | strona główna | pozycja „Szablony i skróty" |
| Obsługa języka polskiego | Pisanie i rozumienie w języku polskim, w realiach branży | strona główna | „Pełny polski" |

### b) asystenci

| NAZWA | CO ROBI | ŹRÓDŁO | PODSTAWA |
|---|---|---|---|
| Definiowanie asystenta | Tworzenie asystenta z własnym promptem i przypisanym dostępem/modelem | cennik | „Asystenci per rola — własny prompt, dostęp i model" |
| Asystent domyślny | Jeden ogólny asystent obsługujący firmę, gdy nie wybrano specjalisty | strona główna | „Wise (asystent ogólny, domyślny)" |
| Asystenci per rola/dział | Osobne asystenty przypisane do funkcji biznesowej | strona główna | „Strateg sprzedaży (oferty, follow-upy)" |
| Wybór modelu pod zadanie | Dobór lub przełączenie modelu AI do konkretnego zadania | strona główna / cennik | „przełączasz w dwa kliki" |
| Rozdzielenie ról twórca/użytkownik | Prompt i dobór modelu ustawia twórca asystenta, zwykły użytkownik tylko opisuje zadanie | strona główna | „Prompty i dobór modelu to robota twórcy asystenta" |

### c) wiedza

| NAZWA | CO ROBI | ŹRÓDŁO | PODSTAWA |
|---|---|---|---|
| Kolekcje wiedzy | Grupowanie wiedzy firmowej w nazwane zestawy | strona główna | „Kolekcje z procesami firmy, udostępnione zespołom" |
| Przypisanie kolekcji do zespołów | Każda kolekcja ma listę zespołów, którym jest udostępniona | strona główna | tabela kolekcji, np. „Ton komunikacji marki" → Marketing, Sprzedaż, Cała firma |
| Źródła wiedzy z narzędzi zewnętrznych | Wiedza czerpana wprost z zewnętrznych systemów, np. transkrypt spotkania czy rekord CRM | strona główna | „transkrypt z Meet jako źródło, deal z CRM jako źródło" |
| Analiza wgranych plików | Odczyt i analiza treści pliku PDF, arkusza, zdjęcia | strona główna | „Analiza plików — PDF, arkusz, zdjęcie" |
| Pamięć kontekstu zespołu | Zapamiętanie ustaleń zespołu i użycie ich w kolejnych rozmowach | strona główna / cennik | „Pamięta ustalenia zespołu" |

Osobno warto nazwać wprost: scenariusz pokazowy strony głównej „asystent spisuje
ustalenia ze spotkania, dodaje zadania do projektu i wrzuca informację na kanał
zespołu" **nie jest jedną nową funkcją** — to złożenie trzech pozycji już
wymienionych w katalogu: źródła wiedzy z narzędzi zewnętrznych (transkrypt jako
źródło), dodawanie zadań do projektu (sekcja f) i obecność w komunikatorze
(sekcja f). Piszemy to wprost, żeby nie wyglądało na przeoczenie.

### d) umiejętności

| NAZWA | CO ROBI | ŹRÓDŁO | PODSTAWA |
|---|---|---|---|
| Budowanie umiejętności | Tworzenie gotowego, uruchamialnego procesu („umiejętności") | cennik | „Umiejętności — gotowe procesy do uruchomienia" |
| Udostępnianie umiejętności | Raz zbudowana umiejętność jest udostępniana zespołowi zamiast kopiowania promptu | strona główna | „Udostępniaj umiejętności, nie przeklejaj promptów" |
| Wersjonowanie | Asystenci i umiejętności mają numerowane wersje | strona główna | „asystenci i umiejętności mają wersje" |
| Natychmiastowa propagacja poprawki | Zmiana w wersji trafia od razu do wszystkich użytkowników | strona główna | „poprawka trafia od razu do wszystkich" |

### e) działanie w narzędziach

| NAZWA | CO ROBI | ŹRÓDŁO | PODSTAWA |
|---|---|---|---|
| Integracje z narzędziami zewnętrznymi | Appto łączy się z narzędziem firmy i wykonuje w nim operacje (czyta, zakłada, aktualizuje) | strona główna / integracje | „956 narzędzi, w których appto klika za Was" |
| Liczba akcji per integracja | Każde narzędzie w katalogu ma podaną liczbę możliwych operacji | integracje | np. GitHub 893, Stripe 432 |
| Praca w tle / autonomiczne działanie | Asystent wykonuje zadania bez obecności użytkownika | strona główna | „Pracuje, gdy Was nie ma" |
| Automatyzacje wieloetapowe | Agent prowadzi zadanie przez wiele kroków aż do rezultatu | cennik | „agent prowadzi zadanie aż do efektu" |
| Rutyny i harmonogram | Zaplanowane, cykliczne zadanie uruchamiane automatycznie | strona główna | „rutyna daje znać i podsyła gotowe podsumowanie" |
| Powiadomienia o wyniku | Wynik rutyny dostarczany mailem albo na kanał zespołu | strona główna | „mailem albo na kanał" |

### f) praca zespołowa

| NAZWA | CO ROBI | ŹRÓDŁO | PODSTAWA |
|---|---|---|---|
| Praca w projektach | Ten sam asystent i ta sama wiedza dostępne w kontekście projektu zespołowego | strona główna | „W projektach, razem z zespołem" |
| Obecność w komunikatorze (Slack) | Asystent dostępny i piszący w wątku kanału Slack | strona główna | „Na Slacku, w wątku zespołu" |
| Udostępnianie wątków i artefaktów | Rozmowa lub artefakt mogą być udostępnione innym, zgodnie z ich dostępami | strona główna | „Udostępnianie wątków i artefaktów pod dostępami" |
| Dodawanie zadań do projektu z poziomu asystenta | Asystent po spotkaniu dopisuje zadania bezpośrednio do projektu | strona główna | scenariusz pokazowy |

### g) dostępy

| NAZWA | CO ROBI | ŹRÓDŁO | PODSTAWA |
|---|---|---|---|
| Role Member / Manager / Admin | Trzy poziomy uprawnień różnicujące zakres dostępu | strona główna | „każdy ma dokładnie tyle dostępu, ile trzeba" |
| Logowanie Google | Logowanie kontem Google z 2FA zamiast osobnego hasła firmowego | strona główna | „wchodzisz kontem Google, z 2FA" |
| Logowanie SSO (Google/Microsoft/własne) | Rozszerzone opcje logowania firmowego przez SSO | cennik | „zaloguj zespół przez Google, Microsoft albo własne SSO" |
| Uwierzytelnianie dwuskładnikowe (2FA) | Dodatkowy składnik logowania | strona główna FAQ | „2FA i audit log" |
| Gotowy komplet dostępu dla nowej osoby | Konfiguracja ról/wiedzy/modeli ustawiona raz działa od dnia startu | strona główna | „nowa osoba dostaje gotowy komplet w dniu startu" |

### h) nadzór

| NAZWA | CO ROBI | ŹRÓDŁO | PODSTAWA |
|---|---|---|---|
| Dziennik zdarzeń (audit log) | Zapis kto, co i kiedy zrobił | strona główna / cennik | „Pełny dziennik zdarzeń" |
| Analityka zużycia zespołu | Zestawienie liczby wiadomości i trendu w czasie | strona główna | „Wiadomości 12 480, wzrost 18%" |
| Widoczność kosztu wobec planu | Bieżący koszt zużycia pokazany na tle limitu planu | strona główna | „Koszt 2 940 zł w planie" |
| Lista aktywnych użytkowników | Liczba aktywnych kont i ranking najaktywniejszych osób | strona główna | „Aktywni 34 z 38 osób" |

### i) koszty

| NAZWA | CO ROBI | ŹRÓDŁO | PODSTAWA |
|---|---|---|---|
| Kredyt jako jednostka rozliczeniowa | Jedna wspólna jednostka miary zużycia, niezależna od modelu | cennik | „Jednostką jest kredyt" |
| Wspólna pula kredytów firmy | Wszyscy użytkownicy czerpią z jednej puli | cennik | „Wszyscy w firmie czerpią z tej samej puli" |
| Rozliczenie za wykonaną pracę, nie za miejsce | Kredyt schodzi tylko przy realnym wykonaniu zadania | cennik | „Nie rozliczamy za miejsca" |
| Automatyczne doładowanie (top-up) | Dokupienie kredytów w trakcie miesiąca, wg wyższej stawki | cennik | „22 zł / 10 tys." poza abonamentem |
| Miesięczne odnawianie puli bez przenoszenia | Niewykorzystane kredyty przepadają na koniec okresu | cennik | „Pula odnawia się co miesiąc" |
| Konfigurator planu wg wielkości zespołu | Dobór planu przez próg liczby osób i suwak kredytów | cennik | progi do 10/25/50/100 osób |

### j) dane i zgodność

| NAZWA | CO ROBI | ŹRÓDŁO | PODSTAWA |
|---|---|---|---|
| Hosting danych w UE | Serwery zlokalizowane w Irlandii | strona główna / cennik | „Hosting w UE — serwery w Irlandii" |
| Brak trenowania modeli na danych klienta | Dane firmy nie są używane do trenowania modeli | strona główna | „brak trenowania modeli na Waszych danych" |
| Zero-retention u dostawców modeli | Zewnętrzni dostawcy modeli bazowych nie zatrzymują przetwarzanych danych | cennik | „zero-retention u dostawców modeli" |
| Zgodność z RODO (i deklaracja DSA) | Deklarowana zgodność z regulacjami europejskimi | cennik | „Serwery w UE i RODO (…) DSA" |
| Eksport danych na żądanie | Możliwość wyeksportowania własnych danych w dowolnym momencie | cennik | „eksport w każdej chwili" |

**Wszystkie pozycje tej kolumny to twierdzenie producenta o sobie, nie potwierdzona
praktyka.** Dotyczy to zwłaszcza tego wiersza — bezpieczeństwo i zgodność danych to
kategoria, w której naciąganie marketingu na fakt kosztuje najwięcej, gdyby ktoś
czytał tę tabelę bez reszty noty.

### k) wyjście

| NAZWA | CO ROBI | ŹRÓDŁO | PODSTAWA |
|---|---|---|---|
| Eksport do plików biurowych | Generowanie wyniku pracy jako PDF, DOCX lub arkusz | strona główna | „Eksport plików — PDF, DOCX, arkusze" |
| Artefakty gotowe do użycia | Wygenerowany artefakt (mail, task, faktura, dokument) gotowy do akceptacji i wysłania | cennik / strona główna | „Artefakty — maile, taski, faktury, dokumenty" |

**Razem w katalogu: 54 funkcje appto**, plus cztery pozycje jawnie wyłączone
(program wdrożeniowy, „Wsparcie we wdrożeniu", kalkulator zwrotu, pakiet
enterprise powyżej ok. 100 osób) i jedno złożenie nazwane wprost (asystent
spisujący ustalenia ze spotkania).

---

## 3. Zestawienie A — mamy już w planie (25 pozycji)

Poniżej funkcje appto, które nasza specyfikacja już przewiduje w którymś etapie —
inaczej sformułowane, czasem węziej, ale ten sam mechanizm.

Czat/kokpit (MVP1) · historia wątków (MVP1) · definiowanie asystenta (MVP1) ·
asystenci per rola/dział (MVP1/MVP2) · wybór modelu pod zadanie — u nas ustawiany
przez twórcę agenta w rejestrze, nie wybierany ręcznie w rozmowie (MVP1) ·
rozdzielenie ról twórca/użytkownik (MVP1/MVP2) · kolekcje wiedzy (MVP3) ·
przypisanie kolekcji do zespołów — **uwaga o różnicy struktury**: appto opisuje
to jako listę zespołów przy każdej kolekcji; u nas jest to relacja odwrotna —
każdy agent ma w swojej konfiguracji listę zespołów, których wiedzę czyta
(`kontekst: zespol: [...]`, MVP3 §2). Praktyczny efekt bywa podobny, ale to inna
struktura danych, nie to samo rozwiązanie opisane innym słowem — dlatego samo
„JEST" byłoby mylące bez tego zastrzeżenia · pamięć kontekstu zespołu — jest
w specyfikacji (MVP3), ale zablokowana do czasu decyzji D-011 · wersjonowanie
(git, MVP3/D-005) · natychmiastowa propagacja poprawki (MVP3 §3: testy zielone →
wiedza rozsyłana do profili) · integracje z narzędziami zewnętrznymi —
**uwaga o podziale odpowiedzialności**: nasza architektura (§1, §21.3) mówi
wprost, że wykonywanie operacji w narzędziu to zadanie silnika Hermes (warstwa 3),
nie kodu nAgents — „nAgents nie jest silnikiem agenta". To, co faktycznie mamy
w naszej specyfikacji, to `agent_secret_ref` (MVP1 §5) — tabela z odwołaniami do
sekretów, którą Hermes wykorzystuje, żeby połączyć się z narzędziem w imieniu
agenta. Sam mechanizm wykonania integracji nie jest naszym kodem do napisania —
to ważne rozróżnienie przy odpowiedzi „kto to buduje" · praca w tle/autonomiczne
działanie (MVP3 §5) · rutyny i harmonogram (MVP3 §5) · powiadomienia o wyniku
(MVP3 §5, kanał Teams) · role Member/Manager/Admin — u nas dwupoziomowo
(`user`/`owner`, architektura §5) · logowanie SSO (Entra ID, MVP1 §2) ·
gotowy komplet dostępu dla nowej osoby (MVP2 §3, synchronizacja z katalogiem) ·
dziennik zdarzeń (MVP1 §9, `audit_event`) · analityka zużycia zespołu (MVP2 §7) ·
widoczność kosztu wobec planu (MVP2 §4, budżety z progami ostrzegawczymi) ·
wspólna pula budżetu — u nas w dolarach per agent, nie w kredytach (MVP2 §4) ·
rozliczenie za wykonaną pracę, nie za miejsce — naturalna konsekwencja
`usage_event` (MVP1 §5) · zgodność z RODO — rozproszona w D-009, barierze 6
i MVP4 §7 · eksport danych na żądanie (MVP4 §7).

**Korekta dwóch pozycji po ocenie Evaluatora** (przeniesione z tej listy do
Zestawienia B poniżej):

- **2FA** — poprzednia wersja tej noty pisała „JEST W SPECYFIKACJI". Sprawdziliśmy:
  żaden z naszych dokumentów specyfikacji nie ustala tego wprost. Zakładaliśmy, że
  2FA zapewni sama dzierżawa Microsoft Entra ID, ale to jest założenie, nie zapisane
  ustalenie projektu — dlatego pozycja przechodzi do luk jako drobne doprecyzowanie
  do sprawdzenia i zapisania.
- **DSA** — cennik appto deklaruje zgodność z RODO **i z DSA** (Digital Services
  Act, unijne prawo o platformach obsługujących treści użytkowników zewnętrznych,
  np. serwisy społecznościowe czy marketplace'y). Nasz projekt nie wspomina o DSA
  nigdzie, i **prawdopodobnie DSA w ogóle nas nie dotyczy** — nAgents jest
  wewnętrznym narzędziem firmowym, nie platformą pośredniczącą w treściach
  publikowanych przez osoby trzecie, co jest warunkiem stosowania tej regulacji.
  Zapisujemy to jako wniosek do potwierdzenia prawnego, nie jako lukę do zamknięcia
  kodem — patrz pytanie 11 w sekcji 8.

---

## 4. Zestawienie B — luki, od najważniejszej (23 pozycje)

To, co appto pokazuje, a czego nasza specyfikacja dziś nie przewiduje. Brak
wzmianki nie znaczy, że funkcja jest niemożliwa albo niepotrzebna — znaczy, że nie
jest jeszcze zaplanowana.

1. **Artefakty w osobnym panelu / artefakty gotowe do użycia.** Bez tego nasz czat
   zwraca wyłącznie tekst; appto sprzedaje gotowy dokument, mail albo ofertę do
   akceptacji obok rozmowy. Konsekwencja: pracownik dostaje odpowiedź, nie efekt
   gotowy do użycia — traci się część obietnicy „appto zdejmuje pracę z rąk".
2. **Integracje: źródła wiedzy z narzędzi zewnętrznych, zadania w narzędziu
   projektowym.** Bez konektorów agent nie zasili się danymi z CRM/Meet ani nie
   dopisze zadania — tracimy „pracuje w Waszych narzędziach", zostaje sama rozmowa.
3. **Automatyzacje wieloetapowe jako ustalenie, nie domysł.** Prawdopodobnie
   zapewnia to Hermes natywnie, ale nie mamy tego potwierdzonego jako ustalenie
   projektu — ryzykujemy agentów ograniczonych do pojedynczej tury bez wiedzy o tym
   z wyprzedzeniem.
4. **Asystent domyślny (ogólny).** Bez niego pracownik bez przypisanego agenta nie
   ma dokąd napisać; appto ma zawsze punkt startowy (asystent „Wise").
5. **Formalne, kontraktowe ustalenia o danych u dostawców modeli** (brak trenowania,
   zero-retention, region hostingu). appto to deklaruje wprost jako fakt handlowy;
   u nas to zależy od umów z dostawcami modeli w bramie, nieopisanych w specyfikacji.
   Bez tego nie potwierdzimy zgodności, którą i tak deklarujemy w D-009.
6. **Lista aktywnych użytkowników z rankingiem.** Bez tego administrator widzi, kto
   ma dostęp, ale nie kto faktycznie korzysta.
7. **Cmd+K, szablony i skróty.** Bez tego start pracy z agentem jest wolniejszy niż
   w gotowym produkcie — koszt czysto użytkowy, nie bezpieczeństwa.
8. **Praca w projektach, udostępnianie wątków i artefaktów.** Bez tego każda
   rozmowa jest izolowana jeden-do-jednego (agent–pracownik); appto pozwala
   pracować zespołowo nad tym samym wątkiem.
9. **Analiza wgranych plików, eksport do plików biurowych, wyszukiwanie w
   sieci.** Prawdopodobnie zapewnia je Hermes natywnie, ale nie mamy tego jako
   ustalenia — nie wiadomo, czy działa bez dodatkowej konfiguracji.
10. **Marketplace umiejętności** — udostępnianie zbudowanej „umiejętności" jednym
    kliknięciem osobie/działowi/firmie, z wersjonowaniem. Mamy wersjonowanie wiedzy
    w gicie, ale nie osobny mechanizm „udostępnij to działowi jednym kliknięciem".
11. **Dostęp mobilny jako natywna aplikacja** (mamy tylko przeglądarkę, D-008).
12. Miesięczne odnawianie budżetu bez przenoszenia nadwyżki — doprecyzowanie
    reguły, nie nowy mechanizm.
13. Budowanie i udostępnianie „umiejętności" jako oddzielnego bytu od wiedzy —
    nasz model danych nie rozróżnia dziś tych dwóch pojęć.
14. Obsługa języka polskiego jako jawne ustalenie (dziś to cecha wybranego modelu,
    nie kod do napisania).
15. Wyszukiwanie w sieci, jeśli ma być poza tym, co robi Hermes natywnie.
16. Analityka wiadomości jako trend w czasie (mamy sam koszt i budżet, nie wykres
    liczby wiadomości).
17. Wspólne wątki widoczne w zespole pod kontrolą dostępu — dziś rozmowa jest
    własnością osoby/agenta, nie zespołu.
18. Kanał e-mail jako miejsce dostarczenia wyniku rutyny (mamy tylko Teams,
    MVP3 §8).
19. Trzeci poziom uprawnień pośredni między `user` a `owner` (appto ma trzy
    poziomy, my dwa).
20. Szablon startowy „gotowy komplet" opisany jako osobna, widoczna dla
    administratora funkcja UI, nie tylko efekt synchronizacji z katalogiem.
21. Analiza treści plików jako jawnie potwierdzone zachowanie (dziś zakładamy, że
    Hermes to robi, nie mamy tego zapisanego).
22. Kanał głosowy/telefoniczny — appto też go nie wspomina, więc brak jest
    symetryczny; wymieniamy dla kompletności testu pokrycia.
23. **2FA jako jawne ustalenie specyfikacji** — patrz korekta w sekcji 3. Dziś to
    tylko założenie, że dzierżawa Entra ID ma je włączone; wymaga potwierdzenia
    i zapisania, nie kodu.

---

## 5. Zestawienie C — świadomie odrzucamy (6 pozycji)

- **Katalog integracji z policzoną liczbą akcji, na wzór agregatora.** Budowa
  takiego katalogu jest gonieniem parytetu funkcji z appto, co `CLAUDE.md` zabrania
  wprost — to appto sprzedaje szerokość, my sprzedajemy kontrolę.
- **Obecność w Slacku.** Nasz plan komunikatora to Microsoft Teams, zgodnie
  z ekosystemem firmy (Entra ID, Graph) — Slack obok Teams byłby drugim,
  niepotrzebnym kanałem do utrzymania.
- **Logowanie kontem Google.** Tożsamość wyłącznie przez Entra ID — Google jest
  poza ekosystemem firmy.
- **Kredyt jako jednostka rozliczeniowa.** Sprzeczne z D-002: kredyt celowo ukrywa
  realny koszt zapytania za jedną liczbą sprzedawcy; my chcemy koszt przypisywalny
  do konkretnego agenta i człowieka, w realnej walucie i tokenach.
- **Automatyczne doładowanie (top-up) jako mechanizm marżowy.** To model
  sprzedawcy platformy dla swoich klientów zewnętrznych; my nie sprzedajemy
  dostępu do nAgents na zewnątrz, więc nie ma komu doładowywać.
- **Konfigurator planu wg wielkości zespołu (progi 10/25/50/100 osób).** Mamy
  jednego najemcę (D-007) — oś cennika appto jest u nas nieadekwatna.

---

## 6. Pięć największych różnic

1. **appto ma gotowy dokument do akceptacji obok czatu; nAgents dziś jest czystą
   rozmową tekstową.** To największa różnica funkcjonalna. Wymaga decyzji, czy
   budowa takiego panelu to w ogóle zadanie nAgents, czy Hermesa — zanim trafi do
   któregokolwiek etapu.
2. **appto ma szerokość integracji nieosiągalną ręcznie (956 albo 40+ narzędzi —
   źródła producenta są w tej liczbie ze sobą sprzeczne), nAgents planuje wąską
   listę własną.** To świadomy wybór (D-001), nie zaniedbanie — ale różnica rośnie
   z czasem: każda nowa prośba „podłącz nam X" to u nas tygodnie pracy, u appto
   włącznik.
3. **appto rozlicza w kredytach zacierających realny koszt; nAgents rozlicza
   w przypisywalnym koszcie per agent i człowiek (D-002).** To różnica celowa
   i odwrotna do appto — u nas to zaleta zgodności z wymogiem przejrzystości
   kosztu, nie luka do uzupełnienia.
4. **appto ma płaską strukturę „asystent per rola" z własnym dostępem każdego;
   nAgents rozważa hierarchię agent projektowy / stanowiskowy z odciętymi
   kluczami (D-003, D-010 odroczona).** appto nie ujawnia architektury poświadczeń
   za swoim modelem, więc nie wiadomo, czy rozwiązuje ten sam problem
   bezpieczeństwa, czy go pomija.
5. **appto deklaruje start „w jeden dzień, bez działu IT"; nAgents wymaga
   rejestracji w Entra ID, budowy szkieletu i przećwiczonego odtworzenia z kopii
   przed zamknięciem etapu.** Różnica jest zamierzona — appto sprzedaje prostotę
   wdrożenia kosztem zależności od jednego dostawcy, nAgents kupuje niezależność
   kosztem czasu pracy jednej osoby technicznej.

---

## 7. Integracje — warianty podejścia do 956 narzędzi

appto deklaruje **956 narzędzi** (strona integracji), przy czym strona główna
i cennik mówią o „40+" — rozbieżność, którą samo źródło integracji nazywa
niewyjaśnioną. Niezależnie od tego, która liczba jest bliższa prawdzie, żadna
z nich nie jest zadaniem, które kilkuosobowy zespół zbuduje ręcznie, integracja po
integracji. Trzeba wybrać podejście, nie próbować dogonić liczby.

| Wariant | Na czym polega | Cena | Główne ryzyko |
|---|---|---|---|
| **A — lista własna, wąska** | Budujemy tylko to, czego NASTER faktycznie używa (Microsoft 365/Teams, poczta, kalendarz, może ifirma) | Niska licencyjnie, wysoka we własnej pracy — każda integracja to kod do napisania i utrzymania | Zmiana API zewnętrznego systemu jest naszym problemem do wykrycia i naprawienia, bez pośrednika ostrzegającego. Tracimy szerokość: nowa integracja to tygodnie pracy |
| **B — pośrednik integracyjny** | Podłączamy jeden zewnętrzny produkt (typu agregator konektorów) i przez niego osiągamy szerokość | Opłata abonamentowa do pośrednika, rosnąca z użyciem | **Ten sam mechanizm ryzyka, przez który odrzucono zakup appto (D-001), odtwarzamy jeden poziom niżej**: jeśli dostawca pośrednika zniknie, zostanie przejęty albo zmieni cennik, cała warstwa integracji NASTER jest zakładnikiem decyzji obcej firmy |
| **C — otwarty protokół** | Przyjmujemy standard, w którym narzędzie samo się rejestruje, bez pisania integracji ręcznie po naszej stronie | Niska pieniężnie, wysoka na starcie w opanowanie standardu | Młodość i tempo zmian standardu, nie firma pośrednika. **Jeśli standard zostanie porzucony przez społeczność:** ryzyko jest niższe niż w wariancie B, bo każda integracja pozostaje osobnym, wymiennym modułem za naszą bramą — migracja do innego standardu albo powrót do wariantu A robi się integracja po integracji, kosztem tylko tych aktualnie używanych, nie całej floty naraz. To założenie architektoniczne, nie gwarancja — do zweryfikowania dopiero przy realnym wyborze standardu |
| **D — mieszanka** | Wariant A dla systemów krytycznych/wrażliwych (poczta, kadry, księgowość), wariant C dla reszty | Rozłożona — najwyższy koszt własnej pracy tylko tam, gdzie uzasadniony | Dwa mechanizmy do utrzymania zamiast jednego — więcej powierzchni operacyjnej dla jednej osoby technicznej |

**Rekomendacja rodzaju rozwiązania** (nie marki konkretnego dostawcy — źródła nie
dają podstaw do wskazania pośrednika ani protokołu): zacząć od wariantu A dla
MVP1–MVP2, gdy lista integracji jest krótka, i rozstrzygnąć B/C/D dopiero gdy
potrzeby realnie przekroczą to, co jedna osoba techniczna utrzyma ręcznie — a wtedy
zapisać wybór w dzienniku decyzji, bo dotyczy kosztu i odwracalności.

---

## 8. Wnioski wymagające decyzji właściciela

1. **[KRYTYCZNE] Korekta uzasadnienia D-001.** Cennik appto pokazuje, że wszystkie
   modele (Claude, GPT, Gemini, Kimi) są dostępne bez dopłat i przełączane przez
   użytkownika w dwa kliki, w ramach jednej jednostki rozliczeniowej („kredyt").
   To podważa zapisane uzasadnienie decyzji D-001 („rozliczenie za tokeny u
   dostawcy odbiera swobodę wyboru modelu") — appto nie ogranicza wyboru modelu
   cennikiem. Pełna korekta i propozycja nowego uzasadnienia — w nocie 06, sekcja 5.
   **Wariant do wyboru:** (a) przyjąć poprawione uzasadnienie zaproponowane w nocie
   06, (b) sformułować własne, (c) uznać, że oryginalne uzasadnienie mimo wszystko
   pozostaje aktualne z innego powodu i nie wymaga zmiany.
2. **Artefakty w osobnym panelu.** Czy to w ogóle zadanie nAgents, czy Hermesa?
   Czy warto budować taki panel, biorąc pod uwagę koszt UI?
3. **Integracje — który wariant (A/B/C/D z sekcji 7)** i dla jakich systemów
   najpierw.
4. **Źródła wiedzy z narzędzi zewnętrznych.** Czy to przyspiesza potrzebę
   konektorów zaplanowanych dopiero na MVP4?
5. **Dodawanie zadań do projektu.** Jakiego narzędzia do zadań faktycznie używa
   NASTER, żeby zaplanować integrację pod właściwy system, nie pod domysł?
6. **Role: dwa poziomy czy trzy?** Czy `user`/`owner` wystarcza, czy potrzebny
   trzeci, pośredni poziom (manager zespołu), jak u appto?
7. **Obecność w Slacku.** Potwierdzenie, że nikt w NASTER nie potrzebuje Slacka
   obok Teams, zanim odrzucimy to ostatecznie.
8. **Miesięczne odnawianie budżetu.** Czy niewykorzystany budżet ma przepadać co
   miesiąc (jak u appto), czy się kumulować?
9. **Hosting danych w UE.** Gdzie fizycznie hostujemy produkcję — potwierdzenie
   regionu.
10. **Trenowanie / zero-retention u dostawców modeli.** Czy każdy dostawca modelu
    w naszej bramie ma to potwierdzone kontraktowo, nie tylko jako nasze
    założenie?
11. **Zgodność z RODO i DSA.** Czy potrzebny formalny przegląd prawny przed pracą
    na danych produkcyjnych? Przy okazji: DSA prawdopodobnie w ogóle nas nie
    dotyczy (patrz sekcja 3) — wart potwierdzenia u prawnika zamiast domysłu.
12. **Dostęp mobilny.** Czy potrzebna natywna aplikacja, czy wystarczy responsywna
    przeglądarka na telefonie?
13. **2FA jako jawne ustalenie.** Czy potwierdzić i zapisać w specyfikacji, że
    dzierżawa Entra ID ma włączone uwierzytelnianie dwuskładnikowe, zamiast
    zostawiać to jako niepisane założenie?

---

## 9. Czego nie wiemy

Ta nota opiera się na czterech stronach appto wklejonych przez właściciela.
Nawigacja jednej z nich (strona główna) wymienia adresy stron, których **nie
mamy**:

- `appto.ai/pl/funkcje` — osobna strona funkcji, może zawierać pozycje nieobecne
  na stronie głównej i w cenniku.
- `appto.ai/pl/zastosowania` — strona zastosowań/branż, mogłaby pokazać, dla kogo
  appto projektuje swoje funkcje w praktyce.
- `appto.ai/pl/polityka-prywatnosci` — jedyne źródło, które rozstrzygnęłoby
  rzeczywisty zakres deklaracji o danych (podprzetwarzający, okres retencji,
  podstawa prawna) zamiast haseł typu „dane zostają u Was".
- `appto.ai/pl/regulamin` — warunki umowne wiążące klienta, w tym rzeczywisty
  zakres SLA i odpowiedzialności — cennik i wdrożenie tylko o nich wspominają.
- `appto.ai/pl/polityka-cookies`, `/kontakt`, `/webinar`, `/partnerzy` — bez
  znaczenia dla katalogu funkcji, wymienione dla porządku.

Inne braki, których żadne z czterech źródeł nie rozstrzyga: czy appto ma
certyfikat ISO 27001/SOC 2; czy istnieje lista podprzetwarzających (dostawca
modelu, hosting, baza danych) potwierdzająca deklaracje z sekcji „dane
i zgodność"; czy appto oferuje wariant instalowany u klienta (on-premise); czy
appto pozwala podłączyć własny klucz API do dostawcy modelu (BYOK) zamiast
rozliczać się w kredytach; czy marketplace umiejętności zawiera gotowe szablony
od samego appto, czy wyłącznie mechanizm dzielenia się tym, co zbudował klient.

**Przypomnienie na koniec:** cała ta nota operuje wyłącznie na materiale
sprzedażowym producenta — najniższym poziomie wiarygodności w naszej hierarchii
źródeł. Żadna pozycja katalogu nie jest potwierdzeniem, że appto rzeczywiście
działa tak, jak twierdzi jego własna strona.
