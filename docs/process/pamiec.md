# Pamięć projektu — jak doszliśmy do tego, co jest

**Kondensat rozmowy z 22 sierpnia 2026.** Dokument istnieje po to, żeby nowy agent
albo nowa osoba rozumiała **dlaczego** projekt wygląda tak, jak wygląda — łącznie
z wariantami, które odpadły, i błędami, które popełniono po drodze.

To nie jest routing. Aktywny stan jest w `tematy.md` i `handoff.md`.
To jest kontekst.

---

## 1. Punkt wyjścia

**Firma:** NASTER, branża energetyczna. Około 20 osób. Rozliczenia, prowizje,
umowy, numery PPE. Microsoft Teams jako komunikator, Microsoft Entra ID jako katalog.
**Jedna osoba techniczna** — właściciel, wspierany przez Claude Code.

**Impuls:** transkrypcja rozmowy z Guillermo Rauchem (Vercel) o wewnętrznym agencie V,
z którego korzysta blisko tysiąc osób. Pytanie właściciela brzmiało: *jak zbudować
coś podobnego u siebie.*

**Co już istniało:** trzy działające skille do powtarzalnej roboty — podkładki
audytorskie, weryfikacja korekt rozliczeniowych, porządkowanie plików.
To była warstwa umiejętności, zanim powstała jakakolwiek warstwa zarządzania.

## 2. Oś rozumowania — jedenaście faz

### Faza 1 · Wzorzec z transkrypcji
Z rozmowy Raucha wyjęto pięć rzeczy: jeden agent-router zamiast zoo agentów,
agent jako zwykły folder plików, **prawdziwą trudnością są uprawnienia a nie AI**,
zdarzenia zamiast promptów, pętla samodoskonalenia, niezależność od jednego modelu.

### Faza 2 · Trzy drogi (nota 01)
Rozważono: (A) wspólne repozytorium skilli, (B) własny bot na Claude Agent SDK,
(C) framework Eve od Vercela. Microsoft Copilot Studio odpadł decyzją właściciela.

**Kluczowe ustalenie fazy:** Teams nie był wtedy kanałem dla agenta u nikogo —
Claude Tag wystartował wyłącznie dla Slacka, a konektor M365 daje tylko odczyt.
Wniosek: kanał nie jest kryterium wyboru, bo każda droga kończy się aplikacją w Azure.

**Odkrycie poboczne:** Managed Agents od Anthropica jako wariant B bez własnego
serwera — 0,08 USD za godzinę aktywnej sesji, bezczynność darmowa.

### Faza 3 · Wymagania właściciela zmieniają obraz
Właściciel doprecyzował pięć wymagań: uprząż nad wieloma agentami dziedzinowymi,
dostępność przypisywana konkretnym pracownikom, wolny wybór modelu, docelowo Teams
lub Azure, gotowe narzędzia zamiast pisania od zera. Plus: **Claude Code jako
narzędzie do kodowania, nie jako silnik produkcyjny — bo drogi.**

Porównano Eve, Hermesa i Azure Foundry. Buzz i Grok Bot odrzucono.

### Faza 4 · Propozycja właściciela: konto na pracownika
Właściciel zauważył, że skoro Hermes jest otwarty, można mnożyć instancje.
Weryfikacja potwierdziła **trzy z czterech punktów** — w tym obaliła zastrzeżenie
agenta, że wielu agentów wymaga napisania routera (Hermes ma zwielokrotnioną bramkę).

Powstała zasada porządkująca: **granica profilu ma pokrywać się z granicą uprawnień.**

### Faza 5 · Azure wraca, Teams odroczony
Potwierdzono: odpada Copilot jako platforma, Azure jako hosting zostaje.
Sprostowano rozpowszechnione nieporozumienie: **aplikacja wewnętrzna w dzierżawie
nie wymaga weryfikacji Microsoftu** — ta dotyczy wyłącznie sklepu publicznego.

### Faza 6 · Topologia 27 agentów
Właściciel podał konkretną strukturę: 1 zarządzający, 6 projektowych, 20 stanowiskowych.
Weryfikacja wykazała, że **wszystkie trzy potrzebne mechanizmy istnieją w Hermesie**:
wspólny workspace pamięci, dystrybucja profilu przez git, eksport i import profilu.

Zasada: **profil to stanowisko, a nie osoba.** Przejęcie konta to zmiana jednego wpisu.
Decyzja: **agenci stanowiskowi nie mają żadnych kluczy** do systemów firmowych.

### Faza 7 · Materiały: Buzz, Openbot, poradnik budowania
Buzz okazał się otwarty i samohostowalny — ale jest komunikatorem, więc konkuruje
z Teamsem, nie z Hermesem. Openbot (CopilotKit, MIT) był trzy dni na rynku, alfa —
za wcześnie, ale jego wzorzec **bramki z domyślną odmową i audytem prób** przyjęto.

Z poradnika o budowaniu oprogramowania osobistego wzięto cztery pliki trwałej prawdy,
regułę o kopiach zapasowych i rewizję pilota.

### Faza 8 · appto i pytanie kupić-czy-zbudować
Materiał o polskiej platformie appto pokazał, że dostawca **sprzedaje dokładnie tę
warstwę, którą projektowano od czterech not.** Agent postawił pytanie wprost, wbrew
własnemu planowi, i przygotował 31 pytań na demo.

**Właściciel rozstrzygnął:** rozliczenie za tokeny u dostawcy odbiera swobodę wyboru
modelu, więc odpada. Budujemy. appto zostaje **wzorcem projektowym.**

### Faza 9 · Dokumentacja
Plan na jednej stronie plus specyfikacja czterech etapów, dziennik decyzji,
scenariusze. Łącznie 60–74 dni robocze.

### Faza 10 · Proces AutoBot
Właściciel dostarczył uniwersalny szkielet procesu pisany dla innego projektu.
Powstały wiązania projektowe, potem — na jego polecenie — **jeden samowystarczalny
dokument**, ze scaleniem szkieletu, uzupełnieniem ośmiu niezwiązanych zasad
i usunięciem warstwy „ustal per projekt".

### Faza 11 · Pytania ABC
Workflow: 7 Operatorów plus Final Control, wszyscy Sonnet 5 high.
26 pytań, po kontroli 25 w ośmiu tierach.

## 3. Pytania zadane właścicielowi i odpowiedzi

| Pytanie | Odpowiedź | Skutek |
|---|---|---|
| Na czym komunikuje się firma? | **Microsoft Teams** | zdefiniowało kanał docelowy i wykluczyło Slacka |
| Kto utrzyma agenta technicznie? | **Tylko właściciel + Claude** | przesądziło o odrzuceniu wariantów wymagających zespołu |
| Który proces pierwszy? | wskazano cztery naraz | **odrzucone** — wybrano rozliczenia jako jedyny pilot |
| Czy Microsoft wrócił do gry? | **Copilot odpada, Azure zostaje** | odblokowało hosting i Entra ID |
| Ilu agentów na start? | **1 zarządzający, 6 projektowych, 20 stanowiskowych** | zdefiniowało topologię |
| Kupić gotowe czy budować? | **Budować** — rozliczenie za tokeny odbiera wybór modelu | D-001 |
| Orkiestracja wieloagentowa? | **Włączona, Sonnet 5 high** | ECHO-001 |
| Jak zlecać subagentów? | **Zawsze przez workflow** — tylko tam da się ustawić effort | ECHO-002 |

## 4. Warianty odrzucone i powody

| Wariant | Powód odrzucenia |
|---|---|
| Microsoft Copilot Studio | decyzja właściciela; platforma agentowa Microsoftu odpada |
| Własny bot od zera na Agent SDK | projekt platformowy bez zespołu; bus factor równy jeden |
| Buzz (Block) | jest komunikatorem — wdrożenie oznacza zmianę Teamsa |
| Grok Bot (xAI) | jeden dostawca modelu; łamie wymaganie niezależności |
| Openbot (CopilotKit) | alfa, trzy dni na rynku; wzorzec przyjęty, narzędzie nie |
| Azure Foundry jako platforma | uzależnienie od Azure; wraca jako możliwy backend bramy |
| appto (zakup) | rozliczenie za tokeny u dostawcy odbiera wybór modelu |
| Budowa własnego komunikatora | najdroższa i najmniej potrzebna rzecz w projekcie |
| Eve jako silnik | przegrała jednym punktem: brakiem katalogu „kto widzi którego agenta" |

**Uwaga o Eve:** po sprostowaniu błędu o kanale Teams przewaga Hermesa zawęziła się
z dwóch punktów do jednego. Gdyby doszedł deweloper TypeScriptu, wybór mógłby być inny.

## 5. Błędy popełnione i korekty

Odnotowane, bo z nich powstała dyscyplina źródeł (§12 skilla).

| Błąd | Źródło błędu | Korekta |
|---|---|---|
| „Hermes ma panel administracyjny" | podsumowanie w wyszukiwarce | dokumentacja mówi wprost, że nie ma |
| „Eve nie ma kanału do Teams" | opis repozytorium z przykładem Slacka | dokumentacja wymienia Teams jako wbudowany; **zmieniło wynik porównania** |
| „osiem agentów" sugerujące równoległość | brak sprawdzenia limitu współbieżności | dwóch naraz przy 4 CPU |
| „jedyna zależność zewnętrzna MVP1" | pominięcie w handoffie | są trzy; zawiadomienie pracowników ma zegar dłuższy niż etap |

## 6. Zasady wyprowadzone z rozumowania

1. **Nie budujemy agenta — budujemy warstwę zarządzania.** Hermes robi pracę wykonawczą.
2. **Brama modeli należy do nas.** Fundament, nie detal — z niego wynika wolny wybór modelu.
3. **Granica profilu = granica uprawnień.** Nie po pracowniku, nie po firmie — po domenie.
4. **Profil to stanowisko, nie osoba.** Przejęcie konta to zmiana jednego wpisu.
5. **Agenci stanowiskowi nie mają kluczy.** Dwadzieścia profili z kluczami to dwadzieścia wycieków.
6. **Brak dostępu zwraca 404.** Odmowa nie może ujawniać istnienia zasobu.
7. **Nie gonimy parytetu funkcji.** Budujemy pod pięć wymagań, nie pod cudze demo.
8. **Tokenizacja nie zwalnia z umowy powierzenia.** Pseudonimizacja to nadal dane osobowe.
9. **Kopia, której nie odtworzono, nie jest kopią.**

## 7. Stan i co otwarte

**Etap:** przed MVP1. Dokumentacja i proces gotowe, kod nie istnieje.

**Decyzje przyjęte:** D-001…D-009 w `docs/spec/decisions.md`, ECHO-001 i ECHO-002
w `docs/process/echo.md`.

**Otwarte i blokujące:**

| Co | Blokuje |
|---|---|
| D-011 — rezydencja wspólnej pamięci | MVP3 |
| D-010 — topologia agentów | decyzja po MVP1, rejestr obsługuje oba warianty |
| 25 pytań ABC z zestawu nr 1 | pięć pierwszych blokuje start MVP1 |
| Rejestracja w Entra ID | MVP1 od dnia trzeciego |
| Umowa powierzenia z dostawcą modelu | scenariusz 8, dni 11–12 |
| Zawiadomienie pracowników o monitoringu | pilot; **zegar ~2 tygodnie** |

**Najważniejsza rzecz do zapamiętania:** trzy niezależne źródła — Rauch, Negacz
i konstrukcja Eve — układają architekturę jako **poziomy kontekstu w jednym systemie**,
a nie jako mnożenie profili. Topologia 27 profili jest po części obejściem otwartego
błędu wspólnej pamięci w Hermesie, a nie czystym wyborem architektonicznym.
D-010 to rozstrzygnie na danych z MVP1.
