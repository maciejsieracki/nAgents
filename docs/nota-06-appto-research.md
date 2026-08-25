# Nota 06 — Research appto.ai

**NASTER · projekt nAgents · temat `NAG-INFO-001-appto-research` → dokończony
w ramach `NAG-INFO-002-katalog-funkcji` · 25 sierpnia 2026**

**Co się zmieniło od poprzedniej wersji tej noty:** poprzednia wersja powstała, gdy
dostęp sieciowy do appto.ai był w tej sesji całkowicie zablokowany, a wszystko, co
wiedzieliśmy o produkcie, pochodziło ze streszczeń wyszukiwarki — bez jednego
potwierdzonego cytatu ze strony. Właściciel dostarczył od tego czasu cztery strony
appto wklejone bezpośrednio (strona główna, cennik, wdrożenie kohortowe,
integracje). Ta wersja jest przepisana na tych czterech źródłach pierwotnych.
Ustalenia o infrastrukturze sieciowej (DNS), które nie zależą od treści stron,
zostały utrzymane z poprzedniej rundy i sprawdzone pod kątem zgodności z nowymi
źródłami w sekcji 6.

**Znaczniki użyte w tekście:**

- **[F] FAKT** — to, co strona appto dosłownie mówi o sobie (mamy cytat), albo
  ustalenie techniczne sprawdzone niezależnie (np. zapytanie DNS). Nie jest to
  potwierdzenie, że produkt naprawdę tak działa — tylko że appto tak twierdzi, albo
  że infrastruktura tak wygląda z zewnątrz.
- **[W] WNIOSEK** — wyprowadzone rozumowaniem z faktów, zawsze z „ponieważ".
- **[D] DOMYSŁ** — prawdopodobne, niepotwierdzone wprost w żadnym z czterech
  źródeł; z poziomem pewności i tym, co by je rozstrzygnęło.

**Przypomnienie ważne dla całej noty:** cztery źródła, na których stoi ta nota, to
materiał sprzedażowy producenta appto — najniższy poziom wiarygodności w naszej
hierarchii źródeł. „appto twierdzi, że…" opisuje wyłącznie to, co firma o sobie
mówi, nigdy nie jest to samo co potwierdzenie, że produkt tak działa. To dotyczy
zwłaszcza sekcji 4 (bezpieczeństwo) — najbardziej wrażliwej kategorii na
naciąganie marketingu na fakt.

---

## 1. Co ustaliliśmy w skrócie

[F] Cztery strony appto.ai — strona główna, cennik, wdrożenie kohortowe,
integracje — są teraz dostępne jako wklejony materiał źródłowy. To pierwsza runda
tego zwiadu z realnym cytatem zamiast streszczenia wyszukiwarki. [F] appto to
platforma AI osadzona w procesach firmy: asystenci przypisani do ról, wiedza
firmowa dostępna bez wklejania jej ręcznie, integracje z narzędziami zewnętrznymi,
rozliczenie w jednostce zwanej „kredytem". [F] Podmiotem stojącym za appto jest,
zgodnie z własną stopką strony, **„Let's Automate Sp. z o.o."** — to koryguje
poprzedni domysł tej noty (szczegóły w sekcji 7). [F] Jednostką rozliczeniową
appto jest **kredyt, nie token** — cennik podaje konkretne liczby (sekcja 5).
**Przesłanka decyzji D-001 nie potwierdza się w świetle tych liczb** — sekcja 5
zawiera pełną korektę i propozycję nowego uzasadnienia do zatwierdzenia przez
właściciela. [W] Funkcjonalnie appto pokrywa się w dużej mierze z naszą
specyfikacją MVP1–MVP3 (pełne zestawienie — `docs/nota-07-katalog-funkcji.md`) —
to potwierdza trafność obranego kierunku, nie podważa go.

---

## 2. Czym jest appto

[F] appto to, w opisie własnym: „AI, którego potrzebuje biznes" — „nie kolejny
czat do wszystkiego i niczego", tylko funkcje zbudowane pod konkretne zadania
zespołu, z wiedzą i narzędziami firmy, dowożące efekt, nie sam tekst. Adresat:
cała firma, jedno miejsce pracy z AI dla całego zespołu.

[F] Trzy ścieżki wdrożenia, wskazane wprost w stopce strony głównej:
„Samodzielnie", „Z opiekunem", „Kohortowo" — ta ostatnia jest osobnym, płatnym
programem doradczym (opisanym w sekcji 5), nie wariantem cenowym samej platformy.

[F] Model sprzedaży platformy: brak przycisku samoobsługowego zakupu. Cennik
kończy się wezwaniem „zostaw kontakt" i „umów demo" — sprzedaż idzie przez
rozmowę handlową, nie przez natychmiastowe założenie konta z kartą płatniczą.

[D] Pewność niska, nieobecne w naszych czterech źródłach: segmentacja branżowa
(e-commerce, agencje, IT) i jakiekolwiek niezależne recenzje (Trustpilot, G2,
Capterra) — żadne z czterech źródeł, będących materiałem samego producenta, nie
zawiera ani nie mogłoby zawierać takiego potwierdzenia z definicji. To pozostaje
nieustalone, nie zaprzeczone.

---

## 3. Funkcjonalność — skrót

Pełny katalog funkcji appto, pogrupowany i zestawiony z naszymi etapami, jest
osobnym dokumentem: `docs/nota-07-katalog-funkcji.md` (54 pozycje w jedenastu
grupach, plus zestawienia „mamy w planie" / „luki" / „odrzucamy"). Tu tylko
najważniejszy szkielet:

[F] appto ma asystentów przypisanych do ról (Wise — ogólny domyślny, Strateg
sprzedaży — oferty i follow-upy), z własnym promptem, dostępem i modelem dla
każdego. [F] Integracje: appto deklaruje **956 narzędzi** na osobnej stronie
integracji, ale **„40+"** na stronie głównej i w cenniku — sprzeczność, którą
samo źródło integracji nazywa niewyjaśnioną, nie naszą pomyłką odczytu. [F]
Praca w tle: rutyny czasowe/zdarzeniowe z powiadomieniem mailem albo na kanał
zespołu. [F] Marketplace: „udostępniaj umiejętności, nie przeklejaj promptów",
z wersjonowaniem i natychmiastową propagacją poprawki do wszystkich użytkowników.

---

## 4. Zarządzanie i bezpieczeństwo — deklaracje appto

To jest sedno naszego projektu, dlatego każde ustalenie poniżej jest oznaczone
wprost jako cytat producenta, nie jako potwierdzony mechanizm.

**Role i uprawnienia.** [F] appto deklaruje trzy poziomy: „Member, Manager,
Admin — każdy ma dokładnie tyle dostępu, ile trzeba. Asystenci, wiedza
i integracje pod jednym zarządem." Brak w źródłach opisu granularności per
integracja czy per agent poza tym ogólnym podziałem.

**Logowanie.** [F] Domyślne logowanie opisane wprost: „wchodzisz kontem Google,
z 2FA, bez kolejnego firmowego hasła". [F] Cennik dodaje szerszą opcję: „zaloguj
zespół przez Google, Microsoft albo własne SSO" — SSO firmowe (w tym
prawdopodobnie Microsoft Entra ID) jest więc deklarowane jako opcja w planach
płatnych, nie tylko zwykłe logowanie kontem prywatnym, jak sugerowała
poprzednia wersja tej noty opartej na streszczeniach. To koryguje wcześniejszą
niepewność w tym punkcie.

**Dziennik zdarzeń / audyt.** [F] „Pełny dziennik zdarzeń — kto, co i kiedy
zrobił — w asystentach, wiedzy i działaniach. Audit log z historią, gotowy na
wymogi zgodności." Deklarowane wprost, z konkretnym sformułowaniem, nie tylko
hasłem ogólnym — mocniejsze potwierdzenie niż w poprzedniej wersji tej noty.

**Zatwierdzanie przez człowieka przed działaniem.** [F] Artefakty (maile,
oferty, dokumenty) opisane jako „gotowe do akceptacji" — sugeruje krok
zatwierdzenia przed wysyłką, choć źródła nie opisują mechanizmu tego kroku
(kto zatwierdza, czy jest to wymuszone czy opcjonalne).

**Limity wydatków, budżety.** [F] Kredyty i koszty pod kontrolą: „pula
współdzielona w zespole i automatyczne doładowanie — koniec niespodzianek na
fakturze". Rozliczenie jest zbiorowe dla zespołu; źródła nie wspominają
twardego limitu per pojedynczą osobę.

**Lokalizacja danych, RODO.** [F] „Hosting w UE — serwery w Irlandii."
[F] „Zgodność z RODO — 2FA, audit log" (strona główna); cennik dodaje „zero-
retention u dostawców modeli" i deklarację zgodności z **DSA** (Digital Services
Act — unijne prawo o platformach obsługujących treści użytkowników trzecich,
np. serwisy społecznościowe czy marketplace'y; prawdopodobnie nieadekwatne dla
appto jako narzędzia B2B, ale to jest nasz wniosek, nie zaprzeczenie ze strony
appto).

**Powierzenie przetwarzania, lista podprzetwarzających.** [F] Brak — żadne
z czterech źródeł nie zawiera umowy powierzenia przetwarzania danych (skrót
branżowy: DPA) ani listy podprzetwarzających (podwykonawców, którym appto
przekazuje dane — np. dostawca modelu, hosting). Pole całkowicie puste, tak
jak w poprzedniej wersji tej noty.

**Certyfikaty (ISO 27001, SOC 2).** [F] Brak wzmianki w żadnym z czterech
źródeł. Brak potwierdzenia nie jest równoznaczny z zaprzeczeniem.

**BYOK — własny klucz API do dostawcy modelu.** [F] Brak wzmianki. Model
rozliczenia w kredytach (sekcja 5) sugeruje pośrednictwo appto we wszystkich
wywołaniach modeli, ale żadne źródło nie potwierdza ani nie wyklucza wariantu
z własnym kluczem klienta.

**Polityka prywatności / regulamin.** [F] Adresy istnieją (`/pl/polityka-
prywatnosci/`, `/pl/regulamin/`, `/pl/polityka-cookies/`), wymienione w nawigacji
strony głównej — ale treści tych stron nie mamy, właściciel ich nie wkleił.
Rozstrzygnęłoby to dostarczenie tych trzech stron.

Podsumowanie: appto deklaruje więcej konkretu w tej rundzie niż w poprzedniej
(cytaty wprost, nie parafrazy wyszukiwarki), ale nadal nic z tego nie jest
niezależnie zweryfikowanym mechanizmem — to, co mamy, to spójna, dość szczegółowa
obietnica marketingowa, nie audyt bezpieczeństwa.

---

## 5. Model rozliczenia — konkretne liczby i odpowiedź w sprawie D-001

[F] Hasło i zasada: „Płacisz, gdy zarabiasz. Rozliczasz się za realną pracę, nie
za miejsca w zespole. Jednostką jest kredyt — appto zużywa go tylko wtedy, gdy
naprawdę coś dla Was zrobi."

[F] **Konkretny rozkład ceny, plan do 10 osób** (jedyny w pełni podany wprost
w cenniku):

| Składnik | Cena |
|---|---|
| Platforma, do 10 osób | 330 zł |
| 80 000 kredytów, stawka 20 zł / 10 tys. | 160 zł |
| **Razem** | **490 zł / mc netto** |
| Kredyty poza abonamentem (top-up) | 22 zł / 10 tys. |

[F] Drugi punkt cenowy z kalkulatora: **12 osób, 180 000 kredytów miesięcznie →
990 zł/mc.** Teza producenta o zwrocie: koszt zwraca się przy odzyskaniu
14 minut tygodniowo na osobę z 40-godzinnego tygodnia — niesprawdzalne bez
danych o realnym zużyciu czasu zespołu, nie traktujemy tego jako fakt.

[F] Progi konfiguratora: do 10, do 25, do 50, do 100 osób. Powyżej — wycena
indywidualna („wdrożenie enterprise").

[F] Zasady puli: wspólna dla całej firmy, odnawia się co miesiąc, **niewykorzystane
kredyty nie przechodzą**, top-up w trakcie miesiąca po wyższej stawce niż
w abonamencie (22 zł vs 20 zł / 10 tys.).

[F] **Wszystkie modele dostępne bez dopłat, przełączane przez użytkownika**:
„Wszystkie modele AI — Claude Opus i Sonnet, GPT, Gemini, Kimi — bez dopłat,
przełączasz w dwa kliki" (sekcja „Każdy plan to pełne appto"). Mocniejszy model
zużywa więcej kredytów za to samo zadanie, ale nie ma osobnej dopłaty ani planu
wymagającego droższego poziomu, żeby w ogóle uzyskać dostęp do danego modelu.

[F] Widełki zużycia kredytów wg rodzaju zadania (przykłady z cennika):
odpowiedź na maila 280–600 kr., streszczenie spotkania 770–1540 kr., analiza
arkusza 1460–2930 kr., research wieloźródłowy 5280–10 560 kr.

[F] Brak nazw planów (Starter/Team/Enterprise) — jedyna oś to liczba osób
i suwak kredytów.

[F] BYOK (własny klucz API do dostawcy modelu) i wariant on-premise
(instalacja u klienta zamiast w chmurze appto): brak wzmianki w żadnym
z czterech źródeł — nierozstrzygnięte, tak jak poprzednio.

### Odpowiedź wprost na pytanie o D-001 — KOREKTA WYMAGANA

**Zapisane uzasadnienie D-001** (`docs/spec/decisions.md`): „rozliczenie za
tokeny u dostawcy odbiera swobodę wyboru modelu, co jest wymaganiem numer
trzy. Wymóg podłączania dowolnych modeli przeważył nad oszczędnością czasu."

**Cennik appto tego nie potwierdza.** Jednostką rozliczeniową jest kredyt, nie
token przeliczany wprost od dostawcy modelu. Wszystkie wymienione modele
(Claude Opus i Sonnet, GPT, Gemini, Kimi) są dostępne w każdym planie, bez
dopłat za konkretny model, i przełączalne przez użytkownika „w dwa kliki".
Jedyna zależność ceny od modelu to liczba zużytych kredytów za zadanie — silniejszy
model kosztuje więcej kredytów, ale nie jest zablokowany planem ani dopłatą.
**To jest odwrotność tego, co mówi zapisane uzasadnienie** — appto nie odbiera
swobody wyboru modelu, tylko różnicuje koszt korzystania z niego.

**To jest korekta przesłanki, nie decyzji.** Decyzja o budowie własnej
platformy pozostaje w mocy — z woli właściciela, wyrażonej wprost 2026-08-25:
„naszym celem jest osiągnąć to samo i te same funkcjonalności, ale jako własna
platforma." Ta nota nie zmienia `docs/spec/decisions.md` — to zastrzeżone dla
właściciela. Poniżej propozycja poprawionego uzasadnienia, do zatwierdzenia albo
odrzucenia.

**Dlaczego to ważne, jednym zdaniem:** decyzja słuszna, ale oparta na
nieprawdziwej przesłance, przewróci się przy pierwszym, kto tę przesłankę
sprawdzi.

**Propozycja poprawionego uzasadnienia D-001** (do zatwierdzenia przez
właściciela, nie wprowadzone do `decisions.md` przez tę notę):

> Budujemy własną platformę, bo chcemy, żeby koszt każdego zapytania był
> policzalny wprost — w realnej walucie i w tokenach zużytych u dostawcy modelu,
> przypisany do konkretnego agenta i konkretnego człowieka (D-002) — a nie
> ukryty za jedną, uproszczoną jednostką sprzedawcy platformy z jego własną
> marżą, jak „kredyt" appto. Chcemy też, żeby nikt poza nami nie pośredniczył
> w kontrakcie z dostawcami modeli i w rezydencji danych (D-011, bariera 1) —
> a nie dlatego, że appto formalnie ogranicza wybór modelu, bo cennik appto
> tego nie potwierdza. Dodatkowo: appto wygląda na produkt wczesnego etapu
> (early access, brak niezależnych recenzji), wydany przez firmę, dla której to
> nie jest główny udokumentowany biznes (sekcja 7) — to samodzielne ryzyko
> dostawcy, niezależne od modelu rozliczenia, i osobny powód do budowy własnej
> platformy zamiast uzależnienia się od cudzej.

Warianty odpowiedzi właściciela: (a) przyjąć powyższe brzmienie w całości,
(b) przyjąć część (np. samą korektę o kredycie, bez akapitu o ryzyku
dostawcy), (c) sformułować własne uzasadnienie, (d) uznać, że zmiana
uzasadnienia nie jest potrzebna i pozostawić zapis bez zmian mimo tej korekty.

---

## 6. Domeny i infrastruktura — ustalenia z poprzedniej rundy, sprawdzone pod kątem zgodności

Poniższe ustalenia pochodzą z zapytań DNS wykonanych w poprzedniej rundzie tego
zwiadu (niezależnie od treści stron, więc nie mogą być ani potwierdzone, ani
zaprzeczone przez cztery nowe źródła tekstowe) — utrzymane tu bez zmian, bo
żadne z czterech źródeł im nie przeczy. **Fragment techniczny, dla porządku:**
DNS to adresowa „książka telefoniczna" domeny appto.ai — mówi, na jakim
serwerze stoi strona i przez jaką pocztę appto wysyła maile. Można pominąć,
jeśli nie interesuje Was warstwa techniczna infrastruktury:

[F] `NS appto.ai` → `ns1.vercel-dns.com`, `ns2.vercel-dns.com` — pełne
przekazanie strefy DNS na nameservery Vercela.
[F] `MX appto.ai` → wyłącznie `smtp.google.com` — poczta firmowa appto.ai idzie
przez Google Workspace.
[F] `TXT appto.ai` → SPF z `include:mail47.mydevil.net` i `include:_spf.google.com`,
wpisy `google-site-verification` i `hubspot-developer-verification`.
[W] Ponieważ obsługa DNS jest w całości przekazana na `vercel-dns.com`, appto
najprawdopodobniej hostuje przynajmniej warstwę frontendową na platformie
Vercel, ponieważ pełne przekazanie strefy DNS na nameservery Vercela jest
wymagane przez tę platformę wyłącznie przy faktycznym podłączeniu domeny.

[D] Pewność średnia, nierozstrzygnięte: hosting na Vercelu nie dowodzi
konkretnego frameworka frontendu; hipoteza, że appto jest warstwą
pośredniczącą (routerem) nad kilkoma zewnętrznymi dostawcami modeli, opiera
się wyłącznie na tym, co appto twierdzi o sobie w materiale sprzedażowym
(wybór modelu w jednym miejscu), nie na żadnym śladzie technicznym
niezależnym od marketingu.

Nowość tej rundy, z czterech źródeł: **hubspot-developer-verification** w
rekordzie TXT domeny (ustalone niezależnie od treści stron w poprzedniej
rundzie) jest teraz spójne z tym, że appto wymienia HubSpot jako jedną
z integracji katalogowanych na stronie integracji — to nie jest nowy dowód,
tylko potwierdzenie, że poprzednie ustalenie DNS i nowa treść źródeł nie są ze
sobą sprzeczne.

---

## 7. Kto stoi za produktem

**Korekta wobec poprzedniej wersji tej noty.** Poprzednia wersja domyślała się
(pewność średnia, źródło pośrednie — streszczenie materiału wideo), że podmiotem
prawnym za appto jest **SELLWISE SZYMON NEGACZ SPÓŁKA KOMANDYTOWA** (KRS
0000922240) — bo to ten sam podmiot, który figuruje w regulaminie wisegroup.pl.
**Źródła pierwotne tej rundy dają inną, bardziej bezpośrednią odpowiedź:**

[F] Stopka strony głównej appto.ai: **„© 2026 appto · Let's Automate Sp. z o.o."**
— to jest podmiot wskazany przez samo appto, nie domysł wyprowadzony z innej
strony grupy.

[F] Strona wdrożenia kohortowego wymienia wprost troje ludzi za produktem:

| Osoba | Funkcja podana na stronie |
|---|---|
| **Szymon Kita** | CEO appto · Let's Automate |
| **Szymon Negacz** | Founder WiseGroup |
| **Filip Kulikowski** | Head of WiseTools |

[F] „Wdrażamy AI w firmach od 2023 roku" — deklaracja doświadczenia grupy,
opisanej jako trzy światy: szkolenia i strategia, wdrożenia, własna technologia
AI.

[W] Zestawiając te dwa fakty: appto jest produktem spółki **Let's Automate
Sp. z o.o.**, która działa w ramach grupy marek **WiseGroup** (założonej przez
Szymona Negacza) — „Let's Automate" pojawiał się już w poprzedniej wersji tej
noty jako jedna z sześciu marek WiseGroup, ale bez wskazania, że to właśnie ta
konkretna spółka, nie SellWise sp.k., stoi bezpośrednio za appto. To jest
poprawka do zanotowania: **SellWise sp.k. i Let's Automate Sp. z o.o. to
prawdopodobnie dwa różne podmioty prawne w tej samej grupie kapitałowej** —
poprzednia nota mogła wskazywać niewłaściwy z dwóch, bo opierała się na
regulaminie innej marki grupy (wisegroup.pl), nie na stopce appto.ai samego.

[D] Pewność średnia, nierozstrzygnięte przez te cztery źródła: numer KRS, NIP
i adres siedziby „Let's Automate Sp. z o.o." — żadne z czterech źródeł ich nie
podaje. Rozstrzygnąłby to bezpośredni odczyt regulaminu appto.ai albo
wyszukiwarki KRS pod tą nazwą.

[F] Program wdrożeniowy: pierwsza kohorta ma **30 miejsc** w pakietach
z mentorem, start **wrzesień 2026**, ceny **7900 / 19 900 / 29 900 zł netto**.
Adresat: „głównie zarządy polskich firm 10–500 osób". To potwierdza, że appto
celuje w segment małych i średnich firm, zgodny z wielkością NASTER (ok.
dwudziestu osób).

[F] Studium przypadku „firma B2B usługowa, 35 osób" jest przez sam producent
oznaczone jako **wdrożenie testowe, liczby zaokrąglone, bez nazwy firmy** — nie
jest to referencja klienta i nie nadaje się do traktowania jako dowód (patrz
`docs/nota-07-katalog-funkcji.md`, część B).

### Ocena ryzyka dostawcy

[W] appto pozostaje produktem, co do którego żadne z czterech źródeł nie
wskazuje niezależnej weryfikacji (recenzji, referencji klienta poza jednym
zaznaczonym jako testowe wdrożenie) — ponieważ wszystkie cztery źródła to
materiał wyprodukowany przez samego sprzedawcę. To nie zmienia się względem
poprzedniej rundy: mamy teraz więcej konkretu o tym, co appto twierdzi, ale
wciąż zero niezależnego potwierdzenia z zewnątrz. Ryzyko dostawcy — appto jako
młody produkt organizacji, dla której doradztwo sprzedażowe i szkolenia (marki
WiseGroup/SellWise) są głównym, dłużej udokumentowanym biznesem — pozostaje
wnioskiem, nie ustaleniem obalonym albo potwierdzonym przez te cztery strony.

---

## 8. Czego nadal nie wiemy

- **Treści stron appto.ai spoza czterech dostarczonych**: `/pl/funkcje/`,
  `/pl/zastosowania/`, `/pl/polityka-prywatnosci/`, `/pl/regulamin/`,
  `/pl/polityka-cookies/`, `/pl/kontakt/`, `/pl/webinar/`, `/pl/partnerzy/` —
  wszystkie wymienione w nawigacji strony głównej, żadna nie dostarczona.
  Rozstrzygnęłoby to wklejenie ich treści przez właściciela albo odblokowanie
  dostępu sieciowego do domeny w tej sesji.
- **Numer KRS/NIP „Let's Automate Sp. z o.o."** — nowa, bardziej precyzyjna luka
  względem poprzedniej rundy (sekcja 7).
- **Czy istnieje twardy limit budżetu per pojedynczy agent/osoba** u appto, czy
  tylko zbiorcze rozliczenie zespołu.
- **Czy appto ma certyfikat ISO 27001/SOC 2** i czy publikuje dowód (raport,
  trust center).
- **Listy podprzetwarzających appto** (dostawca modelu z nazwy, hosting, baza
  danych) — najlepsze źródło do rozstrzygnięcia hipotezy o architekturze
  pośredniczącej appto (sekcja 6), nadal nie sprawdzone.
- **Czy appto oferuje wariant on-premise / instalowany w chmurze klienta.**
- **Czy Marketplace appto zawiera gotowe szablony OD APPTO**, czy wyłącznie
  mechanizm dzielenia się tym, co zbudował klient.
- **Czy appto ma kanał głosowy/telefoniczny** — brak w źródłach nie jest
  dowodem braku funkcji.
- **Czy DSA rzeczywiście dotyczy appto** — deklaracja zgodności jest w cenniku,
  ale żadne źródło nie tłumaczy, dlaczego prawo o platformach z treścią
  użytkowników trzecich miałoby dotyczyć narzędzia B2B. Możliwe, że to
  nadgorliwa deklaracja marketingowa — do wyjaśnienia, gdyby temat zgodności
  prawnej appto był kiedyś istotny dla decyzji.

**Rekomendacja, nie rozstrzygnięcie:** jeśli którakolwiek z powyższych luk stanie
się istotna dla przyszłej decyzji (np. porównawczej), właściciel powinien albo
dostarczyć brakujące strony bezpośrednio, albo rozważyć odblokowanie dostępu
sieciowego do `appto.ai` w polityce proxy tej sesji dla kolejnej rundy badania.
