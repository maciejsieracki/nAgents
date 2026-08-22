# Nota decyzyjna 01 — Trzy drogi do agenta

**NASTER · projekt nAgents · 22 sierpnia 2026**

Repozytorium skilli, własny bot na Claude Agent SDK, albo framework Eve od Vercela.
Co każda z tych dróg naprawdę daje, czego wymaga od firmy — i którą wybrałbym z fotela prezesa.

Wersja do czytania (artefakt): https://claude.ai/code/artifact/16a0904b-7247-4b03-b715-e2bf28ed2763

---

## Rekomendacja

**Wchodzimy drogą A, ale budujemy agenta w strukturze plików Eve. Decyzję o kanale
i hostingu odkładamy o kwartał — i to jest oszczędność, nie zwłoka.**

Powód jest prozaiczny: jedyną osobą techniczną w tym projekcie jest właściciel.
Opcje B i C nie są trudne intelektualnie — są pracochłonne w utrzymaniu, a utrzymanie
spada na jednego człowieka, który ma też firmę do prowadzenia. Droga A daje działającego
agenta w dni zamiast tygodni, a jeśli od pierwszego dnia trzymamy pliki w układzie,
którego oczekuje Eve, przejście na C jest później zmianą opakowania, nie przepisaniem.

| Kiedy | Co | Dlaczego |
|---|---|---|
| Teraz, 4 tyg. | Droga A | Agent jako folder plików. Jeden proces: rozliczenia i prowizje. |
| Kwartał 2 | Warunkowo C | Gdy pierwszy skill jest stabilny i ma evals — migracja po harmonogramy i zatwierdzenia. |
| Gdy będzie dev | Kanał w Teams | Dopiero wtedy B ma sens. Wcześniej to hosting bez zespołu. |

---

## Fakt, który rozstrzyga połowę sprawy: Teams

**Żadna z trzech dróg nie da dziś „@agent" w Teamsie bez własnej aplikacji w Azure.**

- Claude Tag (mechanizm „napisz @Claude na kanale") wystartował w czerwcu 2026
  **wyłącznie dla Slacka**, na planach Team i Enterprise. Wersja dla Teams jest
  zapowiadana, bez daty.
- Dziś Claude sięga do Teams przez konektor Microsoft 365 **tylko do odczytu**:
  potrafi przeszukać rozmowy, ale nie może w Teamsie pisać, odpowiadać ani tworzyć.
- Eve ma katalog `channels/` z przykładem dla Slacka — Teams trzeba dopisać samemu.
- Własny bot (B) oznacza rejestrację aplikacji w Entra ID i Bot Framework, czyli
  tę samą pracę po stronie Microsoftu, którą chcieliśmy ominąć.

**Wniosek:** „Microsoft odpada" wyklucza Copilot Studio jako platformę agenta, ale nie
usuwa Teamsa jako komunikatora firmy. To dwie różne rzeczy. Dopóki kanałem ma być Teams,
każda droga kończy się aplikacją w Azure — więc kanał **nie jest kryterium wyboru**
między A, B i C. Jest osobnym projektem.

---

## Opcja A — wspólne repozytorium skilli w Claude Code

Agent to katalog plików: `instructions.md` (dusza firmy), `skills/` (procesy),
`tools/` (dostęp do systemów). Każdy uruchamia go u siebie, na swoich uprawnieniach.

**Za**
- Zero infrastruktury — nie ma serwera, który padnie w piątek wieczorem.
- Uprawnienia rozwiązane za darmo: agent widzi to, co widzi pracownik (dziedziczy
  z Windows/OneDrive). Przy danych z umów i PPE ma to realne znaczenie.
- Startujemy z połowy drogi — trzy działające skille już istnieją.
- Odwracalne: pliki są przenośne w każdą stronę.

**Przeciw**
- Nie działa, gdy nikogo nie ma — brak nocnych przeglądów i raportów cyklicznych.
- Zasięg ograniczony do osób technicznych (realnie 3–10, nie cała firma).
- Brak centralnego audytu — każdy pracuje u siebie.
- Rozjazd wersji bez dyscypliny w repozytorium.

**Zasoby:** serwer — żaden. Zewnętrznie — tylko API modelu. Koszt: licencje + tokeny.

---

## Opcja B — własny bot na Claude Agent SDK

Aplikacja nasłuchująca wiadomości z komunikatora, uruchamiająca agenta po stronie serwera.
Tą drogą poszedł Vercel przy swoim `@V`.

Co dokładnie oznacza „postawić bota" (wg dokumentacji Anthropica):
każda sesja to osobny proces z własnym katalogiem i transkryptem na dysku;
punkt wyjścia to **1 GiB RAM, 1 CPU i 5 GiB dysku na jednego agenta** (podłoga, nie sufit);
stan ginie przy restarcie kontenera, więc potrzebne jest trwałe przechowywanie transkryptów
(S3/Redis/Postgres), proxy wyjściowe z listą dozwolonych domen, telemetria i izolacja
użytkowników.

**Za**
- Pełna kontrola i pełen audyt — jeden punkt, przez który przechodzi wszystko.
- Dowolny kanał: Teams, mail, webhooki z systemu rozliczeniowego.
- Cała firma jako użytkownicy — jedyna droga realnie obsługująca osoby nietechniczne.
- Dane zostają tam, gdzie chcemy (poza wywołaniami modelu).

**Przeciw**
- To projekt platformowy, nie skrypt: sesje, trwałość, skalowanie, izolacja, telemetria.
- Bus factor = 1. Proces rozliczeniowy zależny od systemu, którego nikt inny nie naprawi.
- Serwer trzeba pilnować: aktualizacje, backupy, klucze, monitoring.
- Buduje się od zera to, co C ma w pudełku (zatwierdzenia, wznawianie, evals).

**Zasoby:** 1 GiB / 1 CPU na agenta + baza, proxy, logi. Kontener ~$0,05/godz.

> Uwaga kosztowa: **rachunek za tokeny przewyższa koszt infrastruktury o rząd wielkości
> lub więcej.** Oszczędzanie na serwerze to optymalizacja niewłaściwej pozycji —
> prawdziwym kosztem drogi B jest czas człowieka.

---

## Opcja B′ — ten sam bot, ale bez serwera (Managed Agents)

Anthropic prowadzi usługę **Managed Agents**: agent i jego piaskownica działają po ich
stronie. Znika cała warstwa, która w drodze B kosztuje najwięcej pracy. Rozliczenie:
tokeny po zwykłych stawkach API + **0,08 USD za godzinę aktywnej sesji**, czas oczekiwania
na człowieka darmowy.

Dla firmy bez zespołu platformowego to sensowniejsza wersja drogi B — nadal piszemy
integrację z kanałem, ale nie utrzymujemy infrastruktury.
**Zastrzeżenie:** usługa jest w becie, a stawki to ceny bety, nie zobowiązanie cenowe.

---

## Opcja C — Eve, framework Vercela z transkrypcji

Ten sam pomysł co A (agent = katalog plików), ale skompilowany do działającej usługi.
Kod otwarty na **Apache 2.0** — bez opłaty licencyjnej i bez przymusu hostowania
u Vercela; sandbox potrafi działać lokalnie na Dockerze.

**Co dostajemy w standardzie**
- Trwałe wykonanie — każdy krok checkpointowany, sesja przeżywa awarię i wdrożenie.
- Piaskownica per agent, w innym kontekście bezpieczeństwa niż harness.
- Zatwierdzenia przez człowieka — wbudowane. Przy przelewach i korektach nieopcjonalne.
- Evals jako element frameworka — pętla samodoskonalenia z transkrypcji.
- Śledzenie OpenTelemetry i subagenci.
- Katalogi `channels/` i `schedules/` — proaktywność jest plikiem, nie projektem.

**Za**
- Najlepszy stosunek bezpieczeństwa do wysiłku.
- Ta sama struktura plików co droga A — migracja to przeprowadzka, nie przepisanie.
- Sprawdzone w boju: Vercel prowadzi na tym własną firmę (~1000 osób).
- Bez uwiązania do jednego modelu.

**Przeciw**
- Beta i warunki bety — framework ma dwa miesiące.
- TypeScript — bez dewelopera wąskim gardłem jest właściciel.
- Pełnia możliwości ciągnie ku infrastrukturze Vercela (Workflow, Sandbox, AI Gateway).
- Kanał do Teams i tak trzeba dopisać (gotowy przykład jest dla Slacka).

---

## Porównanie — sześć wymiarów

| Wymiar | A · Repozytorium skilli | B · Własny bot | C · Eve |
|---|---|---|---|
| **Wpływ na pracę** | Ograniczony — 3–10 osób technicznych, agent działa na żądanie | Największy — cała firma, jeden punkt wejścia, praca w nocy | Duży — harmonogramy i kanały w standardzie |
| **Bezpieczeństwo i RODO** | Dobre z natury — uprawnienia dziedziczone; minus: brak centralnego audytu | Najwyższy pułap, ale największa powierzchnia błędu — izolację trzeba zbudować | Najlepszy stosunek — piaskownica, zgody, ślad audytowy bez pisania od zera |
| **Łatwość wdrożenia** | Dni | Miesiące — wymaga dewelopera, którego nie ma | Tygodnie — wymaga TS, ale nie warstwy platformowej |
| **Zasoby serwerowe** | Zero | 1 GiB + 1 CPU na agenta, plus baza, proxy, monitoring | Przenośne — Docker u siebie albo infrastruktura Vercela |
| **Struktura kosztów** | Tylko zmienne (licencje + tokeny) | Serwer za grosze, ale miesiące pracy; tokeny i tak dominują | Framework za darmo, płacimy za hosting i tokeny |
| **Ryzyko odwrotu** | Żadne — pliki przenośne | Wysokie — własny kod platformowy to koszt utopiony | Umiarkowane — licencja chroni, beta i ciążenie ku Vercelowi nie |

---

## Gdybym siedział w fotelu prezesa

- **Nie kupuję infrastruktury przed sprawdzeniem procesu.** Droga B wymaga decyzji
  o serwerze, zanim będziemy wiedzieć, czy agent rozlicza prowizje lepiej od arkusza.
  To odwrotna kolejność.
- **Największym ryzykiem nie jest technologia, tylko jedna osoba.** Wszystko z drogi B
  ma dokładnie jednego opiekuna. Jeśli proces rozliczeniowy zaczyna od niego zależeć,
  to nie usprawnienie, tylko nowe ryzyko operacyjne.
- **Płacę za odwracalność, bo jest tania.** Ułożenie plików w strukturze Eve od pierwszego
  dnia kosztuje kilka godzin i kupuje możliwość przejścia na C bez przepisywania.
- **Wartość mierzę godzinami, nie funkcjami.** Kryterium wyjścia z etapu pierwszego:
  **trzy zamknięte miesiące przeliczone przez agenta zgadzają się co do złotówki
  z liczeniem ręcznym.** Do tego czasu ani złotówki na kanał i hosting.
- **Proaktywność to moment, w którym wchodzi C.** Gdy pierwszy proces jest sprawdzony,
  największa dźwignia leży w pracy agenta bez nas — wtedy `schedules/` przestaje być
  ciekawostką, a staje się powodem migracji.

---

## Trzy rzeczy do rozstrzygnięcia przed startem

1. **Kanał — czy Teams jest warunkiem koniecznym, czy przyzwyczajeniem?**
   Jeśli koniecznym, aplikację w Azure trzeba wycenić osobno; nie ma jej w tym planie.
   Jeśli wyniki mogą trafiać na SharePoint i mailem — cała ta praca odpada.
2. **Dane osobowe — kto podpisze umowę powierzenia i gdzie mają być przetwarzane dane?**
   Numery PPE i umów to dane osobowe. Plany Team i Enterprise mają umowny zakaz trenowania
   na danych klienta; Enterprise dokłada logi audytowe, SSO i kontrolę retencji
   (przy minimum 20 miejsc). Rezydencję danych trzeba potwierdzić u dostawcy **przed**
   pierwszym prawdziwym plikiem.
3. **Ciągłość — co się dzieje, gdy właściciela nie ma przez dwa tygodnie?**
   Odpowiedź wyklucza drogę B do czasu zatrudnienia dewelopera i przesądza, że pierwszy
   proces musi mieć wersję ręczną jako plan awaryjny.

---

## Źródła

1. [Hosting the Agent SDK](https://code.claude.com/docs/en/agent-sdk/hosting) — wymagania
   sprzętowe, trwałość sesji, izolacja, relacja kosztu infrastruktury do kosztu tokenów.
2. [Introducing eve](https://vercel.com/blog/introducing-eve) i
   [github.com/vercel/eve](https://github.com/vercel/eve) — struktura katalogów,
   sześć wbudowanych możliwości, licencja Apache 2.0.
3. [Claude Tag eyes Microsoft Teams integration](https://cryptobriefing.com/anthropic-claude-tag-microsoft-teams/)
   oraz [Claude + Microsoft Teams: what it can and can't do](https://www.usecarly.com/blog/claude-microsoft-teams-integration/)
   — status Teams, tryb tylko do odczytu przez konektor M365.
4. [Rauch on splitting models from agents](https://techcrunch.com/2026/07/06/vercel-ceo-guillermo-rauch-on-the-fight-to-split-off-models-from-agents/)
   — kontekst strategiczny i skala wewnętrznego agenta Vercela.
5. [Managed Agents — cennik i limity bety](https://www.verdent.ai/guides/claude-managed-agents-pricing)
   — stawka za godzinę sesji, darmowy czas bezczynności, zastrzeżenie o cenach bety.
