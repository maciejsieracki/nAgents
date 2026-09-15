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

**Ta runda, tego samego dnia:** właściciel dostarczył dwa kolejne źródła — politykę
prywatności i regulamin appto wraz z umową powierzenia przetwarzania (Załącznik
nr 1). To dokumenty prawne, nie sprzedażowe — mają wagę dowodową wyższą niż cztery
strony wyżej: gdzie się rozchodzą, wiąże treść prawna, a sama rozbieżność jest
ustaleniem. Nowe ustalenia z tych dwóch źródeł są w skorygowanych fragmentach
sekcji 4 i 7 oraz w nowych sekcjach 8 i 9. **Najważniejsze pojedyncze
sprostowanie** — poprzednia wersja tej noty (sekcja 4) błędnie twierdziła, że
umowa powierzenia przetwarzania nie istnieje; to było błędem wynikającym
z niepełnych źródeł, poprawionym niżej wprost w tym samym miejscu.

**Znaczniki użyte w tekście:**

- **[F] FAKT** — to, co strona appto dosłownie mówi o sobie (mamy cytat), albo
  ustalenie techniczne sprawdzone niezależnie (np. zapytanie DNS). Nie jest to
  potwierdzenie, że produkt naprawdę tak działa — tylko że appto tak twierdzi, albo
  że infrastruktura tak wygląda z zewnątrz.
- **[W] WNIOSEK** — wyprowadzone rozumowaniem z faktów, zawsze z „ponieważ".
- **[D] DOMYSŁ** — prawdopodobne, niepotwierdzone wprost w żadnym z czterech
  źródeł; z poziomem pewności i tym, co by je rozstrzygnęło.

**Przypomnienie ważne dla całej noty:** cztery pierwotne źródła, na których stoi
ta nota, to materiał sprzedażowy producenta appto — najniższy poziom
wiarygodności w naszej hierarchii źródeł. „appto twierdzi, że…" opisuje wyłącznie
to, co firma o sobie mówi, nigdy nie jest to samo co potwierdzenie, że produkt
tak działa. To dotyczy zwłaszcza sekcji 4 (bezpieczeństwo) — najbardziej
wrażliwej kategorii na naciąganie marketingu na fakt. **Wyjątek stanowią dwa
źródła prawne dodane w tej rundzie** (polityka prywatności, regulamin z umową
powierzenia) — to dokumenty, za których treść Let's Automate sp. z o.o. odpowiada
wobec kontrahenta i organu nadzorczego, więc mają wagę dowodową najwyższą, nie
najniższą, i tam gdzie się rozchodzą z materiałem sprzedażowym, to one wiążą.

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
osobnym dokumentem: `docs/nota-07-katalog-funkcji.md` (58 pozycji w jedenastu
grupach, po dodaniu czterech pozycji z dokumentów prawnych w tej rundzie, plus
zestawienia „mamy w planie" / „luki" / „odrzucamy"). Tu tylko
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

**Lokalizacja danych, RODO — z korektą po dokumentach prawnych.** [F] „Hosting
w UE — serwery w Irlandii." [F] „Zgodność z RODO — 2FA, audit log" (strona
główna); cennik dodaje „zero-retention u dostawców modeli" i deklarację
zgodności z **DSA** (Digital Services Act — unijne prawo o platformach
obsługujących treści użytkowników trzecich, np. serwisy społecznościowe czy
marketplace'y; prawdopodobnie nieadekwatne dla appto jako narzędzia B2B, ale to
jest nasz wniosek, nie zaprzeczenie ze strony appto).

[F] **Polityka prywatności dodaje warstwę, o której cztery strony sprzedażowe
milczą:** dane mogą być przekazywane do **szesnastu państw trzecich** — Wielka
Brytania, Kanada, USA, Chile, Brazylia, Izrael, Arabia Saudyjska, Katar, Indie,
**Chiny**, Korea Południowa, Japonia, Singapur, Tajwan, Indonezja, Australia —
w związku z narzędziami dostarczanymi m.in. przez Google LLC (część na
podstawie decyzji Komisji Europejskiej o adekwatności, część na podstawie
standardowych klauzul umownych, w tym USA, Chiny i Arabia Saudyjska).

**Dlaczego to niekoniecznie sprzeczność:** „serwery w Irlandii" opisuje, gdzie
stoi sama aplikacja appto; szesnaście państw trzecich to inna warstwa —
podmioty przetwarzające, którym appto zleca fragmenty przetwarzania (np.
narzędzia analityczne, narzędzia AI/LLM). Lokalizacja aplikacji i przepływ
danych do poddostawców to dwie różne rzeczy, więc obie deklaracje mogą być
prawdziwe jednocześnie.

**Dlaczego mimo to ma znaczenie:** hasło „dane zostają u Was" na stronie
sprzedażowej tej różnicy nie ujawnia i sugeruje więcej niż polityka faktycznie
gwarantuje. Firma oceniająca dostawcę pod kątem ochrony danych, czytając
wyłącznie materiał sprzedażowy, nie dowie się o przekazaniu danych do Chin czy
Arabii Saudyjskiej — dowiaduje się o tym dopiero z dokumentu prawnego, który
przeciętny kupujący rzadko czyta w całości przed zakupem. Dla nas, jako
przyszłego dostawcy analogicznych zapisów, to wzór tego, czego **nie** warto
kopiować w komunikacji sprzedażowej: skrót niebędący kłamstwem, ale
niewspółmierny do pełnego obrazu w dokumencie wiążącym.

**Powierzenie przetwarzania — SPROSTOWANIE (2026-08-25).** Poprzednia wersja tej
sekcji twierdziła: „Brak — żadne z czterech źródeł nie zawiera umowy powierzenia
przetwarzania danych (skrót branżowy: DPA) (…) Pole całkowicie puste." **To
twierdzenie było błędne.** Powód: opierało się wyłącznie na czterech źródłach
sprzedażowych, które o umowie powierzenia w ogóle nie wspominają — nie dlatego,
że appto jej nie ma, tylko dlatego, że strona sprzedażowa nie jest miejscem, gdzie
taki dokument by się pojawił. Właściciel dostarczył tego samego dnia regulamin
appto, który zawiera **Załącznik nr 1 — umowę powierzenia przetwarzania danych,
zawartą w trybie art. 28 RODO** (przepis nakładający na administratora obowiązek
zawarcia takiej umowy z każdym podmiotem przetwarzającym dane w jego imieniu),
jako integralną część regulaminu.

[F] Najważniejsze postanowienia umowy: zakres — wszelkie dane osobowe wprowadzone
do aplikacji przez klienta; dane szczególnych kategorii (art. 9/10 RODO — np.
dane o zdrowiu, poglądach, wyrokach) **wyłączone** z zakresu powierzenia;
zgłoszenie naruszenia w **48 godzin** od wykrycia; realizacja żądań osób,
których dane dotyczą, w **48 godzin**; udostępnienie dokumentów na żądanie
w **14 dni**; zwrot i usunięcie danych po zakończeniu współpracy w **14 dni
roboczych**; podpowierzenie dalszym podmiotom na zasadzie zgody ogólnej
udzielonej z góry, z prawem sprzeciwu klienta w ciągu 7 dni od zawiadomienia —
ale skutkiem sprzeciwu jest prawo appto do natychmiastowego odstąpienia od
umowy głównej, co czyni to prawo sprzeciwu w praktyce trudnym do wykonania bez
utraty usługi (ocena, czy to zgodne z art. 28 RODO, wymaga prawnika — nie
rozstrzygamy tego tu).

**Co nadal pozostaje nieznane:** wyłącznie wykaz podprocesorów z nazwami
własnymi (regulamin odsyła do `appto.ai/podprocesorzy`, strony nie mamy — patrz
sekcja 10). Sama umowa powierzenia jako dokument **istnieje i jest dostępna** —
to jedyna rzecz, którą to sprostowanie zmienia względem poprzedniej wersji.

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

[F] **Dane rejestrowe potwierdzone tego samego dnia** wprost z polityki
prywatności appto (dokument prawny, piąte źródło tej rundy) — luka z poprzedniej
wersji tej noty jest tym samym zamknięta:

| Pole | Wartość |
|---|---|
| Nazwa | Let's Automate sp. z o.o. |
| Siedziba | ul. Nowy Świat 33/13, 00-029 Warszawa |
| KRS | 0000972542 |
| Sąd rejestrowy | Sąd Rejonowy dla m.st. Warszawy, XII Wydział Gospodarczy |
| NIP | 5252908405 |
| REGON | 52206686800000 |
| **Kapitał zakładowy** | **10 000 zł, w całości opłacony** |

Kapitał zakładowy w tej wysokości to ustawowe minimum dla spółki z ograniczoną
odpowiedzialnością — odnotowane jako fakt rejestrowy, nie ocena zdolności
majątkowej spółki (rozwinięte w sekcji 9, ryzyko 6).

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

## 8. Rozbieżności między materiałem sprzedażowym a dokumentami prawnymi

Regulamin i polityka prywatności wiążą; strona główna, cennik i wdrożenie
kohortowe to materiał sprzedażowy. Poniżej pięć miejsc, w których się rozchodzą
— sprawdzone otwierając oba typy źródeł, nie tylko cytując jedną stronę.

| # | Co mówi strona sprzedażowa | Co mówi dokument prawny | Charakter | Dlaczego ma znaczenie |
|---|---|---|---|---|
| 1 | „Hosting w UE — serwery w Irlandii"; „Dane zostają u Was" | Polityka: dane mogą trafiać do **szesnastu państw trzecich**, w tym Chin i Arabii Saudyjskiej | Uzupełnienie, nie dosłowna sprzeczność (rozwinięte w sekcji 4) | Kupujący czytający tylko stronę sprzedażową nie dowie się o przekazaniu do Chin |
| 2 | „Pula odnawia się co miesiąc" (sugeruje przepadanie kredytów) | § 9 ust. 7: „Kredyty nie wygasają (…) nie mają okresu ważności" | Sprzeczność literalna, częściowo do pogodzenia rozróżnieniem kredytów abonamentowych i dokupionych — regulamin tego rozróżnienia jednak nie zapisuje wprost | Klient może planować budżet zakładając przepadanie puli, podczas gdy dokument wiążący mówi przeciwnie |
| 3 | „Pełne appto — żadnych ukrytych poziomów ani dopłat za premium" | § 8: funkcjonalność dzieli się na moduły; aktywacja części następuje na indywidualną wycenę, z osobną opłatą | Sprzeczność częściowa — prawdziwe tylko dla modułów objętych cennikiem | Klient dowiaduje się o dopłatach dopiero przy próbie włączenia modułu |
| 4 | „Dane zostają u Was" | § 4 ust. 3–4: stały dostęp administracyjny usługodawcy do wszystkich Treści Usługobiorcy, ograniczony tylko co do usuwania/zmiany, nie co do wglądu | Sprzeczność w warstwie znaczeniowej — **największa waga praktyczna z całej tabeli** | Klient wprowadzający dane firmowe powinien wiedzieć o stałym dostępie wglądowym dostawcy; hasło sprzedażowe sugeruje przeciwnie |
| 5 | Zgodność z **DSA** wymieniona w cenniku jako atut bezpieczeństwa, obok RODO | Polityka i regulamin rzeczywiście przywołują DSA (art. 16/20, punkt kontaktowy z art. 11–12) | Uzupełnienie w sensie literalnym, ale DSA reguluje platformy pośredniczące w treściach **osób trzecich** — appto jako narzędzie B2B jest nietypowym adresatem | Powołanie może być formalnie prawdziwe, a jednocześnie sugerować szerszy zakres regulacyjny niż appto naprawdę reprezentuje — ocena należy do prawnika |

**Ustalenie dodatkowe:** sama strona sprzedażowa ma wewnętrzną sprzeczność
liczbową — „40+ integracji" wobec „956 narzędzi" (sekcja 3, `nota-07`). To nie
jest rozbieżność strona-kontra-prawo, ale obniża wiarygodność materiału
sprzedażowego jako źródła w ogóle.

---

## 9. Ryzyka dostawcy, uszeregowane

To jest część, o którą właściciel pytał, prosząc o research konkurencji.
Uszeregowane od najpoważniejszego. Właściciel przy własnej platformie znajdzie
się po drugiej stronie analogicznych zapisów — stąd wartość tej części także
dla nas, nie tylko dla oceny appto jako dostawcy.

**1. Trwałe usunięcie danych po 30 dniach zaległości (§ 10 ust. 9).** Po 30
dniach od powstania zadłużenia dane, konto i subkonta są trwale usuwane — bez
okresu przejściowego poza samym brakiem dostępu do edycji w trakcie zaległości.
Najgorszy przypadek: spór o fakturę albo awaria płatności powodują
nieodwracalną utratę całej historii pracy zespołu. Do wynegocjowania
prawdopodobnie tylko w umowie enterprise — nic w źródłach nie wskazuje na taką
możliwość w planie standardowym.

**2. Granica odpowiedzialności ograniczona do trzech miesięcy opłat
(§ 14 ust. 8).** Przy planie 490–990 zł/mc daje to pułap rzędu 1500–3000 zł.
Limit nie działa przy **rażącym niedbalstwie** (skrajnie niestaranne działanie,
odbiegające od minimum ostrożności, jakiego można oczekiwać) i **winie
umyślnej** (celowe działanie ze świadomością skutku) — próg trudny do wykazania
przez klienta. Dodatkowo wyłączona **rękojmia** (ustawowe uprawnienie kupującego
do żądania naprawy albo obniżenia ceny wadliwego świadczenia, niezależne od
odrębnej gwarancji) oraz odpowiedzialność za utracone korzyści. Najgorszy
przypadek: błąd automatyzacji powoduje szkodę biznesową rzędu dziesiątek albo
setek tysięcy złotych — odzyskanie ograniczone do ułamka rocznego kosztu
subskrypcji. Standardowa klauzula SaaS, możliwa do podniesienia tylko
w negocjacjach enterprise.

**3. Dostęp administracyjny usługodawcy do treści klienta (§ 4 ust. 3–4).**
Patrz też sekcja 8, punkt 4. Najgorszy przypadek: pracownik dostawcy, złośliwie
albo przez błąd, przegląda poufne dokumenty firmowe klienta — nic w źródłach
nie ogranicza tego prawa wglądu w standardowej umowie.

**4. Sprzeciw wobec podpowierzenia kończący umowę (Załącznik nr 1).**
Podpowierzenie kolejnym podprzetwarzającym odbywa się na zgodę ogólną z góry;
klient może się sprzeciwić w 7 dni, ale skutkiem jest prawo appto do
natychmiastowego odstąpienia od umowy głównej. Prawo sprzeciwu istnieje
formalnie, ale jego wykonanie kosztuje utratę usługi — w praktyce iluzoryczne
dla klienta zależnego od appto operacyjnie. Czy taka konstrukcja jest zgodna
z art. 28 RODO w części o rzeczywistej możliwości sprzeciwu — wymaga oceny
prawnika, nie rozstrzygamy tego tu.

**5. Brzmienie licencji na treści i opinie klienta (§ 13 ust. 21–26).**
Przesłanie „Treści Usługobiorcy" lub opinii jest równoznaczne z udzieleniem
appto nieodpłatnej licencji niewyłącznej, bezterminowej (wypowiadalnej z
dwuletnim wyprzedzeniem), z prawem udzielania dalszych licencji podmiotom
trzecim i z rezygnacją klienta z wykonywania **autorskich praw osobistych**
(więź twórcy z własnym utworem — prawo do autorstwa i do integralności dzieła;
inna kategoria niż prawa majątkowe, które można sprzedać). Czytane literalnie
razem z definicją „Treści Usługobiorcy" (§ 2 pkt 15 — wszelkie dane i pliki na
koncie), zapis obejmowałby licencję na wszystkie dokumenty firmowe, nie tylko
opinie o produkcie. To prawdopodobnie wada redakcyjna, ale treść zapisu jest
szersza niż prawdopodobna intencja — rozstrzygnięcie należy do prawnika, nie do
nas.

**6. Kapitał zakładowy w wysokości ustawowego minimum (10 000 zł).** Przy
sporze o odszkodowanie przewyższające limit z ryzyka 2, zdolność majątkowa
spółki do pokrycia zobowiązań jest ograniczona strukturalnie — kapitał
zakładowy nie jest jedynym majątkiem spółki, ale jest sygnałem, nie gwarancją
wypłacalności. Fakt rejestrowy, nie zapis umowny — nie do wynegocjowania.

**7. Zgoda marketingowa na nazwę i logo klienta (§ 16 ust. 1).** O ile odrębna
umowa nie stanowi inaczej, klient z góry udziela zgody na wykorzystanie swojej
nazwy i logo w materiałach marketingowych appto. Do wynegocjowania — ale wymaga
inicjatywy klienta, nie jest opt-in.

---

## 10. Czego nadal nie wiemy

Po dwóch rundach źródeł (cztery strony sprzedażowe, dwa dokumenty prawne)
zostaje **jeden** rodzaj luki, nie kilka jak w poprzednich wersjach tej noty —
KRS/NIP i istnienie umowy powierzenia zostały rozstrzygnięte w tej rundzie
(sekcje 4 i 7).

**Wykaz podprocesorów z nazwami** — `appto.ai/subprocessors` i
`appto.ai/pl/podprocesorzy` — nie jest jednym z sześciu dostarczonych źródeł;
oba dokumenty prawne wprost do niego odsyłają, zamiast podawać nazwy. To
jedyny dokument, który powiedziałby wprost: kto jest dostawcą modelu (albo
dostawcami — polityka mówi mnogo o „dostawcach narzędzi AI/LLM"), co
pozwoliłoby zweryfikować hipotezę o appto jako bramce nad kilkoma modelami
(sekcja 6); kto fizycznie hostuje bazę i aplikację, nazwą, nie kategorią
„firma hostingowa"; które z szesnastu państw trzecich odpowiadają któremu
konkretnie podprzetwarzającemu.

Poza tym: czy istnieje twardy limit budżetu per pojedynczy agent/osoba, czy
tylko zbiorcze rozliczenie zespołu; czy appto ma certyfikat ISO 27001/SOC 2;
czy Marketplace appto zawiera gotowe szablony od samego appto, czy wyłącznie
mechanizm dzielenia się tym, co zbudował klient; czy appto ma kanał
głosowy/telefoniczny (brak w źródłach nie jest dowodem braku funkcji); czy DSA
rzeczywiście dotyczy appto (deklaracja jest, uzasadnienie regulacyjne — nie,
patrz sekcja 8 punkt 5).

**Rekomendacja, nie rozstrzygnięcie:** jeśli wykaz podprocesorów stanie się
istotny dla przyszłej decyzji (np. porównawczej albo przy ocenie ryzyka
łańcucha dostaw appto), właściciel powinien albo dostarczyć treść strony
bezpośrednio, albo rozważyć odblokowanie dostępu sieciowego do `appto.ai`
w polityce proxy tej sesji.
