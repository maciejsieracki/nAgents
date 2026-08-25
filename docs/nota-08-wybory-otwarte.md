# Nota 08 — wybory otwarte: zaplecze merytoryczne

NASTER · projekt nAgents · temat `NAG-DEC-001-wybory-otwarte` · 25 sierpnia 2026

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

**Ustalenie:** `00-architektura.md` §1 mówi wprost — „nAgents to warstwa
zarządzania nad flotą instancji Hermesa. Nie jest silnikiem agenta — Hermes
nim jest (…) wykonuje całą pracę: rozmowę, narzędzia, piaskownicę, pamięć,
kanały, wybór modelu." Samo łączenie się z programem firmowym i wykonywanie
w nim czynności (np. zapisanie wyniku, wysłanie maila) jest więc **pracą
Hermesa**, nie naszą. `nota-07` §7 to potwierdza od strony konkurenta:
appto rozwiązuje ten sam problem, kupując dostęp do gotowego pośrednika —
czyli traktuje to jako osobną warstwę wykonawczą, nie jako część własnej
warstwy zarządzania.

**Nasza praca jest węższa, ale realna i dziś nierozstrzygnięta.** Cztery
rzeczy, wszystkie mieszczące się w czterech pytaniach nAgents z
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
lukę dla nAgents.

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
