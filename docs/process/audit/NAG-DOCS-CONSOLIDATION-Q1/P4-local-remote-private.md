STATUS: PASS_WITH_EXPLICIT_OWNER_GATES
DOMAIN: INFORMACYJNY
TEMAT: NAG-DOCS-CONSOLIDATION-Q1 / P4
CEL: pokazać granice local/remote/private runtime oraz nie mieszać AutoBot Monitor z The-Game.
P2 OBSERVED_AT: 2026-09-14T13:56:47+00:00 UTC
P4 READBACK_AT: 2026-09-14T14:47:54+00:00 UTC

# P4 — local / remote / private / project boundaries

## Snapshot inventory

P2 ma 833 rekordy: 695 z lokalnych checkout/worktree, 104 z nazwanych remote tree i 34 z archiwum handoffów. Nie oznacza to 833 niezależnych dokumentów: jest 254 rodzin ścieżek i 272 raw hashy.

| source_id | projekt | typ | rekordów | unikalnych ścieżek | ref | tracked states |
|---|---|---|---:|---:|---|---|
| ABM_ACTIVE_RELAY_WORKTREE | AutoBot Monitor | registered_git_worktree | 9 | 9 | refs/heads/wt/abm-cron-001-operator | tracked_modified, untracked, tracked |
| ABM_CHECKOUT | AutoBot Monitor | local_git_checkout | 161 | 161 | refs/heads/main | tracked, tracked_modified, untracked |
| GITHUB_ABM_MAIN_REF | AutoBot Monitor | github_remote_tree | 56 | 56 | refs/heads/main | tracked |
| GITHUB_NAGENTS_MAIN_REF | nAgents | github_remote_tree | 1 | 1 | refs/heads/main | tracked |
| GITHUB_NAGENTS_WORK_REF | nAgents | github_remote_tree | 47 | 47 | refs/heads/claude/git-connection-9sz6dg | tracked |
| HANDOFFS_NAGENTS | nAgents | external_handoff_archive | 34 | 34 | — | external_archive |
| NAGENTS_AUX_WORKTREES | nAgents | registered_git_worktree | 474 | 48 | refs/heads/auto/NAG-INFRA-001-interfejs-hermesa, refs/heads/auto/NAG-INFRA-002-entra-instrukcja, refs/heads/auto/NAG-MVP1-001-szkielet, refs/heads/auto/NAG-MVP1-003-uprawnienia, refs/heads/auto/NAG-MVP1-004-rejestr, refs/heads/auto/NAG-MVP1-005-rozmowa, refs/heads/auto/NAG-MVP1-006-proxy-hermes, refs/heads/auto/NAG-MVP1-007-brama-modeli, refs/heads/auto/NAG-MVP1-008-audyt, refs/heads/auto/NAG-MVP1-009-wdrozenie | tracked |
| NAGENTS_CHECKOUT | nAgents | local_git_checkout | 51 | 51 | refs/heads/claude/git-connection-9sz6dg | tracked, tracked_modified, untracked |

## Local-only

`LOCAL_ONLY` dotyczy proweniencji, nie automatycznej rangi treści. Łącznie: 132 rekordy, 118 rodzin ścieżek po uwzględnieniu kopii w aux worktrees. Rozbicie P2:

| source_id | rekordów LOCAL_ONLY | rodzin ścieżek | główne zakresy |
|---|---:|---:|---|
| NAGENTS_CHECKOUT | 4 | 4 | docs |
| NAGENTS_AUX_WORKTREES | 14 | 2 | docs |
| ABM_CHECKOUT | 105 | 105 | AUTOBOT-KANBAN.md, PLAN-AUTOBOT-PLUGIN.md, docs, hermes-patches, runs |
| ABM_ACTIVE_RELAY_WORKTREE | 9 | 9 | docs, runs |

Szczegółowa lista wszystkich ścieżek local-only jest w `P4-classification.json` w `path_family_classifications[].presence_statuses` i `record_classifications[]`. Najważniejsze skupiska:

- `NAGENTS_CHECKOUT`: 4 rekordy — `docs/nota-09-interfejs-hermesa.md`, `docs/nota-10-entra-instrukcja-dla-administratora.md`, plan tej konsolidacji i dispatch NAG-INFRA-002. To treść lokalna/nieopublikowana; noty 09/10 mają gate OWNER_DECISION_REQUIRED.
- `NAGENTS_AUX_WORKTREES`: 14 rekordów / 2 rodziny — kopie not 09/10, wyłącznie jako snapshot provenance.
- `ABM_CHECKOUT`: 105 rekordów / 105 rodzin — 92 run/dispatch evidence, 10 docs operational, AUTOBOT-KANBAN, PLAN-AUTOBOT-PLUGIN i hermes-patches README. Nie przenosić do nAgents.
- `ABM_ACTIVE_RELAY_WORKTREE`: 9 rekordów / 9 rodzin — 7 runów owner-chat-relay i 2 dokumenty relay/cron. To aktywny kandydat operacyjny, ale nie opublikowany source of truth.

Wszystkie rekordy local-only mają status/rolę w JSON; status `LOCAL_ONLY` jest secondary flagiem, gdy dokument ma rolę CANONICAL/SOURCE/EVIDENCE.

## Remote-only

Nie znaleziono rekordów `REMOTE_ONLY` (`0`). Każdy remote record ma lokalny odpowiednik w zakresie P2. Zdalna obecność nie rozstrzyga, czy treść jest bieżąca; source-of-truth pochodzi z P3 i live readbacku.

## Remote readback i rozjazdy

104 remote body zostały ponownie odczytane read-only przez GitHub API pod zapisanymi revision/ref. 92 mają identyczny raw hash z bieżącym lokalnym reprezentantem, 12 różni się treścią/line count/nagłówkami, a w 104/104 zbiór linków jest równy. Rozjazdy:

| projekt | ścieżka | local SHA | remote SHA | linie L/R | local-added headings | linki +/− |
|---|---|---|---|---:|---|---:|
| AutoBot Monitor | `AGENTS.md` | b3f05aa330fa | a8c147f5a7b5 | 127/118 | ## Versioning, backup i ciągłość GitHub | 0/0 |
| AutoBot Monitor | `package/README.md` | 3e4ea56b8c60 | d66fbd771952 | 84/40 | ## Native namespace artifact (r4) | 0/0 |
| nAgents | `CLAUDE.md` | 8de583cc4fec | 83fcb63c4e32 | 121/81 | ## Główny kierunek produktu: web-first, ### Kolejność dostarczenia, ### Podział ról użytkowników, ### Pomocnik procesu podczas nieobecności właściciela | 0/0 |
| nAgents | `HANDOFF-nagents.md` | 2e9ccd808e81 | 50395ae262e7 | 421/371 | ## 2A. Aktualna dyrektywa właściciela — web-first, ## 2B. Aktualna decyzja właściciela — serwerowy pomocnik procesu | 0/0 |
| nAgents | `NAGENTS-PROJECT.md` | bd0637953c52 | 6da275008231 | 576/568 | brak | 0/0 |
| nAgents | `docs/process/echo.md` | e631fb908120 | 9928ebc85dde | 146/120 | ### NAG-INFRA-002-pomocnik-serwerowy-Q1 — wybór trybu pomocnika, ### NAG-MVP1-008-audyt-Q1 — trwałość i dostęp do audytu | 0/0 |
| nAgents | `docs/process/tematy.md` | 9257a6cd1c08 | 56dfb5d451e5 | 63/51 | brak | 0/0 |
| nAgents | `docs/spec/00-architektura.md` | 322a6c2a76dc | 17318e9014ca | 283/233 | ## 12. Kolejność interfejsu i niezależność od klienta, ### 12.1 Web jest podstawową powierzchnią NAgents, ### 12.2 Serwer jest właścicielem pracy, ### 12.3 Desktop jest drugim etapem, ### 12.4 Serwerowy pomocnik procesu | 0/0 |
| nAgents | `docs/spec/01-mvp1.md` | e2dfb80d1980 | cc2687413e02 | 214/214 | brak | 0/0 |
| nAgents | `docs/spec/README.md` | 2f5719a2a41e | d075b08f5f14 | 54/38 | ## Priorytet dostarczenia interfejsu — web-first | 0/0 |
| nAgents | `docs/spec/decisions.md` | 57264a14bcf3 | ee793c30d8a2 | 264/175 | ## D-012 · Web-first i serwerowa własność pracy, ## D-013 · Serwerowy pomocnik procesu dla autonomicznej pętli | 0/0 |
| nAgents | `docs/spec/scenarios.md` | 8b8be2a4e631 | 7084dc99b657 | 92/71 | ## Interfejs pracownika i administracja, ## Serwerowy pomocnik procesu | 0/0 |

Znaczenie różnic: remote named ref jest snapshotem, a obecny checkout może mieć nieopublikowane zmiany. Nie wybieramy automatycznie local ani remote jako publikacji. 11 różnic istniało już w P2 (9 nAgents + 2 ABM); bieżący readback wykazał dodatkowy drift `NAGENTS-PROJECT.md`, więc obecnie widzimy 12.

## Current local drift po P2

Bieżący odczyt lokalny wykazał 3 pliki, których SHA-256 nie jest już hashem zapisanym w P2. To sygnał konkurencyjnej/prywatnej pracy, nie zgoda na nadpisanie:

| źródło | ścieżka | P2 SHA | current SHA | P2/current bytes | P2/current lines | current H/L |
|---|---|---|---|---:|---:|---:|
| ABM_CHECKOUT | `AUTOBOT-KANBAN.md` | 87c426598b7d | 64b99a0fbf76 | 25883/26296 | 592/601 | 31/0 |
| ABM_CHECKOUT | `docs/ABM-CARD-TAGGING-GUIDE.md` | 2bfddbac9e47 | c109d5f6015c | 9306/15586 | 171/568 | 31/4 |
| NAGENTS_CHECKOUT | `NAGENTS-PROJECT.md` | 6da275008231 | bd0637953c52 | 32249/33293 | 568/576 | 39/7 |

P4 nie nadpisuje żadnego z tych plików. Przed P5 trzeba wykonać świeży readback, rozstrzygnąć, czy zmiany są autoryzowane, i ponownie policzyć hash/heading/link.

## Private runtime i sekrety

P1 wyklucza z konsolidacji wszystkie runtime/state/auth/vault/.env/token/credential/log material. P4 nie otwierał tych plików i nie kopiuje ich wartości. W klasyfikacji są tylko bezpieczne etykiety:

| zakres | status | reguła |
|---|---|---|
| Hermes state/auth/vault, .env, tokens, credentials, runtime DB, logs | `PRIVATE_RUNTIME` | poza repo/P4; w przyszłości wyłącznie `vault_ref` i autoryzowany readback |
| The-Game repository i jego pętla | `SEPARATE_PROJECT` | nie jest rekordem P2; nie mieszać z nAgents/ABM |
| ABM auxiliary worktrees poza allowlistą P2 | `LOCAL_ONLY` | metadata-only; nie traktować jako źródła treści |
| P4 output artifacts | `SOURCE` | audit artifacts, nie instrukcje runtime |

Nie ma żadnej wartości sekretu ani prawdziwych danych osobowych w tych artefaktach.

## Granica projektów

- nAgents jest warstwą zarządzania i ma własny source-of-truth w CLAUDE/spec/skills.
- AutoBot Monitor jest osobnym komponentem/projektem z kontraktem `AUTOBOT-KANBAN.md`, run evidence i własnym live readbackiem.
- The-Game ma osobną pętlę Operator → Evaluator → Obrona → Final Control i nie wchodzi do P4.
- Wspólne słowa „Autobot”, „Kanban”, „relay” nie są podstawą do konsolidacji; granica projektu ma pierwszeństwo przed podobieństwem tekstu.

## Decyzja operacyjna

P4 kończy się bez delete/move/merge/push/deploy. P5 powinien użyć JSON jako allowlisty, ponownie potwierdzić 3 drift files i 12 hash-variant families, a dopiero potem przekazać wybrane sekcje do P6. W razie braku dowodu zatrzymać właściwy strumień, nie rozszerzać zakresu.
