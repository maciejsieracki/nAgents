# 8gent — NAGENTS-PROJECT

> **Kanoniczny indeks nawigacyjny dokumentacji projektu.** Ten plik mówi
> agentowi, gdzie szukać informacji i jak odróżniać źródło prawdy od historii,
> propozycji, raportu oraz bieżącego stanu runtime. Jest mapą, a nie zamiennikiem
> źródeł normatywnych ani świeżego readbacku.
>
> **Audyt źródłowy:** P1 `2026-09-14T13:39:16Z`, P2 `13:56:47Z`, P3
> `14:05:37Z`, P4 `14:47:54Z`; wynik P5 i jego końcowy hash są w raporcie
> [`P5-index.md`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P5-index.md).
> **Zakres audytu:** checkout 8gent, checkout AutoBot Monitor, aktywne
> worktree, `/home/ubuntu/handoffs/`, znane pliki koordynacji oraz nazwane
> drzewa GitHuba dla `maciejsieracki/nAgents` i
> `maciejsieracki/Autoboot-Monitor`.
>
> **Ważne:** indeks jest mapą. Nie jest kopią wszystkich treści i nie zastępuje
> świeżego readbacku Git, Kanbana ani usług serwera.

---

## 0. Zacznij tutaj — minimalny kontekst

Nowy agent może przeczytać **tylko ten plik na start**, ale traktuje go jako
mapę. Nie nadaje mu rangi decyzji ani bieżącego stanu. Kanoniczna kolejność
lektury projektu z `CLAUDE.md` pozostaje następująca; po niej agent wybiera
jedną ścieżkę z tabeli w §2. Nie ładuje wszystkich raportów, handoffów i historii
naraz.

```text
0. NAGENTS-PROJECT.md                         — lekka mapa; nie norma nadrzędna
1. CLAUDE.md                                  — punkt startowy i twarde zasady
2. docs/process/tematy.md                     — bieżący rejestr tematów
3. docs/process/handoff.md                    — bieżący format handoffu
4. docs/spec/README.md                         — skrót projektu i etapów
5. docs/spec/decisions.md                      — decyzje, także otwarte i blokujące
6. docs/spec/scenarios.md                      — kontrakt scenariuszy/testów
7. docs/spec/01-mvp1.md                        — specyfikacja bieżącego etapu
8. właściwy dispatch, raport i artefakt         — dopiero dla konkretnego tematu
9. live readback                               — zawsze przed twierdzeniem o stanie
```

`01-mvp1.md` jest bieżącą specyfikacją, ponieważ projekt jest przed MVP1; po
zmianie etapu należy wybrać właściwy plik `0X-mvpX.md`, a nie czytać wszystkie
etapy.

### Czym jest projekt

```text
8gent
├── aplikacja i warstwa zarządzania użytkownikiem, rolami, kosztami i audytem
├── dokumentacja architektury, decyzji i scenariuszy
├── AutoBot jako proces pracy i kontrola faz
├── AutoBot Monitor jako osobne repozytorium wykonawcze/obserwacyjne
└── Hermes jako zewnętrzny silnik agentów i runtime
```

Zasada marki i identyfikatorów technicznych jest opisana w
[`docs/N8GENT-BRAND-NAMING.md`](docs/N8GENT-BRAND-NAMING.md). W nowych tekstach
używaj marki `8gent`; literalnych nazw repozytorium, plików, ścieżek i locatorów
nie zmieniaj.

`The-Game` jest osobnym projektem, boardem i profilem. Może być opisany w
indeksie jako zależność lub przykład działania AutoBot, ale jego kodu,
branchy i dokumentacji nie wolno mieszać z 8gent.

## 0A. Pakiet dokumentacji po konsolidacji tematycznej

Poniższe pliki są zatwierdzonymi pakietami tematycznymi po cyklu
Operator → Evaluator → Defense (tylko gdy były zarzuty) → Final Control →
lokalny P7. Kopie zachowują nagłówki i proweniencję pakietów stagingowych;
nie są samodzielnym źródłem bieżącego runtime.

| Pakiet | Gdzie szukać | Co odpowiada | Źródło, które nadal ma pierwszeństwo |
|---|---|---|---|
| SPEC | `NAGENTS-SPEC.md` | tożsamość, architektura, zakres MVP, bezpieczeństwo, scenariusze | `docs/spec/` i `CLAUDE.md` |
| DECISIONS | `NAGENTS-DECISIONS.md` | decyzje D-001…D-013, ECHO, pytania i historia | `docs/spec/decisions.md`, `docs/process/echo.md` |
| PROCESS | `NAGENTS-PROCESS.md` | role, pętla, allowlista, evidence, watchdog, recovery, P1–P7 | `.claude/skills/nagents-autobot/SKILL.md`, `CLAUDE.md` |
| HANDOFF | `NAGENTS-HANDOFF.md` | format przekazania, blokady, następna bramka, live-readback boundary | `docs/process/handoff.md` + świeży odczyt |
| RESEARCH | `NAGENTS-RESEARCH.md` | research, źródła, porównania i korekty | `docs/process/pamiec.md` i źródła z datą/hash/linkiem |
| USER GUIDE | `NAGENTS-USER-GUIDE.md` | instrukcja pracownika i granica pracownik–administrator | `docs/proces-dla-pracownikow.md` oraz norma techniczna |
| AUTOBOT PROJECT | `AUTOBOT-PROJECT.md` | kontrakt AutoBot Monitor, routing, statusy i granice ownera | `Autoboot-Monitor/AUTOBOT-KANBAN.md` + live readback |
| ABM HISTORY | `docs/ABM-HISTORY.md` | historyczny indeks kart, runów, eventów i artefaktów ABM | Kanban i świeży event/run readback |
| ABM LIFECYCLE | `docs/ABM-LIFECYCLE.md` | manual installation/uninstall/rollback/backup i granice live | aktualny runbook ABM + jawna zgoda ownera |

### Instrukcja wejściowa dla nowego agenta

1. Przeczytaj ten plik, potem `CLAUDE.md`, rejestr tematów, format handoffu,
   specyfikację, decyzje i scenariusze — w tej kolejności.
2. Z pytania wybierz **jeden** pakiet z tabeli powyżej; nie ładuj wszystkich
   raportów ani całego katalogu `docs/`.
3. Przeczytaj odpowiedni plik pakietu oraz wskazane źródło pierwszeństwa.
4. Szukaj po identyfikatorze sekcji, nie po podobieństwie nazwy. Używaj
   `SPEC-*`, `DEC-*`, `PROCESS-*`, `HANDOFF-*`, `RESEARCH-*`, `USER-*`,
   `ABM-*` albo `LIFE-*`; coverage i raport podają exact locator.
5. Gdy pytanie dotyczy stanu „teraz”, wykonaj świeży odczyt Git/Kanbana/usługi.
   Pakiet, raport `PASS`, `queued`, `done` ani stary handoff nie dowodzi live state.
6. Gdy pytanie dotyczy decyzji, sprawdź `docs/process/echo.md` i
   `docs/spec/decisions.md`; rekomendacja lub research nie są decyzją.
7. Gdy pytanie dotyczy integracji Microsoft/Entra/Graph/Hermes, zatrzymaj się
   na `OWNER_HOLD`: wybór A oznacza research-only, bez integracji live.
8. Gdy znajdziesz starą lub podobną treść, sprawdź `NAGENTS-CONSOLIDATION-PLAN.md`
   i `docs/CONSOLIDATION-CLEANUP-MANIFEST.md`. Nie usuwaj pliku tylko dlatego,
   że jego temat pojawia się w nowym pakiecie.

Publikacyjny manifest paczki, listę plików wyłączonych oraz status kontroli
znajdziesz w `docs/NAGENTS-DOCUMENTATION-MANIFEST.md`. Rejestr wykonania,
hashe, raporty i decyzje o zachowaniu źródeł są w `docs/process/`.

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
| Czym jest 8gent i jaki ma zakres | [`docs/spec/00-architektura.md`](docs/spec/00-architektura.md) | `docs/spec/01-mvp1.md` … `04-mvp4.md` |
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

### 2.1. Pytanie → pakiet → sekcja

Poniższa mapa wybiera najmniejszy pakiet i sekcję wystarczające do odpowiedzi.
Nazwy `NAGENTS-*` i `AUTOBOT-PROJECT.md` oznaczają pakiety docelowe z planu,
nie zgodę na ich utworzenie ani na mechaniczne scalanie. Linki prowadzą do
istniejących źródeł; status `LIVE_READBACK_REQUIRED` zawsze wymaga odczytu
stanu, nie lektury kolejnego snapshotu.

| Pytanie | Pakiet | Sekcja / pierwszy odczyt |
|---|---|---|
| Gdzie agent zaczyna i jakie zasady są twarde? | `NAGENTS-PROJECT.md` jako indeks; `CLAUDE.md` jako norma | [`CLAUDE.md`](CLAUDE.md) §Kolejność czytania, §Proces, §Siedem barier; indeks §0–§1 |
| Co budujemy, z jakich warstw i z jakimi granicami? | `NAGENTS-SPEC.md` (kandydat P6); obecnie `docs/spec/` | [`docs/spec/00-architektura.md`](docs/spec/00-architektura.md) §1–§12, szczególnie §6 i §12 |
| Jaki jest zakres i kolejność MVP1–MVP4? | `NAGENTS-SPEC.md` (kandydat P6) | [`docs/spec/README.md`](docs/spec/README.md) §Skrót, potem wybrany etap; obecnie [`docs/spec/01-mvp1.md`](docs/spec/01-mvp1.md) §1–§12 |
| Jak działa AutoBot i jak zmieniać proces? | `NAGENTS-PROCESS.md` (kandydat P6); skill pozostaje osobno | `.claude/skills/nagents-autobot/SKILL.md` §2–§16 oraz [`docs/process/zmiana-procesu.md`](docs/process/zmiana-procesu.md) §1–§7 |
| Która faza lub karta może ruszyć teraz? | `NAGENTS-PROCESS.md` + audit evidence; stan tylko live | indeks §3–§4.5; board `autobot-monitor`: task, rodzice, run, event i receipt — `LIVE_READBACK_REQUIRED` |
| Jaki jest bieżący stan repozytorium, tematów i usług? | `NAGENTS-HANDOFF.md` (kandydat P6) + runtime readback | indeks §4 i §5; `git status/branch/HEAD`, rejestr tematów, board, profil i usługa |
| Gdzie jest bieżący handoff i jaki ma format? | `NAGENTS-HANDOFF.md` (kandydat P6) | [`docs/process/handoff.md`](docs/process/handoff.md) §Gdzie jesteśmy, §Co blokuje, §Następna bramka |
| Jakie decyzje właściciela obowiązują? | `NAGENTS-DECISIONS.md` (kandydat P6) | [`docs/spec/decisions.md`](docs/spec/decisions.md) §D-001–D-013 oraz [`docs/process/echo.md`](docs/process/echo.md) §Wpisy |
| Jakie pytania właścicielskie są otwarte? | `NAGENTS-DECISIONS.md` (kandydat P6) | `docs/process/pytania/2026-08-25-wybory.md` §Pytanie 1–8; przed wysłaniem porównaj `decisions.md` i `echo.md` |
| Jakie sytuacje muszą działać i z czego wynikają testy? | `NAGENTS-SPEC.md` (kandydat P6); scenariusze pozostają osobno | [`docs/spec/scenarios.md`](docs/spec/scenarios.md) §Dostęp i tożsamość, §Rozliczenia, §Wiedza, §Koszty, §Proaktywność, §Ciągłość, §Interfejs, §Pomocnik |
| Jakie integracje są wybrane i co dowiedziono? | `NAGENTS-INTEGRATIONS.md` (dopiero po decyzji) | `OWNER_DECISION_REQUIRED`: `docs/nota-09-*`, `docs/nota-10-*`, handoff Microsoft 365; wybór właściciela i live test przed konsolidacją |
| Co jest dowodem wykonania fazy? | `NAGENTS-PROCESS.md` + `docs/process/audit/` | exact task/run/report/artifact + terminalny event + niezależny readback; indeks §11A i artefakty P1–P5 |
| Skąd wzięły się wcześniejsze ustalenia? | `NAGENTS-RESEARCH.md` / `NAGENTS-HANDOFF.md` (kandydaci P6) | `docs/process/pamiec.md` oraz `docs/nota-*.md`; historia wyjaśnia „dlaczego”, nie steruje routingiem |
| Czy zmiana jest opublikowana i na jakim refie? | `NAGENTS-PROCESS.md` / przyszła paczka publikacyjna | indeks §8; `LIVE_READBACK_REQUIRED`: `git ls-remote` nazwanego refu i odczyt drzewa po jawnej bramce publikacji |
| Jak działa AutoBot Monitor jako projekt towarzyszący? | `AUTOBOT-PROJECT.md` (kandydat P6); kontrakt ABM pozostaje osobno | `/home/ubuntu/projects/Autoboot-Monitor/AUTOBOT-KANBAN.md`, runbooki i live board/usługa — indeks §3.1, §7 |
| Czy materiał The-Game należy do 8gent? | brak pakietu 8gent; `SEPARATE_PROJECT` | indeks §3 i §4.3; tylko zadanie jawnie przypisane do The-Game, jego własny board/runtime |

---

## 3. Routing projektu

### 3.1. 8gent → AutoBot Monitor → Hermes

```text
8gent
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
| 8gent project anchor | `nagents-docs / p_e90c30bc` | `hermes --profile default project show nagents-docs`; używać jako `project_id` na wspólnym boardzie |
| 8gent card assignee | `default` (właściciel/orkiestrator) | decyzja właściciela; nie zmieniać na `autobotmonitor` |
| 8gent card tenant | `nagents-docs` | karta/body i readback |
| 8gent card prefix | `NAG-` | stabilny topic/title |
| 8gent card contract | `process_phase`, stabilny `topic`, unikalny `idempotency_key` | create/show/readback; nie tworzyć duplikatu aktywnego P5 |
| The-Game profile | `the-game` | osobny projekt, nie fallback dla 8gent |
| The-Game board | `the-game-real24` | osobny board |
| AutoBot Cron | `LIVE_READBACK_REQUIRED`; statyczne materiały podają historyczne ID `83e4098a9f87` i `6911e5eac7d3`, nominalnie every 5m, `no_agent=true`, `deliver=local` | `hermes --profile autobotmonitor cron list --all` + status receivera |
| Worker completion | native `kanban_complete` albo `kanban_block` | karta, event, run, raport |
| Dostawa | `queued → claimed → settled` | native delivery receipt; `queued` nie oznacza ukończenia |
| Fallbacki | `default_assignee: ''`, `orchestrator_profile: ''` | profil Hermes, nie zgadywać |

### 3.3. Live readback — komendy

```bash
# 8gent: wersja lokalna i różnice; bez pull/reset/stash/clean
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

### 4.1. 8gent

| Miejsce | Stan odczytany 2026-09-14 | Znaczenie |
|---|---|---|
| OVH checkout (P1 snapshot) | HEAD `f5a91010f3c4655bd85e8a2c0c536922a937935b`, branch `claude/git-connection-9sz6dg`, 19 commitów ponad znany `origin`, 8 tracked/staged zmian i 3 untracked | lokalny stan roboczy; nie jest automatycznie gotowym wydaniem |
| GitHub `claude/git-connection-9sz6dg` | HEAD `fad6f26c48d31ea7cb5a3e4aa8ad53b5da14ab81` | drzewo ma 56 wpisów i 47 Markdownów; checkout P1 miał 51 Markdownów, w tym 4 nazwane ścieżki lokalne-only |
| GitHub `main` | HEAD `9522836d79679f5296ad23822c9a8efba9f949fb` | drzewo ma 1 wpis i 1 Markdown; nie jest bieżącym branchem pracy |
| testy lokalne | `git diff --check` przechodzi; `python3 -m pytest` nie działa, bo brak modułu `pytest` | testy aplikacji są `N/D/INFRA`, nie `PASS` |

Nazwane ścieżki checkoutu 8gent występujące lokalnie, ale nie na zdalnej
gałęzi roboczej (P2; pełna lista wszystkich lokalnych-only jest w
[`P2-inventory-summary.md`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P2-inventory-summary.md)):

```text
docs/nota-09-interfejs-hermesa.md
docs/nota-10-entra-instrukcja-dla-administratora.md
docs/process/NAGENTS-DOCS-CONSOLIDATION-PLAN.md
docs/process/dispatch/NAG-INFRA-002-pomocnik-serwerowy.md
```

P2 policzył 695 rekordów lokalnych (8gent checkout/worktree i AutoBot
Monitor), 104 rekordy z trzech nazwanych refów GitHuba oraz 34 rekordy
zewnętrznego archiwum handoffów — razem 833. W tej unii `LOCAL_ONLY` = 132,
`REMOTE_ONLY` = 0. Te liczniki są rekordami snapshotów, nie liczbą unikalnych
plików.

### 4.2. AutoBot Monitor

| Miejsce | Stan odczytany 2026-09-14 | Znaczenie |
|---|---|---|
| OVH checkout | HEAD `c44c381c2deaabccec15686494eb4c63bf54394d`, `main`, 11 tracked/staged zmian i 3266 untracked w P1 | bieżący warsztat, nie publikacja |
| GitHub `main` | HEAD `6b057bad349ccda4c4b8ec5c26ae03eea81b635a`, drzewo 228 wpisów i 56 Markdownów | starszy snapshot; nie zawiera całego obecnego lokalnego kontraktu AutoBot |
| lokalny root AutoBot Monitor | P1: 198 Markdownów przed wyłączeniem `runs/**/*-tmp`, 160 w normalnej granicy, 53 surowe logi wyłączone; P2: 161 rekordów checkoutu | część to runy/evidence i materiały lokalne; nie wszystko jest normą |
| worktree AutoBot | 37 zarejestrowanych łącznie z checkoutem; 36 pomocniczych, 35 metadata-only, 1 z treścią w zakresie P1 | głównie kopie, raporty i fixtures; nie ładować zbiorczo |
| aktywny relay | worktree `t_771608cd`, 9 rekordów P2; dirty snapshot | nie dotykać współdzielonego worktree podczas pracy |
| aktywna karta | `t_822096ba`, Defense r2 relayu, run `177` | nie dotykać współdzielonego worktree podczas pracy |

### 4.3. Usługi i karta The-Game

- Gateway `autobotmonitor` i `autobot-monitor-cron-receiver.service` były
  aktywne w ostatnim odczycie.
- R20 Save/Load w The-Game: Operator `t_310eb82d` i Evaluator `t_e32fea1c`
  zakończyli się `PASS`, bez merge/push/deploy. To osobny projekt i osobna
  bramka Final Control.
- Historyczne handoffy z `/home/ubuntu/autobot-real24-20260911/` nie są
  bieżącym źródłem prawdy dla 8gent.

### 4.4. P4 — klasyfikacja i granice konsolidacji

P4 ma status `PASS_WITH_EXPLICIT_OWNER_GATES` i nie wykonywał operacji
destrukcyjnych. Przeanalizowano każdy rekord P2, ale nie wolno utożsamiać
rekordu snapshotu z unikalnym plikiem ani statusu podobieństwa z decyzją o
scaleniu.

| Metryka P4 | Wynik |
|---|---:|
| Rekordy P2 łącznie | 833 |
| Rekordy: 8gent / AutoBot Monitor | 607 / 226 |
| Rodziny ścieżek: 8gent / AutoBot Monitor | 85 / 169 (łącznie 254) |
| Unikalne SHA-256 | 272 |
| Rodziny identycznego raw SHA-256 | 105 |
| Nadmiarowe rekordy po jednym reprezentancie rodziny hash | 561 |
| Rodziny identyczne pod różnymi ścieżkami | 0 |
| Rodziny tej samej ścieżki z różnymi hashami | 12 (9 8gent, 3 AutoBot Monitor) |
| `LOCAL_ONLY` / `REMOTE_ONLY` | 132 / 0 |
| Remote readback / zgodny z bieżącym lokalnym | 104 / 92 |
| Remote różny od bieżącego lokalnego / różne zbiory linków | 12 / 0 |
| Pliki z lokalnym driftem po P2 | 3 |

Rozkład statusów rekordów P4:

| Status | Rekordy |
|---|---:|
| `CANONICAL` | 21 |
| `SOURCE` | 22 |
| `EVIDENCE` | 145 |
| `HISTORY` | 153 |
| `DUPLICATE` | 478 |
| `CONSOLIDATION_CANDIDATE` | 12 |
| `OWNER_DECISION_REQUIRED` | 2 |
| **Razem** | **833** |

Rozkład statusów głównych rodzin ścieżek: `CANONICAL` 22, `EVIDENCE` 145,
`HISTORY` 37, `SOURCE` 27, `CONSOLIDATION_CANDIDATE` 15, `STALE` 4,
`OWNER_DECISION_REQUIRED` 4 — razem 254. `STALE` w P4 jest oceną cyklu życia
(zastąpiony snapshot), nie automatycznym wnioskiem z wieku pliku.

| Relacja logiczna | Status | Pakiet docelowy / bramka |
|---|---|---|
| `LD-NAG-ENTRY-INDEX` | `CONSOLIDATION_CANDIDATE` | jeden lekki indeks; P5 aktualizuje ten plik, źródła pozostają |
| `LD-NAG-SPEC-LEGACY` | `CONSOLIDATION_CANDIDATE` | `NAGENTS-SPEC.md` po macierzy sekcja→źródło w P6 |
| `LD-NAG-DECISIONS` | `CONSOLIDATION_CANDIDATE` | `NAGENTS-DECISIONS.md`; najpierw rekonsyliacja ID/statusów |
| `LD-NAG-PROCESS` | `CONSOLIDATION_CANDIDATE` | `NAGENTS-PROCESS.md` i `NAGENTS-USER-GUIDE.md`; warstwy osobno |
| `LD-NAG-RESEARCH` | `CONSOLIDATION_CANDIDATE` | `NAGENTS-RESEARCH.md`; zachować hash/link źródła |
| `LD-NAG-INTEGRATIONS` | `OWNER_DECISION_REQUIRED` | `NAGENTS-INTEGRATIONS.md` dopiero po decyzji i live teście |
| `LD-ABM-CONTRACT` | `CONSOLIDATION_CANDIDATE` | `AUTOBOT-PROJECT.md`; `AUTOBOT-KANBAN.md` pozostaje kontraktem |
| `LD-ABM-PACKAGE` | `CONSOLIDATION_CANDIDATE` | pakiet ABM po release review, bez publikacji w P4 |
| `LD-ABM-RUN-TEMPLATES` | `EVIDENCE` | runy pozostają osobno; nie łączyć różnych ID |

Pełne członkostwo 254 rodzin, powody, ryzyka, kontradykcje i rekomendacje są
w [`P4-consolidation-candidates.md`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-consolidation-candidates.md)
i [`P4-classification.json`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json).

### 4.5. P4 — drift i warianty tej samej ścieżki

P4 wykazał trzy pliki, których bieżący hash lokalny różnił się od P2. Pierwsze
dwa pozostają poza allowlistą P5; `NAGENTS-PROJECT.md` jest jedynym z nich,
który P5 może zmienić, więc jego nowy hash po tej fazie jest oczekiwany.

| Źródło | Ścieżka | P2 SHA-12 | Bieżący SHA-12 | P2→current bajty / linie |
|---|---|---|---|---:|
| AutoBot Monitor checkout | `AUTOBOT-KANBAN.md` | `87c426598b7d` | `64b99a0fbf76` | 25883→26296 / 592→601 |
| AutoBot Monitor checkout | `docs/ABM-CARD-TAGGING-GUIDE.md` | `2bfddbac9e47` | `c109d5f6015c` | 9306→15586 / 171→568 |
| 8gent checkout | `NAGENTS-PROJECT.md` | `6da275008231` | `bd0637953c52` (przed P5) | 32249→33293 / 568→576 |

Warianty P4 tej samej ścieżki są zachowane jako snapshoty. Zapis `hash-12 / linii
(rekordy)` nie wybiera zwycięzcy ani nie pozwala na usunięcie wariantu.

| Projekt / ścieżka | Warianty P2 (`hash-12 / linie (rekordy)`) | Bieżący lokalny readback |
|---|---|---|
| AutoBot Monitor / `AGENTS.md` | `a8c147f5a7b5 / 118 (1)`; `b3f05aa330fa / 127 (1)` | 127 linii |
| AutoBot Monitor / `docs/CRON-DIRECTIVE-LOOP.md` | `b64ed49d5d60 / 151 (1)`; `cbe44f709567 / 180 (1)` | checkout 151; relay worktree 180 |
| AutoBot Monitor / `package/README.md` | `3e4ea56b8c60 / 84 (1)`; `d66fbd771952 / 40 (1)` | 84 linii |
| 8gent / `CLAUDE.md` | `83fcb63c4e32 / 81 (11)`; `8de583cc4fec / 121 (1)` | 121 linii |
| 8gent / `HANDOFF-nagents.md` | `2e9ccd808e81 / 421 (1)`; `50395ae262e7 / 371 (11)` | 421 linii |
| 8gent / `docs/process/echo.md` | `24c4e6bb92ed / 133 (2)`; `9928ebc85dde / 120 (9)`; `e631fb908120 / 146 (1)` | 146 linii |
| 8gent / `docs/process/tematy.md` | `34e675e6a834 / 59 (1)`; `3b4f72fe7ecf / 62 (1)`; `56dfb5d451e5 / 51 (6)`; `9257a6cd1c08 / 63 (1)`; `ac99b0371347 / 61 (1)`; `beac7256c93e / 56 (1)`; `c26e7a441689 / 60 (1)` | 63 linie |
| 8gent / `docs/spec/00-architektura.md` | `17318e9014ca / 233 (11)`; `322a6c2a76dc / 283 (1)` | 283 linie |
| 8gent / `docs/spec/01-mvp1.md` | `cc2687413e02 / 214 (10)`; `e2dfb80d1980 / 214 (2)` | 214 linii |
| 8gent / `docs/spec/README.md` | `2f5719a2a41e / 54 (1)`; `d075b08f5f14 / 38 (11)` | 54 linie |
| 8gent / `docs/spec/decisions.md` | `57264a14bcf3 / 264 (1)`; `ee793c30d8a2 / 175 (11)` | 264 linie |
| 8gent / `docs/spec/scenarios.md` | `7084dc99b657 / 71 (11)`; `8b8be2a4e631 / 92 (1)` | 92 linie |

Remote readback 104 rekordów wykazał 12 różnic względem bieżącego lokalnego
reprezentanta, przy równym zbiorze linków dla wszystkich 104: AutoBot Monitor
`AGENTS.md`, `package/README.md`; 8gent `CLAUDE.md`, `HANDOFF-nagents.md`,
`NAGENTS-PROJECT.md`, `docs/process/echo.md`, `docs/process/tematy.md`,
`docs/spec/00-architektura.md`, `docs/spec/01-mvp1.md`, `docs/spec/README.md`,
`docs/spec/decisions.md`, `docs/spec/scenarios.md`. Pełne hashe i line deltas
są w [`P4-duplicates-stale.md`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-duplicates-stale.md).

---

## 5. Katalog Markdownów repozytorium 8gent — 51 plików lokalnie

Statusy w tabeli:

- **CANONICAL** — czytać dla bieżącej pracy;
- **HISTORY** — kontekst, nie routing;
- **LOCAL-ONLY** — jest na OVH, nie ma go na audytowanej gałęzi GitHuba;
- **SOURCE** — materiał pierwotny, nie zmieniać;
- **EVIDENCE** — dowód konkretnego tematu;
- **STALE** — zawiera stan lub założenia, które trzeba porównać z live readbackiem;
- **REMOTE-ONLY** — występuje w nazwanym refie zdalnym, nie w lokalnym snapshotcie;
- **DUPLICATE** — identyczny surowy SHA-256 tej samej ścieżki, kopia do zachowania;
- **CONSOLIDATION-CANDIDATE** — wymaga macierzy pokrycia, nie jest poleceniem scalania;
- **OWNER-DECISION-REQUIRED** — wymaga jednoznacznej decyzji właściciela;
- **PRIVATE-RUNTIME** — runtime/prywatny materiał poza normalnym katalogiem;
- **SEPARATE-PROJECT** — obcy projekt, nie źródło prawdy 8gent.

P4 używa w danych maszynowych nazw z podkreśleniami: `LOCAL_ONLY`,
`REMOTE_ONLY`, `CONSOLIDATION_CANDIDATE`, `OWNER_DECISION_REQUIRED`,
`PRIVATE_RUNTIME`, `SEPARATE_PROJECT`. Status rekordu, status rodziny ścieżki
i status proweniencji nie są zamienne. `LIVE_READBACK_REQUIRED` oraz
`INFRA/DECISION_REQUIRED` są statusami routingu P1/P3, nie dowodem treści.

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
| `docs/nota-07-katalog-funkcji.md` | katalog funkcji appto i mapowanie na 8gent | HISTORY/RESEARCH |
| `docs/nota-08-wybory-otwarte.md` | rozwinięte tło pytań właścicielskich | HISTORY; porównywać z pakietem pytań |
| `docs/nota-09-interfejs-hermesa.md` | ustalenia o interfejsie Hermesa | LOCAL-ONLY/REFERENCE |
| `docs/nota-10-entra-instrukcja-dla-administratora.md` | instrukcja Entra/Microsoft dla administratora | LOCAL-ONLY/TECHNICAL; brak testu realną rejestracją |
| `docs/proces-dla-pracownikow.md` | nietechniczny opis zasady AutoBot dla pracowników | CANONICAL dla odbiorcy nietechnicznego |
| `docs/process/NAGENTS-DOCS-CONSOLIDATION-PLAN.md` | playbook i graf faz P1–P7 | EVIDENCE/SOURCE; plan nie dowodzi wykonania |
| `docs/process/NAGENTS-BOARD-SEPARATION-HANDOFF.md` | instrukcja rozdzielenia boardu 8gent od AutoBot Monitor; wymagany preflight i stop conditions | OWNER HANDOFF/INFRA GATE; nie tworzy boardu ani profilu |
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
| `AUTOBOOT-INDEKS.md` | starszy indeks warstwy Autoboot/8gent | STALE częściowo; odsyła do nowszego pakietu, ale zawiera stare ścieżki i statusy |
| `DOKUMENTACJA-MVP1.md` | wcześniejsza dokumentacja funkcjonalna MVP1 | HISTORY; porównać z `docs/spec/01-mvp1.md` |
| `HANDOFF-centrum-projektow.md` | historyczny handoff centrum projektów | HISTORY |
| `HANDOFF-kolejne-kroki.md` | historyczny handoff następnych kroków | HISTORY |
| `HANDOFF-serwer.md` | ustalenia o serwerze i środowisku | MIGRATION/HISTORY; live stan sprawdzać komendami |
| `HANDOFF-zespol-agentow.md` | historyczny opis zespołu agentów | HISTORY |
| `INTEGRACJA-MICROSOFT365.md` | projekt integracji Microsoft 365, zakresy i testy | CURRENT DESIGN / NOT PROVEN; brak realnego testu NASTER |
| `KONTYNUACJA-PO-MIGRACJI.md` | przekazanie po migracji z dawnymi ścieżkami/runtime | PRIVATE/STALE; nie publikować, nie kopiować wartości wrażliwych |
| `LLM-OPEN-SOURCE-nAgents-HANDOFF.md` | research własnego LLM/open source | RESEARCH/OPEN QUESTIONS |
| `MIGRACJA-OVH-STATUS.md` | status migracji OVH | HISTORY; live stan z usług i repozytoriów |
| `NAGENTS-INDEKS.md` | starszy indeks 8gent z pakietem 10.09 | STALE/REPLACED przez ten plik; zachowany jako historia |
| `PLAN-WDROZENIA-nAgents.md` | plan z 27.08 | HISTORY; część założeń została zmieniona |
| `PYTANIA-DO-ODPOWIEDZI-28-08.md` | historyczny pakiet pytań | HISTORY; nie zadawać ponownie bez rejestru decyzji |
| `PYTANIA-I-ODPOWIEDZI-nAgents.md` | historyczny rejestr odpowiedzi | HISTORY; porównywać z `echo.md`/`decisions.md` |
| `RAPORT-nAgents-scenariusz-i-plan.md` | raport struktury i planu MVP z 27.08 | HISTORY/PROPOSAL; nie dowodzi wdrożenia |
| `RAPORTY-Z-HETZNERA-README.md` | indeks raportów starego serwera | HISTORY; ścieżki Hetznera nie są lokalnym runtime |
| `ROLE-I-UPRAWNIENIA-nAgents.md` | macierz ról i zgód | PROPOSAL/OPEN; propozycja nie jest implementacją |
| `SCHEMAT-PODZIALU-AUTOBOT.md` | schemat dzielenia pracy AutoBot | REFERENCE; kontrakt boardu ma pierwszeństwo |
| `SERWERY-nAgents-ustalenia.md` | historyczne rozeznanie VPS/OVH | STALE dla parametrów zakupu; zachować jako historię |
| `SPECYFIKACJA-nAgents.md` | duża specyfikacja historyczna | STALE/HISTORY; sama oznacza się jako historyczna |
| `THE-GAME-INTEGRATION-HANDOFF.md` | handoff osobnego projektu The-Game | SEPARATE PROJECT; nie używać jako 8gent routing |
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

Ten katalog ma 13 Markdownów. Nie jest źródłem prawdy 8gent, ale nowy agent
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
runy są oddzielone od 8gent, ale 8gent musi wiedzieć, gdzie ich szukać.

| Plik | Funkcja | Status |
|---|---|---|
| `AGENTS.md` | zasady repozytorium AutoBot Monitor | PROJECT RULES; sprawdzić przed zmianą |
| `AUTOBOT-KANBAN.md` | jedyny kanoniczny kontrakt AutoBot Monitor | CANONICAL; zawsze przed Kanbanem |
| `docs/ABM-CARD-INDEX.md` | wymagane dokumenty, klasyfikacja kart i procedura przyszłych porządków | LOCAL-ONLY/CANONICAL CARD INDEX; przed audytem kart |
| `docs/ABM-CARD-TAGGING-GUIDE-3.md` | instrukcja właściciela oznaczania i rekonsyliacji kart | OWNER SOURCE/REQUIRED; nie zastępuje kontraktu Kanbana |
| `docs/ABM-CARD-AUDIT.md` | aktualny read-only audyt 145 kart | LOCAL-ONLY/EVIDENCE; nie zmienia kart |
| `docs/ABM-CARD-AUDIT.json` | maszynowe dane audytu kart | LOCAL-ONLY/EVIDENCE; używać programowo |
| `docs/ABM-CARD-ATTACHMENT-MANIFEST.json` | manifest załączników kart potwierdzonych | LOCAL-ONLY/EVIDENCE; readback załączników |
| `docs/ABM-CARD-DOCUMENT-INDEX.md` | wcześniejszy indeks dokumentów kart | EVIDENCE/HISTORY; nie jest nowszą normą |
| `docs/ABM-CARD-ROUTING-AUDIT.md` | wcześniejszy audyt project/tenant/profile | EVIDENCE/HISTORY; stare project ID wymagają obecnego guide |
| `docs/ABM-CARD-ATTACHMENT-MATRIX.csv` | wcześniejsza macierz maszynowa załączników | EVIDENCE/MACHINE REPORT |
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

### 8gent

```text
GitHub main:                         9522836d79679f5296ad23822c9a8efba9f949fb — 1 Markdown
GitHub claude/git-connection-9sz6dg: fad6f26c48d31ea7cb5a3e4aa8ad53b5da14ab81 — 47 Markdownów
OVH checkout (P1):                  f5a91010f3c4655bd85e8a2c0c536922a937935b — 51 Markdownów, 4 nazwane lokalnie-only
```

Nie traktuj lokalnych zmian `CLAUDE.md`, `HANDOFF-nagents.md`, `echo.md`,
`tematy.md`, `00-architektura.md`, `docs/spec/README.md`, `decisions.md`,
`scenarios.md`, `NAGENTS-PROJECT.md` ani dispatchu `NAG-INFRA-002` jako
opublikowanych na GitHubie.

### AutoBot Monitor

```text
GitHub main: 6b057bad349ccda4c4b8ec5c26ae03eea81b635a — 56 Markdownów
OVH checkout: c44c381c2deaabccec15686494eb4c63bf54394d — 198 Markdownów w root checkoutcie przed wyłączeniami
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
| „kod 8gent nie istnieje” z pakietu 10.09 | późniejszy checkout 8gent istnieje i ma kod/commity | sprawdzać lokalny repozytorium i GitHub, nie stary snapshot |
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

### Wariant A — dokumentacja/specyfikacja 8gent

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
gh api repos/maciejsieracki/nAgents/git/trees/fad6f26c48d31ea7cb5a3e4aa8ad53b5da14ab81?recursive=1
gh api repos/maciejsieracki/Autoboot-Monitor/git/trees/6b057bad349ccda4c4b8ec5c26ae03eea81b635a?recursive=1
```

Nie przechowuj wyników tych komend razem z sekretami, tokenami, auth.json,
`state.db`, transcriptami ani surowymi logami.

### Reguły ograniczania kontekstu wynikające z P1–P4

1. Zacznij od indeksu, potem przejdź przez siedem plików startowych z §0. Dla
   pytania wybierz jeden wiersz z §2.1: jedno pytanie → najmniejszy pakiet →
   jedna sekcja. Nie czytaj wszystkich czterech etapów ani wszystkich raportów.
2. Najpierw użyj metadanych i agregatów P2/P4 (`P2-counts.json` oraz
   `P4-classification.json`), a dopiero potem otwórz konkretny plik. Nie ładuj
   833 rekordów ani 272 rodzin hash do kontekstu jednocześnie.
3. Dla konkretnego tematu czytaj wyłącznie jego dispatch, raport, artefakt i
   terminalny readback. W AutoBot Monitor wybieraj wskazany run, nie cały
   katalog `runs/`; podobne szablony różnych ID pozostają osobnym evidence.
4. Nie otwieraj `.git/`, zależności, cache, buildów, `state.db`, sesji,
   surowych logów, credentiali, `.env*`, kluczy ani danych osobowych. Zachowaj
   tylko bezpieczne metadane i zsanityzowany identyfikator dowodu. Wyłączenie
   jest granicą odczytu, nie zgodą na kopiowanie.
5. Kopie identycznego raw SHA-256 tej samej ścieżki czytaj przez manifest/hash,
   nie przez ponowne ładowanie treści. Nie deduplikuj różnych ścieżek ani
   wariantów o różnych hashach na podstawie nazwy lub podobieństwa.
6. Przed P5/P6 wykonaj celowany readback trzech plików z driftem i dwunastu
   rodzin wariantów wymienionych w §4.5. Nie nadpisuj dirty worktree innego
   projektu i nie odświeżaj snapshotu przez `fetch`, `pull`, reset, stash lub
   clean.
7. Raportuj destylat: ścieżka, status, hash-12, licznik, wynik i następna
   bramka. Pełne listy oraz surowe dane zostają w artefaktach audytu; `PASS`,
   `queued`, nazwa pliku i status UI nie zastępują eventu ani readbacku.

---

## 11. Uzupełnienia wykonane przez ten indeks

- dodano jeden punkt wejścia dla przyszłego agenta;
- rozpisano katalog 8gent, handoffów OVH, pakietu 10.09 i AutoBot Monitor;
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
2. P6 ma przygotować macierz pokrycia `źródło/grupa → status → pakiet → sekcja`
   przed jakimkolwiek przenoszeniem, scalaniem lub usuwaniem.
3. Po akceptacji właściciela ujednolicić odnośniki w `README.md` i `CLAUDE.md`,
   aby oba wskazywały ten plik jako pierwszy punkt wejścia.
4. Przy osobnym zleceniu opracować plan redukcji duplikatów; nie usuwać plików
   na podstawie samego podobieństwa nazw.
5. Po readbacku, skanie sekretów, `git diff --check` i jawnej liście ścieżek przygotować allowlistowany commit indeksu oraz push samego indeksu; nie dołączać przy tym lokalnych 19 commitów ani dirty zmian.

---

## 11A. Artefakty audytu P1–P5 i readback

Artefakty są dowodem konkretnej fazy, nie nowym źródłem prawdy projektu.
Zachowują proweniencję i pełne dane maszynowe, ale do kontekstu należy ładować
tylko potrzebny fragment. Linki poniżej są względne względem katalogu repozytorium
8gent.

| Faza / status | Artefakt | Rola |
|---|---|---|
| P1 / `PASS` | [`P1-scope.md`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P1-scope.md) | zakres korzeni, refów i granic |
| P1 / `PASS` | [`P1-sources.json`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P1-sources.json) | maszynowy manifest source ID, liczników i bezpieczeństwa |
| P1 / `PASS` | [`P1-exclusions.md`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P1-exclusions.md) | wyłączenia runtime, sekretów, PII, logów i obcych projektów |
| P2 / `PASS` | [`P2-inventory-local.jsonl`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P2-inventory-local.jsonl) | metadane lokalnych snapshotów |
| P2 / `PASS` | [`P2-inventory-github.jsonl`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P2-inventory-github.jsonl) | metadane trzech nazwanych refów GitHuba |
| P2 / `PASS` | [`P2-inventory-external.jsonl`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P2-inventory-external.jsonl) | 34 bezpieczne rekordy handoffów |
| P2 / `PASS` | [`P2-inventory-summary.md`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P2-inventory-summary.md) | metoda, agregaty i jawne różnice |
| P2 / `PASS` | [`P2-counts.json`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P2-counts.json) | odtwarzalne liczniki i walidacja |
| P3 / `PASS` | [`P3-sources-of-truth.md`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P3-sources-of-truth.md) | mapa 16 kategorii pytań do źródeł |
| P3 / `PASS` | [`P3-source-map.json`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P3-source-map.json) | maszynowa mapa source ID, statusów i proweniencji |
| P4 / `PASS_WITH_EXPLICIT_OWNER_GATES` | [`P4-duplicates-stale.md`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-duplicates-stale.md) | metody, duplikaty, warianty, drift i cykl życia |
| P4 / `PASS_WITH_EXPLICIT_OWNER_GATES` | [`P4-classification.json`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json) | klasyfikacja 833 rekordów i 254 rodzin |
| P4 / `PASS_WITH_EXPLICIT_OWNER_GATES` | [`P4-consolidation-candidates.md`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-consolidation-candidates.md) | grupy, relacje, pakiety docelowe i bramki |
| P4 / pomocniczy readback | [`P4-local-remote-private.md`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-local-remote-private.md) | granice local/remote/private; nie jest zgodą na publikację |
| P5 / raport tej fazy | [`P5-index.md`](docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P5-index.md) | zakres zmiany, testy, hash końcowy i następna bramka |

### Minimalny readback przed użyciem indeksu

1. Sprawdź, że wszystkie linki względne z tego pliku wskazują istniejące pliki;
   brak celu oznacza `INFRA`, nie domyślną ścieżkę.
2. Odtwórz z `P2-counts.json` wartości 695 lokalnych, 104 GitHub, 34
   zewnętrzne, 833 razem, `LOCAL_ONLY` 132 i `REMOTE_ONLY` 0. Niezgodność
   licznika wymaga ponownego odczytu artefaktu, nie ręcznej korekty tabeli.
3. Dla P4 sprawdź status fazy, 833 rekordy, 254 rodziny, 12 wariantów,
   3 drifty oraz readback remote 104/104. `P4-classification.json` i raport
   tekstowy muszą wskazywać tę samą fazę i timestamp.
4. Przed twierdzeniem o bieżącym stanie wykonaj świeży `git status`,
   `git branch --show-current`, board `stats/list/show`, readback eventu/receiptu
   i — gdy dotyczy — status profilu/usługi. Snapshot nie zastępuje live state.
5. Przed publikacją wykonaj skan sekretów/PII, `git diff --check`, sprawdzenie
   usunięć i jawnej allowlisty. Push, merge i deploy pozostają osobną bramką
   właściciela; P5 ich nie wykonuje.

### Stan P5

P5 zmienia wyłącznie `NAGENTS-PROJECT.md` oraz raport P5. Nie zmienia żadnego
źródła P1–P4, nie usuwa ani nie przenosi dokumentów, nie rozstrzyga bramek
właścicielskich i nie publikuje lokalnego checkoutu. Następna faza to P6 —
macierz konsolidacji po zachowaniu obecnych źródeł i statusów.

---

## 12. Granice tego dokumentu

- Nie zawiera haseł, tokenów, kluczy, prawdziwych adresów IP ani danych
  uwierzytelniających.
- Nie zawiera pełnych transcriptów ani surowych logów.
- Nie nadaje uprawnień i nie zmienia decyzji właściciela.
- Nie oznacza kodu, workera, relayu, merge, pushu ani deployu jako gotowego.
- Nie zastępuje świeżego readbacku stanu runtime.
- Nie usuwa ani nie nadpisuje historycznych dokumentów.
