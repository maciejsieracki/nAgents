# NAGENTS-PROCESS.md — pakiet stagingowy

STATUS: STAGING_ONLY
DOMAIN: INFORMACYJNY
TEMAT: NAG-CONSOLIDATE-PROCESS-Q1
FAZA: Operator
RUNDA: 1 z 3
GENERATED_AT_UTC: 2026-09-14T18:52:37Z

Ten plik jest kandydatem pakietu `NAGENTS-PROCESS.md`, nie zmianą kanonicznej
normy. Źródła procesu pozostają na swoich miejscach i zachowują pierwszeństwo.
Pakiet nie nadaje uprawnień, nie zmienia decyzji, nie zastępuje live readbacku
Kanbana/Git/usługi i nie jest publikacją.

## 0. Zakres i sposób czytania

Cel pakietu: zebrać w siedmiu sekcjach normę procesu, rejestr tematów, handoff,
dispatch, evidence, watchdog, recovery oraz granicę pracownik–proces techniczny.
Wartości projektowe 8gent są oddzielone od reguł przenośnych; historyczne
raporty i snapshoty nie stają się normą przez samo umieszczenie tutaj.

Źródła wskazane dla tej fali:

- `CLAUDE.md`
- `docs/process/tematy.md`
- `docs/process/zmiana-procesu.md`
- `docs/process/handoff.md`
- `docs/process/dispatch/SZABLON.md`
- `docs/process/dispatch/NAG-PROC-005-skill-uniwersalny.md`
- `docs/process/dispatch/NAG-INFRA-002-pomocnik-serwerowy.md`
- `docs/proces-dla-pracownikow.md`
- `NAGENTS-PROJECT.md`
- `NAGENTS-CONSOLIDATION-PLAN.md`
- `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json`

Pakiet korzysta dodatkowo z kanonicznego `docs/process/echo.md`, projektowego
`.claude/skills/nagents-autobot/SKILL.md` oraz źródła uniwersalnego wskazanego
przez P4. Ich ścieżki, hashe, statusy i zakres użycia są w `coverage.json`.

Reguła interpretacji: `CANONICAL`/`SOURCE` opisuje źródło, `EVIDENCE` opisuje
dowód konkretnego przebiegu, `HISTORY` opisuje kontekst, a `LIVE_READBACK_REQUIRED`
oznacza, że statyczny plik nie wystarcza. `STAGING_ONLY` opisuje ten wytwór,
nie stan źródeł.

## 1. Mapa sekcji i pokrycia P6

| Sekcja | Zakres | Pierwsze źródło normatywne | P4 / zakres | Status pakietu |
|---|---|---|---|---|
| `PROCESS-01` | role i pętla faz | `SKILL.md` §3–§4, `CLAUDE.md` §Proces | `NAG-PROCESS` 7/73 | `STAGING_ONLY` |
| `PROCESS-02` | ABC/ECHO i zmiana procesu | `SKILL.md` §8, `zmiana-procesu.md` §1–§7, `echo.md` | `NAG-PROCESS` 7/73 | `STAGING_ONLY` |
| `PROCESS-03` | dispatch, GOAL, allowlista, izolacja, raport | `SKILL.md` §1, §4–§6, §9, §16 | `NAG-EVIDENCE` 7/84; `NAG-AUDIT` 2/2 | `STAGING_ONLY` |
| `PROCESS-04` | evidence, readback, event/run/receipt, integracja | `SKILL.md` §1, §3–§4, §9, §12; `tematy.md` | `NAG-EVIDENCE` 7/84 | `STAGING_ONLY` |
| `PROCESS-05` | watchdog, limity, fail-closed, recovery | `SKILL.md` §4.5–§4.7, §10–§11; `NAG-INFRA-002` | `NAG-AUDIT` 2/2 | `STAGING_ONLY` |
| `PROCESS-06` | P1–P7, owner gates, cleanup | `NAGENTS-CONSOLIDATION-PLAN.md` §2–§9 | `NAG-AUDIT` 2/2; P4 | `STAGING_ONLY` |
| `PROCESS-07` | pracownik versus norma techniczna, kontrakt raportu | `SKILL.md` §9, §14, §16; przewodnik pracownika | `NAG-USER` 1/12 jako materiał pomocniczy | `STAGING_ONLY` |

Macierz P6 przypisuje dokładnie `PROCESS-01`…`PROCESS-07` do powyższych
zakresów (`NAGENTS-CONSOLIDATION-PLAN.md` §4.3, lines 119–129). Dla każdego
fragmentu oznaczonego `F-*` `coverage.json` podaje źródło, hash, status i locator.

## 2. Rozdział normy, historii i bieżącego stanu

### Norma

Norma obowiązująca jest w `CLAUDE.md`, `.claude/skills/nagents-autobot/SKILL.md`,
kanonicznych plikach `docs/process/**` oraz w jednoznacznych wpisach ECHO. Jej
zmiana ma własny temat domeny `PROCES`, uzasadnienie z konkretnego przypadku,
oddzielny dispatch i niezależne sprawdzenie.

### Historia i evidence

Dispatch, raport, artefakt i snapshot P4 opisują konkretną fazę lub przebieg.
Nie ustanawiają globalnego stanu. `NAG-EVIDENCE` zachowuje immutable evidence
indeksowane po pełnym ID/run/event; `HISTORY` zachowuje tok rozumowania i stare
warianty. Nie wolno wybierać nowszego lub podobnie nazwanego pliku bez
porównania hashy i statusu.

### Bieżący stan

Bieżący stan rozstrzyga świeży readback: Git/worktree, karta i rodzice Kanbana,
run, event terminalny, receipt, profil/usługa lub test wykonany na wskazanej
wersji. Ten pakiet nie udaje takiego odczytu. `PASS` w raporcie, nazwa worktree,
status interfejsu, `queued` ani deklaracja wykonawcy nie są same w sobie dowodem
integracji, publikacji ani wdrożenia.

### Kolejność rozstrzygania konfliktu

1. świeży stan wykonany i odczytany niezależnie;
2. jednoznaczna decyzja właściciela w ECHO/dzienniku;
3. aktualna specyfikacja i `CLAUDE.md`;
4. aktualna norma AutoBot;
5. bieżący handoff;
6. raport fazy;
7. noty, pytania, stare handoffy i rozmowy.

Każdy konflikt bez rozstrzygnięcia jest `UNKNOWN`, `INFRA` albo
`DECISION_REQUIRED`; pakiet nie wygładza go własnym domysłem.

[FRAGMENT F-18] Źródła: `CLAUDE.md` | `8de583cc4fe98c55c49b5fac9f1562669040204f571ee2daacc9905e9390d2` | `CANONICAL` | §Kolejność czytania, §Siedem barier; `NAGENTS-PROJECT.md` | `84cc8a89c0e7bd53268fc78826938bec062f9223340aa2e0934d64719764a2fb` | `CONSOLIDATION_CANDIDATE` | §1, §2.1, §11A; `docs/process/handoff.md` | `16f22b7a1bad1d22226be206f8b3f827a4b9f24eca6ef3d6c4f00bc663ac06f8` | `CANONICAL` | §Gdzie jesteśmy, §Następna bramka.

## PROCESS-01 — role i pętla faz

STATUS: STAGING_ONLY
TARGET: przyszły `NAGENTS-PROCESS.md`, bez zastąpienia skilla
UNIQUE_FRAGMENTS: `F-01`, `F-02`, `F-03`

ŹRÓDŁA FRAGMENTU:

- `.claude/skills/nagents-autobot/SKILL.md` | `aa5aab364e0a86c6053e46532d835a35a466357a81c562021bf78555dbb6ac3e` | `CANONICAL` | §3, §4.3–§4.7, §9
- `CLAUDE.md` | `8de583cc4fe98c55c49b5fac9f1562669040204f571ee2daacc9905e9390d2` | `CANONICAL` | §Proces, §Siedem barier
- `docs/process/tematy.md` | `9257a6cd1c08ce221a7e9be1039e07019dac82d1d37024aa11f25875da59c125` | `CANONICAL` | lines 3–6, 10–23, 45–64
- `docs/process/dispatch/NAG-INFRA-002-pomocnik-serwerowy.md` | `ce5ac57d3f65942aaad53771a43a8597bc6588f0cac86aee1ae925029f3fc158` | `SOURCE` | lines 15–46
- `docs/process/zrodla/autobots-szkielet-uniwersalny.md` | `74c72b76a65b14341b87c9f9d805ea84245a361b14dd29f2eb1621bb93a42858` | `CANONICAL` | pełny szkielet, reguły przenośne; referencja, nie lokalna norma

### Role

- `Operator` wykonuje jeden temat w izolacji, wyłącznie w allowliście. Nie
  ocenia własnej pracy, nie integruje wyniku i nie przekazuje go na zewnątrz.
  Przy decyzji produktowej zatrzymuje się jako `DECISION_REQUIRED`.
- `Evaluator` jest niezależnym adwokatem diabła. Czyta artefakt, powtarza
  kryteria i sprawdza zakres, bariery, scenariusze, sekrety, usunięcia,
  kolizje i zgodność GOAL. Nie naprawia po cichu i nie publikuje.
- `Defense` nie jest stałym krokiem. Uruchamia się wyłącznie, gdy Evaluator
  wystawi niepustą, numerowaną listę zarzutów. Odpowiada na każdy numer
  `PRZYJMUJĘ` albo `ODRZUCAM` z dowodem; nie rozszerza zakresu.
- `Final Control` jest niezależny od Operatora i zlecającego. Kontroluje
  rzeczywisty wytwór, ślad dispatchu, ID, rundę, werdykt i rejestr. Dla zarzutów
  wydaje `NAPRAW`, `ODDAL` albo `DO DECYZJI CZŁOWIEKA`.
- `Orkiestrator` prowadzi właściciela, tworzy dispatch, utrzymuje rejestry,
  wykonuje readback i jako jedyny może włączyć zatwierdzony wynik do stanu
  obowiązującego. Dopiero wtedy może oznaczyć `READY_FOR_DEPLOY`.
- `Właściciel` rozstrzyga koszt, dane, dostęp, prawo, ryzyko, zakres i
  odwracalność w głównym wątku. Kanał techniczny nie przyjmuje decyzji za
  właściciela.

### Pętla

```text
dispatch → Operator → Evaluator
                         ├─ brak numerowanych zarzutów → Final Control
                         └─ zarzuty → Defense → Final Control
                                               → readback Orkiestratora
                                               → włączenie do stanu obowiązującego
                                               → READY_FOR_DEPLOY
                                               → osobna bramka przekazania
```

`PASS` Evaluatora uruchamia Final Control. `PASS-WITH-NOTES` kończy tylko wtedy,
gdy uwagi są kosmetyczne i stają się osobnym tematem; uwaga o GOAL, dowodzie,
zakresie, barierze lub integracji wraca jak `FAIL`. `FAIL`, `BLOCK`, `TIMEOUT`,
`INFRA`, `ZWIS`, brak artefaktu lub błąd izolacji wraca pod tym samym ID i
z zachowanym licznikiem rund. Przy podziale na węzły wraca wyłącznie wadliwy
węzeł, a pozostałe nie są ponownie wykonywane.

Final Control nie oznacza publikacji. Po jego `PASS` Orkiestrator sprawdza
faktyczny stan i allowlistę, a przekazanie na zewnątrz pozostaje osobną bramką
właściciela. Worker nie wykonuje merge, push, deploy ani publikacji.

## PROCESS-02 — ABC/ECHO i bezpieczna zmiana procesu

STATUS: STAGING_ONLY
TARGET: przyszły `NAGENTS-PROCESS.md`; rejestr ECHO pozostaje osobno
UNIQUE_FRAGMENTS: `F-04`, `F-05`, `F-06`

ŹRÓDŁA FRAGMENTU:

- `.claude/skills/nagents-autobot/SKILL.md` | `aa5aab364e0a86c6053e46532d835a35a466357a81c562021bf78555dbb6ac3e` | `CANONICAL` | §7–§8, §12–§15, §21.5
- `docs/process/zmiana-procesu.md` | `2c2d366e3ed54a28dae14b47aa78387366e20d2c9b20b1eac9c286af380e0546` | `CANONICAL` | §1–§7, lines 16–107
- `docs/process/echo.md` | `e631fb90812066690a1c1a927abb885472157c2607f8c0ba4f2e6ad56996bbde` | `CANONICAL` | §Format, §Wpisy, ECHO-001–003
- `docs/process/dispatch/NAG-PROC-005-skill-uniwersalny.md` | `de4fa883b9581186f424920a31e46cb1fea683ece5f96ce300b3325250320b83` | `EVIDENCE` | lines 21–57, 87–98
- `CLAUDE.md` | `8de583cc4fe98c55c49b5fac9f1562669040204f571ee2daacc9905e9390d2` | `CANONICAL` | §Zasady pracy z właścicielem

### Kiedy pytać właściciela

Decyzja musi poprzedzać realizującą ją pracę, gdy dotyczy kosztu, danych,
dostępu, prawa, ryzyka, zakresu, odwracalności, retencji/audytu, modelu,
bramy, bazy albo którejkolwiek bariery. Techniczną kwestię bez tych skutków
rozstrzyga Orkiestrator i informuje; nie przerzuca jej na właściciela.

Pytanie ma sytuację, cel i powód „dlaczego teraz”, a następnie warianty A/B/C.
Każdy wariant musi mieć co najmniej dwa argumenty `ZA` i dwa `PRZECIW`.
Rekomendacja jest oznaczona jako rekomendacja, nie jako decyzja; warianty
opisują skutek dla firmy, nie sam mechanizm techniczny.

### ECHO

Niejasna odpowiedź, „chyba”, milczenie, luźna rozmowa ani rekomendacja agenta
nie są decyzją. Po jednoznacznej odpowiedzi literą wpis trafia do
`docs/process/echo.md` w formacie:

```text
<ID pytania> = <litera>
data: <RRRR-MM-DD>
kto: <właściciel>
pytanie: <jedno zdanie>
wariant: <znaczenie wybranej litery>
skutek: <ADR, jeśli potrzebny>
```

Dopiero po tym można kontynuować ten sam temat i to samo ID. W źródle są
literalne wpisy `ECHO-001` (bezterminowa zgoda na delegowanie), `ECHO-002`
(workflow z jawnym modelem i effortem) i `ECHO-003` (potwierdzenie ECHO-001
przy sprzeczności), a także wpisy audytu i pomocnika. Ten pakiet ich nie
zmienia ani nie tworzy nowej decyzji.

### Zmiana samego procesu

Zmiana `.claude/skills/**`, `CLAUDE.md` albo `docs/process/**` jest osobnym
tematem domeny `PROCES`, nigdy dodatkiem do tematu produktowego. Najpierw
zamyka lub świadomie zawiesza dotknięte tematy, zapisuje przypadek, który
wykazał brak obecnej reguły, tworzy dispatch, pracuje w izolacji i uruchamia
niezależnego Evaluatora oraz Final Control. Zmiana bariery, limitu rund,
puli, progu `ZWIS` albo ścieżek zakazanych wymaga ABC/ECHO. Uniwersalny szkielet
właściciela jest read-only; różnice 8gent zapisuje się w skillu projektowym.
Zmiana wchodzi z datą, ID, powodem i śladem korekty.

## PROCESS-03 — dispatch, GOAL, allowlista, izolacja i raport

STATUS: STAGING_ONLY
TARGET: przyszły `NAGENTS-PROCESS.md`; dispatchy i artefakty zostają evidence
UNIQUE_FRAGMENTS: `F-07`, `F-08`, `F-09`, `F-21`

ŹRÓDŁA FRAGMENTU:

- `.claude/skills/nagents-autobot/SKILL.md` | `aa5aab364e0a86c6053e46532d835a35a466357a81c562021bf78555dbb6ac3e` | `CANONICAL` | §1, §4.1–§4.2, §5–§6, §9, §11.3.2–§11.3.3, §16
- `docs/process/dispatch/SZABLON.md` | `6f6317cfba20f461c1a62f3bf4edfe4f1fd987e3e57aa140c4e222717ed5b0a1` | `EVIDENCE` | lines 3–58
- `docs/process/dispatch/NAG-PROC-005-skill-uniwersalny.md` | `de4fa883b9581186f424920a31e46cb1fea683ece5f96ce300b3325250320b83` | `EVIDENCE` | lines 59–98
- `docs/process/dispatch/NAG-INFRA-002-pomocnik-serwerowy.md` | `ce5ac57d3f65942aaad53771a43a8597bc6588f0cac86aee1ae925029f3fc158` | `SOURCE` | lines 22–69
- `NAGENTS-CONSOLIDATION-PLAN.md` | `b282d9e49bf77994a290fbfab71803217d02c9565ab8809e6fbf33501ca4b15c` | `SOURCE` | §3 lines 77–90, §4.3 lines 119–129

### Minimalny dispatch

Dispatch powstaje przed startem Operatora i ma pełne ID, domenę, datę, rundę,
wyzwalacz, jednozdaniowy GOAL, binarne kryteria końca z numerami scenariuszy,
zakres i wyłączenia, allowlistę, izolację, plan testów, zależności, decyzje i
bariery. „Bo była kolej” nie jest wyzwalaczem. Bez pliku dispatchu nie da się
sprawdzić, czy GOAL przesunął się w trakcie.

Dla węzła zlecenia obowiązkowo dochodzą cztery pola: wąskie zadanie, reguła
przeciw samooszukiwaniu, binarne `PRAWDA/FAŁSZ` i z góry zapisana procedura
naprawcza. To nie są synonimy: kryterium sprawdza kompletność wyniku, a reguła
anty-samooszukiwaniu zakazuje sposobu uznania błędnej pracy za gotową.

### Allowlista i izolacja

Allowlista jest per temat i per pozycja, nigdy „cały projekt”. Dla 8gent
proces obejmuje odpowiednie `docs/process/**`, `.claude/skills/**` i `CLAUDE.md`,
ale nie pliki z sekretami, `.env*`, `docs/spec/decisions.md`, `.git/**` ani
konfigurację produkcyjną bez jawnej zgody. Zmiana procesu nie jedzie w
allowliście tematu produktowego; dostaje osobny temat `PROCES`.

Jeden temat ma jedno worktree i jeden aktywny przebieg Operatora. Ustalona
postać 8gent to `../nagents-<ID>`, branch `auto/<ID>`, baza wskazana przez
właściciela, nie domyślnie `main`. Współdzielony checkout wymaga allowlisty
per plik/hunk; nigdy `git add -A` ani `git add .`.

### Plan sprawdzenia i raport

Sprawdzenie obejmuje pełny dowód wykonania, obszar tematu, ręczne scenariusze z
kryteriów końca, a przy dotknięciu uprawnień także obowiązkowe scenariusze A2 i
A3. Kryterium bez numeru scenariusza jest niekompletne. Raport terminalny jest
destylatem, nie surowym logiem:

```text
STATUS: PASS | PASS-WITH-NOTES | FAIL | BLOCK | TIMEOUT | INFRA | DECISION_REQUIRED
DOMAIN: PRODUKT | PROCES | INFRA | INFORMACYJNY
TEMAT:  NAG-<ETAP>-<NNN>-<slug>
GOAL:   <jedno zdanie>
ZMIANY: <allowlista + punkt kontrolny albo brak zmian>
TESTY:  <dowód + numery scenariuszy + sprawdzenie ręczne>
BLOKADY: <lista albo brak>
NASTĘPNY KROK: <kolejna bramka>
DEPLOY/PUSH: NIE WYKONANO
```

Raport węzła ma domyślny limit 400 słów. `DEPLOY/PUSH: WYKONANO` może wpisać
wyłącznie Orkiestrator po osobnym poleceniu właściciela i dokładnym wskazaniu
celu.

## PROCESS-04 — evidence, readback, event/run/receipt i integracja

STATUS: STAGING_ONLY
TARGET: pakiet procesu jako indeks; raw evidence pozostaje osobno
UNIQUE_FRAGMENTS: `F-10`, `F-11`, `F-20`

ŹRÓDŁA FRAGMENTU:

- `.claude/skills/nagents-autobot/SKILL.md` | `aa5aab364e0a86c6053e46532d835a35a466357a81c562021bf78555dbb6ac3e` | `CANONICAL` | §1.1–§1.2, §3–§4, §9, §12, §14.3
- `docs/process/tematy.md` | `9257a6cd1c08ce221a7e9be1039e07019dac82d1d37024aa11f25875da59c125` | `CANONICAL` | lines 3–6, 10–23, 45–64
- `NAGENTS-PROJECT.md` | `84cc8a89c0e7bd53268fc78826938bec062f922334aa2e0934d64719764a2fb` | `CONSOLIDATION_CANDIDATE` | §1, §3.3, §11A
- `docs/process/dispatch/SZABLON.md` | `6f6317cfba20f461c1a62f3bf4edfe4f1fd987e3e57aa140c4e222717ed5b0a1` | `EVIDENCE` | lines 24–53
- `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json` | `95d2a06fc80e4cf1c27d59959d84b8631e70acbb3e0b879b83df95c38bf382b8` | `P4_AUDIT` | `logical_group_rollups`, `path_family_classifications`, `record_classifications`

### Co jest dowodem

Najwyżej stoi świeży odczyt faktycznego stanu: karta, rodzice, run, event,
receipt, artefakt, Git/worktree, usługa albo test wykonany na tej wersji. Potem
stoi literalna decyzja właściciela; dalej specyfikacja i kanoniczna norma.
Handoff jest snapshotem, a raport Operatora/Evaluatora/Final Control jest
dowodem określonej fazy, nie globalnym stanem.

`PASS` w raporcie, obecność pliku, nazwa brancha/worktree, status UI, deklaracja
„zrobione”, brak artefaktu i `queued` nie dowodzą zakończenia. Brak eventu
terminalnego albo rozbieżny readback oznacza `UNKNOWN`/`INFRA`, a nie ręczne
podniesienie statusu.

### Minimalny ślad

Dla każdego przebiegu zachowaj:

- pełne, niezmienne ID tematu i numer runu;
- dispatch z GOAL, allowlistą i kryteriami;
- artefakt z hashem, statusem i locatorami;
- raport terminalny każdej roli;
- event terminalny oraz niezależny readback karty/runu;
- receipt dostarczenia; `queued` nie oznacza `settled`;
- wpis rejestru tematów zgodny z faktycznym stanem;
- przy integracji — dokładny ref/commit/plik oraz oddzielny readback.

Dostarczenie ma przejść `queued → claimed → settled`. Stagingowy pakiet może
indeksować evidence, lecz nie może przedstawiać statycznego dispatchu jako
terminalnego eventu. Pozytywny Final Control prowadzi do `INTEGRATION_REQUIRED`:
Orkiestrator dopiero wtedy może wykonać dozwolone włączenie, a publikacja jest
kolejną bramką.

## PROCESS-05 — watchdog, limity, fail-closed i recovery

STATUS: STAGING_ONLY
TARGET: przyszły pakiet procesu; implementacja pomocnika pozostaje osobnym tematem
UNIQUE_FRAGMENTS: `F-12`, `F-13`, `F-14`

ŹRÓDŁA FRAGMENTU:

- `.claude/skills/nagents-autobot/SKILL.md` | `aa5aab364e0a86c6053e46532d835a35a466357a81c562021bf78555dbb6ac3e` | `CANONICAL` | §4.5–§4.7, §10–§11.5
- `docs/process/dispatch/NAG-INFRA-002-pomocnik-serwerowy.md` | `ce5ac57d3f65942aaad53771a43a8597bc6588f0cac86aee1ae925029f3fc158` | `SOURCE` | lines 15–46, 60–75
- `docs/process/echo.md` | `e631fb90812066690a1c1a927abb885472157c2607f8c0ba4f2e6ad56996bbde` | `CANONICAL` | lines 137–146
- `NAGENTS-CONSOLIDATION-PLAN.md` | `b282d9e49bf77994a290fbfab71803217d02c9565ab8809e6fbf33501ca4b15c` | `PLAN_ONLY / OWNER_HOLD_REQUIRED` | §7 lines 438–484
- `docs/process/NAGENTS-DOCS-CONSOLIDATION-PLAN.md` | `b05ff225880269d35f6833953f896b374f8ebace971e0a196688dfd3b87cc282` | `SOURCE / LOCAL_ONLY` | §2–§4, current local readback

### Watchdog i pojemność

Stałe tokeny procesu to `Watchdog` i `ZWIS`. W 8gent obowiązuje jeden aktywny
przebieg Operatora na temat, próg ciszy 20 minut i pula dwóch tematów
równolegle, bo każdy wynik musi przejść realny przegląd. Przy `ZWIS` najpierw
sprawdź przebieg, worktree i artefakty; nie anuluj ani nie restartuj w ciemno.

Dla wykonawcy-programu limit technicznego fan-outu wynosi
`min(16, liczba_CPU − 2)`. To nie jest to samo co pula tematów: pierwsze
zależy od zasobów kontenera, drugie od pojemności przeglądu. Model, effort i
sposób delegowania są jawne; w 8gent praca subagenta idzie przez workflow.

### Pomocnik i recovery

Przyjęty wariant A to serwerowy, nadzorowany pomocnik niezależny od Desktopu.
Cron jest generatorem read-only dyspozycji; pomocnik wykonuje wyłącznie
kwalifikowane, wcześniej zatwierdzone przejścia. Każde przejście wymaga
readbacku karty, runu, eventu, artefaktu i receiptu. Pomocnik nie tworzy nowego
zakresu, nie rozstrzyga właścicielskich decyzji, nie zmienia GOAL/allowlisty i
nie wykonuje push/merge/deploy/restartu bez osobnej bramki. Desktop jest klientem,
nie rodzicem procesu.

Recovery jest fail-closed:

- `FAIL`, `BLOCK`, `TIMEOUT`, `INFRA`, `UNKNOWN`, `DECISION_REQUIRED`, crash,
  orphan albo brak dowodu zatrzymuje tylko właściwy strumień;
- nie odblokowuje następcy i nie tworzy Defense bez zarzutów;
- zachowuje to samo ID, GOAL, rundę, evidence i idempotency key;
- replay tej samej dyspozycji reużywa kwalifikowanego przebiegu, nie tworzy
  drugiego runu, successora ani dostarczenia;
- po limicie trzech prób Orkiestrator eskaluje do właściciela zamiast zerować
  licznik lub zmieniać nazwę tematu;
- po braku jednoznaczności pomocnik zatrzymuje właściwy strumień i eskaluje,
  nie podejmuje decyzji za człowieka.

Canary z zamkniętym Desktopem i readback następnej fazy są kryterium osobnego
tematu implementacyjnego. Ten pakiet opisuje kontrakt, nie dowodzi canary.

## PROCESS-06 — playbook P1–P7, owner gates i sprzątanie

STATUS: STAGING_ONLY / OWNER_HOLD_REQUIRED_FOR_PUBLICATION
TARGET: przyszły pakiet procesu; brak przeniesień i usunięć
UNIQUE_FRAGMENTS: `F-15`, `F-16`, `F-17`

ŹRÓDŁA FRAGMENTU:

- `NAGENTS-CONSOLIDATION-PLAN.md` | `b282d9e49bf77994a290fbfab71803217d02c9565ab8809e6fbf33501ca4b15c` | `PLAN_ONLY / OWNER_HOLD_REQUIRED` | §2 lines 79–161, §3 lines 165–195, §4–§9
- `docs/process/NAGENTS-DOCS-CONSOLIDATION-PLAN.md` | `b05ff225880269d35f6833953f896b374f8ebace971e0a196688dfd3b87cc282` | `SOURCE / LOCAL_ONLY` | §2–§9
- `docs/process/zmiana-procesu.md` | `2c2d366e3ed54a28dae14b47aa78387366e20d2c9b20b1eac9c286af380e0546` | `CANONICAL` | §3–§5
- `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json` | `95d2a06fc80e4cf1c27d59959d84b8631e70acbb3e0b879b83df95c38bf382b8` | `P4_AUDIT` | `logical_group_rollups` i `path_family_classifications`

### Kolejność faz

```text
P1 zakres audytu
 → P2 inwentaryzacja
 → P3 źródła prawdy
 → P4 duplikaty i nieaktualność
 → P5 aktualizacja indeksu
 → P6 plan konsolidacji
 → OWNER_HOLD / decyzja właściciela
 → P7 zatwierdzona paczka publikacyjna
```

Każda faza ma własną kartę, rodzica, run, idempotency key, workspace, artefakt,
Operatora, niezależnego Evaluatora i Final Control. Defense powstaje tylko przy
numerowanych zarzutach. `P6` opisuje treść i macierz; nie scala, nie przenosi,
nie usuwa, nie zmienia routingu i nie publikuje. `P7` może działać dopiero po
jawnej decyzji właściciela o zakresie publikacji.

### Kryteria macierzy i publikacji

Dla każdego źródła/grupy macierz musi zawierać status, źródło prawdy, pakiet i
sekcję docelową, unikalną treść, konflikty, ryzyko, zgodę oraz kryterium
pokrycia. Usunięcie wymaga kolejno: zachowania/przeniesienia treści, kompletnej
macierzy, działających linków/redirectów, zachowania dowodów/proweniencji,
niezależnego PASS Evaluatora, PASS Final Control, konkretnej allowlisty
właściciela i readbacku po zmianie.

Publikacyjny readback wymaga `git diff --check`, kontroli linków, skanu sekretów
i PII, kontroli usunięć, porównania hashy, jawnego commitu tylko allowlisty,
pushu na wskazaną gałąź i zdalnego readbacku SHA. Merge do `main` jest osobną
decyzją. W tej fali nie wykonano żadnego z tych działań; utworzono wyłącznie
trzy pliki w nowym katalogu staging.

### P4 proweniencja użyta przez pakiet

P4 ma status `PASS_WITH_EXPLICIT_OWNER_GATES`, obserwacja
`2026-09-14T14:47:54+00:00`. Dla zakresu procesu zachowano dokładne agregaty:

| Grupa P4 | Status | Rodziny | Rekordy | Trasa |
|---|---|---:|---:|---|
| `NAG-PROCESS` | `CANONICAL` | 7 | 73 | `PROCESS-01..07`, ECHO do decyzji, tematy do handoff/evidence |
| `NAG-EVIDENCE` | `EVIDENCE` | 7 | 84 | `PROCESS-04`, raw evidence osobno |
| `NAG-AUDIT` | `SOURCE` | 2 | 2 | `PROCESS-03/04/06`, artefakty audytu osobno |

`NAG-USER` (1 rodzina / 12 rekordów) jest materiałem pomocniczym dla
`PROCESS-07`, nie jest doliczany do wymaganego agregatu `NAG-PROCESS`.
Pełne członkostwo, statusy, hash wariantów i record-level traceability są w
`coverage.json`; nie kopiowano surowych 833 rekordów do pakietu.

## PROCESS-07 — pracownik, norma techniczna i kontrakt raportu

STATUS: STAGING_ONLY
TARGET: techniczna norma pozostaje w skillu; przewodnik pracownika pozostaje odrębnym kandydatem
UNIQUE_FRAGMENTS: `F-16`, `F-17`, `F-19`

ŹRÓDŁA FRAGMENTU:

- `docs/proces-dla-pracownikow.md` | `09f3a374b861e6ca2171bdded8a6af909f703c66f1c04f57610fbc7a233d6f20` | `CONSOLIDATION_CANDIDATE` | §Czym jest ta zasada, §Wersja minimalna, §Ile to kosztuje, §Kiedy tego nie stosować
- `.claude/skills/nagents-autobot/SKILL.md` | `aa5aab364e0a86c6053e46532d835a35a466357a81c562021bf78555dbb6ac3e` | `CANONICAL` | §9, §14.2, §16
- `docs/process/dispatch/NAG-PROC-005-skill-uniwersalny.md` | `de4fa883b9581186f424920a31e46cb1fea683ece5f96ce300b3325250320b83` | `EVIDENCE` | lines 43–57, 77–98
- `CLAUDE.md` | `8de583cc4fe98c55c49b5fac9f1562669040204f571ee2daacc9905e9390d2` | `CANONICAL` | §Podział ról użytkowników, §Zasady pracy z właścicielem

### Granica odbiorcy

Przewodnik pracownika mówi językiem skutków, nie mechanizmów. Pracownik
otrzymuje gotowy przydzielony profil i czat; nie konfiguruje gatewaya, serwera,
modelu, poświadczeń ani routingu. Administrator zarządza profilami, agentami,
nadaniami, limitami, poświadczeniami i obiegiem w powierzchni administracyjnej
lub terminalu. Techniczna norma, bariery, statusy i live readback nie są
zastępowane skrótem dla pracownika.

### Minimalna zasada

Dla zadania, które jest dłuższe niż drobiazg: przed startem zapisz, co ma
powstać, po czym poznasz dobrą pracę i czego nie wolno zrobić; poproś inną
osobę niż wykonawca o sprawdzenie wyniku; zapisz ustalenie od razu; przy
nietypowym wyborze dopisz powód. Zasady nie stosuje się mechanicznie do
trzyminutowych drobiazgów, jednorazowej pracy bez następcy ani sytuacji
awaryjnej, w której dokumentowanie opóźni reakcję.

Ta wersja informacyjna nie nadaje statusu ani dostępu i nie rozstrzyga,
kiedy pracownik może ominąć techniczną kontrolę. Jeśli potrzebny jest pełny
proces, wróć do `PROCESS-01`…`PROCESS-06` i do źródeł kanonicznych.

### Kontrakt raportu

Raport każdej roli ma być krótki, czytelny i odtwarzalny: `STATUS`, `DOMAIN`,
`TEMAT`, `GOAL`, `ZMIANY`, `TESTY`, `BLOKADY`, `NASTĘPNY KROK` oraz
`DEPLOY/PUSH: NIE WYKONANO`. Zawiera destylat ścieżek, hash/punktu kontrolnego,
wyniku testu/scenariuszy i kolejnej bramki; nie zawiera surowych logów, sekretów
ani niezweryfikowanych twierdzeń. `N/D`, `UNKNOWN` i `INFRA` są lepsze niż
wymyślone liczby lub deklaracja bez artefaktu.

## 7. Siedem barier 8gent

[FRAGMENT F-19] Źródła: `.claude/skills/nagents-autobot/SKILL.md` | `aa5aab364e0a86c6053e46532d835a35a466357a81c562021bf78555dbb6ac3e` | `CANONICAL` | §7; `CLAUDE.md` | `8de583cc4fe98c55c49b5fac9f1562669040204f571ee2daacc9905e9390d2` | `CANONICAL` | §Siedem barier; `docs/process/zmiana-procesu.md` | `2c2d366e3ed54a28dae14b47aa78387366e20d2c9b20b1eac9c286af380e0546` | `CANONICAL` | §3.2.

Naruszenie dowolnej bariery to `FAIL`, niezależnie od jakości reszty:

1. w repozytorium wyłącznie `vault_ref`, żadnych wartości sekretów;
2. agent `stanowiskowy` nie ma własnych poświadczeń;
3. brak dostępu ujawnia minimum informacji: `404`, nie `403`;
4. domyślna odmowa, nigdy „wszyscy mogą, chyba że”;
5. integracja tylko według jawnej allowlisty; nigdy `git add -A` ani `git add .`;
6. żadnych prawdziwych danych osobowych poza `prod`;
7. przekazanie na zewnątrz wyłącznie tam, gdzie wskazał właściciel.

Osłabienie, usunięcie lub wyjątek wymaga pytania ABC i ECHO. Pakiet nie tworzy
wyjątku i nie przenosi żadnej wartości sekretu.

## 8. Statusy i granice tego pakietu

- `STAGING_ONLY`: treść robocza w nowym katalogu; nie zastępuje źródeł.
- `CANONICAL`: źródło normatywne, odczytane z podanym hashem.
- `SOURCE`: playbook lub źródło audytu, nie sam dowód wykonania.
- `EVIDENCE`: immutable snapshot konkretnego dispatchu/przebiegu.
- `HISTORY`: kontekst datowany; nie steruje routingiem.
- `CONSOLIDATION_CANDIDATE`: kandydat do osobnego pakietu, wymaga bramy.
- `LIVE_READBACK_REQUIRED`: potrzebny świeży odczyt stanu poza plikiem.
- `OWNER_DECISION_REQUIRED`: nie wolno rozstrzygać w pakiecie.
- `INTEGRATION_REQUIRED`: Final Control przeszedł, ale Orkiestrator jeszcze nie
  włączył wyniku do stanu obowiązującego.

Nie czytano ani nie kopiowano `.env*`, sekretów, credentiali, `state.db`, sesji,
surowych logów, pełnych transcriptów, prywatnych danych ani runtime. Nie
wykonano `git add`, commitu, pushu, merge, deployu, instalacji ani restartu.
Źródła nie były modyfikowane, usuwane ani przenoszone. Wyjście tej fali to
wyłącznie:

```text
docs/process/staging/NAG-CONSOLIDATE-PROCESS-Q1/NAGENTS-PROCESS.md
docs/process/staging/NAG-CONSOLIDATE-PROCESS-Q1/coverage.json
docs/process/staging/NAG-CONSOLIDATE-PROCESS-Q1/operator-report.md
```

Następna bramka: niezależny Evaluator tego samego artefaktu. Defense tylko przy
jego numerowanych zarzutach; następnie Final Control. Po pozytywnym Final
Control potrzebny jest lokalny P7 readback, a publikacja pozostaje osobną
zgodą właściciela.
