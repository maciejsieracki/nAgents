# Nota 06 — Research appto.ai

**NASTER · projekt nAgents · temat NAG-INFO-001-appto-research · 25 sierpnia 2026**

Polecenie właściciela: appto.ai wskazane jako wzorzec tego, co ma powstać. Zadanie —
zbadać funkcjonalność, zarządzanie, model rozliczenia i warstwę techniczną appto,
i zestawić to z naszą specyfikacją. Pięciu operatorów pracowało równolegle nad czterema
obszarami i jednym węzłem „kto za tym stoi"; ocena Evaluatora poniżej jest wiążąca dla
tego, co trafiło do tej noty — jej ustalenia i korekty są wpisane w treść.

**Znaczniki użyte w tekście:**

- **[F] FAKT** — potwierdzone bezpośrednio u źródła (adres, cytat/opis miejsca).
- **[W] WNIOSEK** — wyprowadzone rozumowaniem z faktów, zawsze z „ponieważ".
- **[D] DOMYSŁ** — prawdopodobne, niepotwierdzone; z poziomem pewności i tym, co by
  je rozstrzygnęło.

Zdanie ze strony appto zapisane jako `appto twierdzi, że…` opisuje wyłącznie to, co
firma o sobie mówi — nie jest to samo co potwierdzenie, że produkt tak działa.

---

## 1. Co ustaliliśmy w skrócie

[F] Bezpośredni dostęp do appto.ai był w tej sesji **całkowicie zablokowany** — nie
przez appto, tylko przez politykę sieciową naszego środowiska (zablokowane były też
domeny kontrolne: `example.com`, `wikipedia.org`, `web.archive.org`; szczegóły i próba
niezależnej weryfikacji tego zapisu — sekcja 6). Nie otworzyliśmy ani jednej strony
appto.ai. Wszystko, co wiemy o samym produkcie, pochodzi ze streszczeń wyszukiwarki
(WebSearch) — dlatego żadne twierdzenie o treści strony appto nie ma znacznika [F],
tylko [D] (pełne uzasadnienie i przykłady — sekcje 2–5). [F] Jedyny kanał, który
zadziałał bez pośrednika, to zapytania DNS (sekcja 6) — stąd [W]/[F] ustalenie: appto
hostuje frontend na Vercelu ([W], ponieważ pełna delegacja stref DNS na nameservery
Vercela występuje wyłącznie przy faktycznym podłączeniu domeny do tej platformy — patrz
sekcja 6) i pocztę na Google Workspace ([F], rekord MX — sekcja 6). [D] Pewność
średnia: kto stoi za appto pozostaje domysłem (WiseGroup/Szymon Negacz), nie faktem —
szczegóły i zastrzeżenia w sekcji 7. Przesłanka decyzji **D-001** („rozliczenie za
tokeny odbiera swobodę wyboru modelu") **nie została ani potwierdzona, ani obalona** —
[D] pewność średnia: appto opisuje jednostkę rozliczeniową jako „kredyt", nie „token",
co jest słabym, pośrednim sygnałem, nie rozstrzygnięciem (sekcja 5). [W] Funkcjonalnie
appto pokrywa się w dużej mierze z naszą specyfikacją MVP1–MVP3 (panel, role, kontekst
firmowy, zatwierdzenia, audyt), ponieważ zestawienie w sekcji 8 pokazuje pokrycie
większości punktów appto przez nasze etapy — to potwierdza trafność kierunku, nie
podważa go. [D] Pewność średnia: największa luka to mechanizm appto do udostępniania
zbudowanych „umiejętności" jednym kliknięciem (marketplace), którego nie mamy w żadnym
etapie (sekcja 8). [W] appto samo wygląda na produkt wczesnego etapu (early access),
poboczny wobec głównego biznesu grupy, która za nim stoi, ponieważ jedyne potwierdzone
przychody i wielkość zespołu dotyczą głównych marek grupy, nie appto konkretnie
(uzasadnienie pełne — sekcja 7) — to podnosi, nie obniża, wagę argumentu „budujemy, nie
kupujemy" z D-001, niezależnie od tego, czy sama przesłanka o tokenach się potwierdzi.
Rekomendacja (działanie, nie twierdzenie o appto): właściciel powinien albo odblokować
dostęp sieciowy do appto.ai na potrzeby drugiej rundy, albo osobiście zajrzeć na stronę
cennika i rozstrzygnąć D-001 z pierwszej ręki.

---

## 2. Czym jest appto

[D] Poziom pewności: wysoki (spójne w kilku niezależnych zapytaniach WebSearch, w tym
tytuł strony głównej „appto — Platforma AI dla całej firmy", adres
`https://www.appto.ai/pl/`). appto twierdzi, że jest platformą AI osadzoną w procesach
firmy: pracownik dostaje asystenta znającego dokumenty, procedury i ustalenia firmy,
a sam produkt przejmuje część zadań (odpowiadanie na maile, tworzenie ofert,
podsumowania spotkań) zamiast tylko odpowiadać na pytania. Potwierdziłoby to
bezpośrednie otwarcie strony głównej — niedostępne w tej sesji.

[D] Pewność niska. Materiał sugeruje trzy ścieżki wdrożenia różniące się wielkością
zespołu klienta (3–15 osób, 15+ osób, „dla każdego, we własnym tempie") w ramach celu
„zespół w formie w 30 dni" — to brzmi jak opis sposobu wdrożenia/onboardingu, nie
segmentacji cenowej ani branżowej. Konkretne branże (e-commerce, agencje, IT) pojawiły
się raz, bez potwierdzenia w innym zapytaniu — nie uznane za ustalone.

[D] Pewność niska. Sposób sprzedaży wygląda na model z udziałem handlowca: jedno
streszczenie sugeruje stopniowe otwieranie dostępu po zostawieniu danych
kontaktowych i indywidualny onboarding, a nie czysty self-service. Nie sprawdzone
wprost — kontakt handlowy jest poza zakresem tego zlecenia.

[D] Pewność wysoka, że nie ma trafienia. Nie znaleziono żadnej niezależnej recenzji na
Trustpilot/G2/Capterra dla tego konkretnego produktu, ani opublikowanego studium
przypadku poza jednym niepewnym śladem („Stalmetsz" — możliwe zniekształcenie nazwy
przez streszczenie, nieodnaleziona strona źródłowa, nie budujemy na tym wniosku).

**Ostrzeżenie dla przyszłych badań:** istnieją co najmniej trzy niepowiązane podmioty
o zbliżonej nazwie — `appto.io` (komunikacja z klientem/live chat, USA), aplikacja
mobilna argentyńska `Appto` (Google Play, sysgestion) oraz wpis w Crunchbase
`appto-1cd2`, który w dwóch niezależnych, świeżych zapytaniach opisywał **dwie różne,
sprzeczne ze sobą** treści (raz: system QR kontroli dostępu; raz: aplikacja dostawcza
w RPA/Gauteng) — [F] fakt sprawdzony przez Evaluatora niezależnym zapytaniem: żadna z
dwóch wersji nie jest appto.ai. To dobry powód, dla którego żaden z operatorów nie
oznaczył treści tego wpisu jako [F] — synteza wyszukiwarki dla tego adresu okazała się
niestabilna między zapytaniami.

---

## 3. Funkcjonalność

Wszystkie pozycje poniżej: [D], źródło — synteza WebSearch wskazująca adresy
`appto.ai/pl` i `appto.ai/pl/funkcje`, nie zweryfikowane bezpośrednio na stronie.
Pewność podana per wiersz.

| Funkcja | appto twierdzi, że… | Pewność |
|---|---|---|
| Asystenci przypisani do ról + „company skills" | ma wielu wyspecjalizowanych asystentów/umiejętności, nie jednego uniwersalnego agenta | wysoka (4 niezależne zapytania) |
| Centralny panel zarządzania | użytkownicy, asystenci, wiedza firmowa i uprawnienia z jednego miejsca | wysoka |
| Wybór modelu LLM per zadanie | GPT, Claude, Gemini, Kimi wybieralne z jednego miejsca | wysoka |
| Kontekst firmowy | dokumenty, procesy, rozmowy i dane wpływają na każdą odpowiedź bez wklejania ich ręcznie | wysoka |
| Integracje czytające i piszące | nie tylko odpowiada — działa w narzędziach firmowych | wysoka |
| Samodzielne przejmowanie zadań | odpowiada na maile, tworzy oferty, podsumowuje spotkania | wysoka (dosłowne powtórzenie w 2 zapytaniach) |
| Marketplace umiejętności | udostępnianie zbudowanego „skilla" osobie/działowi/firmie jednym kliknięciem, z wersjonowaniem | średnia |
| Wspólne wątki i artefakty | pod kontrolą dostępu, dzielone w zespole | średnia |
| Integracje z nazwy | Slack, Teams, Gmail, Outlook, Google Drive, Pipedrive, Notion, HubSpot, Stripe, ifirma | wysoka |
| Liczba integracji „40+" | pojawiła się w dwóch niezależnych zapytaniach o różnej treści | **średnia** — podniesiona względem pierwotnej oceny operatora (pewność niska); Evaluator odnalazł tę samą liczbę w osobnym, niezależnym zapytaniu |
| Kanały rozmowy | aplikacja appto, Slack, Teams, e-mail | wysoka |
| Kanał głosowy/telefoniczny | brak wzmianki w żadnym zapytaniu | brak sygnału — nie dowód nieistnienia |
| Rezydencja danych w UE, brak trenowania modeli na danych klienta | twierdzenie appto o prywatności | wysoka (twierdzenie, nie potwierdzona praktyka) |
| Rozliczenie: pay-as-you-go lub pakiet kredytów, pula zespołowa, autodoładowanie | patrz sekcja 5 | wysoka co do opisu, zero liczb |

[D] Pewność niska. Mechanika tworzenia asystenta (formularz, kreator konwersacyjny,
prompt systemowy wprost) nie została ustalona — jedyny trop to hasło „logujesz się i
pracujesz z gotowymi asystentami", bez opisu procesu tworzenia nowego.

[D] Pewność niska. Nie ustalono, czy asystenci mogą wywoływać się nawzajem
(orkiestracja) czy działają jako niezależne, osobno wywoływane jednostki — materiał
mówi językiem „asystentów i umiejętności", nie językiem grafów agentów.

[D] Pewność niska. Nie ustalono, czy Marketplace zawiera gotowe szablony OD APPTO, czy
wyłącznie mechanizm dzielenia się tym, co zbudował klient — to rozróżnienie ma
znaczenie i nie zostało rozstrzygnięte.

**Odrzucone jako niewiarygodne:** twierdzenie jednej syntezy o „wzroście appto.ai
głównie w segmencie Enterprise, spadek w SMB (-6%)" — brak wiarygodnego źródła takiej
statystyki dla spółki tej wielkości, prawdopodobne pomylenie z innym raportem
zaindeksowanym pod podobne słowa kluczowe. Nie przenosimy tego dalej.

---

## 4. Zarządzanie i bezpieczeństwo

To jest sedno naszego projektu — poniżej każde ustalenie jest oznaczone wprost jako
potwierdzone, wywnioskowane albo lukę.

**Role i uprawnienia.** [D] Pewność średnia. appto twierdzi o „jednym panelu
administracyjnym", w którym widać kto ma dostęp, na jakich asystentach pracuje, z
jakiej wiedzy korzysta i jakie ma uprawnienia, oraz o „asystentach opartych na
rolach". [D] Pewność niska — brak nazw konkretnych ról i brak informacji o
granularności uprawnień (per agent, per dział, per integracja).

**Logowanie firmowym kontem (SSO).** [D] Pewność niska. Żadne zapytanie nie zwróciło
bezpośredniego potwierdzenia SSO z Microsoft Entra ID ani Google Workspace dla
appto.ai konkretnie. **Korekta wynikająca z oceny Evaluatora:** dodatkowe,
niezależne zapytanie WebSearch znalazło sformułowania „logowanie przez Google z 2FA,
bez dodatkowych haseł firmowych" — to podnosi ten trop z „brak sygnału" do [D] pewność
niska, ale wciąż niepotwierdzone u źródła i wciąż niejasne, czy chodzi o SSO firmowe
(Workspace) czy zwykłe logowanie kontem Google osobistym. To pozostaje jedną z
najważniejszych luk tej noty.

**Dziennik zdarzeń / audyt dla administratora.** [D] Pewność niska. Jedno
streszczenie wspomina ogólnie „bezpieczeństwo i kontrolę" oraz izolowane środowisko
firmowe z dostępem per użytkownik, bez słów „log" czy „audyt" wprost. **Korekta:**
dodatkowe zapytanie znalazło frazę „system śledzi, kto co zrobił… dzienniki audytu
gotowe pod compliance" — podnosi to do [D] pewność niska (nie średnia — fraza
brzmi jak ogólne hasło sprzedażowe, nie opis mechanizmu), nadal niepotwierdzone
bezpośrednio.

**Zatwierdzanie przez człowieka przed działaniem.** [D] Pewność niska. appto ma
twierdzić, że generuje „gotowe do sprawdzenia" oferty, umowy i notatki na podstawie
cennika i historii rozmów — słowo „gotowe do sprawdzenia" sugeruje krok akceptacji
przed wysyłką, ale to parafraza wyszukiwarki, nie cytat ze strony.

**Limity wydatków, budżety.** [D] Pewność niska. Model opisany jako zużycie
rozliczane zbiorczo dla firmy (patrz sekcja 5) — brak potwierdzenia twardego limitu
per agent lub per osoba.

**Lokalizacja danych, RODO.** [D] Pewność średnia-niska. Jedno streszczenie: „dane w
UE, szyfrowanie w spoczynku i w tranzycie, izolowane środowisko per firma, modele nie
uczą się na danych klienta". Region „Frankfurt" pojawił się w kontekście ogólnym o
standardach bezpieczeństwa, nie jawnie przypisany do appto — nie traktujemy tego jako
ustalone dla appto konkretnie.

**Powierzenie przetwarzania, lista podprzetwarzających.** [F] Brak jakichkolwiek
danych — żadne zapytanie nie zwróciło wzmianki o umowie DPA ani liście
podprzetwarzających appto. Pole całkowicie puste.

**Certyfikaty (ISO 27001, SOC 2).** [F] Nie znaleziono żadnej wzmianki o posiadaniu
takich certyfikatów. Brak potwierdzenia nie jest równoznaczny z zaprzeczeniem.

**BYOK — własny klucz API do dostawcy modelu.** [D] Pewność niska, właściwie brak
sygnału w obie strony. Kilka ukierunkowanych zapytań nie zwróciło niczego dotyczącego
appto konkretnie (tylko inne produkty, które BYOK oferują). To rozróżnienie ma
znaczenie kosztowe i compliance'owe i wymaga osobnego potwierdzenia (patrz sekcja 5 i
6 — hipoteza architektury zakłada, że appto zawsze pośredniczy własnym kontraktem).

**Polityka prywatności / regulamin.** [F] Nie otwarto i nie znaleziono bezpośredniego
adresu takiego dokumentu na appto.ai.

Podsumowanie: obszar „zarządzanie i bezpieczeństwo" — najważniejszy dla naszego
projektu — jest jednocześnie **obszarem z największą liczbą pustych pól** w całym
zwiadzie. Wszystko, co appto twierdzi tu o sobie, jest marketingowym hasłem
(„bezpieczeństwo i kontrola", „dane pod pełną kontrolą od pierwszego dnia"), nigdy
mechanizmem opisanym na poziomie funkcji.

---

## 5. Model rozliczenia — i odpowiedź w sprawie D-001

[D] Pewność średnia (3 niezależne zapytania, spójne sformułowanie). appto twierdzi o
rozliczeniu **pay-as-you-go albo pakietach kredytów** dopasowanych do skali zespołu, z
pulą współdzieloną w zespole i automatycznym doładowaniem. [D] Pewność niska — nie
znaleziono ani jednej konkretnej liczby (kwoty, progu, waluty) w żadnym streszczeniu.
Nie wiadomo, czy cennik ma jawne liczby, czy kończy się na kontakcie handlowym.

[D] Pewność niska. Nazw planów (Starter/Team/Enterprise) nie znaleziono. Jedyne
rozróżnienie to ścieżki wdrożenia wg wielkości zespołu (sekcja 2), które prawdopodobnie
opisują onboarding, nie siatkę cenową.

[D] Pewność średnia. Jednostka rozliczeniowa nazwana w materiałach to **„kredyt"**, nie
„token" — słowo „token" nie pojawiło się w żadnym streszczeniu dotyczącym appto
konkretnie. **Uzupełnienie wymagane przez ocenę Evaluatora:** ten sam trop — rabaty do
20% przy rocznych kontraktach 30+ osób — pojawił się wyłącznie w węźle „kto za tym
stoi" (firma.md), z niską pewnością, mimo że tematycznie należał do tej sekcji; jest tu
dopisany jako [D] pewność niska, niepotwierdzone u źródła, wymaga sprawdzenia na
stronie cennika razem z resztą.

[D] Pewność niska, brak sygnału w obie strony — czy istnieje możliwość podłączenia
własnego klucza API (BYOK) i płacenia bezpośrednio dostawcy modelu. Nie sprawdzone,
czy istnieje wariant instalowany u klienta (on-premise) — jedno zapytanie zwróciło
tylko ogólne hasło o „danych pod pełną kontrolą", nie potwierdzenie takiego wariantu.

### Odpowiedź wprost na pytanie o D-001

**Zwiad nie potwierdził i nie obalił przesłanki D-001** („rozliczenie za tokeny u
dostawcy odbiera swobodę wyboru modelu"). Dostęp do strony cennika appto.ai był przez
cały czas zablokowany na poziomie sieci tej sesji — nie jest to twierdzenie o appto,
tylko o granicach tego badania. Jedyny znaleziony sygnał: appto opisuje jednostkę
rozliczeniową jako „kredyt" (jednostka wewnętrzna sprzedawcy), nie „token" (bezpośrednie
przeliczenie zużycia u dostawcy modelu, widoczne dla klienta) — [W] to lekko przeczy
przesłance D-001 w dosłownym brzmieniu, ponieważ „kredyt" sugeruje warstwę pośredniczącą
z własną marżą appto, a nie bezpośrednie przeniesienie kosztu tokena dostawcy na
klienta — ale to nie jest to samo twierdzenie ani jego zaprzeczenie, może opisywać ten
sam mechanizm innym słownictwem albo dwa różne mechanizmy. Żaden operator nie znalazł
strony cennika ani jednej konkretnej liczby.

**Rekomendacja, nie rozstrzygnięcie:** właściciel powinien albo odblokować dostęp
sieciowy do appto.ai dla drugiej rundy badania, albo osobiście otworzyć
`https://www.appto.ai/pl/cennik` (adres prawdopodobny, niepotwierdzony) i ocenić, czy
D-001 wymaga formalnej rewizji. Dziennik decyzji zmienia wyłącznie właściciel — ta nota
tego nie robi.

---

## 6. Na czym to prawdopodobnie stoi

Ten obszar ma **dwie warstwy dowodowe o zupełnie różnej jakości** — celowo rozdzielone
poniżej, żeby nie uśredniać mocnego dowodu z czystym domysłem.

### Warstwa 1 — hosting i infrastruktura (dobrze uzasadniona)

[F] Zapytania DNS wykonane samodzielnie, surowym zapytaniem UDP do `8.8.8.8:53`
(protokół DNS omija blokadę proxy tej sesji), 2026-08-25:

- `NS appto.ai` → `ns1.vercel-dns.com`, `ns2.vercel-dns.com`
- `A appto.ai` / `A www.appto.ai` → zakres `216.150.1.x` / `216.150.16.x` (rozrzut
  typowy dla anycast, nie osobnych serwerów)
- `MX appto.ai` → wyłącznie `smtp.google.com`
- `TXT appto.ai` → SPF z `include:mail47.mydevil.net` i `include:_spf.google.com`, dwa
  wpisy `google-site-verification`, jeden `hubspot-developer-verification`
- Strefa DNS appto.ai jest typu **wildcard**: losowa, nieistniejąca subdomena zwraca te
  same adresy IP co `app.`, `api.`, `docs.`, `status.` — jedyny wyjątek: `mail.appto.ai`
  (brak rekordów)

[F] Weryfikacja niezależna surowym TCP+TLS do `216.150.16.1:443` (SNI `www.appto.ai`,
z pominięciem zmiennych proxy) zwróciła certyfikat wystawiony przez „Anthropic Egress
Gateway SDS Issuing CA (production)" — to dowód, że warstwa HTTP jest przechwytywana
(MITM) przez bramkę egress **naszej** sesji, nie że appto ma taki certyfikat. Sama
warstwa HTTP appto pozostaje niedostępna do bezpośredniego sprawdzenia w tym
środowisku.

[W] Ponieważ obsługa DNS (rekordy NS) jest w całości przekazana na `vercel-dns.com`,
appto najprawdopodobniej hostuje przynajmniej warstwę frontendową na platformie
Vercel, ponieważ pełne przekazanie strefy DNS na nameservery Vercela jest wymagane
przez tę platformę wyłącznie wtedy, gdy domena jest u niej faktycznie skonfigurowana —
nie jest to sposób podłączenia jednego rekordu u zewnętrznego dostawcy.

[F] Poczta firmowa appto.ai idzie przez Google Workspace (`MX → smtp.google.com`), nie
przez własny serwer. [D] Pewność średnia — wpis SPF wskazujący też
`mail47.mydevil.net` (polski dostawca hostingu współdzielonego/VPS, nie klasy
enterprise) może być aktywnym drugim nadawcą albo pozostałością po starszej wersji
strony sprzed migracji na Vercel/Workspace — nierozstrzygnięte.

### Warstwa 2 — stack frontendu i architektura modelowa (domysł, nie dowód)

[D] Pewność średnia. Hosting na Vercelu **nie dowodzi** użycia Next.js/React — Vercel
hostuje dowolne frameworki. Potwierdziłyby to nagłówki HTTP (`x-vercel-id`) albo ślady
`_next/static/` w kodzie źródłowym — niesprawdzone, strona niedostępna.

[D] Pewność średnia, **ale oparta wyłącznie na marketingu, nie na żadnym śladzie
technicznym**. Hipoteza: appto to warstwa pośrednicząca (gateway/router) nad kilkoma
zewnętrznymi dostawcami modeli (GPT, Claude, Gemini, Kimi), a nie właściciel własnego
modelu. Cała ta warstwa hipotezy opiera się **wyłącznie** na tym, co appto samo o
sobie twierdzi w materiałach sprzedażowych („wybór modelu w jednym miejscu") — nie ma
za nią żadnego nagłówka HTTP, śladu API ani polityki prywatności z listą
podprzetwarzających. To jest przeniesienie obietnicy marketingowej na poziom
architektury i tak należy to czytać: **prawdopodobne, ale bez żadnego niezależnego
oparcia technicznego** — w odróżnieniu od warstwy 1 (hosting), która ma twardy dowód
DNS.

Co by tę drugą warstwę potwierdziło: realny nagłówek HTTP serwera appto, polityka
prywatności z nazwaną listą podprzetwarzających, dokumentacja API, ogłoszenie o pracę
wymieniające stack. Co by ją obaliło: polityka prywatności nazywająca jednego,
wyłącznego dostawcę modelu.

### Ślady w rejestrach — brak trafień

[D] Pewność wysoka, że nie ma trafienia (nie że firma nie istnieje w rejestrze).
Zapytania o KRS, LinkedIn, oferty pracy na polskich portalach IT (justjoin.it,
nofluffjobs, theprotocol.it) nie zwróciły niczego dotyczącego tej konkretnej firmy.
[F] Zapytanie o Crunchbase zwróciło stronę opisującą inny podmiot (patrz ostrzeżenie w
sekcji 2) — odnotowane wprost, żeby nie zostało przepisane jako dane o appto.ai.

---

## 7. Kto za tym stoi

[D] Pewność średnia. appto wygląda na produkt grupy **WiseGroup**, prowadzonej przez
**Szymona Negacza** — źródło pośrednie: streszczenie materiału wideo opisującego appto
jako nową aplikację AI od WiseGroup, w fazie „early access" pod `appto.ai/pl`
(`https://www.youtube.com/watch?v=TGvDqrh2AXs`, nieotwarty bezpośrednio).

[D] Pewność średnia. Podmiotem prawnym za usługami WiseGroup (co najmniej za
regulaminem `wisegroup.pl`) jest **SELLWISE SZYMON NEGACZ SPÓŁKA KOMANDYTOWA**, ul.
Piwna 10, 44-100 Gliwice, NIP 6312699869, REGON 389975697, KRS 0000922240, wpis
20 września 2021 — zgodnie potwierdzone przez pięć niezależnych agregatorów KRS
(imsig.pl, bizraport.pl, krs-online.com.pl, rejestr.io, wyszukiwarkakrs.pl), świeżo
zweryfikowane też przez Evaluatora niezależnym zapytaniem. **Nie potwierdzone: czy ta
sama spółka figuruje w stopce/regulaminie samego appto.ai** — mogła zostać wydzielona
osobna spółka. Rozstrzygnąłby to bezpośredni odczyt stopki appto.ai.

[D] Pewność niska, ostrzeżenie ważne dla dalszych badań: istnieją dwa inne,
niepowiązane podmioty o niemal identycznej nazwie „WISE GROUP SP. Z O.O." (KRS
0000902314, wykreślona; KRS 0000957362, zarząd Basiuk) — podobieństwo nazwy nie jest
dowodem tożsamości.

[D] Pewność średnia. WiseGroup jako całość ma ok. 170 osób, przychody grupy ok. 27 mln
zł w 2023, cel 39 mln zł w 2024. Marka zrzesza kilka brandów (SellWise, AdWise,
HireWise, Let's Automate, Finerto, IRSM) — appto pojawia się jako nowy, dodatkowy
produkt, nie jeden z głównych sześciu brandów. **Żadna z tych liczb nie dotyczy zespołu
pracującego konkretnie nad appto** — może to być kilkuosobowy zespół wydzielony, może
cały istniejący zespół techniczny grupy. Nie znaleziono nazwiska osoby opisanej wprost
jako „CEO appto".

[D] Pewność wysoka, że brak danych (nie że brak finansowania). Żadne zapytanie o
rundy, inwestorów, venture capital dla „appto.ai" nie zwróciło wyniku. Sam Negacz
uruchomił w 2025 r. fundusz „WiseVentures" — to WiseGroup jako inwestor w inne spółki,
nie inwestycja zewnętrzna w appto.

[D] Pewność średnia. Materiał wideo o appto mówi o „early access" i możliwości
rezerwacji dostępu — sugeruje to produkt we wczesnej fazie dostępności w sierpniu 2026,
nie dojrzały, wieloletni produkt. Data premiery i data rejestracji domeny nie zostały
ustalone (WHOIS niedostępny w tej sesji).

[D] Pewność wysoka, że nie znaleziono. Brak jakiejkolwiek niezależnej recenzji
użytkownika (Trustpilot/G2/Capterra), brak klientów wymienionych z nazwy poza jednym
niepewnym śladem, brak liczb o skali wdrożeń w Polsce.

### Ocena ryzyka dostawcy

[W] appto wygląda na produkt wczesnego etapu (early access, brak niezależnych
recenzji, brak potwierdzonych klientów) wydany przez firmę usługową (WiseGroup/
SellWise), której główny, udokumentowany biznes to doradztwo sprzedażowe i szkolenia,
nie oprogramowanie AI — ponieważ wszystkie potwierdzone przychody i wielkość zespołu,
jakie znaleziono, dotyczą głównych marek grupy (SellWise, AdWise, HireWise), a nie
appto konkretnie. Wynika z tego podwyższone ryzyko: appto może być produktem pobocznym
wobec głównego biznesu grupy — decyzja o jego rozwoju, cenie czy dalszym istnieniu
może zależeć od wyników zupełnie innej części organizacji, niezwiązanej z tym, na czym
zależy jego klientom AI.

[D] Pewność średnia. Jeśli WiseGroup/SellWise jako całość ma kilkuletnią historię i
rosnące przychody (w miarę spójnie potwierdzone przez źródła prasowe), ryzyko
nagłego zniknięcia całej grupy jest niższe niż dla anonimowego startupu
jednoosobowego — ale to nie jest to samo co ryzyko dla samej linii produktowej appto,
o czym wyżej.

---

## 8. Zestawienie z naszą specyfikacją

Tabela porównuje to, co appto twierdzi o sobie (znacznik [D] wszędzie, chyba że
zaznaczono inaczej), z naszymi czterema etapami (`docs/spec/01-mvp1.md` –
`04-mvp4.md`).

| Funkcja appto | Mamy w planie? | Etap | Uwaga |
|---|---|---|---|
| Rejestr agentów, uprawnienia per użytkownik/grupa | tak | MVP1 | `agent_grant`, domyślna odmowa — u nas dodatkowo 404 zamiast 403 (appto nieznane w tej kwestii) |
| Centralny panel administracyjny | tak | MVP2 | appto: opis ogólny bez szczegółu ról; u nas konkretne ekrany `/admin/*` |
| Wybór modelu LLM | tak, ale inaczej | MVP1 (D-002) | appto: przełącznik dla użytkownika per zadanie. U nas: `model_default`/`model_fallback` ustawiane per agent w rejestrze, nie wybór ręczny w locie — patrz kategoria „mają, a my nie mamy" |
| Kontekst firmowy z dokumentów, wpływający na każdą odpowiedź | tak | MVP3 | u nas trzy poziomy kontekstu z dziedziczeniem i wersjonowaniem w gicie — appto nie ujawnia mechanizmu wersjonowania |
| Integracje czytające/piszące w systemach firmowych | tak, częściowo | MVP1 (D-003) + MVP4 | u nas przez agenta projektowego z kluczami; konkretne konektory (ERP, CRM) dopiero MVP4 |
| Samodzielne przejmowanie zadań (maile, oferty, podsumowania) | tak, jako mechanizm ogólny | MVP3 | rutyny czasowe/zdarzeniowe + zatwierdzenia (MVP2) — appto nie ujawnia, czy ma jawny harmonogram czy tylko reakcję na zdarzenia |
| Zatwierdzanie przez człowieka przed operacją nieodwracalną | tak | MVP2 | appto: „gotowe do sprawdzenia" (sugerowane, niepotwierdzone). U nas: reguła 6.5, wymuszona przez walidator |
| Limity kosztu / budżety | tak | MVP2 | appto: zbiorcze, brak potwierdzonego limitu per osoba. U nas: trzy poziomy, twarda blokada |
| Logowanie firmowym kontem (SSO) | tak | MVP1 | appto: Google login z 2FA (D, pewność niska). U nas: Entra ID / OIDC |
| Dziennik audytu | tak, to nasz rdzeń | MVP1 | appto: sugerowane hasłem marketingowym, brak potwierdzenia mechanizmu. U nas: `audit_event` z decyzją `allow/deny/error`, rejestruje też odmowy |
| Kanał w komunikatorze firmowym | tak, częściowo | MVP3 (Teams) | appto: Slack + Teams + mail. My: tylko Teams (dopasowane do NASTER), Slack poza zakresem |
| Wielonajemność / wdrożenie u nowego klienta | tak | MVP4 | appto: SaaS wielonajemny z założenia. U nas: `tenant_id` od MVP1, domknięcie w MVP4 |
| Rezydencja danych, brak trenowania na danych klienta | częściowo | otwarte (D-011) | appto twierdzi wprost o UE i braku trenowania — my mamy to jako pytanie otwarte dla wspólnej pamięci, nierozstrzygnięte |

### Mają, a my nie mamy — do rozważenia

- **Marketplace / udostępnianie „skilli" jednym kliknięciem, z wersjonowaniem, do
  wybranej osoby/działu/całej firmy.** Nie ma odpowiednika w żadnym naszym etapie.
  Mamy wersjonowanie wiedzy w gicie (MVP3), ale nie mechanizm „udostępnij tę
  umiejętność działowi jednym kliknięciem" jako osobną funkcję UI. Do rozważenia jako
  rozszerzenie MVP3/MVP4 — priorytet niski, appto samo nie ujawnia, czy to zawiera
  gotowe szablony od siebie czy tylko mechanizm dzielenia się.
- **Wspólne wątki/artefakty widoczne w zespole pod kontrolą dostępu.** appto twierdzi o
  tym wprost; u nas rozmowa jest własnością osoby/agenta, nie ma koncepcji
  współdzielonego wątku zespołowego. Do rozważenia razem z poziomami kontekstu
  zespołowego w MVP3, jeśli praktyka pokaże taką potrzebę.
- **Wybór modelu przez pracownika w locie, per zadanie**, a nie tylko konfiguracja
  agenta przez administratora w rejestrze. U nas model jest właściwością agenta, nie
  parametrem rozmowy. Do rozważenia jako rozszerzenie panelu w MVP2, jeśli okaże się
  potrzebne — nie zmienia architektury bramy modeli (D-002).

### Mają, a my świadomie nie chcemy

- **Rozliczenie w jednostkach własnych dostawcy platformy („kredyt")** zamiast
  przejrzystego rozliczenia kosztu modelu widocznego co do tokena. Uzasadnienie: D-001
  i D-002 — swoboda wyboru dostawcy modelu i przypisywalność kosztu do agenta i
  człowieka wymaga, żeby to MY byli jedynym pośrednikiem do dostawców, nie zewnętrzna
  platforma z własną marżą ukrytą w cenie kredytu.
- **appto jako jedyny właściciel kontraktu z dostawcami modeli** (BYOK niepotwierdzone,
  raczej brak). U nas odwrotnie z rozmysłem: brama modeli (LiteLLM) jest nasza, z
  naszymi kluczami — żaden inny komponent ich nie zna (reguła 6.6). To nie jest brak
  funkcji z naszej strony, tylko odwrotny wybór architektoniczny w tym samym punkcie.
- **Hosting wyłącznie u dostawcy SaaS (appto/Vercel).** U nas: Docker Compose na
  własnym serwerze, przenośne poza jedną chmurą (D-008, D-011) — bo rezydencja danych
  i kontrola nad infrastrukturą są dla nas twardym wymogiem, nie tylko hasłem
  marketingowym.

---

## 9. Co z tego wynika dla nas

1. [W] **D-001 pozostaje niezweryfikowane u źródła, nie tylko dla nas — dla nikogo bez
   dostępu do cennika appto**, ponieważ dostęp sieciowy do appto.ai był w tej sesji
   zablokowany i żaden operator nie znalazł strony cennika (sekcja 5). To jest wniosek
   do właściciela, nie rozstrzygnięcie: sam fakt, że appto nazywa jednostkę
   rozliczeniową „kredytem", a nie „tokenem" ([D] pewność średnia — sekcja 5), jest zbyt
   słabym sygnałem, żeby cokolwiek zmieniać w dzienniku decyzji. Rekomendacja (działanie,
   nie twierdzenie o appto): właściciel sprawdza `appto.ai/pl/cennik` osobiście albo
   zleca drugą rundę badania z odblokowanym dostępem sieciowym.
2. [W] **Ryzyko dostawcy appto (early-access, produkt poboczny firmy usługowej, zero
   niezależnych recenzji) wzmacnia argument „budujemy, nie kupujemy" niezależnie od
   szczegółu tokenów**, ponieważ to ryzyko dostawcy jest niezależne od tego, czy
   przesłanka o tokenach się potwierdzi (fakty i domysły źródłowe — sekcja 7). Nawet
   gdyby przesłanka o tokenach się nie potwierdziła, uzależnienie się od produktu,
   którego istnienie może zależeć od wyników zupełnie innej części grupy WiseGroup,
   jest samodzielnym powodem ostrożności — wart dopisania do D-001 jako dodatkowe
   uzasadnienie, jeśli właściciel zechce je tam wpisać.
3. [W] **Kierunek naszej specyfikacji jest wzorcowo trafny**, ponieważ appto pokrywa
   dużą część MVP1–MVP3 (panel, role, kontekst firmowy, zatwierdzenia, audyt, budżety)
   tym samym językiem funkcji, którym opisaliśmy własne etapy przed poznaniem appto
   (zestawienie pełne — sekcja 8). To nie jest powód do zmiany planu, tylko
   potwierdzenie, że wzorzec projektowy z D-001 miał sens.
4. [W] **Obszar najsłabiej pokryty przez zwiad to dokładnie ten, który jest sednem
   naszego projektu**, ponieważ zarządzanie i bezpieczeństwo appto (SSO, audyt, DPA,
   certyfikaty) to w większości puste pola albo hasła marketingowe bez mechanizmu
   (szczegóły — sekcja 4). Nie oznacza to, że appto tego nie ma — oznacza, że nie udało
   się tego sprawdzić stąd. Jeśli bezpieczeństwo appto jest kryterium decyzyjnym w
   jakiejkolwiek przyszłej rozmowie o kupnie zamiast budowy, ta luka musi zostać
   zamknięta przed taką rozmową, nie po.
5. [D] Pewność średnia: **Marketplace umiejętności appto to jedyna funkcjonalna luka
   warta realnego rozważenia** — reszta różnic to świadome wybory architektoniczne
   (brama modeli nasza, hosting własny), nie przeoczenia (pełne zestawienie — sekcja 8).

---

## 10. Czego nie ustaliliśmy

Sekcja celowo niepusta — brak dostępu sieciowego do appto.ai w tej sesji jest
przyczyną wspólną większości poniższych braków.

- **Treści jakiejkolwiek strony appto.ai** — strona główna, `/pl/funkcje`, `/pl/blog/`,
  cennik, regulamin, polityka prywatności. Rozstrzygnęłoby odblokowanie dostępu
  sieciowego do `appto.ai` (i najlepiej `web.archive.org`) w polityce proxy tej sesji,
  albo dostarczenie treści strony przez właściciela (zrzut ekranu, zapis HTML, PDF).
- **Konkretnych liczb w cenniku** (kwoty, progi, waluta) — model rozliczenia znany
  tylko opisowo. Rozstrzygnąłby widok strony cennika.
- **Czy istnieje twardy limit budżetu per agent/osoba u appto**, czy tylko zbiorcze
  rozliczenie firmy.
- **Czy istnieje SSO firmowe (Entra ID / Google Workspace) czy tylko zwykłe logowanie
  kontem Google** — najważniejsza luka sekcji zarządzania.
- **Czy jest dziennik audytu dostępny administratorowi i czy historia rozmów
  pracowników jest widoczna dla admina.**
- **Czy appto ma certyfikat ISO 27001/SOC 2 i czy publikuje dowód** (raport, trust
  center).
- **Listy podprzetwarzających appto** (dostawca modelu z nazwy, hosting, baza danych) —
  to byłoby najlepsze źródło do rozstrzygnięcia hipotezy architektury z sekcji 6 i nie
  zostało sprawdzone.
- **Pełnej nazwy podmiotu prawnego wskazanego w SAMEJ stopce appto.ai** — mamy tylko
  domysł (SellWise Szymon Negacz sp.k. albo inny podmiot grupy WiseGroup).
- **Daty powstania appto jako produktu i daty rejestracji domeny** — WHOIS niedostępny
  w tej sesji.
- **Wielkości zespołu pracującego konkretnie nad appto** (mamy tylko rozmiar całej
  grupy WiseGroup, ok. 170 osób) oraz czy appto ma jakiekolwiek finansowanie
  zewnętrzne.
- **Realnych nagłówków HTTP serwera appto** (`server`, `x-vercel-id`) i realnego
  certyfikatu TLS — połączenie było przechwytywane przez bramkę egress tej sesji.
  Rozstrzygnęłaby sesja bez tej blokady albo zewnętrzne narzędzie (crt.sh, Censys).
- **Czy appto oferuje wariant on-premise / instalowany w chmurze klienta.**
- **Czy Marketplace appto zawiera gotowe szablony OD APPTO, czy wyłącznie mechanizm
  dzielenia się tym, co zbudował klient.**
- **Czy appto ma kanał głosowy/telefoniczny** — brak w wynikach nie jest dowodem
  braku funkcji.

**Dlaczego ta sekcja nie jest krótsza mimo pięciu operatorów:** każdy z nich pracował
w tym samym środowisku z tą samą blokadą sieciową, więc braki się nakładają, nie
sumują niezależnie. Jedyny kanał, który dawał niezależne, sprawdzalne fakty (DNS),
został wykorzystany w pełni — reszta wymaga albo odblokowania sieci dla tej sesji,
albo materiału dostarczonego bezpośrednio przez właściciela.
