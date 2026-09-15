# NAGENTS — wykonawczy plan konsolidacji dokumentacji

STATUS: PLAN_ONLY / OWNER_HOLD_REQUIRED
DOMAIN: INFORMACYJNY
TEMAT: NAG-DOCS-P6-CONSOLIDATION-PLAN-Q1
GOAL: Dla każdej zweryfikowanej grupy źródeł wskazać status, źródło prawdy,
pakiet docelowy, unikalną treść, konflikty, ryzyko, kryterium pokrycia i zgodę,
bez zmiany któregokolwiek źródła.
OBSERVED_AT_UTC: 2026-09-14T15:13:12Z

## 0. Granica wykonania

Ten dokument jest artefaktem P6 i planem, nie paczką publikacyjną. Nazwy
`NAGENTS-SPEC.md`, `NAGENTS-DECISIONS.md`, `NAGENTS-PROCESS.md`,
`NAGENTS-HANDOFF.md`, `NAGENTS-RESEARCH.md`, `NAGENTS-INTEGRATIONS.md`,
`NAGENTS-USER-GUIDE.md`, `AUTOBOT-PROJECT.md`, `docs/ABM-HISTORY.md` oraz
`docs/ABM-LIFECYCLE.md` są kandydatami pakietów; w tej fazie nie zostały
utworzone i nie zastępują obecnych źródeł prawdy.

P6 wykonuje wyłącznie mapowanie treści. Nie scala, nie przenosi, nie usuwa,
nie nadpisuje, nie zmienia routingu, nie tworzy decyzji właściciela, nie wykonuje
`git add`, commit, push, merge, deployu ani restartu. Oryginały, źródła
pierwotne, dispatchy, runy i artefakty audytu pozostają na swoich ścieżkach.

Granice projektów są twarde: nAgents i AutoBot Monitor są odrębnymi projektami,
a The-Game jest `SEPARATE_PROJECT`. Wspólne słowa „AutoBot”, „Kanban” i
„relay” nie uzasadniają przeniesienia treści między projektami.

## 1. Zweryfikowana podstawa planu

P6 korzysta z terminalnych artefaktów P1–P5, a nie z podobieństwa nazw ani
z deklaracji statusu w interfejsie:

| Faza | Status | Artefakt / zakres | Wniosek używany w P6 |
|---|---|---|---|
| P1 | `PASS` | zakres, korzenie, refy i wyłączenia | każda jednostka ma `source_id` oraz granicę; runtime, sekrety, PII, logi i The-Game nie są normalnym wejściem |
| P2 | `PASS` | 695 lokalnych + 104 GitHub + 34 handoffowych rekordów | pełny inwentarz to 833 rekordy; `LOCAL_ONLY` 132, `REMOTE_ONLY` 0 |
| P3 | `PASS` | 16 kategorii źródeł prawdy | kategoria ma jedno źródło kanoniczne albo jawny `OWNER_DECISION_REQUIRED`, `LIVE_READBACK_REQUIRED` lub `SEPARATE_PROJECT` |
| P4 | `PASS_WITH_EXPLICIT_OWNER_GATES` | 254 rodziny ścieżek i klasyfikacja rekordów | każda rodzina ma status, relację, unikalną treść, konflikt, ryzyko i kandydat docelowy; nie wybrano zwycięzcy przez podobieństwo |
| P5 | `PASS` | `NAGENTS-PROJECT.md` jako indeks | indeks prowadzi do pakietów i artefaktów, ale sam nie jest źródłem normy ani live state |

Liczby kontrolne P4: 272 unikalne SHA-256; 105 rodzin identycznego raw hash;
561 nadmiarowych rekordów po jednym reprezentancie rodziny hash; 12 rodzin
z różnymi hashami tej samej ścieżki; 3 pliki z driftem po P2; 104/104 zdalne
hashe odczytane ponownie, w tym 92 zgodne i 12 różne względem bieżącego
lokalnego reprezentanta; 104/104 miały równy zbiór linków.

Źródła dowodowe: [P1 zakres](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P1-scope.md),
[P1 wyłączenia](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P1-exclusions.md),
[P2 podsumowanie](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P2-inventory-summary.md),
[P2 liczniki](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P2-counts.json),
[P3 mapa źródeł](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P3-sources-of-truth.md),
[P4 klasyfikacja](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json),
[P4 kandydaci](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-consolidation-candidates.md),
[P5 indeks](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P5-index.md).

## 2. Hierarchia rozstrzygania

Przy ekstrakcji przyszły wykonawca stosuje tę kolejność, osobno dla każdej
tezy i każdego fragmentu:

1. świeży readback wskazanego Git/worktree, Kanbana, eventu, receiptu, usługi
   lub testu;
2. literalna decyzja właściciela w `docs/process/echo.md` albo aktualnym
   `docs/spec/decisions.md`;
3. aktualna specyfikacja nAgents i `CLAUDE.md`;
4. `AUTOBOT-KANBAN.md` i aktualne dokumenty operacyjne AutoBot Monitor — tylko
   dla AutoBot Monitor;
5. handoff, raport, nota i stary snapshot jako datowany kontekst, nigdy jako
   samodzielny routing.

`CANONICAL`, `SOURCE`, `HISTORY`, `STALE`, `EVIDENCE` i `LOCAL_ONLY` opisują
rolę/proweniencję, nie zgodę na kopiowanie, usuwanie lub publikację.
`PASS`, `queued`, nazwa worktree, obecność pliku i lokalny hash nie dowodzą
integracji ani publikacji.

## 3. Macierz główna: pakiet → źródło → zachowanie

| Grupa / pakiet docelowy | Obecny status | Źródło prawdy | Unikalna treść do zachowania | Konflikty / rozjazdy | Ryzyko | Kryterium pokrycia | Wymagana zgoda |
|---|---|---|---|---|---|---|---|
| `NAGENTS-SPEC.md` | `CANONICAL` w `docs/spec/`; kandydat konsolidacji | `docs/spec/README.md`, `docs/spec/00-architektura.md`, `docs/spec/scenarios.md`, właściwe `0X-mvpX.md`; decyzje pozostają nadrzędne dla wyborów właściciela | Tożsamość i granice nAgents; warstwy, model danych, bezpieczeństwo, stos; zakres MVP1–MVP4; kryteria i wymagania web-first/serwer; techniczna część RBAC; tylko zweryfikowane unikalne fragmenty legacy | lokalny checkout zawiera późniejsze D-012/D-013 i sekcje web-first/helper; zdalny ref i stare specyfikacje nie zawierają tych zmian; `01-mvp1.md` ma wariant tabeli bez nowych nagłówków | regresja zakresu, barier bezpieczeństwa albo decyzji przez kopiowanie starego/zdalnego snapshotu | 8 rodzin / 96 rekordów `NAG-SPEC` + 1 / 1 `NAG-RBAC` + 4 / 4 `NAG-LEGACY-SPEC` mają przypisanie do `SPEC-*`; scenariusze A/R/K/P/C/U zachowują numery z rejestru | owner: zakres i wybór, co z legacy; Evaluator i Final Control sprawdzają brak utraty; publikacja osobno |
| `NAGENTS-DECISIONS.md` | `CANONICAL` rejestr; kandydat pakietu; wymaga rekonsyliacji | `docs/spec/decisions.md` + literalne `docs/process/echo.md` + aktualny pakiet pytań | D-001…D-013, opcje, wybory, uzasadnienia i konsekwencje; ECHO; statusy pytań otwartych; historia odpowiedzi tylko jako historia | stare pytania mogą wyglądać na otwarte; lokalne `decisions.md`/ECHO są nowsze od remote; podobny akapit nie jest nową decyzją | powtórzenie rozstrzygniętego pytania albo cicha zmiana decyzji właściciela | każda decyzja ma ID, status, źródło literalne, datę, konsekwencje i link; `docs/spec/decisions.md` oraz ECHO nie są skracane bez śladu; NAG-HISTORY ma oznaczenie `HISTORY` | owner dla statusu/treści decyzji i ewentualnego pakietu; żadna rekomendacja nie zastępuje ECHO |
| `NAGENTS-PROCESS.md` | `CANONICAL` skill i `CLAUDE.md`; pakiet docelowy warstwowy | `.claude/skills/nagents-autobot/SKILL.md` + `CLAUDE.md`; `docs/process/zmiana-procesu.md`, dispatch template i audyt pomocniczy | pętla Operator → Evaluator → Obrona → Final Control; ABC/ECHO; bariery; allowlista, izolacja, raport, watchdog, recovery, readback i granice P1–P7 | uniwersalny szkielet jest referencją, nie normą; przewodnik pracownika jest uproszczeniem; dispatch/evidence są snapshotami | mechaniczne scalenie może zmienić normę albo nadać referencji rangę procesu | 7 rodzin / 73 rekordy `NAG-PROCESS` + 7 / 84 `NAG-EVIDENCE` + 2 / 2 `NAG-AUDIT` mają przypisanie do `PROCESS-*`; ECHO i rejestr tematów mają dodatkową trasę do decyzji/handoffu | owner/ECHO przy zmianie bariery lub normy; proces-change procedure; Evaluator/Final Control |
| `NAGENTS-HANDOFF.md` | `CANONICAL` format; bieżąca treść jest snapshotem | `docs/process/handoff.md`; live state zawsze z readbacku | schemat krótkiego handoffu, przejęcie kontekstu, blokady, następna bramka, bezpieczne rozdzielenie stanu od historii; datowane ostrzeżenia tylko z etykietą historii | handoff może być sprzeczny z live state; `HANDOFF-nagents.md` ma nowszy lokalny web-first/helper; część zewnętrznych kopii jest przestarzała | stary stan infrastruktury lub zadania zostanie potraktowany jako bieżący | `NAG-HANDOFF` 1 / 12 trafia do `HANDOFF-*`; nAgents `NAG-HISTORY` ma per-plik trasę; każde twierdzenie statusowe ma pole `LIVE_READBACK_REQUIRED` | owner nie musi zatwierdzać formatu technicznego, ale zatwierdza treści o stanie/zakresie; live readback obowiązkowy |
| `NAGENTS-RESEARCH.md` | `HISTORY` + `SOURCE`; nie jest routingiem | `docs/process/pamiec.md` oraz właściwe źródła pierwotne z datą, hashem i linkiem | porównania dróg, topologii, modeli i serwerów; tok rozumowania; analiza appto; captured OAuth/Entra/Graph/Hermes; korekty i uzasadnienia odrzuceń | ceny, funkcje, API, topologia i status wdrożenia starzeją się; źródło producenta, analiza i decyzja mają różną rangę | stara rekomendacja zostanie użyta jako aktualny fakt lub decyzja | 11 / 88 `NAG-RESEARCH` + 8 / 96 `NAG-APPT0` + 5 / 5 `NAG-SOURCES` mają trasę `RESEARCH-*`; każda teza ma datę, rangę źródła i status potwierdzenia | owner tylko dla decyzji wynikającej z researchu; źródła pierwotne pozostają read-only; integracje dodatkowo wymagają bramy INT |
| `NAGENTS-INTEGRATIONS.md` | `OWNER_DECISION_REQUIRED` | wybór właściciela zakresu i głównego punktu styku; materiały lokalne i handoffy są pomocnicze | rozdział: fakty/protokół/test; instrukcje Entra; projekt M365/Graph i kryteria MS-01…MS-12; ograniczenia dostępu i proweniencja | opis możliwości nie dowodzi runtime; M365 jest oznaczone `NOT PROVEN`; nie wybrano mechanizmu ani zakresu | zbyt wczesne utrwalenie endpointu lub rozszerzenie dostępu | 4 / 18 `NAG-INTEGRATIONS` + wybrane `NAG-SOURCES` mają `INT-*`; nie ma statusu „potwierdzone” bez decyzji ownera i live testu | owner: zakres, główny punkt styku, dostęp i dane; następnie niezależny live readback/test; bez decyzji nic nie konsolidować |
| `NAGENTS-USER-GUIDE.md` | `CONSOLIDATION_CANDIDATE` | `docs/proces-dla-pracownikow.md`; techniczna norma pozostaje w skillu/spec | język nietechniczny, pięć sytuacji pracy, gotowy profil/czat, granica pracownik–administrator, minimalna zasada przekazania/odbioru | przewodnik nie zawiera pełnych barier, statusów ani live readbacku; dispatch P6 jest evidence, nie instrukcją | pracownik dostanie zbyt mało informacji albo przewodnik zostanie użyty jako norma techniczna | 1 / 12 `NAG-USER` oraz odbiorcza część `NAG-RBAC` mają `USER-*`; każdy zakaz techniczny ma odnośnik do normy, nie jej kopię | owner review języka i zakresu dla pracownika; nie wymaga zmiany normy procesu |
| `AUTOBOT-PROJECT.md` | `CANONICAL` kontrakt ABM + `SOURCE`/kandydat pakietu | AutoBot Monitor `AUTOBOT-KANBAN.md`; live board/profile/service; supporting ops docs | kontrakt Kanbana, status native/process, preflight, idempotency, model/provider/effort, Cron/receiver/helper, owner-chat boundary, package/patch boundaries | ABM jest osobnym projektem; `AUTOBOT-KANBAN.md` ma drift po P2; relay i package mają lokalne warianty; static docs nie dowodzą live state | stary project ID/profil/target może uruchomić obcy lub zdublowany strumień | `ABM-CONTRACT` 3 / 5, `ABM-OPS` 7 / 7, `ABM-RELAY` 9 / 10, `ABM-PACKAGE` 1 / 2, `ABM-HANDOFF` 1 / 1 i `ABM-INTEGRATION` 1 / 1 mają `ABM-*`; kontrakt pozostaje osobnym źródłem | owner ABM dla wyboru wariantu/trybu i publikacji; live canary dla relayu; release review dla package/patch |
| `docs/ABM-HISTORY.md` | `EVIDENCE` + `HISTORY`; indeks, nie bieżący kontrakt | exact task/run/event/artifact readback; ABM contract/live readback rozstrzyga bieżący stan | indeks wszystkich runów i ID, raporty V1/V2, acceptance mappings, test counts, hardening notes, plany i handoffy jako historia | podobne szablony mają różne run ID/role/parents; `PASS` raportu nie dowodzi live install; plany/ledger mogą być zamknięte | usunięcie runu lub pomylenie historii z routingiem niszczy audytowalność | `ABM-EVIDENCE` 136 / 180, `ABM-REPORTS` 2 / 4, `ABM-HISTORY` 5 / 9 i `ABM-HANDOFF` 1 / 1 trafiają do `HISTORY-*`; każdy run zachowuje ID, rundę i event | owner review zakresu indeksu; Evaluator sprawdza kompletność; żadnego łączenia runów ani usuwania |
| `docs/ABM-LIFECYCLE.md` | `SOURCE`; instalacja/upgrade nadal owner-gated | ABM lifecycle docs, package manifest i live install readback po zgodzie | bezpieczna instalacja, odinstalowanie, rollback, backup/GitHub policy, relacja package/patch/manifest | stare raporty/package mogą opisywać inną rewizję; brak dowodu live install; automatyczna instalacja/restart są poza P6 | nadpisanie prywatnego stanu, instalacja z niezweryfikowanego pakietu lub fałszywe twierdzenie o gotowości | 4 / 7 `ABM-LIFECYCLE` oraz lifecycle fragmenty `ABM-OPS`/`ABM-INTEGRATION` mają `LIFE-*`; instrukcja oddziela procedurę od dowodu wykonania | owner ABM dla install/upgrade/uninstall i publikacji; release review + manifest/hash/test; live mutation osobną zgodą |

## 4. Macierz sekcji docelowych

Poniższe identyfikatory sekcji są kontraktem planu. Przyszły dokument może użyć
innych nagłówków wyświetlanych, ale nie może zgubić żadnego identyfikatora,
źródła, statusu ani kryterium bez aktualizacji tej macierzy.

### 4.1 `NAGENTS-SPEC.md`

| ID sekcji | Zakres | Źródła | Warunek pokrycia / bramka |
|---|---|---|---|
| `SPEC-01` | czym jest nAgents, czego nie robi, granice projektu i siedem barier | `docs/spec/00-architektura.md` §1, `CLAUDE.md`, `NAG-ENTRY`, `NAG-LEGACY-SPEC` | wszystkie unikalne tezy o tożsamości i granicach zachowane; The-Game/ABM nie stają się nAgents |
| `SPEC-02` | warstwy, słownik, model danych, bezpieczeństwo, zagrożenia, stos i środowiska | `docs/spec/00-architektura.md` §2–§10 | każda tabela/model/constraint ma odnośnik do źródła; brak sekretów i PII; żadna bariera nie jest osłabiona |
| `SPEC-03` | roadmapa i zakres MVP1–MVP4 | `docs/spec/README.md`, `01-mvp1.md`–`04-mvp4.md`, `NAG-LEGACY-SPEC` | cele, zależności, out-of-scope i kryteria etapów są kompletne; stary plan nie nadpisuje bieżącego zakresu |
| `SPEC-04` | kontrakt scenariuszy i mapowanie odbioru | `docs/spec/scenarios.md`, tabele etapów | zachowane wszystkie identyfikatory A1–A7, R1–R7, W1–W6, K1–K5, P1–P4, C1–C6, U1–U11; pełna lista pozostaje osobnym kontraktem |
| `SPEC-05` | web-first, serwer jako właściciel pracy, Desktop jako klient, pomocnik jako wymaganie produktu | `docs/spec/00-architektura.md` §12, `docs/spec/README.md`, bieżące `CLAUDE.md`; decyzje D-012/D-013 jako odnośnik | wymagania U1–U11 nie są zastąpione samym opisem „połączono”; wybór decyzji nie jest tworzony w specyfikacji |
| `SPEC-06` | techniczna macierz ról, nadawań i egzekwowania | `docs/spec/00-architektura.md` §3/§6, `02-mvp2.md`, `NAG-RBAC` | każda operacja ma aktualne źródło i status; stara macierz nie nadaje uprawnień; decyzje dostępu idą do `NAGENTS-DECISIONS.md` |
| `SPEC-07` | ślad unikalnego legacy/rationale bez ustanawiania normy | `NAG-LEGACY-SPEC`, odpowiednie pliki historyczne | każda unikalna sekcja legacy ma `HISTORY/STALE`, źródło, datę i decyzję „zachowaj jako kontekst / odrzuć”; brak cichej regresji |

### 4.2 `NAGENTS-DECISIONS.md`

| ID sekcji | Zakres | Źródła | Warunek pokrycia / bramka |
|---|---|---|---|
| `DEC-01` | rejestr D-001…D-013: opcje, wybór, uzasadnienie, konsekwencje | `docs/spec/decisions.md`, `NAG-SPEC` split | 13/13 ID obecnych i literalnie zgodnych z aktualnym rejestrem; brak wyboru dopisanego z noty |
| `DEC-02` | literalne ECHO i ich daty | `docs/process/echo.md`, `NAG-PROCESS` split | każde ECHO ma literalną odpowiedź, autora/datę zgodnie ze źródłem i link do decyzji; żadnej rekomendacji jako ECHO |
| `DEC-03` | pytania otwarte i kolejność | `docs/process/pytania/2026-08-25-wybory.md`, `decisions.md`, ECHO | status każdego pytania jest zrekoncyliowany; D-010/D-011 i bramy integracyjne pozostają jawne |
| `DEC-04` | historia wariantów i korekt | `NAG-HISTORY`, starsze pakiety pytań, `docs/process/pamiec.md` | historyczne warianty nie zmieniają bieżącego statusu; zachowany link/hash i opis skutku korekty |

### 4.3 `NAGENTS-PROCESS.md`

| ID sekcji | Zakres | Źródła | Warunek pokrycia / bramka |
|---|---|---|---|
| `PROCESS-01` | norma ról i pętla faz | `SKILL.md`, `CLAUDE.md`, universal skeleton jako referencja | Operator nie integruje ani nie ocenia siebie; Defense tylko przy zarzutach; Final Control niezależny |
| `PROCESS-02` | ABC/ECHO i bezpieczna zmiana procesu | `SKILL.md` §8, `docs/process/zmiana-procesu.md`, `echo.md` | decyzje koszt/dane/dostęp/odwracalność przed pracą; bariery wymagają ECHO |
| `PROCESS-03` | dispatch, GOAL, allowlista, izolacja i raport | `SKILL.md`, `dispatch/SZABLON.md`, `NAG-AUDIT` | każde zlecenie ma ID, GOAL, binarne kryterium, anti-self-deception, repair path, workspace i brak integracji |
| `PROCESS-04` | evidence, Kanban readback, event/run/receipt i `INTEGRATION_REQUIRED` | `NAG-EVIDENCE`, P1–P5 audit, `tematy.md` | 833 rekordy są możliwe do prześledzenia; `PASS`/UI/queued nie wystarcza; brak eventu = `UNKNOWN/INFRA` |
| `PROCESS-05` | watchdog, limity, fail-closed i recovery | `SKILL.md` §4/§10/§11, `NAG-AUDIT` | jedna aktywna rola na temat, limit rund i brak cichego resetu; crash/orphan/timeout nie odblokowuje następcy |
| `PROCESS-06` | playbook audytu P1–P7 i zasady sprzątania | `docs/process/NAGENTS-DOCS-CONSOLIDATION-PLAN.md`, audyt P1–P5 | P6/P7 mają własne bramki; usunięcie dopiero po coverage, evaluator, Final Control, owner allowlist i readback |
| `PROCESS-07` | granica pracownik–proces techniczny i kontrakt raportu | `SKILL.md`, `docs/proces-dla-pracownikow.md`, dispatch/evidence | przewodnik użytkownika nie staje się normą; raport jest destylatem, nie surowym logiem |

### 4.4 `NAGENTS-HANDOFF.md`

| ID sekcji | Zakres | Źródła | Warunek pokrycia / bramka |
|---|---|---|---|
| `HANDOFF-01` | format bieżącego handoffu i zasada zastępowania | `docs/process/handoff.md` | jeden snapshot bieżący; brak dopisywania historii do formatu |
| `HANDOFF-02` | przejęcie kontekstu i live readback | `handoff.md`, `NAG-ENTRY`, `NAG-PROCESS` (`tematy.md`) | stan repo, boardu/usługi/profilu potwierdzany osobno; handoff oznacza snapshot |
| `HANDOFF-03` | blokady, następna bramka, granice i nie robić | `handoff.md`, `tematy.md`, decyzje | każdy blocker ma właściciela/warunek odblokowania; nie tworzyć blokady z samej historii |
| `HANDOFF-04` | indeks historycznych handoffów i korekt | `NAG-HISTORY`, `HANDOFF-nagents.md`, `/home/ubuntu/handoffs` przez bezpieczne metadane | zachować proweniencję, datę, hash i status; nie kopiować PII, sekretów ani runtime |

### 4.5 `NAGENTS-RESEARCH.md`

| ID sekcji | Zakres | Źródła | Warunek pokrycia / bramka |
|---|---|---|---|
| `RESEARCH-01` | wcześniejsze drogi, topologia i wybory konstrukcyjne | `NAG-RESEARCH`, `docs/process/pamiec.md`, noty 01–05/08 | każda teza ma datę/rangę; odrzucona opcja nie staje się decyzją |
| `RESEARCH-02` | appto: synteza, katalog, ceny, prywatność, regulamin, wdrożenie | `NAG-APPT0`, nota 06/07, sześć `docs/process/zrodla/appto-*` | trzy warstwy pozostają rozdzielone: source, analiza, decyzja; cena nie jest aktualnym faktem bez nowej weryfikacji |
| `RESEARCH-03` | captured źródła OAuth/Entra/Graph/Hermes | `NAG-SOURCES`, pakiet `nagents-2026-09-10/sources/*` | zachowany source ID, data, hash/link i status snapshot; pusty `sources/8.md` nie jest dowodem |
| `RESEARCH-04` | korekty, lekcje, pewność i aktualność | `docs/process/pamiec.md`, `NAG-HISTORY` | każda korekta ma wskazany skutek; historyczne statusy nie trafiają do routingu |
| `RESEARCH-05` | granica „research ≠ decyzja ≠ live state” | wszystkie grupy research | automatyczny audyt odrzuca tezę bez źródła/rangi/statusu; integracje przekierowane do `INT-*` |

### 4.6 `NAGENTS-INTEGRATIONS.md`

| ID sekcji | Zakres | Źródła | Warunek pokrycia / bramka |
|---|---|---|---|
| `INT-01` | decyzja właściciela: zakres i główny punkt styku | `P3` kat. integrations, `NAG-INTEGRATIONS` | status pozostaje `OWNER_DECISION_REQUIRED` do literalnej decyzji; brak tworzenia endpointu z planu |
| `INT-02` | Hermes: lokalne fakty, protokół i test runtime | `docs/nota-09-interfejs-hermesa.md`, `NAG-SOURCES` | rozdzielić opis protokołu od lokalnej instalacji i live testu; brak „działa” bez readbacku |
| `INT-03` | Entra/OIDC: instrukcja administratora i uprawnienia | `docs/nota-10-entra-instrukcja-dla-administratora.md`, source snapshots | procedura nie nadaje dostępu; rejestracja i uprawnienia admina wymagają zewnętrznego potwierdzenia |
| `INT-04` | Microsoft 365/Graph: projekt, zakresy, testy | `INTEGRACJA-MICROSOFT365.md`, `nagents-2026-09-10/04-MICROSOFT365.md` | zachować MS-01…MS-12; `NOT PROVEN` do kont kontrolnych i live testu; dane/zakres po decyzji ownera |
| `INT-05` | dowód i rollback integracji | P3/P4, źródła pierwotne, live readback | decyzja → test → sanitized evidence → rollback; brak poszerzenia dostępu i publikacji przed bramą |

### 4.7 `NAGENTS-USER-GUIDE.md`

| ID sekcji | Zakres | Źródła | Warunek pokrycia / bramka |
|---|---|---|---|
| `USER-01` | jak pracownik otwiera gotowy profil i czat | `docs/proces-dla-pracownikow.md`, `NAG-USER`, D-012 | język skutków; brak gatewaya, serwera, modelu, kluczy i routingu w zadaniach pracownika |
| `USER-02` | pięć sytuacji pracy i minimalne zasady | `docs/proces-dla-pracownikow.md` | pięć sytuacji obecnych; przewodnik nie dopowiada uprawnień ani procedur technicznych |
| `USER-03` | ciągłość, zamknięcie klienta, odbiór i eskalacja | scenariusze U1–U4/U7–U11, D-012/D-013 | klient może się zamknąć bez końca pracy serwera; wyjątek jest zgłaszany, nie ukrywany |
| `USER-04` | granica pracownik–administrator i RBAC dla odbiorcy | `NAG-RBAC`, architektura, MVP2 | każda pozycja „kto może” ma źródło aktualne; historyczna macierz nie nadaje roli |

### 4.8 `AUTOBOT-PROJECT.md`

| ID sekcji | Zakres | Źródła | Warunek pokrycia / bramka |
|---|---|---|---|
| `ABM-01` | projekt, namespace, kontrakt i granica względem nAgents/The-Game | `AUTOBOT-KANBAN.md`, `AGENTS.md`, P3 | AutoBot Monitor pozostaje odrębny; kontrakt ABM ma pierwszeństwo nad supporting docs |
| `ABM-02` | status native/process, board/project/profile identity i preflight | `AUTOBOT-KANBAN.md`, ABM ops, live board | każdy readback kwalifikuje project ID, board, assignee, parents, run, event, receipt; static plan nie wystarcza |
| `ABM-03` | Cron → spool → receiver/helper → fazy Kanbana | ABM ops, `ABM-RELAY`, live service | Cron read-only; helper fail-closed; Defense tylko przy zarzutach; po Final Control stop na `INTEGRATION_REQUIRED` |
| `ABM-04` | provider/model/effort/Fast i owner-chat boundary | model policy, relay docs, contract | provider/model/effort jawne; `deliver=local`/Bot Chat/notify nie są zamienne z visible owner chat |
| `ABM-05` | package, artifact, patch i release review | `ABM-PACKAGE`, package README, `ABM-INTEGRATION` | lokalny/remote wariant oraz manifest/base SHA/testy zapisane; nie publikować z samego README |
| `ABM-06` | recovery, replay, receipts i auditability | `ABM-RELAY`, run evidence, contract | ten sam idempotency key nie tworzy drugiego runu; brak receipt/eventu = `INFRA/UNKNOWN` |
| `ABM-07` | live-readback and owner escalation | all ABM groups + live board/service | każde twierdzenie statusowe ma timestamp, źródło i dokładny readback; owner gate pozostaje jawny |

### 4.9 `docs/ABM-HISTORY.md`

| ID sekcji | Zakres | Źródła | Warunek pokrycia / bramka |
|---|---|---|---|
| `HISTORY-01` | katalog per-run/per-task evidence | `ABM-EVIDENCE`, `runs/**`, exact event/receipt | 136 rodzin / 180 rekordów; zachować każde ID, rolę, rundę, rodzica i artefakt; nie łączyć podobnych szablonów |
| `HISTORY-02` | final reports i acceptance mappings | `ABM-REPORTS`, `FINAL-REPORT*.md` | raport związany z rewizją/runem; test count i gate z dowodem; `PASS` nie oznacza live install |
| `HISTORY-03` | ewolucja planów, wymagań i ledgerów | `ABM-HISTORY`, plany V1/V2, ledger | oznaczyć `HISTORY/STALE`; nie używać do nowego dispatchu; zachować hash/proweniencję |
| `HISTORY-04` | handoffy, migracje i ostrzeżenia | `ABM-HANDOFF`, run evidence | extract tylko stabilnych reguł po readbacku; statusy profilu/receivera sprawdzać live |
| `HISTORY-05` | granice archiwum i wyłączenia | P1/P4, ABM sources | runtime, credentiale, logi i The-Game nie są kopiowane; indeks przechowuje bezpieczny identyfikator dowodu |

### 4.10 `docs/ABM-LIFECYCLE.md`

| ID sekcji | Zakres | Źródła | Warunek pokrycia / bramka |
|---|---|---|---|
| `LIFE-01` | bezpieczna instalacja i wymagania | `docs/INSTALL.md`, `ABM-LIFECYCLE`, package/manifest | tylko po owner approval; jawna wersja/base/hash; brak auto-install/restart w P6 |
| `LIFE-02` | upgrade, backup i GitHub policy | `docs/UPGRADE-BACKUP-AND-GITHUB-POLICY.md`, ops docs | backup i rollback opisane; push/publikacja osobną zgodą i allowlistą |
| `LIFE-03` | uninstall i odwrócenie | `docs/UNINSTALL.md`, lifecycle docs | procedura rozdzielona od dowodu wykonania; prywatny runtime nie jest czyszczony domyślnie |
| `LIFE-04` | package/patch/manifest i walidacja | `V2-PACKAGE.md`, `package/README.md`, `hermes-patches/.../README.md` | warianty hash, base SHA, manifest i testy uzgodnione w release review; brak publikacji z lokalnego dirty snapshotu |
| `LIFE-05` | live proof oraz recovery | live install/service readback, `ABM-RELAY` | instalacja/upgrade/rollback mają osobny test i readback; opis procedury nie jest dowodem wykonania |

## 5. Ledger źródeł P4 → sekcja docelowa

Tabela poniżej zachowuje pełne agregaty P4. `rodziny/rekordy` liczą odpowiednio
254 rodzin i 833 rekordy; dokładne członkostwo każdej rodziny oraz każde pole
`unique_content` są w dodatku A i w `P4-classification.json`.

| P4 group | Status | Rodziny / rekordy | Główna trasa | Zachowanie i gate |
|---|---|---:|---|---|
| `ABM-CONTRACT` | `CANONICAL` | 3 / 5 | ABM-01/02/07; `AUTOBOT-KANBAN.md` remains canonical | tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `ABM-EVIDENCE` | `EVIDENCE` | 136 / 180 | HISTORY-01; runs remain separate | indeksować po exact ID/run/event; zachować osobno; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `ABM-HANDOFF` | `HISTORY` | 1 / 1 | HISTORY-04 + ABM-07; live readback | kontekst datowany; tylko unikalny fragment po literalnym porównaniu; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `ABM-HISTORY` | `HISTORY` | 5 / 9 | HISTORY-03 | kontekst datowany; tylko unikalny fragment po literalnym porównaniu; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `ABM-INTEGRATION` | `SOURCE` | 1 / 1 | ABM-05 + LIFE-04/05; base/hash/tests gate | cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `ABM-LIFECYCLE` | `SOURCE` | 4 / 7 | LIFE-01..05; no install in P6 | cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `ABM-OPS` | `SOURCE` | 7 / 7 | ABM-02/03/04/07; lifecycle fragments → LIFE-02/05 | cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `ABM-PACKAGE` | `CONSOLIDATION_CANDIDATE` | 1 / 2 | ABM-05 + LIFE-04; release review | ekstrakcja sekcjami dopiero po owner gate i readbacku; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `ABM-RELAY` | `CONSOLIDATION_CANDIDATE` | 9 / 10 | ABM-03/04/06/07; owner-gated live canary | ekstrakcja sekcjami dopiero po owner gate i readbacku; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `ABM-REPORTS` | `EVIDENCE` | 2 / 4 | HISTORY-02 | indeksować po exact ID/run/event; zachować osobno; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `NAG-APPT0` | `SOURCE` | 8 / 96 | RESEARCH-02; integration claims cross-link to INT-* only after gate | cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `NAG-AUDIT` | `SOURCE` | 2 / 2 | PROCESS-06 + PROCESS-03/04 | cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `NAG-DECISIONS` | `CANONICAL` | 1 / 12 | DEC-01..04 | tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `NAG-ENTRY` | `CANONICAL` | 2 / 25 | RETAIN-INDEX-01 / `CLAUDE.md` + `NAGENTS-PROJECT.md` | tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `NAG-EVIDENCE` | `EVIDENCE` | 7 / 84 | PROCESS-04; audit artefacts remain separate | indeksować po exact ID/run/event; zachować osobno; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `NAG-HANDOFF` | `CANONICAL` | 1 / 12 | HANDOFF-01..04 | tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `NAG-HISTORY` | `HISTORY` | 20 / 75 | HANDOFF-04 / DEC-04 / RESEARCH-04 by path; never routing | kontekst datowany; tylko unikalny fragment po literalnym porównaniu; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `NAG-INDEX` | `CONSOLIDATION_CANDIDATE` | 3 / 4 | RETAIN-INDEX-02 / existing `NAGENTS-PROJECT.md` | ekstrakcja sekcjami dopiero po owner gate i readbacku; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `NAG-INTEGRATIONS` | `OWNER_DECISION_REQUIRED` | 4 / 18 | INT-01..05; hold until owner decision | HOLD; żadna treść nie staje się normą przed decyzją i testem; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `NAG-LEGACY-SPEC` | `STALE` | 4 / 4 | SPEC-03/SPEC-07 + DEC-04 for historical decisions | oznaczyć HISTORY/STALE; nie używać do routingu ani zakresu; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `NAG-PROCESS` | `CANONICAL` | 7 / 73 | PROCESS-01..07; `echo.md` → DEC-02; `tematy.md` → HANDOFF-02/PROCESS-04 | tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `NAG-RBAC` | `CONSOLIDATION_CANDIDATE` | 1 / 1 | SPEC-06 + USER-04; current source wins | ekstrakcja sekcjami dopiero po owner gate i readbacku; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `NAG-RESEARCH` | `HISTORY` | 11 / 88 | RESEARCH-01/04/05 | kontekst datowany; tylko unikalny fragment po literalnym porównaniu; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `NAG-SOURCES` | `SOURCE` | 5 / 5 | RESEARCH-03; integration-specific citations cross-link INT-* | cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `NAG-SPEC` | `CANONICAL` | 8 / 96 | SPEC-01..07; decisions/scenarios remain separately authoritative | tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |
| `NAG-USER` | `CONSOLIDATION_CANDIDATE` | 1 / 12 | USER-01..04 | ekstrakcja sekcjami dopiero po owner gate i readbacku; pełne `unique_content` i członkostwo: dodatek A / P4 JSON |


## 6. Reguły konfliktu i decyzji

### 6.1 Exact duplicates

105 rodzin exact-hash i 561 nadmiarowych rekordów nie są 561 treściami do
skasowania. To proweniencja snapshotów. Przyszła paczka może wskazać jeden
reprezentant do lektury, ale zachowuje record IDs, source IDs i hashy w indeksie;
nie łączy różnych ścieżek ani nie usuwa kopii bez osobnego owner-approved
allowlist.

### 6.2 Warianty tej samej ścieżki

12 rodzin z różnymi hashami wymagają osobnego porównania sekcji. Nie wolno
wybrać wariantu przez długość, nazwę, zdalną obecność ani mtime. Lista jest w
dodatku B; decyzja dla każdej pozycji brzmi: „zachowaj oba snapshoty do
readbacku” albo „wybierz po literalnym porównaniu i zgodzie”, nigdy automatyczne
nadpisanie.

### 6.3 Lokalny/remote drift

P4 odnotował trzy pliki z driftem po P2: ABM `AUTOBOT-KANBAN.md`, ABM
`docs/ABM-CARD-TAGGING-GUIDE.md` oraz nAgents `NAGENTS-PROJECT.md`. P5 podał
hash końcowy `6d6f324135bd1c64c27548fb38b1d28ce068132108ce5afb50185d84e59b5d58`
dla indeksu, natomiast bieżący readback przed zapisaniem P6 ma hash
`ba7c979edd4f4e781b10ca5c321ef11cc00b4ffd2274c2320a3749cd73948449` i `818` linii. Rozbieżność jest
`UNKNOWN/READBACK_REQUIRED`; plan nie wybiera ani nie nadpisuje wersji.

### 6.4 Owner gates

1. `OWNER-01`: właściciel zatwierdza zakres i nazwy kandydackich pakietów,
   zwłaszcza co ma być ekstrakcją, a co tylko indeksem odnośników.
2. `OWNER-02`: właściciel rozstrzyga integracje (`INT-01`), zakres danych,
   dostęp i główny punkt styku; do tego czasu `NAGENTS-INTEGRATIONS.md` jest
   HOLD.
3. `OWNER-03`: właściciel ABM zatwierdza zwycięzcę wariantu relay/package po
   live canary, manifest/hash/base/testach; static snapshot nie wystarcza.
4. `OWNER-04`: przed usunięciem albo przeniesieniem owner zatwierdza dokładną
   allowlistę po niezależnym Evaluatorze, Final Control, kontroli linków,
   sekretów/PII, diffu i readbacku lokalnym/zdalnym.
5. `OWNER-05`: publikacja/push/merge/deploy są osobnymi bramkami; P6 nie nadaje
   na nie zgody.

## 7. Kryterium kompletności macierzy

P6 jest kompletne wyłącznie, gdy niezależny Evaluator może odtworzyć wszystkie
poniższe asercje bez czytania nieobjętego zakresu:

1. `P4-classification.json.path_family_classifications` ma 254 rodziny i
   każda z nich występuje dokładnie raz w dodatku A.
2. Suma rekordów z grup w ledgerze wynosi 833, a każda rodzina ma źródłową
   listę record IDs w P4 JSON; żadna kopia exact-hash nie jest cichym merge.
3. Każda z 26 grup P4 ma status, źródło prawdy, trasę do co najmniej jednej
   sekcji, unikalną treść, konflikt, ryzyko i regułę bramki.
4. Dla każdego wiersza dodatku A `unique_content` z P4 JSON jest niepuste i
   ma przypisany `target_section`; `unique_content` nie może być zastąpione
   samą nazwą pliku.
5. Wszystkie 12 wariantów hash tej samej ścieżki są jawne, a każdy z 3 driftów
   ma osobny readback gate. `LOCAL_ONLY` 132 i `REMOTE_ONLY` 0 są zachowane.
6. Scenariusze `A1–A7`, `R1–R7`, `W1–W6`, `K1–K5`, `P1–P4`, `C1–C6` oraz
   `U1–U11` są wskazane w pakiecie specyfikacji albo pozostają linkowanym,
   odrębnym kontraktem `docs/spec/scenarios.md`.
7. Każdy fragment wymagający decyzji ma `OWNER_DECISION_REQUIRED`, każdy
   bieżący stan ma `LIVE_READBACK_REQUIRED`, a The-Game ma
   `SEPARATE_PROJECT`; brak statusu nie jest domyślną zgodą.
8. Linki w planie wskazują istniejące artefakty; nazwy przyszłych pakietów są
   kodem, nie linkiem do nieistniejącego pliku.

Proponowana procedura niezależnej kontroli: załadować P4 JSON programowo,
sprawdzić unikalność `path_family_id`, dołączyć do tego planu po `PF-*`,
porównać `group_id`, `primary_status`, `unique_content` i trasę, a dopiero
potem oceniać styl lub skróty. Brak możliwości dołączenia oznacza `INFRA`, nie
`PASS`.

## 8. Bezpieczna kolejność przyszłego wykonania

1. Wykonać świeży readback `git status/branch/HEAD`, P4/P5 hashy, 12 wariantów,
   3 driftów oraz live board/project/profile/service. Nie robić `fetch`, `pull`,
   resetu, stashu ani clean.
2. Owner rozstrzyga `OWNER-01`–`OWNER-03`; decyzje zapisać literalnie w
   właściwym rejestrze przed pracą o skutkach dla danych, dostępu, kosztu lub
   odwracalności.
3. Tworzyć pakiety wyłącznie addytywnie i w osobnym workspace, według sekcji
   `SPEC-*`, `DEC-*`, `PROCESS-*`, `HANDOFF-*`, `RESEARCH-*`, `INT-*`,
   `USER-*`, `ABM-*`, `HISTORY-*`, `LIFE-*`.
4. Dla każdej ekstrakcji zachować source path, source ID, observed date,
   SHA-256, status P4, decyzję o wariancie i link zwrotny. Nie przepisywać
   sekretów, PII, runtime ani surowych logów.
5. Uruchomić niezależnego Evaluatora i Final Control. Przy zarzutach obrona
   odpowiada per numer; `NAPRAW` wraca tylko do właściwej sekcji.
6. Dopiero po owner-approved allowlist i pełnym readbacku można rozważyć
   oznaczenie źródła jako przeniesione. P7 publikuje osobno i jawnie.

## 9. Rollback i stan po P6

P6 nie zmienia stanu źródeł, więc jego rollback polega na odrzuceniu tego
artefaktu planu lub przygotowaniu nowej wersji z zachowaniem poprzedniego
hashu; nie wymaga i nie usprawiedliwia czyszczenia checkoutu. Przyszłe pakiety
muszą być odwracalne: najpierw zachowują oryginał, potem dodają ekstrakcję,
a usunięcie pozostaje osobnym zadaniem z własną zgodą i dowodem. Żaden
`PASS` P6 nie oznacza integracji, publikacji ani wdrożenia.

## 10. Stan końcowy tej fazy

- Utworzony jest wyłącznie ten plan w `NAGENTS-CONSOLIDATION-PLAN.md`.
- Wszystkie źródła P1–P5 i źródła wskazane w P4 pozostają nietknięte.
- Pakiety docelowe są opisane jako kandydaci; `docs/spec/`, skill, `CLAUDE.md`,
  `docs/process/handoff.md`, `docs/spec/decisions.md`, `docs/spec/scenarios.md`,
  `AUTOBOT-KANBAN.md` i runy zachowują dotychczasową rangę.
- Następna bramka to owner hold, potem osobny P7/release review; nie jest to
  automatyczny dispatch tylko dlatego, że plan istnieje.

DEPLOY/PUSH: NIE WYKONANO

---

## Dodatek A — kompletna macierz rodzin ścieżek

Poniższa tabela jest generowana z `P4-classification.json`, nie z nazw plików.
Obejmuje wszystkie 254 rodziny. `Treść P4` to dokładne pole
`unique_content` z klasyfikacji P4; zachowanie tej wartości w artefakcie
zapewnia ślad „unikalna treść źródła → sekcja docelowa”. Rekordy o identycznym
hashu są nadal wymienione przez `record_count` i `source_ids`, ale nie są
traktowane jako nowe treści.

| path_family_id | projekt | ścieżka | P4 group | status | source IDs | target_section | rekordów | Treść P4 / zasada zachowania |
|---|---|---|---|---|---|---|---:|---|
| PF-0001 | AutoBot Monitor | `AGENTS.md` | `ABM-CONTRACT` | `CANONICAL` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | ABM-01/02/07; `AUTOBOT-KANBAN.md` remains canonical | 2 | Jednoprofilowy routing, status native/process, preflight, idempotency, readback and server Cron boundaries; AGENTS gives product/safety acceptance context.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0002 | AutoBot Monitor | `AUTOBOT-KANBAN.md` | `ABM-CONTRACT` | `CANONICAL` | ABM_CHECKOUT | ABM-01/02/07; `AUTOBOT-KANBAN.md` remains canonical | 1 | Jednoprofilowy routing, status native/process, preflight, idempotency, readback and server Cron boundaries; AGENTS gives product/safety acceptance context.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0003 | AutoBot Monitor | `FINAL-REPORT-V2.md` | `ABM-REPORTS` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-02 | 2 | V1/V2 acceptance mappings, test counts, known hardening notes and owner gates.; indeksować po exact ID/run/event; zachować osobno |
| PF-0004 | AutoBot Monitor | `FINAL-REPORT.md` | `ABM-REPORTS` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-02 | 2 | V1/V2 acceptance mappings, test counts, known hardening notes and owner gates.; indeksować po exact ID/run/event; zachować osobno |
| PF-0005 | AutoBot Monitor | `PLAN-AUTOBOT-PLUGIN.md` | `ABM-HISTORY` | `HISTORY` | ABM_CHECKOUT | HISTORY-03 | 1 | Evolution of V1/V2 plan, provider requirements and dispatch ledger; useful rationale for future review.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0006 | AutoBot Monitor | `PLAN-V2.md` | `ABM-HISTORY` | `HISTORY` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-03 | 2 | Evolution of V1/V2 plan, provider requirements and dispatch ledger; useful rationale for future review.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0007 | AutoBot Monitor | `PLAN.md` | `ABM-HISTORY` | `HISTORY` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-03 | 2 | Evolution of V1/V2 plan, provider requirements and dispatch ledger; useful rationale for future review.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0008 | AutoBot Monitor | `V2-LEDGER.md` | `ABM-HISTORY` | `HISTORY` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-03 | 2 | Evolution of V1/V2 plan, provider requirements and dispatch ledger; useful rationale for future review.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0009 | AutoBot Monitor | `V2-REQUIREMENTS.md` | `ABM-HISTORY` | `HISTORY` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-03 | 2 | Evolution of V1/V2 plan, provider requirements and dispatch ledger; useful rationale for future review.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0010 | AutoBot Monitor | `docs/ABM-CARD-ROUTING-AUDIT.md` | `ABM-OPS` | `SOURCE` | ABM_CHECKOUT | ABM-02 | 1 | Cron/helper, card tagging, routing audit, model/effort/Fast, remote Desktop and backup policy; instructions encode fail-closed boundaries.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0011 | AutoBot Monitor | `docs/ABM-CARD-TAGGING-GUIDE.md` | `ABM-OPS` | `SOURCE` | ABM_CHECKOUT | ABM-02 | 1 | Cron/helper, card tagging, routing audit, model/effort/Fast, remote Desktop and backup policy; instructions encode fail-closed boundaries.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0012 | AutoBot Monitor | `docs/ABM-CRON-HELPER-OPERATING-RUNBOOK.md` | `ABM-OPS` | `SOURCE` | ABM_CHECKOUT | ABM-03/ABM-07 | 1 | Cron/helper, card tagging, routing audit, model/effort/Fast, remote Desktop and backup policy; instructions encode fail-closed boundaries.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0013 | AutoBot Monitor | `docs/ABM-MODEL-REPAIR-PLAN.md` | `ABM-OPS` | `SOURCE` | ABM_CHECKOUT | ABM-04 | 1 | Cron/helper, card tagging, routing audit, model/effort/Fast, remote Desktop and backup policy; instructions encode fail-closed boundaries.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0014 | AutoBot Monitor | `docs/ABM-REMOTE-DESKTOP-SERVER-PLAN.md` | `ABM-OPS` | `SOURCE` | ABM_CHECKOUT | ABM-03/ABM-07 | 1 | Cron/helper, card tagging, routing audit, model/effort/Fast, remote Desktop and backup policy; instructions encode fail-closed boundaries.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0015 | AutoBot Monitor | `docs/ABM-SAME-PROFILE-CRON-MIGRATION.md` | `ABM-OPS` | `SOURCE` | ABM_CHECKOUT | ABM-03/ABM-07 | 1 | Cron/helper, card tagging, routing audit, model/effort/Fast, remote Desktop and backup policy; instructions encode fail-closed boundaries.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0016 | AutoBot Monitor | `docs/AUTOBOT-MODEL-EFFORT-FAST-POLICY.md` | `ABM-OPS` | `SOURCE` | ABM_CHECKOUT | ABM-04 | 1 | Cron/helper, card tagging, routing audit, model/effort/Fast, remote Desktop and backup policy; instructions encode fail-closed boundaries.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0017 | AutoBot Monitor | `docs/CRON-DIRECTIVE-LOOP.md` | `ABM-RELAY` | `CONSOLIDATION_CANDIDATE` | ABM_ACTIVE_RELAY_WORKTREE; ABM_CHECKOUT | ABM-03/04/06/07 | 2 | Receiver/owner-chat boundary, spool/receipt/replay details and relay run evidence; this content is absent from named GitHub main.; ekstrakcja sekcjami dopiero po owner gate i readbacku |
| PF-0018 | AutoBot Monitor | `docs/INSTALL.md` | `ABM-LIFECYCLE` | `SOURCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | LIFE-01 | 2 | Manual safe install/uninstall, rollback and backup/GitHub boundaries with no live mutation.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0019 | AutoBot Monitor | `docs/OWNER-CHAT-RELAY.md` | `ABM-RELAY` | `CONSOLIDATION_CANDIDATE` | ABM_ACTIVE_RELAY_WORKTREE | ABM-03/04/06/07 | 1 | Receiver/owner-chat boundary, spool/receipt/replay details and relay run evidence; this content is absent from named GitHub main.; ekstrakcja sekcjami dopiero po owner gate i readbacku |
| PF-0020 | AutoBot Monitor | `docs/UNINSTALL.md` | `ABM-LIFECYCLE` | `SOURCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | LIFE-01 | 2 | Manual safe install/uninstall, rollback and backup/GitHub boundaries with no live mutation.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0021 | AutoBot Monitor | `docs/UPGRADE-BACKUP-AND-GITHUB-POLICY.md` | `ABM-LIFECYCLE` | `SOURCE` | ABM_CHECKOUT | LIFE-02 | 1 | Manual safe install/uninstall, rollback and backup/GitHub boundaries with no live mutation.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0022 | AutoBot Monitor | `docs/V2-PACKAGE.md` | `ABM-LIFECYCLE` | `SOURCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | LIFE-04 | 2 | Manual safe install/uninstall, rollback and backup/GitHub boundaries with no live mutation.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0023 | AutoBot Monitor | `docs/handoffs/ABM-SAME-PROFILE-OWNER-HANDOFF.md` | `ABM-HANDOFF` | `HISTORY` | ABM_CHECKOUT | HISTORY-04/ABM-07 | 1 | Migration rationale, owner target distinction and operational warnings for one profile.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0024 | AutoBot Monitor | `hermes-patches/2026-09-12/README.md` | `ABM-INTEGRATION` | `SOURCE` | ABM_CHECKOUT | ABM-05/LIFE-04/05 | 1 | Local patch set and integration boundaries for the native plugin.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0025 | AutoBot Monitor | `package/README.md` | `ABM-PACKAGE` | `CONSOLIDATION_CANDIDATE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | ABM-05/LIFE-04 | 2 | Local 84-line README adds native namespace artifact r4, router/auth/run-root and redaction boundaries over remote 40-line baseline.; ekstrakcja sekcjami dopiero po owner gate i readbacku |
| PF-0026 | AutoBot Monitor | `runs/ABM-A-001/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0027 | AutoBot Monitor | `runs/ABM-A-001/01-operator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0028 | AutoBot Monitor | `runs/ABM-A-001/02-evaluator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0029 | AutoBot Monitor | `runs/ABM-A-001/03-final-control.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0030 | AutoBot Monitor | `runs/ABM-A-001/04-integration.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0031 | AutoBot Monitor | `runs/ABM-ARCH-001/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0032 | AutoBot Monitor | `runs/ABM-ASTRA-001/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0033 | AutoBot Monitor | `runs/ABM-ASTRA-002/00-defense-r3-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0034 | AutoBot Monitor | `runs/ABM-ASTRA-002/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0035 | AutoBot Monitor | `runs/ABM-ASTRA-002/00-luna-defense-r5-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0036 | AutoBot Monitor | `runs/ABM-ASTRA-002/00-luna-final-control-r5-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0037 | AutoBot Monitor | `runs/ABM-ASTRA-002/00-luna-repair-evaluator-r5-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0038 | AutoBot Monitor | `runs/ABM-ASTRA-002/00-luna-repair-final-control-r5-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0039 | AutoBot Monitor | `runs/ABM-ASTRA-002/00-luna-repair-r5-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0040 | AutoBot Monitor | `runs/ABM-ASTRA-002/01-luna-operator-r5.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0041 | AutoBot Monitor | `runs/ABM-ASTRA-002/01-luna-repair-r5.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0042 | AutoBot Monitor | `runs/ABM-ASTRA-002/01-operator-r3.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0043 | AutoBot Monitor | `runs/ABM-ASTRA-002/01-repair-operator-r4.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0044 | AutoBot Monitor | `runs/ABM-ASTRA-002/02-evaluator-r3-review.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0045 | AutoBot Monitor | `runs/ABM-ASTRA-002/02-evaluator-r3.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0046 | AutoBot Monitor | `runs/ABM-ASTRA-002/02-luna-evaluator-r5.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0047 | AutoBot Monitor | `runs/ABM-ASTRA-002/02-luna-repair-evaluator-r5.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0048 | AutoBot Monitor | `runs/ABM-ASTRA-002/03-defense-r3.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0049 | AutoBot Monitor | `runs/ABM-ASTRA-002/03-luna-defense-r5.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0050 | AutoBot Monitor | `runs/ABM-ASTRA-002/04-final-control-r3.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0051 | AutoBot Monitor | `runs/ABM-ASTRA-002/04-luna-final-control-r5.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0052 | AutoBot Monitor | `runs/ABM-ASTRA-002/04-luna-repair-final-control-r5.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0053 | AutoBot Monitor | `runs/ABM-B-001/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0054 | AutoBot Monitor | `runs/ABM-B-001/01-operator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0055 | AutoBot Monitor | `runs/ABM-B-001/02-evaluator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0056 | AutoBot Monitor | `runs/ABM-B-001/03-final-control.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0057 | AutoBot Monitor | `runs/ABM-BRIDGE-001/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0058 | AutoBot Monitor | `runs/ABM-C-001/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0059 | AutoBot Monitor | `runs/ABM-C-001/01-operator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0060 | AutoBot Monitor | `runs/ABM-C-001/02-evaluator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0061 | AutoBot Monitor | `runs/ABM-C-001/03-final-control.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0062 | AutoBot Monitor | `runs/ABM-D-001/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0063 | AutoBot Monitor | `runs/ABM-D-001/01-operator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0064 | AutoBot Monitor | `runs/ABM-D-001/02-evaluator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0065 | AutoBot Monitor | `runs/ABM-D-001/03-final-control.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0066 | AutoBot Monitor | `runs/ABM-D-001/04-integration.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0067 | AutoBot Monitor | `runs/ABM-MODEL-001/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0068 | AutoBot Monitor | `runs/ABM-MONITOR-001/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0069 | AutoBot Monitor | `runs/ABM-MONITOR-001/01-operator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0070 | AutoBot Monitor | `runs/ABM-MONITOR-001/02-evaluator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0071 | AutoBot Monitor | `runs/ABM-MONITOR-001/03-defense.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0072 | AutoBot Monitor | `runs/ABM-MONITOR-001/04-final-control.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0073 | AutoBot Monitor | `runs/ABM-MONITOR-002/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0074 | AutoBot Monitor | `runs/ABM-MONITOR-002/01-backend-operator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0075 | AutoBot Monitor | `runs/ABM-MONITOR-002/01-desktop-operator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0076 | AutoBot Monitor | `runs/ABM-MONITOR-002/02-ABM-MONITOR-002:backend:operator:r1-evaluator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0077 | AutoBot Monitor | `runs/ABM-MONITOR-002/02-ABM-MONITOR-002:desktop:operator:r1-evaluator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0078 | AutoBot Monitor | `runs/ABM-MONITOR-002/03-ABM-MONITOR-002:backend:operator:r1-final-control.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0079 | AutoBot Monitor | `runs/ABM-MONITOR-002/04-ABM-MONITOR-002:desktop:operator:r1-final-control.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0080 | AutoBot Monitor | `runs/ABM-MONITOR-VISIBILITY-001/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0081 | AutoBot Monitor | `runs/ABM-MONITOR-VISIBILITY-001/01-operator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0082 | AutoBot Monitor | `runs/ABM-OWNER-CHAT-RELAY-001/00-dispatch.md` | `ABM-RELAY` | `CONSOLIDATION_CANDIDATE` | ABM_ACTIVE_RELAY_WORKTREE | ABM-03/04/06/07 | 1 | Receiver/owner-chat boundary, spool/receipt/replay details and relay run evidence; this content is absent from named GitHub main.; ekstrakcja sekcjami dopiero po owner gate i readbacku |
| PF-0083 | AutoBot Monitor | `runs/ABM-OWNER-CHAT-RELAY-001/01-operator.md` | `ABM-RELAY` | `CONSOLIDATION_CANDIDATE` | ABM_ACTIVE_RELAY_WORKTREE | ABM-03/04/06/07 | 1 | Receiver/owner-chat boundary, spool/receipt/replay details and relay run evidence; this content is absent from named GitHub main.; ekstrakcja sekcjami dopiero po owner gate i readbacku |
| PF-0084 | AutoBot Monitor | `runs/ABM-OWNER-CHAT-RELAY-001/02-evaluator-dispatch.md` | `ABM-RELAY` | `CONSOLIDATION_CANDIDATE` | ABM_ACTIVE_RELAY_WORKTREE | ABM-03/04/06/07 | 1 | Receiver/owner-chat boundary, spool/receipt/replay details and relay run evidence; this content is absent from named GitHub main.; ekstrakcja sekcjami dopiero po owner gate i readbacku |
| PF-0085 | AutoBot Monitor | `runs/ABM-OWNER-CHAT-RELAY-001/02-evaluator.md` | `ABM-RELAY` | `CONSOLIDATION_CANDIDATE` | ABM_ACTIVE_RELAY_WORKTREE | ABM-03/04/06/07 | 1 | Receiver/owner-chat boundary, spool/receipt/replay details and relay run evidence; this content is absent from named GitHub main.; ekstrakcja sekcjami dopiero po owner gate i readbacku |
| PF-0086 | AutoBot Monitor | `runs/ABM-OWNER-CHAT-RELAY-001/03-defense.md` | `ABM-RELAY` | `CONSOLIDATION_CANDIDATE` | ABM_ACTIVE_RELAY_WORKTREE | ABM-03/04/06/07 | 1 | Receiver/owner-chat boundary, spool/receipt/replay details and relay run evidence; this content is absent from named GitHub main.; ekstrakcja sekcjami dopiero po owner gate i readbacku |
| PF-0087 | AutoBot Monitor | `runs/ABM-OWNER-CHAT-RELAY-001/03-evaluator-r2-dispatch.md` | `ABM-RELAY` | `CONSOLIDATION_CANDIDATE` | ABM_ACTIVE_RELAY_WORKTREE | ABM-03/04/06/07 | 1 | Receiver/owner-chat boundary, spool/receipt/replay details and relay run evidence; this content is absent from named GitHub main.; ekstrakcja sekcjami dopiero po owner gate i readbacku |
| PF-0088 | AutoBot Monitor | `runs/ABM-OWNER-CHAT-RELAY-001/04-evaluator-r2.md` | `ABM-RELAY` | `CONSOLIDATION_CANDIDATE` | ABM_ACTIVE_RELAY_WORKTREE | ABM-03/04/06/07 | 1 | Receiver/owner-chat boundary, spool/receipt/replay details and relay run evidence; this content is absent from named GitHub main.; ekstrakcja sekcjami dopiero po owner gate i readbacku |
| PF-0089 | AutoBot Monitor | `runs/ABM-PLAN-001/00-watchdog-repair-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0090 | AutoBot Monitor | `runs/ABM-PLUGIN-001/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0091 | AutoBot Monitor | `runs/ABM-PROFILE-001/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0092 | AutoBot Monitor | `runs/ABM-V2-A/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0093 | AutoBot Monitor | `runs/ABM-V2-A/01-operator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0094 | AutoBot Monitor | `runs/ABM-V2-A/02-evaluator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0095 | AutoBot Monitor | `runs/ABM-V2-A/03-final-control.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0096 | AutoBot Monitor | `runs/ABM-V2-A/04-integration.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0097 | AutoBot Monitor | `runs/ABM-V2-B/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0098 | AutoBot Monitor | `runs/ABM-V2-B/01-operator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0099 | AutoBot Monitor | `runs/ABM-V2-B/02-evaluator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0100 | AutoBot Monitor | `runs/ABM-V2-B/03-final-control.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0101 | AutoBot Monitor | `runs/ABM-V2-B/04-integration.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0102 | AutoBot Monitor | `runs/ABM-V2-C/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0103 | AutoBot Monitor | `runs/ABM-V2-C/01-operator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0104 | AutoBot Monitor | `runs/ABM-V2-C/02-evaluator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0105 | AutoBot Monitor | `runs/ABM-V2-C/03-final-control.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0106 | AutoBot Monitor | `runs/ABM-V2-C/04-integration.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0107 | AutoBot Monitor | `runs/ABM-V2-D/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0108 | AutoBot Monitor | `runs/ABM-V2-D/01-operator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0109 | AutoBot Monitor | `runs/ABM-V2-D/02-evaluator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0110 | AutoBot Monitor | `runs/ABM-V2-D/03-final-control.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0111 | AutoBot Monitor | `runs/ABM-V2-D/04-integration.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0112 | AutoBot Monitor | `runs/ABM-V2-E/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0113 | AutoBot Monitor | `runs/ABM-V2-E/01-operator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0114 | AutoBot Monitor | `runs/ABM-V2-E/02-evaluator.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0115 | AutoBot Monitor | `runs/ABM-V2-E/03-final-control.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0116 | AutoBot Monitor | `runs/ABM-V2-E/04-integration.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0117 | AutoBot Monitor | `runs/ABM-V2-H-001/00-topic-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0118 | AutoBot Monitor | `runs/ABM-V2-H-001/a/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0119 | AutoBot Monitor | `runs/ABM-V2-H-001/b/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0120 | AutoBot Monitor | `runs/ABM-V2-H-001/b/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0121 | AutoBot Monitor | `runs/ABM-V2-H-001/c/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0122 | AutoBot Monitor | `runs/ABM-V2-H-001/c/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0123 | AutoBot Monitor | `runs/ABM-V2-H-001/d/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0124 | AutoBot Monitor | `runs/ABM-V2-H-001/d/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0125 | AutoBot Monitor | `runs/ABM-V2-H-001/e/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0126 | AutoBot Monitor | `runs/ABM-V2-H-001/e/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0127 | AutoBot Monitor | `runs/ABM-V2-H-001/f/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0128 | AutoBot Monitor | `runs/ABM-V2-H-001/f/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0129 | AutoBot Monitor | `runs/ABM-V2-H-001/g/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0130 | AutoBot Monitor | `runs/ABM-V2-H-001/g/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0131 | AutoBot Monitor | `runs/ABM-V2-H-001/h/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0132 | AutoBot Monitor | `runs/ABM-V2-H-001/h/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0133 | AutoBot Monitor | `runs/ABM-V2-H-001/i/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0134 | AutoBot Monitor | `runs/ABM-V2-H-001/i/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0135 | AutoBot Monitor | `runs/ABM-V2-H-001/j/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0136 | AutoBot Monitor | `runs/ABM-V2-H-001/j/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0137 | AutoBot Monitor | `runs/ABM-V2-H-001/k/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0138 | AutoBot Monitor | `runs/ABM-V2-H-001/k/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0139 | AutoBot Monitor | `runs/ABM-V2-H-001/l/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0140 | AutoBot Monitor | `runs/ABM-V2-H-001/l/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0141 | AutoBot Monitor | `runs/ABM-V2-H-001/m/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0142 | AutoBot Monitor | `runs/ABM-V2-H-001/m/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0143 | AutoBot Monitor | `runs/ABM-V2-H-001/n/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0144 | AutoBot Monitor | `runs/ABM-V2-H-001/n/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0145 | AutoBot Monitor | `runs/ABM-V2-H-001/o/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0146 | AutoBot Monitor | `runs/ABM-V2-H-001/o/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0147 | AutoBot Monitor | `runs/ABM-V2-H-001/p/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0148 | AutoBot Monitor | `runs/ABM-V2-H-001/p/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0149 | AutoBot Monitor | `runs/ABM-V2-H-001/q/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0150 | AutoBot Monitor | `runs/ABM-V2-H-001/q/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0151 | AutoBot Monitor | `runs/ABM-V2-H-001/r/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0152 | AutoBot Monitor | `runs/ABM-V2-H-001/r/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0153 | AutoBot Monitor | `runs/ABM-V2-H-001/s/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0154 | AutoBot Monitor | `runs/ABM-V2-H-001/s/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0155 | AutoBot Monitor | `runs/ABM-V2-H-001/t/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0156 | AutoBot Monitor | `runs/ABM-V2-H-001/t/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0157 | AutoBot Monitor | `runs/ABM-V2-H-001/u/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0158 | AutoBot Monitor | `runs/ABM-V2-H-001/u/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0159 | AutoBot Monitor | `runs/ABM-V2-H-001/v/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0160 | AutoBot Monitor | `runs/ABM-V2-H-001/v/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0161 | AutoBot Monitor | `runs/ABM-V2-H-001/w/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0162 | AutoBot Monitor | `runs/ABM-V2-H-001/w/00-evaluator-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0163 | AutoBot Monitor | `runs/ABM-V2-H-001/x/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0164 | AutoBot Monitor | `runs/ABM-V2-P0-EVAL-R3/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0165 | AutoBot Monitor | `runs/ABM-V2-P0-R2/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0166 | AutoBot Monitor | `runs/ABM-V2-P0-R3/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0167 | AutoBot Monitor | `runs/ABM-V21-GPU-001/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | HISTORY-01 | 2 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0168 | AutoBot Monitor | `runs/ABM-WEB-001/00-dispatch.md` | `ABM-EVIDENCE` | `EVIDENCE` | ABM_CHECKOUT | HISTORY-01 | 1 | Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.; indeksować po exact ID/run/event; zachować osobno |
| PF-0169 | AutoBot Monitor | `runs/README.md` | `ABM-CONTRACT` | `CANONICAL` | ABM_CHECKOUT; GITHUB_ABM_MAIN_REF | ABM-01/02/07; `AUTOBOT-KANBAN.md` remains canonical | 2 | Jednoprofilowy routing, status native/process, preflight, idempotency, readback and server Cron boundaries; AGENTS gives product/safety acceptance context.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0170 | nAgents | `.claude/skills/nagents-autobot/README.md` | `NAG-PROCESS` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | PROCESS-01..07 | 12 | Projektowy skill zawiera szczegółową pętlę, bariery, ABC/ECHO, watchdog i raportowanie; źródło uniwersalne oddziela reguły przenośne od wiązań nAgents; SCHEMAT opisuje bezpieczny podział.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0171 | nAgents | `.claude/skills/nagents-autobot/SKILL.md` | `NAG-PROCESS` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | PROCESS-01..07 | 12 | Projektowy skill zawiera szczegółową pętlę, bariery, ABC/ECHO, watchdog i raportowanie; źródło uniwersalne oddziela reguły przenośne od wiązań nAgents; SCHEMAT opisuje bezpieczny podział.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0172 | nAgents | `AUTOBOOT-INDEKS.md` | `NAG-INDEX` | `CONSOLIDATION_CANDIDATE` | HANDOFFS_NAGENTS | RETAIN-INDEX-02 | 1 | NAGENTS-PROJECT.md mapuje pytanie → źródło → readback i granice projektów; starsze AUTOBOOT/NAGENTS-INDEKS zawierają kontekst historyczny, którego nie ma w CLAUDE.; ekstrakcja sekcjami dopiero po owner gate i readbacku |
| PF-0173 | nAgents | `CLAUDE.md` | `NAG-ENTRY` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RETAIN-INDEX-01 | 12 | Twarde bariery, kolejność czytania i granica web-first/serwer/Desktop w bieżącym checkoutcie; README.md jest tylko markerem repozytorium.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0174 | nAgents | `DOKUMENTACJA-MVP1.md` | `NAG-LEGACY-SPEC` | `STALE` | HANDOFFS_NAGENTS | SPEC-03/SPEC-07 | 1 | Starsza narracja biznesowa, plan etapów, moduły MVP1 i raport struktury; zawiera rationale oraz luki niewidoczne w skrócie.; oznaczyć HISTORY/STALE; nie używać do routingu ani zakresu |
| PF-0175 | nAgents | `HANDOFF-centrum-projektow.md` | `NAG-HISTORY` | `HISTORY` | HANDOFFS_NAGENTS | HANDOFF-04 | 1 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0176 | nAgents | `HANDOFF-kolejne-kroki.md` | `NAG-HISTORY` | `HISTORY` | HANDOFFS_NAGENTS | HANDOFF-04 | 1 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0177 | nAgents | `HANDOFF-nagents.md` | `NAG-HISTORY` | `HISTORY` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | HANDOFF-04 | 12 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0178 | nAgents | `HANDOFF-serwer.md` | `NAG-HISTORY` | `HISTORY` | HANDOFFS_NAGENTS | HANDOFF-04 | 1 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0179 | nAgents | `HANDOFF-zespol-agentow.md` | `NAG-HISTORY` | `HISTORY` | HANDOFFS_NAGENTS | HANDOFF-04 | 1 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0180 | nAgents | `INTEGRACJA-MICROSOFT365.md` | `NAG-INTEGRATIONS` | `OWNER_DECISION_REQUIRED` | HANDOFFS_NAGENTS | INT-01..05 | 1 | Nota Hermesa rozdziela fakty lokalnej instalacji od protokołu; nota Entra i M365 opisują kroki administracyjne, uprawnienia i test dowodu.; HOLD; żadna treść nie staje się normą przed decyzją i testem |
| PF-0181 | nAgents | `LLM-OPEN-SOURCE-nAgents-HANDOFF.md` | `NAG-RESEARCH` | `HISTORY` | HANDOFFS_NAGENTS | RESEARCH-01/04/05 | 1 | Porównania dróg, topologii, dostawców serwerów i modeli oraz uzasadnienia odrzuceń; noty zachowują tok rozumowania.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0182 | nAgents | `MIGRACJA-OVH-STATUS.md` | `NAG-HISTORY` | `HISTORY` | HANDOFFS_NAGENTS | HANDOFF-04 | 1 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0183 | nAgents | `NAGENTS-INDEKS.md` | `NAG-INDEX` | `CONSOLIDATION_CANDIDATE` | HANDOFFS_NAGENTS | RETAIN-INDEX-02 | 1 | NAGENTS-PROJECT.md mapuje pytanie → źródło → readback i granice projektów; starsze AUTOBOOT/NAGENTS-INDEKS zawierają kontekst historyczny, którego nie ma w CLAUDE.; ekstrakcja sekcjami dopiero po owner gate i readbacku |
| PF-0184 | nAgents | `NAGENTS-PROJECT.md` | `NAG-INDEX` | `CONSOLIDATION_CANDIDATE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_CHECKOUT | RETAIN-INDEX-02 | 2 | NAGENTS-PROJECT.md mapuje pytanie → źródło → readback i granice projektów; starsze AUTOBOOT/NAGENTS-INDEKS zawierają kontekst historyczny, którego nie ma w CLAUDE.; ekstrakcja sekcjami dopiero po owner gate i readbacku |
| PF-0185 | nAgents | `PLAN-WDROZENIA-nAgents.md` | `NAG-LEGACY-SPEC` | `STALE` | HANDOFFS_NAGENTS | SPEC-03/SPEC-07 | 1 | Starsza narracja biznesowa, plan etapów, moduły MVP1 i raport struktury; zawiera rationale oraz luki niewidoczne w skrócie.; oznaczyć HISTORY/STALE; nie używać do routingu ani zakresu |
| PF-0186 | nAgents | `PYTANIA-DO-ODPOWIEDZI-28-08.md` | `NAG-HISTORY` | `HISTORY` | HANDOFFS_NAGENTS | DEC-04 | 1 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0187 | nAgents | `PYTANIA-I-ODPOWIEDZI-nAgents.md` | `NAG-HISTORY` | `HISTORY` | HANDOFFS_NAGENTS | DEC-04 | 1 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0188 | nAgents | `RAPORT-nAgents-scenariusz-i-plan.md` | `NAG-LEGACY-SPEC` | `STALE` | HANDOFFS_NAGENTS | SPEC-03/SPEC-07 | 1 | Starsza narracja biznesowa, plan etapów, moduły MVP1 i raport struktury; zawiera rationale oraz luki niewidoczne w skrócie.; oznaczyć HISTORY/STALE; nie używać do routingu ani zakresu |
| PF-0189 | nAgents | `RAPORTY-Z-HETZNERA-README.md` | `NAG-HISTORY` | `HISTORY` | HANDOFFS_NAGENTS | HANDOFF-04/DEC-04/RESEARCH-04 | 1 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0190 | nAgents | `README.md` | `NAG-ENTRY` | `CANONICAL` | GITHUB_NAGENTS_MAIN_REF; GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RETAIN-INDEX-01 | 13 | Twarde bariery, kolejność czytania i granica web-first/serwer/Desktop w bieżącym checkoutcie; README.md jest tylko markerem repozytorium.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0191 | nAgents | `ROLE-I-UPRAWNIENIA-nAgents.md` | `NAG-RBAC` | `CONSOLIDATION_CANDIDATE` | HANDOFFS_NAGENTS | SPEC-06/USER-04 | 1 | Macierz ról, akceptacji i operacji w języku właściciela; uzupełnia skrótową specyfikację.; ekstrakcja sekcjami dopiero po owner gate i readbacku |
| PF-0192 | nAgents | `SCHEMAT-PODZIALU-AUTOBOT.md` | `NAG-PROCESS` | `CANONICAL` | HANDOFFS_NAGENTS | PROCESS-01..07 | 1 | Projektowy skill zawiera szczegółową pętlę, bariery, ABC/ECHO, watchdog i raportowanie; źródło uniwersalne oddziela reguły przenośne od wiązań nAgents; SCHEMAT opisuje bezpieczny podział.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0193 | nAgents | `SERWERY-nAgents-ustalenia.md` | `NAG-RESEARCH` | `HISTORY` | HANDOFFS_NAGENTS | RESEARCH-01/04/05 | 1 | Porównania dróg, topologii, dostawców serwerów i modeli oraz uzasadnienia odrzuceń; noty zachowują tok rozumowania.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0194 | nAgents | `SPECYFIKACJA-nAgents.md` | `NAG-LEGACY-SPEC` | `STALE` | HANDOFFS_NAGENTS | SPEC-03/SPEC-07 | 1 | Starsza narracja biznesowa, plan etapów, moduły MVP1 i raport struktury; zawiera rationale oraz luki niewidoczne w skrócie.; oznaczyć HISTORY/STALE; nie używać do routingu ani zakresu |
| PF-0195 | nAgents | `UZUPELNIENIE-MIGRACJI.md` | `NAG-HISTORY` | `HISTORY` | HANDOFFS_NAGENTS | HANDOFF-04 | 1 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0196 | nAgents | `WLASNY-LLM-nAgents-porownanie.md` | `NAG-RESEARCH` | `HISTORY` | HANDOFFS_NAGENTS | RESEARCH-01/04/05 | 1 | Porównania dróg, topologii, dostawców serwerów i modeli oraz uzasadnienia odrzuceń; noty zachowują tok rozumowania.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0197 | nAgents | `ZALACZNIK-B-serwer-OVH.md` | `NAG-RESEARCH` | `HISTORY` | HANDOFFS_NAGENTS | RESEARCH-01/04/05 | 1 | Porównania dróg, topologii, dostawców serwerów i modeli oraz uzasadnienia odrzuceń; noty zachowują tok rozumowania.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0198 | nAgents | `docs/nota-01-trzy-drogi-do-agenta.md` | `NAG-RESEARCH` | `HISTORY` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RESEARCH-01/04/05 | 12 | Porównania dróg, topologii, dostawców serwerów i modeli oraz uzasadnienia odrzuceń; noty zachowują tok rozumowania.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0199 | nAgents | `docs/nota-02-uprzaz-dla-agentow.md` | `NAG-RESEARCH` | `HISTORY` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RESEARCH-01/04/05 | 12 | Porównania dróg, topologii, dostawców serwerów i modeli oraz uzasadnienia odrzuceń; noty zachowują tok rozumowania.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0200 | nAgents | `docs/nota-02a-aneks-profile-i-zakres-v1.md` | `NAG-RESEARCH` | `HISTORY` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RESEARCH-01/04/05 | 12 | Porównania dróg, topologii, dostawców serwerów i modeli oraz uzasadnienia odrzuceń; noty zachowują tok rozumowania.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0201 | nAgents | `docs/nota-03-topologia-27-agentow.md` | `NAG-RESEARCH` | `HISTORY` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RESEARCH-01/04/05 | 12 | Porównania dróg, topologii, dostawców serwerów i modeli oraz uzasadnienia odrzuceń; noty zachowują tok rozumowania.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0202 | nAgents | `docs/nota-04-korekty-i-nowe-materialy.md` | `NAG-RESEARCH` | `HISTORY` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RESEARCH-01/04/05 | 12 | Porównania dróg, topologii, dostawców serwerów i modeli oraz uzasadnienia odrzuceń; noty zachowują tok rozumowania.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0203 | nAgents | `docs/nota-05-kupic-czy-zbudowac.md` | `NAG-RESEARCH` | `HISTORY` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RESEARCH-01/04/05 | 12 | Porównania dróg, topologii, dostawców serwerów i modeli oraz uzasadnienia odrzuceń; noty zachowują tok rozumowania.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0204 | nAgents | `docs/nota-06-appto-research.md` | `NAG-APPT0` | `SOURCE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RESEARCH-02 | 12 | Nota 06/07 syntetyzuje katalog, ceny, prywatność, regulamin i mapowanie appto; sześć plików zrodla zachowuje treść źródłową i link proweniencji.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0205 | nAgents | `docs/nota-07-katalog-funkcji.md` | `NAG-APPT0` | `SOURCE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RESEARCH-02 | 12 | Nota 06/07 syntetyzuje katalog, ceny, prywatność, regulamin i mapowanie appto; sześć plików zrodla zachowuje treść źródłową i link proweniencji.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0206 | nAgents | `docs/nota-08-wybory-otwarte.md` | `NAG-RESEARCH` | `HISTORY` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RESEARCH-01/04/05 | 12 | Porównania dróg, topologii, dostawców serwerów i modeli oraz uzasadnienia odrzuceń; noty zachowują tok rozumowania.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0207 | nAgents | `docs/nota-09-interfejs-hermesa.md` | `NAG-INTEGRATIONS` | `OWNER_DECISION_REQUIRED` | NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | INT-01..05 | 8 | Nota Hermesa rozdziela fakty lokalnej instalacji od protokołu; nota Entra i M365 opisują kroki administracyjne, uprawnienia i test dowodu.; HOLD; żadna treść nie staje się normą przed decyzją i testem |
| PF-0208 | nAgents | `docs/nota-10-entra-instrukcja-dla-administratora.md` | `NAG-INTEGRATIONS` | `OWNER_DECISION_REQUIRED` | NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | INT-01..05 | 8 | Nota Hermesa rozdziela fakty lokalnej instalacji od protokołu; nota Entra i M365 opisują kroki administracyjne, uprawnienia i test dowodu.; HOLD; żadna treść nie staje się normą przed decyzją i testem |
| PF-0209 | nAgents | `docs/proces-dla-pracownikow.md` | `NAG-USER` | `CONSOLIDATION_CANDIDATE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | USER-01..04 | 12 | Język nietechniczny, pięć sytuacji pracy i minimalna zasada przekazania/odbioru.; ekstrakcja sekcjami dopiero po owner gate i readbacku |
| PF-0210 | nAgents | `docs/process/NAGENTS-DOCS-CONSOLIDATION-PLAN.md` | `NAG-AUDIT` | `SOURCE` | NAGENTS_CHECKOUT | PROCESS-06 | 1 | Granice P1–P7, statusy, allowlisty i konkretna dispatch specyfikacja pomocnika serwerowego.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0211 | nAgents | `docs/process/dispatch/NAG-DEC-001-wybory-otwarte.md` | `NAG-EVIDENCE` | `EVIDENCE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | PROCESS-04 | 12 | GOAL, allowlista, wynik i ograniczenia konkretnych dispatchy; rozróżnia zamknięte tematy od planów.; indeksować po exact ID/run/event; zachować osobno |
| PF-0212 | nAgents | `docs/process/dispatch/NAG-INFO-001-appto-research.md` | `NAG-EVIDENCE` | `EVIDENCE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | PROCESS-04 | 12 | GOAL, allowlista, wynik i ograniczenia konkretnych dispatchy; rozróżnia zamknięte tematy od planów.; indeksować po exact ID/run/event; zachować osobno |
| PF-0213 | nAgents | `docs/process/dispatch/NAG-INFO-002-katalog-funkcji.md` | `NAG-EVIDENCE` | `EVIDENCE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | PROCESS-04 | 12 | GOAL, allowlista, wynik i ograniczenia konkretnych dispatchy; rozróżnia zamknięte tematy od planów.; indeksować po exact ID/run/event; zachować osobno |
| PF-0214 | nAgents | `docs/process/dispatch/NAG-INFRA-002-pomocnik-serwerowy.md` | `NAG-AUDIT` | `SOURCE` | NAGENTS_CHECKOUT | PROCESS-06 | 1 | Granice P1–P7, statusy, allowlisty i konkretna dispatch specyfikacja pomocnika serwerowego.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0215 | nAgents | `docs/process/dispatch/NAG-PROC-004-skill-samowystarczalny.md` | `NAG-EVIDENCE` | `EVIDENCE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | PROCESS-04 | 12 | GOAL, allowlista, wynik i ograniczenia konkretnych dispatchy; rozróżnia zamknięte tematy od planów.; indeksować po exact ID/run/event; zachować osobno |
| PF-0216 | nAgents | `docs/process/dispatch/NAG-PROC-005-skill-uniwersalny.md` | `NAG-EVIDENCE` | `EVIDENCE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | PROCESS-04 | 12 | GOAL, allowlista, wynik i ograniczenia konkretnych dispatchy; rozróżnia zamknięte tematy od planów.; indeksować po exact ID/run/event; zachować osobno |
| PF-0217 | nAgents | `docs/process/dispatch/NAG-PROC-006-ulotka-dla-pracownikow.md` | `NAG-EVIDENCE` | `EVIDENCE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | PROCESS-04 | 12 | GOAL, allowlista, wynik i ograniczenia konkretnych dispatchy; rozróżnia zamknięte tematy od planów.; indeksować po exact ID/run/event; zachować osobno |
| PF-0218 | nAgents | `docs/process/dispatch/SZABLON.md` | `NAG-EVIDENCE` | `EVIDENCE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | PROCESS-04 | 12 | GOAL, allowlista, wynik i ograniczenia konkretnych dispatchy; rozróżnia zamknięte tematy od planów.; indeksować po exact ID/run/event; zachować osobno |
| PF-0219 | nAgents | `docs/process/echo.md` | `NAG-PROCESS` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | DEC-02 | 12 | Projektowy skill zawiera szczegółową pętlę, bariery, ABC/ECHO, watchdog i raportowanie; źródło uniwersalne oddziela reguły przenośne od wiązań nAgents; SCHEMAT opisuje bezpieczny podział.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0220 | nAgents | `docs/process/handoff.md` | `NAG-HANDOFF` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | HANDOFF-01..04 | 12 | Krótki format bieżącego handoffu; root HANDOFF-* i archiwum nagents-2026-09-10 przechowują kontekst, dowody i ostrzeżenia z wcześniejszych sesji.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0221 | nAgents | `docs/process/pamiec.md` | `NAG-HISTORY` | `HISTORY` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RESEARCH-04 | 12 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0222 | nAgents | `docs/process/pytania/2026-08-22-kontrola.md` | `NAG-HISTORY` | `HISTORY` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | DEC-04 | 12 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0223 | nAgents | `docs/process/pytania/2026-08-22-warianty-c.md` | `NAG-HISTORY` | `HISTORY` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | DEC-04 | 12 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0224 | nAgents | `docs/process/pytania/2026-08-22-zestaw-1.md` | `NAG-HISTORY` | `HISTORY` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | DEC-04 | 12 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0225 | nAgents | `docs/process/pytania/2026-08-25-wybory.md` | `NAG-DECISIONS` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | DEC-01..04 | 12 | Literalne decyzje D-001…D-013, ECHO oraz jawne pytania właścicielskie; stare zestawy pytań zawierają warianty i odpowiedzi, których nie wolno przepisać bez rekonsyliacji.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0226 | nAgents | `docs/process/tematy.md` | `NAG-PROCESS` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | PROCESS-04/HANDOFF-02 | 12 | Projektowy skill zawiera szczegółową pętlę, bariery, ABC/ECHO, watchdog i raportowanie; źródło uniwersalne oddziela reguły przenośne od wiązań nAgents; SCHEMAT opisuje bezpieczny podział.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0227 | nAgents | `docs/process/zmiana-procesu.md` | `NAG-PROCESS` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | PROCESS-01..07 | 12 | Projektowy skill zawiera szczegółową pętlę, bariery, ABC/ECHO, watchdog i raportowanie; źródło uniwersalne oddziela reguły przenośne od wiązań nAgents; SCHEMAT opisuje bezpieczny podział.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0228 | nAgents | `docs/process/zrodla/appto-cennik-pl.md` | `NAG-APPT0` | `SOURCE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RESEARCH-02 | 12 | Nota 06/07 syntetyzuje katalog, ceny, prywatność, regulamin i mapowanie appto; sześć plików zrodla zachowuje treść źródłową i link proweniencji.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0229 | nAgents | `docs/process/zrodla/appto-integracje-pl.md` | `NAG-APPT0` | `SOURCE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RESEARCH-02 | 12 | Nota 06/07 syntetyzuje katalog, ceny, prywatność, regulamin i mapowanie appto; sześć plików zrodla zachowuje treść źródłową i link proweniencji.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0230 | nAgents | `docs/process/zrodla/appto-polityka-prywatnosci-pl.md` | `NAG-APPT0` | `SOURCE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RESEARCH-02 | 12 | Nota 06/07 syntetyzuje katalog, ceny, prywatność, regulamin i mapowanie appto; sześć plików zrodla zachowuje treść źródłową i link proweniencji.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0231 | nAgents | `docs/process/zrodla/appto-regulamin-pl.md` | `NAG-APPT0` | `SOURCE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RESEARCH-02 | 12 | Nota 06/07 syntetyzuje katalog, ceny, prywatność, regulamin i mapowanie appto; sześć plików zrodla zachowuje treść źródłową i link proweniencji.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0232 | nAgents | `docs/process/zrodla/appto-strona-glowna-pl.md` | `NAG-APPT0` | `SOURCE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RESEARCH-02 | 12 | Nota 06/07 syntetyzuje katalog, ceny, prywatność, regulamin i mapowanie appto; sześć plików zrodla zachowuje treść źródłową i link proweniencji.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0233 | nAgents | `docs/process/zrodla/appto-wdrozenie-kohortowe-pl.md` | `NAG-APPT0` | `SOURCE` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | RESEARCH-02 | 12 | Nota 06/07 syntetyzuje katalog, ceny, prywatność, regulamin i mapowanie appto; sześć plików zrodla zachowuje treść źródłową i link proweniencji.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0234 | nAgents | `docs/process/zrodla/autobots-szkielet-uniwersalny.md` | `NAG-PROCESS` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | PROCESS-01..07 | 12 | Projektowy skill zawiera szczegółową pętlę, bariery, ABC/ECHO, watchdog i raportowanie; źródło uniwersalne oddziela reguły przenośne od wiązań nAgents; SCHEMAT opisuje bezpieczny podział.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0235 | nAgents | `docs/spec/00-architektura.md` | `NAG-SPEC` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | SPEC-01/SPEC-02/SPEC-05 | 12 | Architektura, MVP1–MVP4, dziennik decyzji i scenariusze odbioru mają rozdzielone role; lokalny checkout wnosi D-012/D-013 i sekcje web-first/helper.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0236 | nAgents | `docs/spec/01-mvp1.md` | `NAG-SPEC` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | SPEC-03/SPEC-04 | 12 | Architektura, MVP1–MVP4, dziennik decyzji i scenariusze odbioru mają rozdzielone role; lokalny checkout wnosi D-012/D-013 i sekcje web-first/helper.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0237 | nAgents | `docs/spec/02-mvp2.md` | `NAG-SPEC` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | SPEC-03/SPEC-04 | 12 | Architektura, MVP1–MVP4, dziennik decyzji i scenariusze odbioru mają rozdzielone role; lokalny checkout wnosi D-012/D-013 i sekcje web-first/helper.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0238 | nAgents | `docs/spec/03-mvp3.md` | `NAG-SPEC` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | SPEC-03/SPEC-04 | 12 | Architektura, MVP1–MVP4, dziennik decyzji i scenariusze odbioru mają rozdzielone role; lokalny checkout wnosi D-012/D-013 i sekcje web-first/helper.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0239 | nAgents | `docs/spec/04-mvp4.md` | `NAG-SPEC` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | SPEC-03/SPEC-04 | 12 | Architektura, MVP1–MVP4, dziennik decyzji i scenariusze odbioru mają rozdzielone role; lokalny checkout wnosi D-012/D-013 i sekcje web-first/helper.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0240 | nAgents | `docs/spec/README.md` | `NAG-SPEC` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | SPEC-01/SPEC-03 | 12 | Architektura, MVP1–MVP4, dziennik decyzji i scenariusze odbioru mają rozdzielone role; lokalny checkout wnosi D-012/D-013 i sekcje web-first/helper.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0241 | nAgents | `docs/spec/decisions.md` | `NAG-SPEC` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | DEC-01 | 12 | Architektura, MVP1–MVP4, dziennik decyzji i scenariusze odbioru mają rozdzielone role; lokalny checkout wnosi D-012/D-013 i sekcje web-first/helper.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0242 | nAgents | `docs/spec/scenarios.md` | `NAG-SPEC` | `CANONICAL` | GITHUB_NAGENTS_WORK_REF; NAGENTS_AUX_WORKTREES; NAGENTS_CHECKOUT | SPEC-04 | 12 | Architektura, MVP1–MVP4, dziennik decyzji i scenariusze odbioru mają rozdzielone role; lokalny checkout wnosi D-012/D-013 i sekcje web-first/helper.; tresc normatywna; źródło zostaje kanoniczne do czasu zatwierdzonej migracji |
| PF-0243 | nAgents | `nagents-2026-09-10/00-NAGENTS-DOKUMENTACJA-GLOWNA.md` | `NAG-HISTORY` | `HISTORY` | HANDOFFS_NAGENTS | SPEC-03/SPEC-07 | 1 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0244 | nAgents | `nagents-2026-09-10/01-STAN-I-KOREKTY.md` | `NAG-HISTORY` | `HISTORY` | HANDOFFS_NAGENTS | HANDOFF-04/DEC-04/RESEARCH-04 | 1 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0245 | nAgents | `nagents-2026-09-10/02-PLAN-ETAPOW.md` | `NAG-HISTORY` | `HISTORY` | HANDOFFS_NAGENTS | SPEC-03/SPEC-07 | 1 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0246 | nAgents | `nagents-2026-09-10/03-ROLE-I-ADMINISTRACJA.md` | `NAG-HISTORY` | `HISTORY` | HANDOFFS_NAGENTS | SPEC-06/USER-04 | 1 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0247 | nAgents | `nagents-2026-09-10/04-MICROSOFT365.md` | `NAG-INTEGRATIONS` | `OWNER_DECISION_REQUIRED` | HANDOFFS_NAGENTS | INT-01..05 | 1 | Nota Hermesa rozdziela fakty lokalnej instalacji od protokołu; nota Entra i M365 opisują kroki administracyjne, uprawnienia i test dowodu.; HOLD; żadna treść nie staje się normą przed decyzją i testem |
| PF-0248 | nAgents | `nagents-2026-09-10/05-PYTANIA-I-DECYZJE.md` | `NAG-HISTORY` | `HISTORY` | HANDOFFS_NAGENTS | DEC-04 | 1 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0249 | nAgents | `nagents-2026-09-10/06-CO-MOGE-REALIZOWAC.md` | `NAG-HISTORY` | `HISTORY` | HANDOFFS_NAGENTS | HANDOFF-04/DEC-04/RESEARCH-04 | 1 | Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.; kontekst datowany; tylko unikalny fragment po literalnym porównaniu |
| PF-0250 | nAgents | `nagents-2026-09-10/sources/10.md` | `NAG-SOURCES` | `SOURCE` | HANDOFFS_NAGENTS | RESEARCH-03 (INT-02/03/04 only after gate) | 1 | Dokumentacja OAuth/Entra/Graph/Hermes zachowana w surowym, datowanym snapshotcie; `sources/8.md` jest pustym artefaktem.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0251 | nAgents | `nagents-2026-09-10/sources/6.md` | `NAG-SOURCES` | `SOURCE` | HANDOFFS_NAGENTS | RESEARCH-03 (INT-02/03/04 only after gate) | 1 | Dokumentacja OAuth/Entra/Graph/Hermes zachowana w surowym, datowanym snapshotcie; `sources/8.md` jest pustym artefaktem.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0252 | nAgents | `nagents-2026-09-10/sources/7.md` | `NAG-SOURCES` | `SOURCE` | HANDOFFS_NAGENTS | RESEARCH-03 (INT-02/03/04 only after gate) | 1 | Dokumentacja OAuth/Entra/Graph/Hermes zachowana w surowym, datowanym snapshotcie; `sources/8.md` jest pustym artefaktem.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0253 | nAgents | `nagents-2026-09-10/sources/8.md` | `NAG-SOURCES` | `SOURCE` | HANDOFFS_NAGENTS | RESEARCH-03 (INT-02/03/04 only after gate) | 1 | Dokumentacja OAuth/Entra/Graph/Hermes zachowana w surowym, datowanym snapshotcie; `sources/8.md` jest pustym artefaktem.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |
| PF-0254 | nAgents | `nagents-2026-09-10/sources/9.md` | `NAG-SOURCES` | `SOURCE` | HANDOFFS_NAGENTS | RESEARCH-03 (INT-02/03/04 only after gate) | 1 | Dokumentacja OAuth/Entra/Graph/Hermes zachowana w surowym, datowanym snapshotcie; `sources/8.md` jest pustym artefaktem.; cytat/odnośnik z datą i hash; źródło read-only, nie mieszać z analizą |


## Dodatek B — jawny rejestr 12 wariantów i 3 driftów

### B.1 Różne hashe tej samej ścieżki

| Projekt / ścieżka | Warianty P2 (hash prefix, rekordy, linie) | Trasa | Decyzja |
|---|---|---|---|
| AutoBot Monitor / `AGENTS.md` | `a8c147f5a7b5`, 1 rekordów, 118 linii; `b3f05aa330fa`, 1 rekordów, 127 linii | ABM-01/02/07; `AUTOBOT-KANBAN.md` remains canonical | Nie wybierać automatycznie; porównać sekcje literalnie, zachować oba hashe i uzyskać zgodę właściwej bramy. |
| AutoBot Monitor / `docs/CRON-DIRECTIVE-LOOP.md` | `b64ed49d5d60`, 1 rekordów, 151 linii; `cbe44f709567`, 1 rekordów, 180 linii | ABM-03/04/06/07 | Nie wybierać automatycznie; porównać sekcje literalnie, zachować oba hashe i uzyskać zgodę właściwej bramy. |
| AutoBot Monitor / `package/README.md` | `3e4ea56b8c60`, 1 rekordów, 84 linii; `d66fbd771952`, 1 rekordów, 40 linii | ABM-05/LIFE-04 | Nie wybierać automatycznie; porównać sekcje literalnie, zachować oba hashe i uzyskać zgodę właściwej bramy. |
| nAgents / `CLAUDE.md` | `83fcb63c4e32`, 11 rekordów, 81 linii; `8de583cc4fec`, 1 rekordów, 121 linii | RETAIN-INDEX-01 | Nie wybierać automatycznie; porównać sekcje literalnie, zachować oba hashe i uzyskać zgodę właściwej bramy. |
| nAgents / `HANDOFF-nagents.md` | `2e9ccd808e81`, 1 rekordów, 421 linii; `50395ae262e7`, 11 rekordów, 371 linii | HANDOFF-04 | Nie wybierać automatycznie; porównać sekcje literalnie, zachować oba hashe i uzyskać zgodę właściwej bramy. |
| nAgents / `docs/process/echo.md` | `24c4e6bb92ed`, 2 rekordów, 133 linii; `9928ebc85dde`, 9 rekordów, 120 linii; `e631fb908120`, 1 rekordów, 146 linii | DEC-02 | Nie wybierać automatycznie; porównać sekcje literalnie, zachować oba hashe i uzyskać zgodę właściwej bramy. |
| nAgents / `docs/process/tematy.md` | `34e675e6a834`, 1 rekordów, 59 linii; `3b4f72fe7ecf`, 1 rekordów, 62 linii; `56dfb5d451e5`, 6 rekordów, 51 linii; `9257a6cd1c08`, 1 rekordów, 63 linii; `ac99b0371347`, 1 rekordów, 61 linii; `beac7256c93e`, 1 rekordów, 56 linii; `c26e7a441689`, 1 rekordów, 60 linii | PROCESS-04/HANDOFF-02 | Nie wybierać automatycznie; porównać sekcje literalnie, zachować oba hashe i uzyskać zgodę właściwej bramy. |
| nAgents / `docs/spec/00-architektura.md` | `17318e9014ca`, 11 rekordów, 233 linii; `322a6c2a76dc`, 1 rekordów, 283 linii | SPEC-01/SPEC-02/SPEC-05 | Nie wybierać automatycznie; porównać sekcje literalnie, zachować oba hashe i uzyskać zgodę właściwej bramy. |
| nAgents / `docs/spec/01-mvp1.md` | `cc2687413e02`, 10 rekordów, 214 linii; `e2dfb80d1980`, 2 rekordów, 214 linii | SPEC-03/SPEC-04 | Nie wybierać automatycznie; porównać sekcje literalnie, zachować oba hashe i uzyskać zgodę właściwej bramy. |
| nAgents / `docs/spec/README.md` | `2f5719a2a41e`, 1 rekordów, 54 linii; `d075b08f5f14`, 11 rekordów, 38 linii | SPEC-01/SPEC-03 | Nie wybierać automatycznie; porównać sekcje literalnie, zachować oba hashe i uzyskać zgodę właściwej bramy. |
| nAgents / `docs/spec/decisions.md` | `57264a14bcf3`, 1 rekordów, 264 linii; `ee793c30d8a2`, 11 rekordów, 175 linii | DEC-01 | Nie wybierać automatycznie; porównać sekcje literalnie, zachować oba hashe i uzyskać zgodę właściwej bramy. |
| nAgents / `docs/spec/scenarios.md` | `7084dc99b657`, 11 rekordów, 71 linii; `8b8be2a4e631`, 1 rekordów, 92 linii | SPEC-04 | Nie wybierać automatycznie; porównać sekcje literalnie, zachować oba hashe i uzyskać zgodę właściwej bramy. |


### B.2 Drift po P2

| Źródło | Ścieżka | P2 SHA prefix | Bieżący SHA prefix | P2/current bajty | P2/current linie | Działanie |
|---|---|---|---|---:|---:|---|
| ABM_CHECKOUT | `AUTOBOT-KANBAN.md` | 87c426598b7d | 64b99a0fbf76 | 25883/26296 | 592/601 | Świeży readback i kwalifikacja właściciela; nie nadpisywać, nie traktować jako publikacji. |
| ABM_CHECKOUT | `docs/ABM-CARD-TAGGING-GUIDE.md` | 2bfddbac9e47 | c109d5f6015c | 9306/15586 | 171/568 | Świeży readback i kwalifikacja właściciela; nie nadpisywać, nie traktować jako publikacji. |
| NAGENTS_CHECKOUT | `NAGENTS-PROJECT.md` | 6da275008231 | ba7c979edd4f | 32249/53435 | 568/818 | Świeży readback i kwalifikacja właściciela; nie nadpisywać, nie traktować jako publikacji. |


## Dodatek C — statusy i wyłączenia

| Klasa | Traktowanie w planie |
|---|---|
| `CANONICAL` | wskazuje normę bieżącą; przyszła paczka cytuje i linkuje, ale nie zmienia jej rangi bez bramy |
| `SOURCE` | zachować treść źródłową, datę, hash/link; nie mieszać z analizą |
| `HISTORY` / `STALE` | zachować jako historię; wyciągać wyłącznie unikalny, zweryfikowany fragment z etykietą |
| `EVIDENCE` | zachować exact task/run/event/artifact; indeks nie zastępuje dowodu |
| `CONSOLIDATION_CANDIDATE` | kandydat do ekstrakcji po coverage i owner gate, nie polecenie wykonania |
| `OWNER_DECISION_REQUIRED` | zatrzymać właściwy strumień; nie wpisywać jako faktu normatywnego |
| `LIVE_READBACK_REQUIRED` | sprawdzić stan runtime/board/service przed każdym twierdzeniem lub przejściem |
| `LOCAL_ONLY` | flaga proweniencji; nie znaczy „najnowsze” ani „opublikowane” |
| `REMOTE_ONLY` | w tym audycie 0; gdy pojawi się później, nie wybierać automatycznie |
| `PRIVATE_RUNTIME` / `EXCLUDED_SECRET` / `EXCLUDED_PII` / `EXCLUDED_RAW_LOG` | nie czytać i nie kopiować; zachować tylko bezpieczną etykietę/status, jeśli potrzebny do proweniencji |
| `SEPARATE_PROJECT` | nie przenosić do nAgents; The-Game ma własny board, runtime i pętlę |

Pełna klasyfikacja 833 rekordów pozostaje w `P4-classification.json`; ten plan
nie zastępuje maszynowego źródła i nie rozszerza zakresu audytu.
