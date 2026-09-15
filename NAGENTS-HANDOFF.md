# NAGENTS-HANDOFF.md — staging handoffu

STATUS: STAGING_ONLY
DOMAIN: INFORMACYJNY
TEMAT: NAG-CONSOLIDATE-HANDOFF-Q1
FAZA: Operator
RUNDA: 1 z 3
GENERATED_AT_UTC: 2026-09-14T19:48:40Z

Ten plik jest addytywnym kandydatem pakietu handoffu. Nie zastępuje
`docs/process/handoff.md`, nie jest bieżącym stanem runtime i nie daje zgody na
przeniesienie, scalanie, usuwanie, integrację, publikację, push ani deploy.
Źródło normy, stan snapshotu i świeży readback są rozdzielone poniżej.

## 0. Jak czytać ten pakiet

`HANDOFF-01` opisuje kształt bieżącego handoffu. `HANDOFF-02` opisuje przejęcie
kontekstu i odczyt stanu. `HANDOFF-03` opisuje blokady, warunki następnej bramki
i granice. `HANDOFF-04` jest indeksem historii oraz korekt, a nie drugim
rejestrem decyzji.

Każdy wpis sekcji ma jawne `ŹRÓDŁO`, `STATUS ŹRÓDŁA` i `LOCATOR`. Status
`CANONICAL` oznacza rangę źródła dla danego rodzaju informacji, nie zgodę na
kopiowanie ani publikację. `HISTORY`, `CONSOLIDATION_CANDIDATE` i
`LIVE_READBACK_REQUIRED` nie są bieżącą decyzją.

W tym pakiecie:

- snapshot dokumentu odpowiada na pytanie „co zapisano wtedy";
- live readback odpowiada na pytanie „co jest prawdą teraz";
- decyzja właściciela odpowiada na pytanie „co wolno zrobić";
- historia wyjaśnia, dlaczego wcześniejszy zapis zmieniono, ale nie steruje
  routingiem.

Jeżeli snapshot i readback są sprzeczne, nie wygładzaj różnicy. Zapisz oba
wyniki, oznacz snapshot jako historyczny, a routing oprzyj na świeżym odczycie
albo zatrzymaj właściwy strumień jako `UNKNOWN/INFRA`.

`STATE_SEPARATION: LIVE_READBACK_REQUIRED` oznacza, że `HISTORY` nie jest
bieżącym routingiem. `NO_PUBLISH_BOUNDARY: ACTIVE` oznacza, że ten pakiet nie
autoryzuje publikacji, integracji ani zastąpienia źródeł.

## 0A. Aktualna podstawa platformowa

D-014 zmienił bieżącą podstawę runtime'u na OpenClaw. Czytaj
[`docs/OPENCLAW-STRATEGY.md`](docs/OPENCLAW-STRATEGY.md) przed decyzjami o
Gatewayu, sesjach, modelach, automatyzacjach, tasks, Task Flow lub pluginie.

Hermes, LiteLLM, profile, Cron, receiver i komendy Kanbana opisane w starszych
sekcjach są zachowanym baseline'em historycznym. Nie wykonuj na ich podstawie
migracji ani operacji live. AutoBot Monitor może zostać zaadaptowany jako
opcjonalny plugin OpenClaw dopiero po potwierdzeniu luki.

## HANDOFF-01 — format bieżącego handoffu i zasada zastępowania

ŹRÓDŁO: [`docs/process/handoff.md`](../../handoff.md)
STATUS ŹRÓDŁA: `CANONICAL_FORMAT`; zawartość tego pliku ma datę i może być
snapshotem
LOCATOR: `docs/process/handoff.md` §§ „Handoff”, „Gdzie jesteśmy”, „Co blokuje”,
„Następna bramka”, „Czego nie robić” (linie 1–74)

Bieżący handoff jest jednym zdjęciem sytuacji. Przy przekazaniu zastępuje się
go w całości; nie dopisuje się do niego dziennika zdarzeń. Historia, korekty,
raporty faz i surowe dowody mają osobne miejsca. Nowa wersja musi zachować
proweniencję i wyraźnie wskazać, co jest snapshotem, a co zostało potwierdzone
na żywo.

Minimalny format bieżącego handoffu:

```text
# HANDOFF — projekt 8gent

STAN_NA: <data i czas UTC>
HEAD: <pełny lub jednoznaczny identyfikator rewizji>
GAŁĄŹ: <nazwa gałęzi>
REPOZYTORIUM: <repozytorium>
ETAP: <etap projektu>

## Gdzie jesteśmy
<krótki snapshot; bez twierdzeń runtime bez readbacku>

## Co zostało ustalone
<wyłącznie decyzje odczytane z rejestru właściciela>

## Co ZROBIONE — z dowodami
<pozycja → artefakt/revizja → sposób niezależnego sprawdzenia>

## Co DO ZROBIENIA
<kolejne legalne kroki, każdy z zależnością i warunkiem>

## Blokady
<blocker → właściciel → warunek odblokowania → status → źródło>

## Następna bramka
<jednoznaczny krok i warunek wejścia/wyjścia>

## Czego nie robić
<granice, zakazy i rzeczy odłożone>
```

Reguły wypełniania:

1. `STAN_NA`, `HEAD`, gałąź i etap są oznaczone czasem oraz źródłem.
2. „Zrobione” ma artefakt i sprawdzalny dowód; sam raport, status UI, nazwa
   worktree ani deklaracja wykonawcy nie wystarczają.
3. Blokada ma identyfikator, zakres, właściciela, warunek odblokowania,
   obserwowany status, datę oraz locator. Historia sama nie tworzy blokady.
4. „Następna bramka” wskazuje dokładnie, kto może wykonać następny krok i co
   musi być prawdą przed jego uruchomieniem.
5. W handoffie nie zapisuje się haseł, tokenów, kluczy, wartości poświadczeń,
   surowych logów ani prawdziwych danych osobowych.
6. Zmiana formatu procesu jest osobnym tematem procesu; nie dopisuje się jej
   cicho do handoffu.

### Rozdzielenie snapshotu od stanu bieżącego

`docs/process/handoff.md` opisuje format, ale jego treść jest datowana. Także
`HANDOFF-nagents.md` jest szczegółowym snapshotem z 2026-08-25 i ma status
`HISTORY`. Nie wolno przepisać z tych plików wartości `HEAD`, stanu kodu,
statusu usługi, karty ani blokady jako faktu bieżącego bez readbacku.

## HANDOFF-02 — przejęcie kontekstu i live readback

ŹRÓDŁO: [`CLAUDE.md`](../../../../CLAUDE.md), [`docs/process/tematy.md`](../../tematy.md),
[`NAGENTS-PROJECT.md`](../../../../NAGENTS-PROJECT.md),
`docs/process/handoff.md`
STATUS ŹRÓDŁA: `CANONICAL` dla kolejności/normy i rejestru; `CONSOLIDATION_CANDIDATE`
dla indeksu; `CANONICAL_FORMAT` dla handoffu
LOCATOR: `CLAUDE.md` §§ „Kolejność czytania”, „Stan na dziś”; `NAGENTS-PROJECT.md`
§§ 0–3.3; `docs/process/handoff.md` §§ „Gdzie jesteśmy” i „Następna bramka”

Przejęcie nie polega na zaufaniu ostatniemu handoffowi. Wykonaj kolejno:

1. Przeczytaj punkt startowy projektu, rejestr tematów, format handoffu, skrót
   specyfikacji, decyzje, scenariusze i bieżący etap.
2. Ustal, który dokument jest normą, który decyzją, który historią, a który
   dowodem konkretnej fazy.
3. Wykonaj odczyt repozytorium i zapisz wynik z czasem UTC.
4. Dla tematu AutoBot wykonaj osobny odczyt karty, rodziców, runu, eventu,
   artefaktu i receiptu. Nie uznawaj samego statusu `done`, `queued` ani raportu
   za dowód.
5. Gdy temat dotyka serwera, profilu lub usługi, odczytaj je osobno. Klient,
   Desktop, gateway, Cron, receiver i worker mają różne cykle życia.
6. Porównaj odczyt z snapshotem. Rozbieżność zostaw jawną i nie podejmuj
   decyzji właściciela za niego.

### Hierarchia rozstrzygania

Stosuj kolejność: świeży readback Git/worktree/Kanbana/eventu/receiptu/usługi
lub testu → literalna decyzja właściciela → aktualna specyfikacja i `CLAUDE.md`
→ bieżący handoff → raport fazy → stary handoff, nota lub rozmowa.

### Macierz readbacku

| Obszar | Źródło rozstrzygające | Rodzaj | Minimalny dowód | Brak dowodu |
|---|---|---|---|---|
| HEAD, gałąź, dirty state | Git w wskazanym checkoutcie | `LIVE` | `status`, `branch`, `HEAD`, diff check | `UNKNOWN/INFRA` |
| aktywne i zablokowane tematy | `docs/process/tematy.md` + board | `MIXED` | rejestr oraz świeża karta/rodzice | nie odblokowywać |
| karta, run i następna faza | Kanban | `LIVE` | task, parent, run, terminal event, artifact, receipt | `INFRA/ROUTING_ERROR` |
| profil, projekt i board | jawny CLI profilu | `LIVE` | project/profile/board readback | nie dispatchować |
| gateway, Cron, receiver | świeży odczyt usługi i joba | `LIVE` | status procesu, job, output i receipt | nie twierdzić, że działa |
| decyzja właściciela | rejestr decyzji/ECHO | `NORMATIVE` | literalny wpis z datą i autorem | `DECISION_REQUIRED` |
| stare handoffy i korekty | `HANDOFF-04`/P4 | `HISTORY` | data, hash, status, locator | nie używać do routingu |

### Readback repozytorium i routingu — tylko odczyt

Poniższe polecenia są receptą, nie wynikiem wykonanym przez ten pakiet. Każdy
wynik trzeba zapisać w raporcie fazy z czasem i dokładnym źródłem.

```bash
cd /home/ubuntu/projects/nAgents-readonly
git status --short --branch
git branch --show-current
git log -5 --oneline --decorate
git diff --check
# odczyt zdalnego refu, bez fetch/pull:
git ls-remote origin refs/heads/main refs/heads/claude/git-connection-9sz6dg

# jawny projekt i board — bez zgadywania profilu:
hermes --profile default project show nagents-docs
hermes --profile autobotmonitor kanban --board autobot-monitor stats --json
hermes --profile autobotmonitor kanban --board autobot-monitor list --status running --json
```

W tym checkoutcie nie ma `AUTOBOT-KANBAN.md`. Jest to jawna luka `INFRA` do
odnotowania, a nie zgoda na wymyślenie kontraktu. Należy użyć istniejących
źródeł projektu i odczytu Kanbana; brak oczekiwanego pliku nie dowodzi braku
funkcji serwerowej.

### Format zapisu odczytu

```text
LIVE_READBACK:
  observed_at_utc: <czas>
  scope: <repo | topic | board | project | profile | service>
  source: <dokładna komenda albo ścieżka>
  identity: <task/run/event/receipt/ref, jeśli dotyczy>
  value: <krótki, bezpieczny wynik>
  status: CONFIRMED | UNKNOWN | INFRA | OWNER_HOLD
  next_action: <legalny następny krok albo eskalacja>
```

`NAGENTS-PROJECT.md` jest mapą nawigacyjną, nie zastępuje powyższego odczytu.
Lokalny hash, stary hash z P4 i hash zdalnego refu zapisuj osobno; nie wybieraj
wersji przez nazwę, długość ani podobieństwo.

## HANDOFF-03 — blokady, następna bramka, granice i „nie robić”

ŹRÓDŁO: [`docs/process/tematy.md`](../../tematy.md),
[`CLAUDE.md`](../../../../CLAUDE.md), `docs/process/handoff.md`,
[`NAGENTS-PROJECT.md`](../../../../NAGENTS-PROJECT.md)
STATUS ŹRÓDŁA: `CANONICAL` dla rejestru i barier; `CANONICAL_FORMAT` dla formatu;
`CONSOLIDATION_CANDIDATE` dla indeksu
LOCATOR: `docs/process/tematy.md` §§ „Aktywne”, „Zablokowane”; `CLAUDE.md`
§§ „Siedem barier”, „Stan na dziś”; `NAGENTS-PROJECT.md` §§ 3.2–3.3, 9–10

### Udokumentowane blokady i odroczenia

Poniższe wpisy są destylatem dokumentów. Przed użyciem do routingu wymagają
świeżego readbacku karty i warunku. Nie należy ich rozszerzać o blokady
wynikające wyłącznie ze starych handoffów.

| Temat / decyzja | Udokumentowany stan | Właściciel / odblokowanie | Status użycia |
|---|---|---|---|
| `NAG-MVP1-009-wdrozenie` | `DECISION_REQUIRED`: serwer, domena/TLS, retencja i magazyn kopii, szyfrowanie, sekrety, przypięte obrazy | właściciel; jednoznaczna decyzja | `DOCUMENTED_BLOCKED`, potem `LIVE_READBACK_REQUIRED` |
| `NAG-MVP3-001-pamiec-wspolna` / `D-011` | rezydencja wspólnej pamięci nierozstrzygnięta; MVP3 nie startuje | właściciel; odpowiedź o bazie | `OWNER_DECISION_REQUIRED` |
| `D-010` | topologia agentów odroczona do danych z MVP1; nie jest automatyczną blokadą MVP1/MVP2 | właściciel przed MVP3 | `OWNER_HOLD`, zakres zależny od etapu |
| `NAG-INFRA-002-pomocnik-serwerowy` | aktywny temat; pomocnik ma prowadzić tylko zatwierdzone przejścia | proces zgodny z grafem; brak dowodu zatrzymuje strumień | `IN_PROGRESS`, wymaga live task/readback |

Każda nowa blokada musi zawierać: identyfikator, temat, właściciela, warunek
odblokowania, źródło, czas obserwacji, skutek i następną czynność. Zależność
zewnętrzna z historycznego snapshotu jest najpierw `HISTORY/READBACK_REQUIRED`,
a nie automatycznie bieżącym blockerem.

### Następne bramki

Dla tego pakietu, po terminalnym raporcie Operatora:

1. Niezależny Evaluator czyta dokładnie trzy pliki stagingu oraz sprawdza
   `HANDOFF-01..04`, źródła, statusy, locatory, hashe i rozdzielenie live/history.
2. Przy pustej liście zarzutów przechodzi bezpośrednio do Final Control. Przy
   niepustej, ponumerowanej liście powstaje wyłącznie warunkowa Obrona.
3. Final Control wydaje werdykt per zarzut. Dopiero po jego terminalnym wyniku
   można wykonać lokalny P7 readback pakietu.
4. Pakiet pozostaje `STAGING_ONLY`. Publikacja, zastąpienie źródła, integracja,
   merge, push, deploy i usunięcie dokumentów są osobnymi bramkami właściciela.
5. Operator nie wystawia `READY_FOR_DEPLOY`; brak publikacji jest stanem
   zamierzonym i musi być zapisany jako `DEPLOY/PUSH: NIE WYKONANO`.

### Siedem granic nienaruszalnych

1. W zapisach projektu nie ma wartości sekretów; używa się wyłącznie odwołań do
   bezpiecznego magazynu.
2. Agent rodzaju `stanowiskowy` nie ma własnych poświadczeń do systemów firmy.
3. Brak dostępu zwraca `404`, nie `403`.
4. Uprawnienia są domyślnie odmawiane; nie ma reguły „wszyscy, chyba że”.
5. Integracja jest allowlist-only; nigdy `git add -A` ani `git add .`.
6. Prawdziwe dane osobowe nie trafiają poza `prod`.
7. Push może iść wyłącznie na gałąź wskazaną przez właściciela.

### Czego nie robić

- Nie przepisywać starych `HEAD`, statusów kodu, usług, kart ani blockerów jako
  bieżących faktów.
- Nie traktować `NAGENTS-PROJECT.md` jako normy nadrzędnej ani handoffu jako
  substytutu Kanbana.
- Nie zamieniać rekomendacji, historii, statusu UI lub raportu w decyzję
  właściciela.
- Nie zmieniać `CLAUDE.md`, `docs/process/handoff.md`, `tematy.md`,
  `NAGENTS-PROJECT.md` ani źródeł kanonicznych w tej fazie.
- Nie czytać ani nie kopiować sekretów, `.env`, `state.db`, sesji, surowych
  logów, credentiali ani PII.
- Nie wykonywać `git add`, commit, push, merge, deployu, instalacji ani restartu.
- Nie tworzyć pustej Obrony, nie omijać rodzica i nie uruchamiać następnej fazy
  bez terminalnego eventu oraz odczytu artefaktu.

## HANDOFF-04 — indeks historycznych handoffów i korekt

ŹRÓDŁO: [`HANDOFF-nagents.md`](../../../../HANDOFF-nagents.md),
`/home/ubuntu/handoffs/` przez bezpieczne metadane P4,
[`P4-classification.json`](../../audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json)
STATUS ŹRÓDŁA: `HISTORY` dla datowanych handoffów; `IN_SCOPE_HISTORY_REFERENCE`
dla archiwum; `PASS_WITH_EXPLICIT_OWNER_GATES` dla P4
LOCATOR: `HANDOFF-nagents.md` §§ 1–9, szczególnie §4, §5, §7, §8 i §9;
`P4-classification.json` `source_catalog.source_id=HANDOFFS_NAGENTS` oraz rodziny
`PF-0177`/`NAG-HISTORY`

Handoff historyczny zachowuje się jako kontekst i dowód proweniencji. Nie jest
źródłem routingu, nie nadaje uprawnień i nie zastępuje świeżego odczytu. W tym
zadaniu nie kopiowano treści prywatnego archiwum handoffów; zachowano tylko
bezpieczny locator, status i liczbę rekordów z P4.

### Rejestr źródeł historycznych

| Źródło | Data / zakres snapshotu | Hash SHA-256 | Rozmiar / linie | Status i użycie |
|---|---|---|---:|---|
| `HANDOFF-nagents.md` | 2026-08-25; szczegółowy handoff | `2e9ccd808e81321a20863d846c2f0521fbb99be01ca8566056e384203b2a8936` | 24196 B / 421 | `HISTORY`; kontekst i korekty, nigdy routing |
| `docs/process/handoff.md` | 2026-08-22; format i stary snapshot | `16f22b7a1bad1d22226be206f8b3f827a4b9f24eca6ef3d6c4f00bc663ac06f8` | 3506 B / 74 | `CANONICAL_FORMAT`; format, nie bieżący runtime |
| `/home/ubuntu/handoffs/` | archiwum P4: 34 rekordy | brak jednego hash z P4 source catalog | N/D | `HISTORY_REFERENCE`; treści nie czytano i nie kopiowano |
| `NAGENTS-PROJECT.md` | indeks P5 i późniejszy readback lokalny | bieżący: `84cc8a89c0e7bd53268fc78826938bec062f9223340aa2e0934d64719764a2fb`; P4 preferred: `6da2750082316e205062cec6154b099740964250fab6b14e8829fb79e63fe143` | bieżący: 53421 B / 819 | `CONSOLIDATION_CANDIDATE`; mapa, nie norma ani live state |

Rozbieżność hashy `NAGENTS-PROJECT.md` jest zachowana jawnie. Nie wybierano
wariantu przez długość ani nazwę; bieżący readback i P4 snapshot są osobnymi
rekordami proweniencji.

### Historia korekt z `HANDOFF-nagents.md` §7

Poniższa tabela jest indeksem korekt zapisanych w historycznym handoffie. Status
odnosi się do chwili tamtego snapshotu, nie do bieżącego boardu.

| Locator | Co skorygowano | Skutek zapisany w historii | Status snapshotu |
|---|---|---|---|
| §7.1 | Niepotwierdzone twierdzenie o panelu administracyjnym narzędzia | pierwszeństwo dokumentacji oficjalnej i właściwego zakresu | `HISTORY_CLOSED` |
| §7.2 | Nieprawdziwe twierdzenie o braku kanału Teams | zmieniono wynik porównania narzędzi | `HISTORY_CLOSED` |
| §7.3 | Obietnica o równoległej pracy ośmiu agentów | limit równoległości ma wynikać z realnych rdzeni i rezerwy | `HISTORY_CLOSED` |
| §7.4 | Rejestracja Entra ID nazwana jedyną zależnością zewnętrzną | ujawniono trzy zależności i ich niezależne zegary | `HISTORY_CLOSED` |
| §7.5 | Zadeklarowano zlecenie pracy, którego nie zlecono | raport ma wskazywać rzeczywisty artefakt albo ID zadania | `HISTORY_CLOSED` |
| §7.6 | Walidacja wymusiła dwa warianty pytań zamiast A/B/C | dodatkowe warianty zmieniły część rekomendacji | `HISTORY_CLOSED` |
| §7.7 | Wstawka rozbiła numerację pytań | usunięto duplikat numeru i przywrócono brakujący numer | `HISTORY_CLOSED` |
| §7.8 | Cudzysłów przerwał polecenie zapisu wersji | ustalono bezpieczny format komunikatów zapisu | `HISTORY_CLOSED` |
| §7.9 | Pytania do właściciela były niezrozumiałe technicznie | przepisano je językiem skutków i rozdzielono decyzje | `HISTORY_CLOSED` |
| §7.10 | Blokadę sieciową pomylono z brakiem funkcji źródła | źródła dostarczono inną drogą; sama blokada trwała | `HISTORY_CLOSED_WITH_BLOCK_PERSISTING` |
| §7.11 | Temat wpisano jako zamknięty przed Final Control | zapisano wyprzedzenie kompetencji jako lekcję procesu | `HISTORY_OPEN_AT_SNAPSHOT` |
| §7.12 | Dwie cechy uznano za obecne w specyfikacji bez trafienia | twierdzenia sprostowano i dopisano lukę | `HISTORY_CLOSED` |
| §7.13 | Błędna przesłanka o rozliczaniu tokenów dostawcy | D-001 pozostała do korekty uzasadnienia przez właściciela | `OWNER_DECISION_REQUIRED_AT_SNAPSHOT` |
| §7.14 | Założono wyłącznie logowanie Google | odczyt cennika ujawnił także Microsoft i własne SSO | `HISTORY_CLOSED` |
| §7.15 | Błędnie stwierdzono brak umowy powierzenia | sprostowanie wskazało umowę jako załącznik regulaminu | `HISTORY_CLOSED` |
| §7.16 | Zbyt szeroko założono brak zastosowania DSA | rozdzielono zakres dostawcy od zakresu narzędzia wewnętrznego | `HISTORY_CLOSED_WITH_SCOPE_NOTE` |
| §7.17 | Integrację programów przypisano naszej warstwie | zakres zmniejszono: Hermes wykonuje operacje, 8gent zarządza | `HISTORY_CLOSED` |
| §7.18 | Program rozliczeniowy opisano jako system z logowaniem | dopasowano integracje do faktycznej pracy na plikach | `HISTORY_CLOSED` |

### Zasada korekty

Korekta nie jest cichym nadpisaniem. Właściwy zapis zawiera: poprzednie
twierdzenie, źródło korekty, nowy fakt, skutek dla decyzji lub routingu oraz
status. Jeżeli korekta zmienia koszt, dane, dostęp albo odwracalność, trafia do
właściciela i jego rejestru decyzji; nie może zostać rozstrzygnięta w handoffie.

## 5. Proweniencja pakietu i granica publikacji

Pakiet obejmuje wyłącznie nowy katalog stagingowy
`docs/process/staging/NAG-CONSOLIDATE-HANDOFF-Q1/`. Oryginały pozostają na
swoich ścieżkach. `coverage.json` zawiera maszynowy ledger źródeł, P4 locatorów,
statusów, hashy oraz kryteriów czterech sekcji. `operator-report.md` zawiera
raport fazy i wyniki sprawdzeń.

Ten pakiet nie oznacza:

- że bieżący runtime został wdrożony albo że działa po zamknięciu Desktopu;
- że jakikolwiek dokument został opublikowany, scalony, przeniesiony lub
  usunięty;
- że decyzje D-010/D-011 albo inne bramy właściciela zostały rozstrzygnięte;
- że raport Operatora zastępuje niezależnego Evaluatora, Defense lub Final
  Control.

DEPLOY/PUSH: NIE WYKONANO
