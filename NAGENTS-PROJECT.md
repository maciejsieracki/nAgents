# nAgents — NAGENTS-PROJECT

> **Kanoniczny indeks dokumentacji projektu.** Ten plik mówi agentowi, gdzie
> szukać informacji i jak odróżniać źródło prawdy od historii, propozycji,
> raportu oraz bieżącego stanu runtime.
>
> **Audyt:** OVH, 2026-09-14, 13:51 Europe/Warsaw.  
> **Zakres audytu:** checkout nAgents, checkout AutoBot Monitor, aktywne
> worktree, `/home/ubuntu/handoffs/`, znane pliki koordynacji oraz drzewa
> GitHuba dostępne dla `maciejsieracki/nAgents` i
> `maciejsieracki/Autoboot-Monitor`.
>
> **Ważne:** indeks jest mapą. Nie jest kopią wszystkich treści i nie zastępuje
> świeżego readbacku Git, Kanbana ani usług serwera.

---

## 0. Zacznij tutaj — minimalny kontekst

Nowy agent czyta **tylko ten plik na start**. Następnie wybiera ścieżkę według
tabeli w §2. Nie ładuje wszystkich raportów, handoffów i historii naraz.

```text
1. NAGENTS-PROJECT.md                         — ta mapa
2. CLAUDE.md                                 — twarde zasady projektu
3. docs/process/tematy.md                    — bieżący rejestr tematów
4. właściwa decyzja/specyfikacja/scenariusz  — tylko dla bieżącego zadania
5. właściwy dispatch i raporty                — dopiero dla konkretnego tematu
6. live readback                              — zawsze przed twierdzeniem o stanie
```

### Czym jest projekt

```text
nAgents
├── aplikacja i warstwa zarządzania użytkownikiem, rolami, kosztami i audytem
├── dokumentacja architektury, decyzji i scenariuszy
├── AutoBot jako proces pracy i kontrola faz
├── AutoBot Monitor jako osobne repozytorium wykonawcze/obserwacyjne
└── Hermes jako zewnętrzny silnik agentów i runtime
```

`The-Game` jest osobnym projektem, boardem i profilem. Może być opisany w
indeksie jako zależność lub przykład działania AutoBot, ale jego kodu,
branchy i dokumentacji nie wolno mieszać z nAgents.

---

## 1. Hierarchia źródeł prawdy

Gdy dwa dokumenty mówią co innego, użyj tej kolejności:

1. **Świeży odczyt stanu obowiązującego** — Git/worktree, Kanban, event,
   receipt, usługa lub test wykonany na wskazanej wersji.
2. **Jednoznaczna decyzja właściciela** zapisana w `docs/process/echo.md` lub
   w aktualnym dzienniku decyzji.
3. **Aktualna specyfikacja repozytorium** — `docs/spec/` i `CLAUDE.md`.
4. **Aktualny kontrakt AutoBot** — `Autoboot-Monitor/AUTOBOT-KANBAN.md`.
5. **Bieżący handoff** — obraz stanu, który może się zestarzeć.
6. **Raport Operatora/Evaluatora/Final Control** — dowód określonej fazy,
   nie globalny stan projektu.
7. **Noty, pytania, stare handoffy i rozmowy** — historia lub propozycja,
   nigdy samodzielny routing.

`PASS` w raporcie, nazwa brancha, obecność pliku, status `done` lub stary
snapshot nie oznaczają samodzielnie integracji, publikacji ani wdrożenia.

---

## 2. Co przeczytać dla konkretnego pytania

| Potrzebna informacja | Czytaj najpierw | Potem, jeśli potrzebne |
|---|---|---|
| Czym jest nAgents i jaki ma zakres | [`docs/spec/00-architektura.md`](docs/spec/00-architektura.md) | `docs/spec/01-mvp1.md` … `04-mvp4.md` |
| Co jest decyzją właściciela | [`docs/process/echo.md`](docs/process/echo.md) | [`docs/spec/decisions.md`](docs/spec/decisions.md) |
| Jakie decyzje są otwarte | `docs/spec/decisions.md` | `docs/process/pytania/2026-08-25-wybory.md`, `docs/nota-08-wybory-otwarte.md` |
| Jakie sytuacje muszą działać | [`docs/spec/scenarios.md`](docs/spec/scenarios.md) | testy wskazane w dispatchu |
| Co jest aktywne/zablokowane | [`docs/process/tematy.md`](docs/process/tematy.md) | świeży board Kanbana |
| Co ustalono ostatnio | [`HANDOFF-nagents.md`](HANDOFF-nagents.md) | `git log`, `git status`, świeży readback |
| Jak zmieniać sam proces | [`docs/process/zmiana-procesu.md`](docs/process/zmiana-procesu.md) | projektowy skill AutoBot |
| Jak utworzyć i ocenić temat | `docs/process/dispatch/SZABLON.md` | konkretny `docs/process/dispatch/<ID>.md` |
| Jak działa AutoBot Monitor | `/home/ubuntu/projects/Autoboot-Monitor/AUTOBOT-KANBAN.md` | runbook i dokument Crona poniżej |
| Jak działa Cron/receiver | `Autoboot-Monitor/docs/CRON-DIRECTIVE-LOOP.md` | `ABM-CRON-HELPER-OPERATING-RUNBOOK.md`, live `cron list` |
| Jak działa routing modelu/effortu | `Autoboot-Monitor/docs/AUTOBOT-MODEL-EFFORT-FAST-POLICY.md` | karta/run i receipt konkretnego workera |
| Jak działa relay owner chatu | aktywny dispatch `ABM-OWNER-CHAT-RELAY-001` | `docs/OWNER-CHAT-RELAY.md` w aktywnym worktree, potem Final Control |
| Jak działa Microsoft 365 | `docs/nota-10-entra-instrukcja-dla-administratora.md` | `/home/ubuntu/handoffs/INTEGRACJA-MICROSOFT365.md` |
| Skąd wzięły się historyczne ustalenia | `docs/process/pamiec.md` | `/home/ubuntu/handoffs/RAPORT-nAgents-scenariusz-i-plan.md` |
| Jak wygląda infrastruktura OVH | świeży odczyt usług i profilu | `/home/ubuntu/handoffs/MIGRACJA-OVH-STATUS.md`, `SERWERY-nAgents-ustalenia.md` |
| Co jest na GitHubie | `git ls-remote`, `git ls-tree` lub `gh api` | porównanie z lokalnym HEAD i diffem |

---

## 3. Routing projektu

### 3.1. nAgents → AutoBot Monitor → Hermes

```text
nAgents
  → repozytorium dokumentacji, aplikacji i decyzji
  → AutoBot Monitor / board autobot-monitor
  → profile: autobotmonitor
  → Cron read-only, no_agent=true, every 5m
  → server spool
  → supervised receiver/helper
  → Kanban task/run/event/receipt
  → Operator
  → Evaluator
  → Defense tylko przy konkretnych zarzutach
  → Final Control
  → INTEGRATION_REQUIRED
  → integracja przez Orkiestratora
```

Worker nie wykonuje merge, push, deploy ani publikacji. Cron nie jest workerem
produktu i nie mutuje Kanbana. Desktop jest klientem/obserwatorem, nie
rodzicem serwerowego procesu.

### 3.2. Kanoniczne identyfikatory runtime

| Element | Wartość / reguła | Gdzie sprawdzać |
|---|---|---|
| AutoBot profile | `autobotmonitor` | `hermes profile list/show`, karta Kanbana |
| AutoBot board | `autobot-monitor` | `hermes kanban boards list`, każda komenda z `--board` |
| The-Game profile | `the-game` | osobny projekt, nie fallback dla nAgents |
| The-Game board | `the-game-real24` | osobny board |
| AutoBot Cron | `83e4098a9f87`, every 5m, `no_agent=true`, `deliver=local` | `hermes --profile autobotmonitor cron list` |
| Worker completion | native `kanban_complete` albo `kanban_block` | karta, event, run, raport |
| Dostawa | `queued → claimed → settled` | native delivery receipt; `queued` nie oznacza ukończenia |
| Fallbacki | `default_assignee: ''`, `orchestrator_profile: ''` | profil Hermes, nie zgadywać |

### 3.3. Live readback — komendy

```bash
# nAgents: wersja lokalna i różnice; bez pull/reset/stash/clean
cd /home/ubuntu/projects/nAgents-readonly
git status --short --branch
git log -5 --oneline --decorate

# AutoBot Monitor: zawsze jawny profil i board
hermes --profile autobotmonitor kanban --board autobot-monitor stats --json
hermes --profile autobotmonitor kanban --board autobot-monitor list --status running --json
hermes --profile autobotmonitor cron list
systemctl --user is-active hermes-gateway-autobotmonitor.service
systemctl --user is-active autobot-monitor-cron-receiver.service

# Sprawdzenie GitHuba — odczyt, bez fetch/pull
cd /home/ubuntu/projects/nAgents-readonly
git ls-remote origin refs/heads/main refs/heads/claude/git-connection-9sz6dg
```

---

## 4. Stan audytu — OVH kontra GitHub

### 4.1. nAgents

| Miejsce | Stan odczytany 2026-09-14 | Znaczenie |
|---|---|---|
| OVH checkout | HEAD `f5a9101`, branch `claude/git-connection-9sz6dg`, 19 commitów ponad znany `origin`, 8 zmienionych plików, nowy dispatch i ten indeks | lokalny stan roboczy; nie jest automatycznie gotowym wydaniem |
| GitHub `claude/git-connection-9sz6dg` | HEAD `55b2d735` | zawiera 46 Markdownów; lokalny checkout ma 4 dodatkowe ścieżki |
| GitHub `main` | HEAD `9522836d` | zawiera tylko `README.md`; nie jest bieżącym branch’em pracy |
| testy lokalne | `git diff --check` przechodzi; `python3 -m pytest` nie działa, bo brak modułu `pytest` | testy aplikacji są `N/D/INFRA`, nie `PASS` |

Lokalne ścieżki nAgents niewystępujące na zdalnej gałęzi roboczej:

```text
docs/nota-09-interfejs-hermesa.md
docs/nota-10-entra-instrukcja-dla-administratora.md
docs/process/dispatch/NAG-INFRA-002-pomocnik-serwerowy.md
NAGENTS-PROJECT.md
```

### 4.2. AutoBot Monitor

| Miejsce | Stan odczytany 2026-09-14 | Znaczenie |
|---|---|---|
| OVH checkout | HEAD `c44c381`, `main`, 19 commitów ponad znany `origin`, kod i dokumentacja dirty | bieżący warsztat, nie publikacja |
| GitHub `main` | HEAD `6b057bad` i 56 Markdownów | starszy pakiet; nie zawiera całego obecnego kontraktu AutoBot |
| lokalny root AutoBot Monitor | 198 Markdownów | część to runy/evidence i materiały lokalne; nie wszystko jest normą |
| worktree AutoBot | 25 worktree, około 2 995 Markdownów | głównie kopie, raporty i fixtures; nie ładować zbiorczo |
| aktywna karta | `t_822096ba`, Defense r2 relayu, run `177` | nie dotykać współdzielonego worktree podczas pracy |

### 4.3. Usługi i karta The-Game

- Gateway `autobotmonitor` i `autobot-monitor-cron-receiver.service` były
  aktywne w ostatnim odczycie.
- R20 Save/Load w The-Game: Operator `t_310eb82d` i Evaluator `t_e32fea1c`
  zakończyli się `PASS`, bez merge/push/deploy. To osobny projekt i osobna
  bramka Final Control.
- Historyczne handoffy z `/home/ubuntu/autobot-real24-20260911/` nie są
  bieżącym źródłem prawdy dla nAgents.

---

## 5. Katalog Markdownów repozytorium nAgents — 50 plików lokalnie

Statusy w tabeli:

- **CANONICAL** — czytać dla bieżącej pracy;
- **HISTORY** — kontekst, nie routing;
- **LOCAL-ONLY** — jest na OVH, nie ma go na audytowanej gałęzi GitHuba;
- **SOURCE** — materiał pierwotny, nie zmieniać;
- **EVIDENCE** — dowód konkretnego tematu;
- **STALE** — zawiera stan lub założenia, które trzeba porównać z live readbackiem.

| Plik | Do czego służy | Status / kiedy czytać |
|---|---|---|
| `NAGENTS-PROJECT.md` | ta mapa: kolejność lektury, katalog plików, routing i korekty aktualności | CANONICAL ENTRYPOINT |
| `.claude/skills/nagents-autobot/README.md` | opis skilla i jego przenoszenia | CANONICAL pomocniczy |
| `.claude/skills/nagents-autobot/SKILL.md` | pełny proces AutoBot, role, bariery, raporty i limity | CANONICAL; czytać sekcjami, nie ładować bez potrzeby |
| `CLAUDE.md` | punkt startowy, kolejność lektury i twarde zasady | CANONICAL; lokalnie zmieniony, porównać z diffem |
| `HANDOFF-nagents.md` | bieżący handoff projektu | STALE/LOCAL-CHANGED; snapshot, nie live routing |
| `README.md` | obecnie tylko tytuł repozytorium | INCOMPLETE; indeks ten uzupełnia brak mapy |
| `docs/nota-01-trzy-drogi-do-agenta.md` | wcześniejsze warianty podejścia do agenta | HISTORY |
| `docs/nota-02-uprzaz-dla-agentow.md` | wcześniejsza koncepcja uprzęży/warstwy zarządzania | HISTORY |
| `docs/nota-02a-aneks-profile-i-zakres-v1.md` | aneks o profilach i zakresie pierwszej wersji | HISTORY/REFERENCE |
| `docs/nota-03-topologia-27-agentow.md` | wcześniejsza propozycja topologii agentów | HISTORY; nie zastępuje D-010 |
| `docs/nota-04-korekty-i-nowe-materialy.md` | korekty wcześniejszych materiałów | HISTORY |
| `docs/nota-05-kupic-czy-zbudowac.md` | analiza kupić czy budować | HISTORY; decyzje czytać z `decisions.md` |
| `docs/nota-06-appto-research.md` | research dostawcy appto.ai | HISTORY/RESEARCH |
| `docs/nota-07-katalog-funkcji.md` | katalog funkcji appto i mapowanie na nAgents | HISTORY/RESEARCH |
| `docs/nota-08-wybory-otwarte.md` | rozwinięte tło pytań właścicielskich | HISTORY; porównywać z pakietem pytań |
| `docs/nota-09-interfejs-hermesa.md` | ustalenia o interfejsie Hermesa | LOCAL-ONLY/REFERENCE |
| `docs/nota-10-entra-instrukcja-dla-administratora.md` | instrukcja Entra/Microsoft dla administratora | LOCAL-ONLY/TECHNICAL; brak testu realną rejestracją |
| `docs/proces-dla-pracownikow.md` | nietechniczny opis zasady AutoBot dla pracowników | CANONICAL dla odbiorcy nietechnicznego |
| `docs/process/dispatch/NAG-DEC-001-wybory-otwarte.md` | dispatch analizy pytań otwartych | EVIDENCE |
| `docs/process/dispatch/NAG-INFO-001-appto-research.md` | dispatch research appto | EVIDENCE |
| `docs/process/dispatch/NAG-INFO-002-katalog-funkcji.md` | dispatch katalogu funkcji | EVIDENCE |
| `docs/process/dispatch/NAG-INFRA-002-pomocnik-serwerowy.md` | dispatch pomocnika serwerowego | LOCAL-ONLY/EVIDENCE; nie ma go na GitHubie audytowanej gałęzi |
| `docs/process/dispatch/NAG-PROC-004-skill-samowystarczalny.md` | dispatch skillu samowystarczalnego | EVIDENCE |
| `docs/process/dispatch/NAG-PROC-005-skill-uniwersalny.md` | dispatch skillu uniwersalnego | EVIDENCE |
| `docs/process/dispatch/NAG-PROC-006-ulotka-dla-pracownikow.md` | dispatch ulotki pracowniczej | EVIDENCE |
| `docs/process/dispatch/SZABLON.md` | szablon nowego dispatchu | CANONICAL PROCESS |
| `docs/process/echo.md` | literalne odpowiedzi/zgody właściciela | CANONICAL DECISIONS |
| `docs/process/handoff.md` | format i zasady tworzenia handoffu | CANONICAL PROCESS; nie jest bieżącym snapshotem |
| `docs/process/pamiec.md` | historia rozumowania, korekt i lekcji | HISTORY; nie używać jako routingu |
| `docs/process/pytania/2026-08-22-kontrola.md` | raport Final Control wcześniejszego zestawu pytań | EVIDENCE/HISTORY |
| `docs/process/pytania/2026-08-22-warianty-c.md` | uzupełnienie wariantów C | HISTORY |
| `docs/process/pytania/2026-08-22-zestaw-1.md` | duży historyczny pakiet pytań | HISTORY; nie zadawać ponownie bez porównania decyzji |
| `docs/process/pytania/2026-08-25-wybory.md` | aktualniejszy pakiet pytań właścicielskich | CANONICAL OPEN-QUESTIONS do czasu rozstrzygnięcia |
| `docs/process/tematy.md` | rejestr aktywnych, zablokowanych i zamkniętych tematów | CANONICAL REGISTRY; lokalnie zmieniony |
| `docs/process/zmiana-procesu.md` | bezpieczna zmiana samego procesu | CANONICAL PROCESS |
| `docs/process/zrodla/appto-cennik-pl.md` | pierwotny cennik appto | SOURCE/read-only |
| `docs/process/zrodla/appto-integracje-pl.md` | pierwotny opis integracji appto | SOURCE/read-only |
| `docs/process/zrodla/appto-polityka-prywatnosci-pl.md` | pierwotna polityka prywatności | SOURCE/read-only |
| `docs/process/zrodla/appto-regulamin-pl.md` | regulamin i umowa powierzenia | SOURCE/read-only |
| `docs/process/zrodla/appto-strona-glowna-pl.md` | pierwotna strona główna | SOURCE/read-only |
| `docs/process/zrodla/appto-wdrozenie-kohortowe-pl.md` | opis wdrożenia kohortowego | SOURCE/read-only |
| `docs/process/zrodla/autobots-szkielet-uniwersalny.md` | zewnętrzny/archiwalny szkielet procesu AutoBot | SOURCE/HISTORY; nie jest lokalnym routingiem |
| `docs/spec/00-architektura.md` | architektura, model danych i bezpieczeństwo | CANONICAL SPEC; lokalnie zmieniony |
| `docs/spec/01-mvp1.md` | zakres i odbiór MVP1 | CANONICAL SPEC |
| `docs/spec/02-mvp2.md` | zakres i odbiór MVP2 | CANONICAL SPEC |
| `docs/spec/03-mvp3.md` | zakres i odbiór MVP3 | CANONICAL SPEC; D-011 może blokować |
| `docs/spec/04-mvp4.md` | kierunek skali/retencji | CANONICAL SPEC/FUTURE |
| `docs/spec/README.md` | lokalny indeks specyfikacji | CANONICAL SPEC INDEX; dubluje część `CLAUDE.md` |
| `docs/spec/decisions.md` | dziennik decyzji architektonicznych D-001… | CANONICAL DECISIONS; lokalnie zmieniony |
| `docs/spec/scenarios.md` | scenariusze acceptance/testów | CANONICAL TEST CONTRACT; lokalnie zmieniony |

---

## 6. Katalog dokumentów OVH poza repozytorium

Katalog `/home/ubuntu/handoffs/` ma 24 Markdowny. Są to materiały pomocnicze,
historyczne lub migracyjne; nie wszystkie są aktualnym routingiem.

| Plik | Przeznaczenie | Ocena aktualności |
|---|---|---|
| `AUTOBOOT-INDEKS.md` | starszy indeks warstwy Autoboot/nAgents | STALE częściowo; odsyła do nowszego pakietu, ale zawiera stare ścieżki i statusy |
| `DOKUMENTACJA-MVP1.md` | wcześniejsza dokumentacja funkcjonalna MVP1 | HISTORY; porównać z `docs/spec/01-mvp1.md` |
| `HANDOFF-centrum-projektow.md` | historyczny handoff centrum projektów | HISTORY |
| `HANDOFF-kolejne-kroki.md` | historyczny handoff następnych kroków | HISTORY |
| `HANDOFF-serwer.md` | ustalenia o serwerze i środowisku | MIGRATION/HISTORY; live stan sprawdzać komendami |
| `HANDOFF-zespol-agentow.md` | historyczny opis zespołu agentów | HISTORY |
| `INTEGRACJA-MICROSOFT365.md` | projekt integracji Microsoft 365, zakresy i testy | CURRENT DESIGN / NOT PROVEN; brak realnego testu NASTER |
| `KONTYNUACJA-PO-MIGRACJI.md` | przekazanie po migracji z dawnymi ścieżkami/runtime | PRIVATE/STALE; nie publikować, nie kopiować wartości wrażliwych |
| `LLM-OPEN-SOURCE-nAgents-HANDOFF.md` | research własnego LLM/open source | RESEARCH/OPEN QUESTIONS |
| `MIGRACJA-OVH-STATUS.md` | status migracji OVH | HISTORY; live stan z usług i repozytoriów |
| `NAGENTS-INDEKS.md` | starszy indeks nAgents z pakietem 10.09 | STALE/REPLACED przez ten plik; zachowany jako historia |
| `PLAN-WDROZENIA-nAgents.md` | plan z 27.08 | HISTORY; część założeń została zmieniona |
| `PYTANIA-DO-ODPOWIEDZI-28-08.md` | historyczny pakiet pytań | HISTORY; nie zadawać ponownie bez rejestru decyzji |
| `PYTANIA-I-ODPOWIEDZI-nAgents.md` | historyczny rejestr odpowiedzi | HISTORY; porównywać z `echo.md`/`decisions.md` |
| `RAPORT-nAgents-scenariusz-i-plan.md` | raport struktury i planu MVP z 27.08 | HISTORY/PROPOSAL; nie dowodzi wdrożenia |
| `RAPORTY-Z-HETZNERA-README.md` | indeks raportów starego serwera | HISTORY; ścieżki Hetznera nie są lokalnym runtime |
| `ROLE-I-UPRAWNIENIA-nAgents.md` | macierz ról i zgód | PROPOSAL/OPEN; propozycja nie jest implementacją |
| `SCHEMAT-PODZIALU-AUTOBOT.md` | schemat dzielenia pracy AutoBot | REFERENCE; kontrakt boardu ma pierwszeństwo |
| `SERWERY-nAgents-ustalenia.md` | historyczne rozeznanie VPS/OVH | STALE dla parametrów zakupu; zachować jako historię |
| `SPECYFIKACJA-nAgents.md` | duża specyfikacja historyczna | STALE/HISTORY; sama oznacza się jako historyczna |
| `THE-GAME-INTEGRATION-HANDOFF.md` | handoff osobnego projektu The-Game | SEPARATE PROJECT; nie używać jako nAgents routing |
| `UZUPELNIENIE-MIGRACJI.md` | uzupełnienie migracji | MIGRATION HISTORY |
| `WLASNY-LLM-nAgents-porownanie.md` | porównanie własnego LLM | RESEARCH/OPEN DECISION |
| `ZALACZNIK-B-serwer-OVH.md` | załącznik do dawnego wyboru serwera | HISTORY; nie traktować jako aktualnego planu |

### Pakiet `handoffs/nagents-2026-09-10/`

Pakiet ma sześć dokumentów i plik stanu technicznego. Jest przydatnym
snapshotem audytu z 10.09, ale nie zastępuje obecnego checkoutu:

| Plik | Rola |
|---|---|
| `00-NAGENTS-DOKUMENTACJA-GLOWNA.md` | dokument główny i granice projektu; sam mówi, że nie dowodzi wdrożenia |
| `01-STAN-I-KOREKTY.md` | korekty starych twierdzeń i rozdzielenie potwierdzone/raportowane |
| `02-PLAN-ETAPOW.md` | szczegółowy plan etapów po audycie |
| `03-ROLE-I-ADMINISTRACJA.md` | robocza macierz ról i akceptacji |
| `04-MICROSOFT365.md` | techniczny projekt Graph/Entra i testy; testy nie są PASS |
| `05-PYTANIA-I-DECYZJE.md` | uzupełniający rejestr pytań; sugestie nie są decyzjami |

### `/home/ubuntu/autobot-real24-20260911/` — osobna koordynacja The-Game

Ten katalog ma 13 Markdownów. Nie jest źródłem prawdy nAgents, ale nowy agent
na OVH może go napotkać przy audycie procesów. Czytaj tylko przy zadaniu
The-Game/telemetria:

| Plik / wzorzec | Rola | Status |
|---|---|---|
| `README.md` | opis koordynacji AutoBot real24 | SEPARATE PROJECT |
| `AUTOBOT-MONITOR-HANDOFF.md` | handoff panelu telemetrii subagentów | HISTORY/TELEMETRY |
| `PLAN-R-RUST-PORT-24-REAL-Q1.md` | plan pakietów migracji Rust | SEPARATE PROJECT PLAN |
| `USAGE-REPORT.md` | opis i ograniczenia raportu usage | TELEMETRY REFERENCE |
| `autobot-state-report.md` | snapshot raportu subagentów | SNAPSHOT; wygenerować ponownie przed użyciem |
| `R-RUSTREAL-01-SCAFFOLD-Q1*.prompt.md` | prompty faz starego tematu Rust scaffold | EVIDENCE/PROMPT HISTORY |
| `R-RUSTREAL-02-STATE-DTO-Q1*.prompt.md` | prompt tematu State DTO | EVIDENCE/PROMPT HISTORY |
| `R-RUSTREAL-03-RNG-Q1.current.prompt.md` | prompt tematu RNG | EVIDENCE/PROMPT HISTORY |

Te pliki nie mogą sterować nAgentsowym boardem ani zastępować aktualnego
Kanbana The-Game.

---

## 7. AutoBot Monitor — dokumenty główne na OVH

Repozytorium: `/home/ubuntu/projects/Autoboot-Monitor`. Kontrakt projektu i
runy są oddzielone od nAgents, ale nAgents musi wiedzieć, gdzie ich szukać.

| Plik | Funkcja | Status |
|---|---|---|
| `AGENTS.md` | zasady repozytorium AutoBot Monitor | PROJECT RULES; sprawdzić przed zmianą |
| `AUTOBOT-KANBAN.md` | jedyny kanoniczny kontrakt AutoBot Monitor | CANONICAL; zawsze przed Kanbanem |
| `FINAL-REPORT.md` | historyczny raport V1 | HISTORY; nie dowodzi obecnego runtime |
| `FINAL-REPORT-V2.md` | historyczny raport V2 | HISTORY; testy historyczne, nie dzisiejszy odbiór |
| `PLAN.md` | najstarszy plan budowy | HISTORY |
| `PLAN-V2.md` | plan V2 | HISTORY |
| `PLAN-AUTOBOT-PLUGIN.md` | plan pluginu Desktop | HISTORY/REFERENCE |
| `V2-REQUIREMENTS.md` | wymagania pakietu V2/adapters | HISTORY/REFERENCE |
| `V2-LEDGER.md` | ledger dispatchu V2 | EVIDENCE/HISTORY |
| `docs/ABM-CRON-HELPER-OPERATING-RUNBOOK.md` | runbook profilu, Crona, receivera i handoffu | OPERATIONAL; statusowe fragmenty mogą się zestarzeć |
| `docs/CRON-DIRECTIVE-LOOP.md` | protokół read-only dyrektywy Crona | OPERATIONAL; porównać z live `cron list` |
| `docs/ABM-SAME-PROFILE-CRON-MIGRATION.md` | decyzja i dowody migracji profilu | MIGRATION HISTORY; nie traktować starego `OWNER_HOLD` jako live bez readbacku |
| `docs/AUTOBOT-MODEL-EFFORT-FAST-POLICY.md` | polityka model/provider/effort/Fast | ROUTING POLICY; receipt konkretnego runu ma pierwszeństwo |
| `docs/ABM-MODEL-REPAIR-PLAN.md` | plan naprawy routingu modeli | ACTIVE/HISTORY zależnie od karty; nie jest samym dowodem naprawy |
| `docs/INSTALL.md` | instalacja pakietu/pluginu | PROCEDURE |
| `docs/UNINSTALL.md` | odwrócenie instalacji | PROCEDURE |
| `docs/UPGRADE-BACKUP-AND-GITHUB-POLICY.md` | backup, aktualizacja i GitHub | OPERATIONS/POLICY |
| `docs/V2-PACKAGE.md` | granica pakietu V2 | PACKAGE REFERENCE |
| `docs/handoffs/ABM-SAME-PROFILE-OWNER-HANDOFF.md` | handoff migracji owner profile | MIGRATION HANDOFF |
| `hermes-patches/2026-09-12/README.md` | lokalne łatki Hermesa | LOCAL PATCH HISTORY; aktualizacje mogą je nadpisać |
| `package/README.md` | opis paczki pluginu | PACKAGE DOC |
| `runs/README.md` | znaczenie katalogu runów | EVIDENCE INDEX |

### Runy i evidence — jak czytać bez przepełniania kontekstu

W `runs/` występują dokumenty dispatchu i raporty faz. W audytowanym checkoutcie
są tematy m.in.:

```text
ABM-A-001, ABM-ARCH-001, ABM-ASTRA-001, ABM-ASTRA-002,
ABM-B-001, ABM-BRIDGE-001, ABM-C-001, ABM-D-001,
ABM-MODEL-001, ABM-MONITOR-001, ABM-MONITOR-002,
ABM-MONITOR-VISIBILITY-001, ABM-PLAN-001, ABM-PLUGIN-001,
ABM-PROFILE-001, ABM-V2-A, ABM-V2-B, ABM-V2-C, ABM-V2-D,
ABM-V2-E, ABM-V2-H-001, ABM-V2-P0-EVAL-R3, ABM-V2-P0-R2,
ABM-V2-P0-R3, ABM-V21-GPU-001, ABM-WEB-001.
```

Dla dowolnego tematu czytaj tylko:

```text
runs/<TOPIC>/00-dispatch*.md       — zakres, GOAL, allowlista, zakazy
runs/<TOPIC>/01-*.md               — Operator
runs/<TOPIC>/02-*.md               — Evaluator
runs/<TOPIC>/03-*.md               — Defense lub Final Control, zależnie od nazwy
runs/<TOPIC>/04-*.md               — integracja, jeśli istnieje
runs/<TOPIC>/*-audit* / *-verify*  — dowód techniczny konkretnego runu
```

Katalogi `*-tmp`, fixture, staged packages i powtarzane raporty z prób są
artefaktami testów, nie dokumentacją startową. Nie kasować ich podczas aktywnej
pracy; nie ładować zbiorczo.

### Aktywny worktree relayu

```text
/home/ubuntu/projects/Autoboot-Monitor/.worktrees/t_771608cd
```

Najważniejsze Markdowny w tym worktree:

```text
docs/OWNER-CHAT-RELAY.md
docs/CRON-DIRECTIVE-LOOP.md
docs/INSTALL.md
docs/UNINSTALL.md
runs/ABM-OWNER-CHAT-RELAY-001/00-dispatch.md
runs/ABM-OWNER-CHAT-RELAY-001/01-operator.md
runs/ABM-OWNER-CHAT-RELAY-001/02-evaluator.md
runs/ABM-HELPER-A-001/01-operator.md
runs/ABM-HELPER-A-001/02-evaluator.md
runs/ABM-HELPER-A-001/03-repair-operator.md
runs/ABM-HELPER-A-001/03-repair-r2.md
runs/ABM-HELPER-A-001/04-evaluator-r2.md
runs/ABM-INTEGRATE-001/02-evaluator.md
runs/ABM-INTEGRATE-001/03-defense.md
runs/ABM-OWNER-CHAT-001/01-operator.md
runs/ABM-READY-WAVE-001/01-operator.md
```

Nie czytaj i nie zapisuj tych plików równolegle z aktywnym workerem bez
kontrolowanego readbacku. Karta `t_822096ba` ma aktywną Defense r2 i wykryty
hotspot `backend/monitor/owner_relay.py`.

---

## 8. GitHub — co jest opublikowane, a czego nie ma

### nAgents

```text
GitHub main:                         9522836d — tylko README.md
GitHub claude/git-connection-9sz6dg: 55b2d735 — 46 Markdownów
OVH checkout:                        f5a9101 — 49 Markdownów, 3 lokalnie dodatkowe
```

Nie traktuj lokalnych zmian `CLAUDE.md`, `HANDOFF-nagents.md`, `echo.md`,
`tematy.md`, `00-architektura.md`, `docs/spec/README.md`, `decisions.md`,
`scenarios.md` ani dispatchu `NAG-INFRA-002` jako opublikowanych na GitHubie.

### AutoBot Monitor

```text
GitHub main: 6b057bad — 56 Markdownów
OVH checkout: c44c381 — 198 Markdownów w root checkoutcie
```

Na GitHubie main brakuje znacznej części obecnej lokalnej dokumentacji
operacyjnej, dispatchów i runów. Lokalny plik nie jest dowodem publikacji.
Branch, commit i remote SHA muszą być sprawdzone przed raportem „wypchnięte”.

### Zasada bezpiecznej publikacji

1. Najpierw terminalny stan aktywnych operatorów.
2. Potem readback pliku i `git diff --check`.
3. Skan sekretów i danych osobowych.
4. Jawna lista plików do commitowania — nigdy `git add .` ani `git add -A`.
5. Push wskazanego branchu.
6. Readback `git ls-remote` oraz zawartości z GitHuba.

Ten indeks opisuje lokalne i zdalne różnice, ale sam fakt jego zapisania w
checkoutcie nie oznacza jeszcze publikacji.

---

## 9. Co jest nieaktualne albo wymaga ostrożności

| Materiał / twierdzenie | Problem | Jak postąpić |
|---|---|---|
| stare handoffy z `/root/...` | dotyczą poprzedniej maszyny i ścieżek | na OVH używać `/home/ubuntu/...`; nie kopiować ścieżek |
| „kod nAgents nie istnieje” z pakietu 10.09 | późniejszy checkout nAgents istnieje i ma kod/commity | sprawdzać lokalny repozytorium i GitHub, nie stary snapshot |
| „trzeba kupić VPS-4” | OVH KS-7 już działa | traktować jako historię zakupu |
| stare ceny/parametry VPS | opis decyzji z 27.08, nie live infrastruktura | sprawdzać system i usługę, nie plan zakupu |
| `HANDOFF-nagents.md` „przed MVP1, kod nie istnieje” | datowany snapshot nie odpowiada obecnemu checkoutowi | użyć jako historii, potem `git status/log` |
| `docs/process/handoff.md` „dokumentacja kompletna, kod nie istnieje” | stary handoff proceduralny | nie używać do obecnego statusu |
| `docs/CRON-DIRECTIVE-LOOP.md` status `paused/inactive` | status z wcześniejszej fazy; live readback może być inny | protocol useful, status tylko z `cron list`/systemd |
| `ABM-SAME-PROFILE-CRON-MIGRATION.md` old `OWNER_HOLD` | historyczny checkpoint migracji | sprawdzać aktualny job, receiver i board |
| historyczne `FINAL-REPORT*.md` | wynik dawnego runu/offline fixtures | nie przedstawiać jako dzisiejszego PASS |
| historyczne raporty Router/Monitor | testy offline nie dowodzą realnej integracji dostawcy | wymagany kontrolowany runtime readback |
| `deliver=local` | zapisuje spool, nie dowodzi visible owner chat | wymagana dokładna sesja i receipt |
| `queued`/`delivered` | admission, nie końcowy wynik fazy | wymagana settled receipt i readback |
| `NAGENTS-INDEKS.md` i `AUTOBOOT-INDEKS.md` | dwa starsze indeksy z częściowo sprzecznymi snapshotami | ten plik jest nową mapą; stare zachować jako historię |
| `KONTYNUACJA-PO-MIGRACJI.md` | zawiera wrażliwe dane runtime i historyczne ścieżki | prywatny handoff; nie publikować ani nie przepisywać wartości |
| `ROLE-I-UPRAWNIENIA-nAgents.md` | wiele pozycji oznaczonych jako propozycja/otwarte | nie traktować propozycji jako uprawnień |
| `INTEGRACJA-MICROSOFT365.md` | dokumentacja możliwości, brak testu kontami NASTER | status `NOT PROVEN`; wymagane MS-01…MS-12 |
| aktywny worktree relayu | worker może zmieniać pliki w czasie audytu | nie scalać, nie czyścić, nie resetować |

Nie usuwam tych materiałów. Ich status jest tu jawnie oznaczony, aby nowy agent
nie pomylił historii z aktualnym źródłem prawdy.

---

## 10. Jak przejąć pracę bez przepełnienia kontekstu

### Wariant A — dokumentacja/specyfikacja nAgents

1. Przeczytaj ten indeks.
2. Przeczytaj `CLAUDE.md`.
3. Przeczytaj właściwy fragment `docs/spec/00-architektura.md`.
4. Sprawdź `docs/spec/decisions.md`, `docs/process/echo.md` i `tematy.md`.
5. Dopiero potem czytaj konkretny dispatch, pytanie albo raport.

### Wariant B — AutoBot/relay/Cron

1. Przeczytaj ten indeks §3 i §7.
2. Przeczytaj `AUTOBOT-KANBAN.md`.
3. Wykonaj świeży `stats`, `list`, `show` i readback usług.
4. Załaduj tylko właściwy runbook lub raport fazy.
5. Nie używaj starego statusu z handoffu jako decyzji o dispatchu.

### Wariant C — historia lub spór o ustalenie

1. Znajdź plik w tabeli katalogu.
2. Odczytaj decyzję właściciela i aktualny dziennik.
3. Porównaj datę, branch, commit i dowód.
4. Jeśli pozostaje sprzeczność, oznacz `UNKNOWN/DECISION_REQUIRED`; nie wygładzaj
   jej własnym domysłem.

### Szybkie wyszukiwanie wszystkich Markdownów

```bash
# nAgents — pliki śledzone
cd /home/ubuntu/projects/nAgents-readonly
git ls-files '*.md'

# AutoBot Monitor — pliki śledzone i jawnie nieśledzone
cd /home/ubuntu/projects/Autoboot-Monitor
git ls-files '*.md'
git status --short --untracked-files=all

# GitHub — bez pobierania zmian do dirty checkoutu
gh api repos/maciejsieracki/nAgents/git/trees/55b2d7355906d4092128f05e7c51c378abc59265?recursive=1
gh api repos/maciejsieracki/Autoboot-Monitor/git/trees/6b057bad349ccda4c4b8ec5c26ae03eea81b635a?recursive=1
```

Nie przechowuj wyników tych komend razem z sekretami, tokenami, auth.json,
`state.db`, transcriptami ani surowymi logami.

---

## 11. Uzupełnienia wykonane przez ten indeks

- dodano jeden punkt wejścia dla przyszłego agenta;
- rozpisano katalog nAgents, handoffów OVH, pakietu 10.09 i AutoBot Monitor;
- wskazano osobno normę, decyzję, scenariusz, rejestr, handoff, historię,
  źródło pierwotne i evidence;
- porównano lokalny checkout z aktualnie znanymi gałęziami GitHuba;
- zaznaczono lokalne Markdowny nieopublikowane na GitHubie;
- oznaczono stare indeksy, snapshoty migracyjne, historyczne testy i statusy,
  których nie wolno traktować jako bieżącego stanu;
- zachowano wszystkie istniejące pliki — ten indeks niczego nie usuwa.

### Otwarte uzupełnienia

1. Po zakończeniu aktywnej Defense wykonać drugi readback i uaktualnić wyłącznie
   sekcję statusową, nie przepisywać raportów do indeksu.
2. Po akceptacji właściciela ujednolicić odnośniki w `README.md` i `CLAUDE.md`,
   aby oba wskazywały ten plik jako pierwszy punkt wejścia.
3. Przy osobnym zleceniu opracować plan redukcji duplikatów; nie usuwać plików
   na podstawie samego podobieństwa nazw.
4. Po readbacku, skanie sekretów, `git diff --check` i jawnej liście ścieżek przygotować allowlistowany commit indeksu oraz push samego indeksu; nie dołączać przy tym lokalnych 19 commitów ani dirty zmian.

---

## 12. Granice tego dokumentu

- Nie zawiera haseł, tokenów, kluczy, prawdziwych adresów IP ani danych
  uwierzytelniających.
- Nie zawiera pełnych transcriptów ani surowych logów.
- Nie nadaje uprawnień i nie zmienia decyzji właściciela.
- Nie oznacza kodu, workera, relayu, merge, pushu ani deployu jako gotowego.
- Nie zastępuje świeżego readbacku stanu runtime.
- Nie usuwa ani nie nadpisuje historycznych dokumentów.
