STATUS: PASS_WITH_EXPLICIT_OWNER_GATES
DOMAIN: INFORMACYJNY
TEMAT: NAG-DOCS-CONSOLIDATION-Q1 / P4
CEL: rozdzielić identyczne snapshoty, logiczne nakładki, treść historyczną i materiały wymagające decyzji; nie wykonywać delete/move/merge.
P2 OBSERVED_AT: 2026-09-14T13:56:47+00:00 UTC
P4 READBACK_AT: 2026-09-14T14:47:54+00:00 UTC

# P4 — duplikaty i stale

## Zakres i wynik

P4 przeanalizował każdy z 833 rekordów P2. Jednostką statusu jest rekord JSONL, a jednostką decyzji o konsolidacji jest rodzina `project + relative_path`. Pełne przypisanie rekordów oraz hashy jest w `P4-classification.json`.

- 833 rekordy: nAgents 607, AutoBot Monitor 226.
- 254 rodziny ścieżek: nAgents 85, AutoBot Monitor 169.
- 272 unikalne SHA-256.
- 105 rodzin identycznego raw content; 561 nadmiarowych rekordów po pozostawieniu jednego reprezentanta każdej rodziny hash.
- 0 rodzin, w których identyczny hash występuje pod różnymi ścieżkami — nie ma podstaw do łączenia nazw plików na podstawie samego hasha.
- 12 rodzin ścieżek miało więcej niż jeden hash w P2: 9 nAgents i 3 AutoBot Monitor.
- 132 rekordy są `LOCAL_ONLY`; 0 jest `REMOTE_ONLY`.
- P2 nie oznaczył żadnego rekordu jako stary wyłącznie z powodu wieku. `STALE` poniżej jest decyzją cyklu życia (zastąpiony snapshot), nie wnioskiem z mtime.

## Metoda porównania

1. Hash: równość raw SHA-256 z P2; nie normalizowano Markdown przed decyzją o duplikacie.
2. Nagłówki: porównano first/top-level headings z P2 oraz pełny digest nagłówków przy readbacku bajtów.
3. Linki: znormalizowano cele linków, porównano count/digest/class; dla 104 rekordów remote wykonano read-only readback przez GitHub API pod nazwanym refem. Nie wykonano fetch/pull/push.
4. Treść: różnica hash/line count/heading set tworzy divergent snapshot; podobieństwo językowe jest wyłącznie sygnałem do ręcznego przeglądu.
5. Cykl życia: P3 source-of-truth ma pierwszeństwo przed datą pliku. Run/dispatch/report pozostaje evidence; archiwalny handoff pozostaje history; bieżący skill/spec/contract pozostaje canonical/source.
6. Bezpieczeństwo: P4 nie czytał ani nie kopiował sekretów, vault/state/auth/.env/logów runtime; nie zmieniał źródeł.

## Exact duplicates — 105 rodzin / 561 nadmiarowych rekordów

Identyczny hash i ta sama ścieżka oznaczają kopię snapshotu między checkoutem, worktree, remote albo handoffem. Status `DUPLICATE` w JSON oznacza kopię preferowanego snapshotu tej ścieżki; różne warianty tej samej ścieżki mają status `HISTORY`, nawet gdy ich starszy wariant ma własne kopie.

Przykłady największych rodzin (pełna lista i record_id w JSON):

| rodzina ścieżki | SHA prefix | rekordów | nadmiar po 1 reprezentancie | źródła |
|---|---:|---:|---:|---|
| nAgents::README.md | `c02ad5db2abf` | 13 | 12 | GITHUB_NAGENTS_MAIN_REF, GITHUB_NAGENTS_WORK_REF, NAGENTS_AUX_WORKTREES, NAGENTS_CHECKOUT |
| nAgents::.claude/skills/nagents-autobot/README.md | `4215172be410` | 12 | 11 | GITHUB_NAGENTS_WORK_REF, NAGENTS_AUX_WORKTREES, NAGENTS_CHECKOUT |
| nAgents::.claude/skills/nagents-autobot/SKILL.md | `aa5aab364e0a` | 12 | 11 | GITHUB_NAGENTS_WORK_REF, NAGENTS_AUX_WORKTREES, NAGENTS_CHECKOUT |
| nAgents::docs/nota-01-trzy-drogi-do-agenta.md | `0927f3dc6168` | 12 | 11 | GITHUB_NAGENTS_WORK_REF, NAGENTS_AUX_WORKTREES, NAGENTS_CHECKOUT |
| nAgents::docs/nota-02-uprzaz-dla-agentow.md | `9af3e69b558f` | 12 | 11 | GITHUB_NAGENTS_WORK_REF, NAGENTS_AUX_WORKTREES, NAGENTS_CHECKOUT |
| nAgents::docs/nota-02a-aneks-profile-i-zakres-v1.md | `62d4e8311ec5` | 12 | 11 | GITHUB_NAGENTS_WORK_REF, NAGENTS_AUX_WORKTREES, NAGENTS_CHECKOUT |
| nAgents::docs/nota-03-topologia-27-agentow.md | `d640062145f8` | 12 | 11 | GITHUB_NAGENTS_WORK_REF, NAGENTS_AUX_WORKTREES, NAGENTS_CHECKOUT |
| nAgents::docs/nota-04-korekty-i-nowe-materialy.md | `c64e83b0f41b` | 12 | 11 | GITHUB_NAGENTS_WORK_REF, NAGENTS_AUX_WORKTREES, NAGENTS_CHECKOUT |
| nAgents::docs/nota-05-kupic-czy-zbudowac.md | `a9745f9eab8b` | 12 | 11 | GITHUB_NAGENTS_WORK_REF, NAGENTS_AUX_WORKTREES, NAGENTS_CHECKOUT |
| nAgents::docs/nota-06-appto-research.md | `19eab8b23a2a` | 12 | 11 | GITHUB_NAGENTS_WORK_REF, NAGENTS_AUX_WORKTREES, NAGENTS_CHECKOUT |
| nAgents::docs/nota-07-katalog-funkcji.md | `8925a223f6e2` | 12 | 11 | GITHUB_NAGENTS_WORK_REF, NAGENTS_AUX_WORKTREES, NAGENTS_CHECKOUT |
| nAgents::docs/nota-08-wybory-otwarte.md | `b05ce062506e` | 12 | 11 | GITHUB_NAGENTS_WORK_REF, NAGENTS_AUX_WORKTREES, NAGENTS_CHECKOUT |
| nAgents::docs/proces-dla-pracownikow.md | `09f3a374b861` | 12 | 11 | GITHUB_NAGENTS_WORK_REF, NAGENTS_AUX_WORKTREES, NAGENTS_CHECKOUT |
| nAgents::docs/process/dispatch/NAG-DEC-001-wybory-otwarte.md | `e752b2c4ccc1` | 12 | 11 | GITHUB_NAGENTS_WORK_REF, NAGENTS_AUX_WORKTREES, NAGENTS_CHECKOUT |
| nAgents::docs/process/dispatch/NAG-INFO-001-appto-research.md | `1b3f845dfaa0` | 12 | 11 | GITHUB_NAGENTS_WORK_REF, NAGENTS_AUX_WORKTREES, NAGENTS_CHECKOUT |

Wniosek: nie ma bezpiecznej operacji „merge all”. Kopie są dowodem proweniencji i mogą być potrzebne do audytu; P4 tylko je oznacza.

## Różne hashe tej samej ścieżki

Tabela pokazuje hash prefix/count/line count oraz liczbę wariantów nagłówków i linków. `?` oznacza wariant, którego body pod P2 hashem nie jest już dostępne w readbacku; JSON zachowuje null zamiast zgadywania.

| projekt | ścieżka | warianty P2 (hash, rekordy, linie, H, L) | różne digesty H | różne digesty L |
|---|---|---|---:|---:|
| AutoBot Monitor | `AGENTS.md` | a8c147f5a7b5 (1 rekordów, 118 linii, H=14, L=0); b3f05aa330fa (1 rekordów, 127 linii, H=15, L=0) | 2 | 1 |
| AutoBot Monitor | `docs/CRON-DIRECTIVE-LOOP.md` | b64ed49d5d60 (1 rekordów, 151 linii, H=8, L=0); cbe44f709567 (1 rekordów, 180 linii, H=8, L=0) | 2 | 1 |
| AutoBot Monitor | `package/README.md` | 3e4ea56b8c60 (1 rekordów, 84 linii, H=4, L=0); d66fbd771952 (1 rekordów, 40 linii, H=3, L=0) | 2 | 1 |
| nAgents | `CLAUDE.md` | 83fcb63c4e32 (11 rekordów, 81 linii, H=7, L=10); 8de583cc4fec (1 rekordów, 121 linii, H=11, L=10) | 2 | 1 |
| nAgents | `HANDOFF-nagents.md` | 2e9ccd808e81 (1 rekordów, 421 linii, H=13, L=1); 50395ae262e7 (11 rekordów, 371 linii, H=11, L=1) | 2 | 1 |
| nAgents | `docs/process/echo.md` | 24c4e6bb92ed (2 rekordów, 133 linii, H=7, L=1); 9928ebc85dde (9 rekordów, 120 linii, H=6, L=1); e631fb908120 (1 rekordów, 146 linii, H=8, L=1) | 3 | 1 |
| nAgents | `docs/process/tematy.md` | 34e675e6a834 (1 rekordów, 59 linii, H=5, L=1); 3b4f72fe7ecf (1 rekordów, 62 linii, H=5, L=1); 56dfb5d451e5 (6 rekordów, 51 linii, H=5, L=1); 9257a6cd1c08 (1 rekordów, 63 linii, H=5, L=1); ac99b0371347 (1 rekordów, 61 linii, H=5, L=1); beac7256c93e (1 rekordów, 56 linii, H=5, L=1); c26e7a441689 (1 rekordów, 60 linii, H=5, L=1) | 1 | 1 |
| nAgents | `docs/spec/00-architektura.md` | 17318e9014ca (11 rekordów, 233 linii, H=19, L=4); 322a6c2a76dc (1 rekordów, 283 linii, H=24, L=4) | 2 | 1 |
| nAgents | `docs/spec/01-mvp1.md` | cc2687413e02 (10 rekordów, 214 linii, H=14, L=0); e2dfb80d1980 (2 rekordów, 214 linii, H=14, L=0) | 1 | 1 |
| nAgents | `docs/spec/README.md` | 2f5719a2a41e (1 rekordów, 54 linii, H=6, L=7); d075b08f5f14 (11 rekordów, 38 linii, H=5, L=7) | 2 | 1 |
| nAgents | `docs/spec/decisions.md` | 57264a14bcf3 (1 rekordów, 264 linii, H=14, L=0); ee793c30d8a2 (11 rekordów, 175 linii, H=12, L=0) | 2 | 1 |
| nAgents | `docs/spec/scenarios.md` | 7084dc99b657 (11 rekordów, 71 linii, H=7, L=0); 8b8be2a4e631 (1 rekordów, 92 linii, H=9, L=0) | 2 | 1 |

W tych 12 rodzinach odrębność jest materialna: różne sekcje/statusy/line counts wymagają macierzy pokrycia. Link digest pozostaje taki sam w zrekonstruowanych wariantach; to nie dowodzi równości całej treści.

## Remote kontra bieżący lokalny readback

P2 zarejestrował 104 rekordy remote. Hash readback potwierdzono read-only dla wszystkich 104. Wobec bieżącego lokalnego checkoutu 92 są identyczne, a 12 różne. Wszystkie 104 porównania miały równy zbiór linków (`link_set_unequal = 0`); różnice dotyczą treści/line count/nagłówków. P2-owy status remote może mówić „identical to local snapshot”, ale nie oznacza to identyczności z późniejszym dirty checkoutem.

| projekt | ścieżka | local vs remote SHA prefix | local lines +/− remote | local-added headings | linki +/− |
|---|---|---|---:|---|---:|
| AutoBot Monitor | `AGENTS.md` | b3f05aa330fa vs a8c147f5a7b5 | +9/-0 | ## Versioning, backup i ciągłość GitHub | +0/-0 |
| AutoBot Monitor | `package/README.md` | 3e4ea56b8c60 vs d66fbd771952 | +44/-0 | ## Native namespace artifact (r4) | +0/-0 |
| nAgents | `CLAUDE.md` | 8de583cc4fec vs 83fcb63c4e32 | +40/-0 | ## Główny kierunek produktu: web-first, ### Kolejność dostarczenia, ### Podział ról użytkowników, ### Pomocnik procesu podczas nieobecności właściciela | +0/-0 |
| nAgents | `HANDOFF-nagents.md` | 2e9ccd808e81 vs 50395ae262e7 | +50/-0 | ## 2A. Aktualna dyrektywa właściciela — web-first, ## 2B. Aktualna decyzja właściciela — serwerowy pomocnik procesu | +0/-0 |
| nAgents | `NAGENTS-PROJECT.md` | bd0637953c52 vs 6da275008231 | +8/-0 | brak | +0/-0 |
| nAgents | `docs/process/echo.md` | e631fb908120 vs 9928ebc85dde | +26/-0 | ### NAG-INFRA-002-pomocnik-serwerowy-Q1 — wybór trybu pomocnika, ### NAG-MVP1-008-audyt-Q1 — trwałość i dostęp do audytu | +0/-0 |
| nAgents | `docs/process/tematy.md` | 9257a6cd1c08 vs 56dfb5d451e5 | +13/-1 | brak | +0/-0 |
| nAgents | `docs/spec/00-architektura.md` | 322a6c2a76dc vs 17318e9014ca | +50/-0 | ## 12. Kolejność interfejsu i niezależność od klienta, ### 12.1 Web jest podstawową powierzchnią NAgents, ### 12.2 Serwer jest właścicielem pracy, ### 12.3 Desktop jest drugim etapem, ### 12.4 Serwerowy pomocnik procesu | +0/-0 |
| nAgents | `docs/spec/01-mvp1.md` | e2dfb80d1980 vs cc2687413e02 | +2/-2 | brak | +0/-0 |
| nAgents | `docs/spec/README.md` | 2f5719a2a41e vs d075b08f5f14 | +16/-0 | ## Priorytet dostarczenia interfejsu — web-first | +0/-0 |
| nAgents | `docs/spec/decisions.md` | 57264a14bcf3 vs ee793c30d8a2 | +89/-0 | ## D-012 · Web-first i serwerowa własność pracy, ## D-013 · Serwerowy pomocnik procesu dla autonomicznej pętli | +0/-0 |
| nAgents | `docs/spec/scenarios.md` | 8b8be2a4e631 vs 7084dc99b657 | +21/-0 | ## Interfejs pracownika i administracja, ## Serwerowy pomocnik procesu | +0/-0 |

Pozostałe 92 porównania są byte-identical względem wskazanego bieżącego lokalnego reprezentanta. Żaden P4 wynik nie publikuje ani nie wybiera remote jako zwycięzcy.

## Readback drift po P2

Bieżący odczyt lokalny wykazał 3 pliki, których SHA-256 nie jest już hashem zapisanym w P2. To sygnał konkurencyjnej/prywatnej pracy, nie zgoda na nadpisanie:

| źródło | ścieżka | P2 SHA prefix | current SHA prefix | P2/current bytes | P2/current lines | current H/L |
|---|---|---|---|---:|---:|---:|
| ABM_CHECKOUT | `AUTOBOT-KANBAN.md` | 87c426598b7d | 64b99a0fbf76 | 25883/26296 | 592/601 | 31/0 |
| ABM_CHECKOUT | `docs/ABM-CARD-TAGGING-GUIDE.md` | 2bfddbac9e47 | c109d5f6015c | 9306/15586 | 171/568 | 31/4 |
| NAGENTS_CHECKOUT | `NAGENTS-PROJECT.md` | 6da275008231 | bd0637953c52 | 32249/33293 | 568/576 | 39/7 |

P4 nie nadpisuje żadnego z tych plików. Przed P5 trzeba wykonać świeży readback, rozstrzygnąć, czy zmiany są autoryzowane, i ponownie policzyć hash/heading/link.

## Cykl życia: stale/history kontra fresh mtime

| **NAG-LEGACY-SPEC / NAG-HISTORY** | `STALE/HISTORY` | Datowane specyfikacje, pytania, indeksy i handoffy przenoszą wcześniejszy stan; ich mtime może być świeży po skopiowaniu, ale cykl życia jest historyczny. | Nie używać jako normy; wyciągać tylko unikalne sekcje po macierzy P5/P6. |
| **NAG-RESEARCH / NAG-SOURCES** | `HISTORY/SOURCE` | Notatki i capture’y są dowodem źródłowym, nie routingiem; zachowują datę, hash i link. | Pozostawić jako evidence/source i cytować w pakietach docelowych. |
| **ABM-EVIDENCE / ABM-REPORTS** | `EVIDENCE` | Runy, dispatches i raporty są dowodem etapów; podobne szablony nie oznaczają tego samego przebiegu. | Nie deduplikować po tytule ani podobieństwie; identyczność tylko po SHA-256 tej samej ścieżki. |
| **ABM-HISTORY / ABM-HANDOFF** | `HISTORY` | Plany i handoffy opisują poprzednie przejścia; nie są aktualnym kontraktem. | Zachować jako historię audytową; kontrakt sprawdzać w AUTOBOT-KANBAN i live readback. |

P2 freshness counts are recorded in JSON (`FRESH`/`RECENT` only). Nie używamy ich do automatycznej promocji dokumentu. Każdy status `STALE`/`HISTORY` ma uzasadnienie treściowe i target pakietu w JSON.

## Nakładki logiczne

| relacja | status | zakres | charakter | decyzja |
|---|---|---|---|---|
| LD-NAG-ENTRY-INDEX | CONSOLIDATION_CANDIDATE | NAG-ENTRY, NAG-INDEX, NAG-HANDOFF | semantic overlap in entry/current-state routing, not exact duplicate | P5 chooses one lightweight index; P4 keeps every source. |
| LD-NAG-SPEC-LEGACY | CONSOLIDATION_CANDIDATE | NAG-SPEC, NAG-LEGACY-SPEC, NAG-RBAC | logical duplicate/coverage overlap between current spec and legacy plans | P6 builds section-level coverage before any extraction; no deletion in P4. |
| LD-NAG-DECISIONS | CONSOLIDATION_CANDIDATE | NAG-DECISIONS, NAG-HISTORY | question/answer snapshots overlap decisions and ECHO | Reconcile question IDs/statuses before copying; do not ask or decide again from an archive. |
| LD-NAG-PROCESS | CONSOLIDATION_CANDIDATE | NAG-PROCESS, NAG-USER, NAG-EVIDENCE | layered process docs and dispatch reports share workflow terms | Keep the layers and map sections into NAGENTS-PROCESS.md/NAGENTS-USER-GUIDE.md in P6. |
| LD-NAG-RESEARCH | CONSOLIDATION_CANDIDATE | NAG-APPT0, NAG-RESEARCH, NAG-SOURCES | research synthesis overlaps captured primary sources | Preserve source hashes/links and cite only dated, verified claims in NAGENTS-RESEARCH.md. |
| LD-NAG-INTEGRATIONS | OWNER_DECISION_REQUIRED | NAG-INTEGRATIONS, NAG-RESEARCH | Hermes/Entra/Microsoft 365 integration documents overlap but do not select a mechanism | No consolidation or access broadening until owner decision plus live readback/test. |
| LD-ABM-CONTRACT | CONSOLIDATION_CANDIDATE | ABM-CONTRACT, ABM-OPS, ABM-RELAY | contract/runbooks/relay documents share routing and lifecycle terms | Use a precedence table in AUTOBOT-PROJECT.md; do not let a static runbook override live readback. |
| LD-ABM-PACKAGE | CONSOLIDATION_CANDIDATE | ABM-PACKAGE, ABM-LIFECYCLE, ABM-REPORTS | package README, lifecycle docs and reports describe overlapping deliverables | Release review reconciles hashes and manifest; no publication or install in P4. |
| LD-ABM-RUN-TEMPLATES | EVIDENCE | ABM-EVIDENCE | semantic template repetition across distinct run IDs, not duplicates | Never collapse across run IDs; deduplicate only exact same SHA-256 copies of the same path. |

Najważniejsze false positive: runy `ABM-V2-H-001` mają podobny szablon (sygnał cosine do 0.935), ale różne run ID/role/parents/stage; pozostają osobnym evidence. Podobnie nota/source, spec/plan i handoff/decision nie są merge tylko dlatego, że mają wspólne nagłówki.

## Brak operacji destrukcyjnych

- Nie usunięto, nie przeniesiono i nie połączono żadnego dokumentu.
- P4 nie zmienił CLAUDE.md, docs/spec, skill, AutoBot Monitor ani The-Game; odnotował jedynie bieżący drift readbacku.
- Nie wykonano `git add`, commit, push, instalacji ani deployu.
- P5 dostaje klasyfikację rekordową; P6 dopiero po decyzjach właściciela może sporządzić macierz sekcja→źródło.

Następny bezpieczny krok: zweryfikować 12 rodzin hash-variant oraz 3 drift files w świeżym readbacku, następnie przejść do P5 allowlist bez automatycznego merge.
