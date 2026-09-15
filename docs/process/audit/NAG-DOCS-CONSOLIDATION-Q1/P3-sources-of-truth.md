# NAG-DOCS-CONSOLIDATION-Q1 — P3 źródła prawdy

STATUS: PASS
DOMAIN: INFORMACYJNY
TEMAT: NAG-DOCS-P3-SOURCES-OF-TRUTH-Q1
GOAL: Każda kategoria pytania agenta ma jedno źródło kanoniczne albo jawny status OWNER_DECISION_REQUIRED/LIVE_READBACK_REQUIRED/SEPARATE_PROJECT.
OBSERVED_AT: 2026-09-14T14:05:37+00:00

## Wynik

Ustalono mapę dla 16 kategorii. Dziesięć ma kanoniczny dokument, cztery
wymagają świeżego odczytu stanu, jedna wymaga decyzji właściciela, a The-Game
pozostaje osobnym projektem. P3 ustala miejsce pierwszego odczytu; nie scala
treści, nie wybiera zwycięzcy duplikatów i nie zmienia decyzji właściciela.

Pełny inwentarz wejściowy pozostaje w artefaktach P1/P2. Ta mapa odwołuje się
do jego `source_id`, korzenia, ścieżki i snapshotu, zamiast kopiować zawartość
833 dokumentów: 695 lokalnych, 104 z nazwanych refów GitHuba i 34 handoffowych.

## Mapa źródeł

| # | Pytanie | Źródło kanoniczne | Pomocnicze | Kiedy czytać | Status | Właściciel aktualizacji | Konflikt |
|---:|---|---|---|---|---|---|---|
| 1 | Gdzie agent zaczyna lekturę i jakie twarde zasady obowiązują? | `NAGENTS_CHECKOUT :: CLAUDE.md` | `NAGENTS-PROJECT.md` jako kandydat indeksu P5; `.claude/skills/nagents-autobot/README.md` | Pierwszy odczyt każdej sesji, przed zmianą zakresu | `CANONICAL` | Orkiestrator; decyzje właściciela tylko w głównym wątku | `NAGENTS-PROJECT.md` proponuje nowy punkt wejścia, ale nie zastępuje `CLAUDE.md` przed osobną bramką P5 i integracją. |
| 2 | Jaki system budujemy, z jakich warstw i z jakimi granicami bezpieczeństwa? | `NAGENTS_CHECKOUT :: docs/spec/00-architektura.md` | `docs/spec/README.md`; właściwy plik MVP | Przy pytaniu o model, zakres, dane, bezpieczeństwo lub stos | `CANONICAL` | Orkiestrator w ramach zatwierdzonego procesu; zmiany decyzji przez ownera | Decyzje właściciela i świeży readback stanu mają pierwszeństwo przed opisem statycznym. |
| 3 | Jaki jest zakres i kolejność MVP1–MVP4? | `NAGENTS_CHECKOUT :: docs/spec/README.md` | `docs/spec/01-mvp1.md` … `04-mvp4.md`; `docs/process/tematy.md` dla kolejności tematów | Najpierw przy planowaniu etapu, potem tylko specyfikacja wybranego MVP | `CANONICAL` | Orkiestrator; zmiana zakresu wymaga właściciela | Specyfikacja mówi, co ma powstać; `tematy.md` i live readback mówią, co faktycznie jest zakończone. |
| 4 | Jak działa proces AutoBot i jak bezpiecznie zmieniać sam proces? | `NAGENTS_CHECKOUT :: .claude/skills/nagents-autobot/SKILL.md` | `CLAUDE.md`; `docs/process/zmiana-procesu.md`; `docs/process/dispatch/SZABLON.md`; `docs/process/tematy.md` | Przed dispatch’em, oceną, obroną, integracją lub zmianą reguł procesu | `CANONICAL` | Orkiestrator zgodnie z trybem zmiany procesu; bariery wymagają ECHO | Uniwersalny szkielet jest materiałem referencyjnym; projektowy skill wiąże zasady dla nAgents. |
| 5 | Która faza i karta mogą być uruchomione teraz? | `LIVE_READBACK_REQUIRED :: board autobot-monitor, task/event/run/receipt readback` | `docs/process/NAGENTS-DOCS-CONSOLIDATION-PLAN.md`; `/home/ubuntu/projects/Autoboot-Monitor/AUTOBOT-KANBAN.md`; runbook Cron/receivera | Przed każdym przejściem, wznowieniem, utworzeniem następcy lub oceną statusu | `LIVE_READBACK_REQUIRED` | Dispatcher/receiver utrzymują stan; orkiestrator tylko odczytuje i kwalifikuje | Statyczny plan, `queued`, UI i raport nie zastępują terminalnego eventu oraz zgodności rodziców, projektu i runu. |
| 6 | Jaki jest bieżący stan repozytorium, tematów, usług i pracy? | `LIVE_READBACK_REQUIRED :: git status/branch/HEAD + board stats/list/show + service/profile readback` | `docs/process/tematy.md`; `NAGENTS-PROJECT.md`; `HANDOFF-nagents.md`; `docs/process/handoff.md`; P2-counts | Bezpośrednio przed twierdzeniem o stanie, integracją albo kolejnym dispatch’em | `LIVE_READBACK_REQUIRED` | Właściciel odpowiedniego systemu; orkiestrator zapisuje tylko bezpieczny snapshot | Handoffy i indeksy są datowanymi snapshotami. Obecny checkout jest dirty i nie wolno utożsamiać go z publikacją. |
| 7 | Gdzie jest bieżący handoff i jaki ma format? | `NAGENTS_CHECKOUT :: docs/process/handoff.md` | `HANDOFF-nagents.md`; `/home/ubuntu/handoffs/AUTOBOOT-INDEKS.md`; `/home/ubuntu/handoffs/NAGENTS-INDEKS.md` | Przy przejęciu kontekstu, po terminalnej zmianie fazy i przed nowym przekazaniem | `CANONICAL` | Orkiestrator zastępuje cały bieżący handoff; nie dopisuje logu | Dokument jest kanonicznym miejscem handoffu, ale jego datowany obraz nie rozstrzyga live state; starszy `HANDOFF-nagents.md` jest historią/snapshotem. |
| 8 | Jakie decyzje właściciela obowiązują? | `NAGENTS_CHECKOUT :: docs/spec/decisions.md` | `docs/process/echo.md`; właściwy wpis pytania; `CLAUDE.md` | Przed pracą dotykającą kosztu, danych, dostępu, prawa lub odwracalności | `CANONICAL` | Właściciel zapisuje decyzję; orkiestrator nie dopowiada ani nie poprawia po cichu | Literalne ECHO i aktualny dziennik rozstrzygają; rekomendacja, stary handoff i brak sprzeciwu nie są decyzją. |
| 9 | Jakie pytania właścicielskie pozostają otwarte i w jakiej kolejności? | `NAGENTS_CHECKOUT :: docs/process/pytania/2026-08-25-wybory.md` | `docs/spec/decisions.md` dla statusu rozstrzygnięcia; `docs/process/echo.md`; starsze pakiety pytań tylko jako historia | Przed zadaniem pytania; najpierw porównać z decyzjami i ECHO | `CANONICAL` | Orkiestrator utrzymuje pakiet pytań; właściciel odpowiada w głównym wątku | Pakiet zawiera także pytania, które mogły zostać później rozstrzygnięte; nie wysyłać ich ponownie bez rekonsyliacji. |
| 10 | Jakie sytuacje muszą działać i z czego wynikają testy? | `NAGENTS_CHECKOUT :: docs/spec/scenarios.md` | Specyfikacje MVP; kryteria konkretnego dispatchu; testy wskazane przez temat | Przed planem testów i przy ocenie spełnienia GOAL | `CANONICAL` | Orkiestrator; zmiana scenariusza wymaga jawnego zakresu | Tabele odbioru w specyfikacjach mogą zawężać widok; rejestr scenariuszy jest kontraktem nadrzędnym. |
| 11 | Jakie integracje są wybrane i które fakty są dowiedzione? | `OWNER_DECISION_REQUIRED :: wybór głównego punktu styku i zakresu integracji` | `NAGENTS_CHECKOUT :: docs/spec/00-architektura.md`; `docs/nota-09-interfejs-hermesa.md`; `docs/nota-10-entra-instrukcja-dla-administratora.md`; `/home/ubuntu/handoffs/INTEGRACJA-MICROSOFT365.md`; oficjalna dokumentacja dostawcy | Przed wpisaniem integracji do decyzji, kodu lub planu wdrożenia | `OWNER_DECISION_REQUIRED` | Właściciel wybiera zakres; orkiestrator dokumentuje i zleca weryfikację | Nota Hermesa rozdziela opis protokołu od testu runtime, a handoff Microsoft 365 oznacza projekt jako nieudowodniony. Nie wybierać mechanizmu za właściciela. |
| 12 | Co jest dowodem wykonania fazy lub tematu? | `LIVE_READBACK_REQUIRED :: exact task run + report/artifact + terminal completion event + independent verification` | `docs/process/dispatch/SZABLON.md`; `/home/ubuntu/projects/Autoboot-Monitor/runs/README.md`; raport i artefakty P1/P2/P3 | Po zakończeniu workera i przed odblokowaniem następnej fazy | `LIVE_READBACK_REQUIRED` | Worker zapisuje artefakt; Evaluator/Final Control odczytują niezależnie; orkiestrator kwalifikuje | `PASS`, status UI, nazwa worktree, `queued` i deklaracja agenta nie są dowodem bez eventu, artefaktu i readbacku. |
| 13 | Gdzie szukać historii rozumowania i wcześniejszych ustaleń? | `NAGENTS_CHECKOUT :: docs/process/pamiec.md` | Git history; `/home/ubuntu/handoffs/` jako datowane archiwum; stare noty `docs/nota-*.md` | Gdy trzeba wyjaśnić genezę lub korektę; nie przy bieżącym routingu | `CANONICAL` | Orkiestrator dopisuje tylko zgodnie z procesem; źródła pierwotne pozostają niezmienione | Historia wyjaśnia „dlaczego”, ale nie zastępuje decyzji, specyfikacji ani live readbacku. |
| 14 | Czy zmiana jest opublikowana i na jakim refie? | `LIVE_READBACK_REQUIRED :: git ls-remote named ref + remote tree/content readback` | `docs/process/NAGENTS-DOCS-CONSOLIDATION-PLAN.md` §P7; `NAGENTS-PROJECT.md` sekcja publikacji; lokalny diff/status | Dopiero po jawnej allowliście publikacji i po pushu wykonanym przez uprawnionego integratora | `LIVE_READBACK_REQUIRED` | Orkiestrator/integrator po zgodzie właściciela; worker nie publikuje | Lokalny commit, branch, plik lub raport nie dowodzi publikacji. `main`, push i merge wymagają osobnej bramki. |
| 15 | Jak działa AutoBot Monitor jako system towarzyszący? | `ABM_CHECKOUT :: AUTOBOT-KANBAN.md` | `ABM_CHECKOUT :: docs/ABM-CRON-HELPER-OPERATING-RUNBOOK.md`; `docs/CRON-DIRECTIVE-LOOP.md`; `docs/AUTOBOT-MODEL-EFFORT-FAST-POLICY.md`; aktywny relay worktree; `runs/README.md` | Przy pytaniu o kontrakt Monitor, Cron, receiver, routing lub evidence | `CANONICAL` | Zespół/owner AutoBot Monitor; zmiany nAgents nie aktualizują tego repozytorium | To osobny projekt wykonawczy. Kontrakt statyczny nie dowodzi bieżącego stanu usług; do tego potrzebny jest live readback Monitor. |
| 16 | Czy materiał dotyczący The-Game jest źródłem nAgents? | `SEPARATE_PROJECT :: The-Game repository and its own board/runtime` | `/home/ubuntu/handoffs/THE-GAME-INTEGRATION-HANDOFF.md` wyłącznie jako metadane granicy; wskazane katalogi The-Game tylko w osobnym zadaniu | Tylko przy zadaniu jawnie przypisanym do The-Game; nigdy jako fallback nAgents | `SEPARATE_PROJECT` | Właściciel i orkiestrator The-Game w jego boardzie | Kod, branche, handoffy i statusy The-Game nie ustanawiają źródła prawdy nAgents i nie mogą sterować jego boardem. |

## Reguły rozstrzygania

1. Świeży odczyt Git/worktree, drzewa nazwanego refu, Kanbana, eventu,
   receiptu, usługi lub testu rozstrzyga bieżący stan.
2. Jednoznaczna decyzja właściciela w `docs/process/echo.md` albo aktualnym
   `docs/spec/decisions.md` rozstrzyga koszt, dane, dostęp, prawo i odwracalność.
3. `CLAUDE.md`, `docs/spec/` i projektowy skill AutoBot opisują aktualną normę;
   nie tworzą decyzji przez samą obecność.
4. Kontrakt AutoBot Monitor obowiązuje dla jego własnego projektu; nie miesza
   się go z kodem ani routingiem nAgents.
5. Handoffy, raporty, noty i stare pytania są kontekstem datowanym. Nie zastępują
   live readbacku ani decyzji.
6. `LOCAL_ONLY`, `REMOTE_ONLY`, `STALE`, `EVIDENCE`, `HISTORY` i
   `SEPARATE_PROJECT` opisują proweniencję lub rolę. Nie są zgodą na kopiowanie,
   usuwanie ani publikację.

## Granica P3

P3 nie klasyfikuje ponownie wszystkich 833 rekordów i nie usuwa żadnego pliku.
P2 pozostaje pełnym inwentarzem. P4 ma rozstrzygnąć duplikaty, nieaktualność,
unikalną treść i kandydatów do konsolidacji, korzystając z tej mapy jako
pierwszego routingu.

Nie czytano sekretów, credentiali, `state.db`, sesji ani surowych logów. Nie
kopiowano PII. Nie wykonywano `fetch`, `pull`, `git add`, commit, push, merge,
deployu ani restartu. Dirty checkout nAgents oraz aktywny dirty worktree
AutoBot Monitor pozostają nienaruszone.

## Dowód i następny krok

TESTY: odtworzono liczniki P2 (695/104/34, razem 833; `LOCAL_ONLY` 132,
`REMOTE_ONLY` 0); sparsowano trzy JSONL i `P2-counts.json`; sprawdzono, że
każda z 16 kategorii ma dokładnie jeden dokument kanoniczny albo jawny
`OWNER_DECISION_REQUIRED`, `LIVE_READBACK_REQUIRED` lub `SEPARATE_PROJECT`;
zweryfikowano istnienie ścieżek pomocniczych względem P2; wykonano `git diff --check`.

BLOKADY: integracje wymagają decyzji właściciela; routing, bieżący stan,
dowody i publikacja wymagają każdorazowego live readbacku. Nie blokuje to
samego artefaktu P3 ani niezależnych faz.

NASTĘPNY KROK: P4 — sklasyfikować duplikaty i nieaktualność bez usuwania źródeł,
zachowując proweniencję oraz statusy z tej mapy.

DEPLOY/PUSH: NIE WYKONANO
