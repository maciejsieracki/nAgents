# AUTOBOT-PROJECT.md — staging kontraktu AutoBot Monitor

STATUS: PASS
PUBLICATION_STATUS: STAGING_ONLY
DOMAIN: INFORMACYJNY
TEMAT: NAG-CONSOLIDATE-AUTOBOT-PROJECT-Q1
OBSERVED_AT_UTC: 2026-09-15T00:14:12Z
P4_STATUS: PASS_WITH_EXPLICIT_OWNER_GATES

Ten plik jest addytywną, stagingową ekstrakcją kontraktu AutoBot Monitor dla
pakietu 8gent. Nie jest publikacją, instalacją, live contractem ani zgodą na
zmianę. `AUTOBOT-KANBAN.md` pozostaje kanonicznym źródłem ABM; źródła supporting
oraz runbooki nie podnoszą rangi ponad ten kontrakt. Statusy runtime są tylko
snapshotami opisanymi w źródłach i wymagają świeżego readbacku.

## Aktualizacja platformy D-014

AutoBot Monitor nie jest bieżącym osobnym runtime'em 8gent. Ten pakiet jest
materiałem do ewentualnej adaptacji jako plugin OpenClaw; Hermes-specific
profile, Cron, receiver i komendy Kanbana są historyczne. Nie wynika z niego
zgoda na instalację pluginu, zmianę Gatewaya ani operację live.
## 0. Zakres, precedencja i granice

- Zakres obejmuje sześć sekcji planu `ABM-01`–`ABM-06`; `ABM-07` pozostaje
  przyszłą bramką live-readback/owner escalation i nie jest udawany jako
  zamknięty w tym artefakcie.
- Staging dotyczy dokumentacji kontraktu ABM, nie implementacji. Nie zmienia
  repozytorium `/home/ubuntu/projects/Autoboot-Monitor`, kart, runów, profili,
  boardów, Crona, receivera, gatewaya ani źródeł 8gent.
- Precedencja: (1) jawna decyzja właściciela i świeży readback, (2)
  `AUTOBOT-KANBAN.md`, (3) `AGENTS.md` i supporting runbooki, (4) ten staging
  pakiet jako mapa/ekstrakcja. P4 jest rejestrem proweniencji, nie instrukcją
  runtime.
- Techniczny projekt Hermes `nAgents Documentation` (marka: `8gent`) jest
  miejscem, w którym powstał ten staging task; AutoBot Monitor jest osobnym
  projektem. The-Game pozostaje osobnym projektem
  i boardem (`the-game-real24`, `the-game-bugs`). Nie tworzymy wspólnej grafy,
  fallbacku ani wspólnego namespace'u.
- Żadne sekcje nie kopiują sekretów, tokenów, haseł, PII, `.env`, `auth.json`,
  `state.db`, sesji, surowych logów ani niesanitizowanych snapshotów.

## 1. Tożsamość ABM i słownik pól

Dokumentowana przez źródła tożsamość projektu (do ponownego sprawdzenia przed
każdą operacją) jest następująca:

| Pole | Wartość dokumentowana | Znaczenie / ograniczenie |
|---|---|---|
| board | `autobot-monitor` | trwała kolejka projektu; wybieraj jawnie |
| project | `autoboot-monitor` / `p_ffb5c6ad` | identyfikator projektu Hermesa, nie profil ani tenant |
| profile/assignee | `autobotmonitor` | profil wykonawczy; nie dowód przynależności karty |
| tenant | `autobot-monitor`, gdy karta faktycznie go używa | miękki filtr, nie granica bezpieczeństwa |
| process phase | `operator`, `evaluator`, `defense`, `final-control` | logiczna faza, jawna metadana karty/receiptu |
| owner target | exact visible owner session w `autobotmonitor` | canonical Bot Chat jest osobnym endpointem i nie jest automatycznie visible owner chat |

To nie jest bieżący readback środowiska. Obecne wartości projektu, profilu,
boardu, sesji, service i kart trzeba sprawdzić ponownie w chwili działania.

## 2. Rejestr źródeł i proweniencji

Każdy wiersz został odczytany read-only z dokładnej ścieżki i ma bieżący SHA-256.
`checkout_status` opisuje stan istniejący przed utworzeniem tego katalogu; dirty
lub untracked źródło nie jest przez ten temat naprawiane ani publikowane.
Pełna maszyna P4 znajduje się w `coverage.json`.

| Source ID | Ścieżka względem repo | Bieżący SHA-256 | bajty / linie | checkout_status | P4 status / relacja |
|---|---|---|---:|---|---|
| SRC-ABM-KANBAN | `AUTOBOT-KANBAN.md` | `64b99a0fbf762238926c4f79adc4c7c60d00e19259df8b64c8c22ad5c9b6dadd` | 26296 / 601 | `M AUTOBOT-KANBAN.md` | PF-0002 / CANONICAL; drift vs P2 87c426598b7d |
| SRC-ABM-AGENTS | `AGENTS.md` | `b3f05aa330fad98023aeae6ad537fe35e39579e031683638e78242d8e4ed99ae` | 6592 / 127 | `clean` | PF-0001 / CANONICAL |
| SRC-ABM-CARD-INDEX | `docs/ABM-CARD-INDEX.md` | `44114a8a7cce0fd6b7c539a850231d20a5ba37f5585dde562118ef3cf996b232` | 7816 / 203 | `?? docs/ABM-CARD-INDEX.md` | NOT_INDEXED_IN_P4: current -3-era index was read after the P4 snapshot; no P4 row is invented. |
| SRC-ABM-TAGGING-3 | `docs/ABM-CARD-TAGGING-GUIDE-3.md` | `c109d5f6015c9614f173e081b2af0d740716f500efebeab155672fab1380bb5e` | 15586 / 568 | `?? docs/ABM-CARD-TAGGING-GUIDE-3.md` | PF-0011 / SOURCE / ABM-OPS; drift vs P2 2bfddbac9e47 |
| SRC-ABM-CRON-RUNBOOK | `docs/ABM-CRON-HELPER-OPERATING-RUNBOOK.md` | `5252f9150e94261e50535e8b6f7f985082fef534f4573258cfe2b4632598c98d` | 21733 / 557 | `?? docs/ABM-CRON-HELPER-OPERATING-RUNBOOK.md` | PF-0012 / SOURCE |
| SRC-ABM-MODEL-POLICY | `docs/AUTOBOT-MODEL-EFFORT-FAST-POLICY.md` | `86d0e4ea372620a179ab2d347186e3ecb89986d0c316d8059fb48f7aba813e18` | 3755 / 96 | `?? docs/AUTOBOT-MODEL-EFFORT-FAST-POLICY.md` | PF-0016 / SOURCE |
| SRC-ABM-SAME-PROFILE | `docs/ABM-SAME-PROFILE-CRON-MIGRATION.md` | `27b8f8e63dcdf3aab1eac8bfd85bd5ee7a88c5c236a23fdd1dfd7d3359c884d6` | 24924 / 499 | `?? docs/ABM-SAME-PROFILE-CRON-MIGRATION.md` | PF-0015 / SOURCE |
| SRC-NAG-PLAN | `NAGENTS-CONSOLIDATION-PLAN.md` | `b282d9e49bf77994a290fbfab71803217d02c9565ab8809e6fbf33501ca4b15c` | 130539 / 665 | `?? NAGENTS-CONSOLIDATION-PLAN.md` | NOT_A_P4_PATH_FAMILY: this is the 8gent consolidation plan that defines the target package and ABM section matrix; P4 JSON is cited separately as the audit artifact. |
| SRC-P4 | `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json` | `95d2a06fc80e4cf1c27d59959d84b8631e70acbb3e0b879b83df95c38bf382b8` | 2655394 / 54556 | `?? docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json` | P4_AUDIT_ARTIFACT: this JSON is the classification input itself, not a product-contract path family. |

Dwa źródła są jawnie obecne w bieżącym checkoutcie, ale nie mają dokładnego
wiersza w P4: `docs/ABM-CARD-INDEX.md` oraz
`docs/ABM-CARD-TAGGING-GUIDE-3.md`. Nie dopisuję im zmyślonej klasyfikacji.
`coverage.json` wskazuje `NOT_INDEXED_IN_P4`, a dla przewodnika zachowuje
historyczny P4 alias `PF-0011` (`docs/ABM-CARD-TAGGING-GUIDE.md`) bez mieszania
jego hashy z bieżącym plikiem `-3`.

## 3. Kontrakt sekcji ABM

Poniższe sekcje zachowują identyfikatory i bramki z `NAGENTS-CONSOLIDATION-PLAN.md`.
Każda ma źródło, status i exact locator; streszczenie nie udaje dowodu live.

## ABM-01 — Projekt, namespace, kontrakt i granica względem 8gent/The-Game

Status: `CANONICAL_SOURCE + STAGING_ONLY; live identity requires readback`
P4 groups: `ABM-CONTRACT`

Źródła, statusy i locatory:

| Source ID | Ścieżka | P4 status | Exact locator |
|---|---|---|---|
| `SRC-ABM-KANBAN` | `AUTOBOT-KANBAN.md` | PF-0002 / CANONICAL; drift vs P2 87c426598b7d | §1–2, lines 44–90: source of truth, project/profile/board namespace and fallback prohibition; §3–4, lines 92–177: native_status/process_phase and dispatch preflight; §5, lines 179–338: phase graph, Cron → receiver → worker and owner target; §6–7, lines 340–490: receipts, readbacks, terminal events, watchdog and recovery; §10, lines 536–558: owner-chat bridge and selective delivery |
| `SRC-ABM-AGENTS` | `AGENTS.md` | PF-0001 / CANONICAL | §Goal, lines 3–5: durable read-only monitor objective; §Architecture constraints, lines 30–39: server execution, independent roles and dispatch contract; §Safety, lines 50–61: read-only/security/no-live-install boundary; §Acceptance criteria D–E, lines 87–96: package and verification gates |
| `SRC-ABM-CARD-INDEX` | `docs/ABM-CARD-INDEX.md` | NOT_INDEXED_IN_P4: current -3-era index was read after the P4 snapshot; no P4 row is invented. | lines 1–22: precedence, map-not-copy rule and readback boundary; §1–2, lines 26–66: ABM scope, project identity and required documents; §4–5, lines 137–184: no-touch classes, safe reconciliation and readback; §6, lines 195–202: source-of-truth and live-status rule |
| `SRC-ABM-TAGGING-3` | `docs/ABM-CARD-TAGGING-GUIDE-3.md` | PF-0011 / SOURCE | lines 1–12: precedence over attachments and current operational scope; §0–2, lines 10–94: owner scope, hierarchy and project separation; §4, lines 125–264: board/project/tenant/assignee/phase/topic/idempotency/model fields; §6–9, lines 307–455: phase cards, UI limits and historical reconciliation; §11–14, lines 495–568: dispatch checklist, prohibitions and source list |
| `SRC-NAG-PLAN` | `NAGENTS-CONSOLIDATION-PLAN.md` | NOT_A_P4_PATH_FAMILY: this is the 8gent consolidation plan that defines the target package and ABM section matrix; P4 JSON is cited separately as the audit artifact. | §4.8, lines 169–179: target package and ABM-01..07 matrix; §5, lines 201–234: P4 group ledger and ABM group counts; §6.2–6.4, lines 247–277: variants, drift and owner gates; §7–8, lines 279–327: completeness and safe additive execution |

Zachowane reguły:

- AUTOBOT-KANBAN.md pozostaje jedynym kanonicznym kontraktem ABM; ten plik jest tylko stagingową ekstrakcją.
- Dokumentowana tożsamość ABM to board `autobot-monitor`, projekt `autoboot-monitor`/`p_ffb5c6ad` i profil `autobotmonitor`; przed użyciem operacyjnym trzeba wykonać świeży readback.
- `project_id`, board, tenant i assignee/profile są odrębnymi polami. Nazwa profilu nie jest nazwą projektu ani zgodą do jego danych.
- Techniczny projekt `nAgents Documentation` (marka: `8gent`, w tym ten staging
  task) i The-Game pozostają osobnymi projektami/strumieniami; nie wolno tworzyć
  cross-board zależności ani używać profilu The-Game jako fallbacku.
- Dla jednego użytkownika/projektu główny chat, Kanban, Cron, receiver/helper i worker sessions mają wspólny profil `autobotmonitor`; wspólny profil nie scala rozmów workerów.

Granica dowodu: ten staging package nie potwierdza `live board/profile/project registration`, `current owner decision beyond the cited static snapshot`, `any mutation or publication`. Każdy taki fakt wymaga świeżego, task-scoped readbacku; statyczny dokument, `queued`, `delivered`, UI lub raport nie zastępuje dowodu.

## ABM-02 — native_status, process_phase, identity i preflight

Status: `CANONICAL/SOURCE; LIVE_READBACK_REQUIRED for every runtime claim`
P4 groups: `ABM-CONTRACT`, `ABM-OPS`

Źródła, statusy i locatory:

| Source ID | Ścieżka | P4 status | Exact locator |
|---|---|---|---|
| `SRC-ABM-KANBAN` | `AUTOBOT-KANBAN.md` | PF-0002 / CANONICAL; drift vs P2 87c426598b7d | §1–2, lines 44–90: source of truth, project/profile/board namespace and fallback prohibition; §3–4, lines 92–177: native_status/process_phase and dispatch preflight; §5, lines 179–338: phase graph, Cron → receiver → worker and owner target; §6–7, lines 340–490: receipts, readbacks, terminal events, watchdog and recovery; §10, lines 536–558: owner-chat bridge and selective delivery |
| `SRC-ABM-CARD-INDEX` | `docs/ABM-CARD-INDEX.md` | NOT_INDEXED_IN_P4: current -3-era index was read after the P4 snapshot; no P4 row is invented. | lines 1–22: precedence, map-not-copy rule and readback boundary; §1–2, lines 26–66: ABM scope, project identity and required documents; §4–5, lines 137–184: no-touch classes, safe reconciliation and readback; §6, lines 195–202: source-of-truth and live-status rule |
| `SRC-ABM-TAGGING-3` | `docs/ABM-CARD-TAGGING-GUIDE-3.md` | PF-0011 / SOURCE | lines 1–12: precedence over attachments and current operational scope; §0–2, lines 10–94: owner scope, hierarchy and project separation; §4, lines 125–264: board/project/tenant/assignee/phase/topic/idempotency/model fields; §6–9, lines 307–455: phase cards, UI limits and historical reconciliation; §11–14, lines 495–568: dispatch checklist, prohibitions and source list |
| `SRC-ABM-CRON-RUNBOOK` | `docs/ABM-CRON-HELPER-OPERATING-RUNBOOK.md` | PF-0012 / SOURCE | §1, lines 12–48: one profile and visible/canonical/worker chat distinction; §5, lines 211–320: Cron → spool → receiver/helper → worker contract; §6, lines 324–377: owner-chat delivery boundary and Desktop/server distinction; §7–8, lines 381–471: handoff, technical/contextual readback and canary; §9, lines 475–491: prohibited shortcuts; §11, lines 543–557: next step remains gated by visible-owner canary |
| `SRC-NAG-PLAN` | `NAGENTS-CONSOLIDATION-PLAN.md` | NOT_A_P4_PATH_FAMILY: this is the 8gent consolidation plan that defines the target package and ABM section matrix; P4 JSON is cited separately as the audit artifact. | §4.8, lines 169–179: target package and ABM-01..07 matrix; §5, lines 201–234: P4 group ledger and ABM group counts; §6.2–6.4, lines 247–277: variants, drift and owner gates; §7–8, lines 279–327: completeness and safe additive execution |

Zachowane reguły:

- Natywny lifecycle Hermesa to `triage → todo → scheduled → ready → running → review → done/blocked`; logiczna faza procesu to `operator → evaluator → defense (warunkowo) → final-control → integration`.
- `native_status` jest statusem dispatchera, a `process_phase` jest jawną metadaną karty/receiptu; fazy nie wolno zgadywać z tytułu, kolumny ani profilu.
- Trasa kontrolna wiąże `board → project_id → tenant → assignee/profile → process_phase → topic → idempotency_key`; bramka procesowa pozostaje bez assignee i workera.
- Preflight musi sprawdzić board, project, profil, tenant, parents, workspace/repo/branch, model/provider/effort/tier, GOAL, binarne kryteria, allowlistę i readback po utworzeniu.
- Brak jawnego assignee, błędny profil/projekt/workspace, otwarty parent albo brak dowodu daje `INFRA/ROUTING_ERROR`; nie wolno użyć globalnego fallbacku.

Granica dowodu: ten staging package nie potwierdza `current Kanban card inventory`, `current project/profile lookup`, `current worker run or event`. Każdy taki fakt wymaga świeżego, task-scoped readbacku; statyczny dokument, `queued`, `delivered`, UI lub raport nie zastępuje dowodu.

## ABM-03 — Cron → spool → receiver/helper → fazy Kanbana

Status: `SOURCE + CONSOLIDATION_CANDIDATE; owner-gated live service/canary`
P4 groups: `ABM-OPS`, `ABM-RELAY`

Źródła, statusy i locatory:

| Source ID | Ścieżka | P4 status | Exact locator |
|---|---|---|---|
| `SRC-ABM-KANBAN` | `AUTOBOT-KANBAN.md` | PF-0002 / CANONICAL; drift vs P2 87c426598b7d | §1–2, lines 44–90: source of truth, project/profile/board namespace and fallback prohibition; §3–4, lines 92–177: native_status/process_phase and dispatch preflight; §5, lines 179–338: phase graph, Cron → receiver → worker and owner target; §6–7, lines 340–490: receipts, readbacks, terminal events, watchdog and recovery; §10, lines 536–558: owner-chat bridge and selective delivery |
| `SRC-ABM-CRON-RUNBOOK` | `docs/ABM-CRON-HELPER-OPERATING-RUNBOOK.md` | PF-0012 / SOURCE | §1, lines 12–48: one profile and visible/canonical/worker chat distinction; §5, lines 211–320: Cron → spool → receiver/helper → worker contract; §6, lines 324–377: owner-chat delivery boundary and Desktop/server distinction; §7–8, lines 381–471: handoff, technical/contextual readback and canary; §9, lines 475–491: prohibited shortcuts; §11, lines 543–557: next step remains gated by visible-owner canary |
| `SRC-ABM-SAME-PROFILE` | `docs/ABM-SAME-PROFILE-CRON-MIGRATION.md` | PF-0015 / SOURCE | §1–2, lines 1–73: one-project/one-profile decision and no manual state.db migration; §5, lines 322–367: one canonical Cron, receiver and visible owner target; §6, lines 368–394: server-only cutover and replay checks; §7–8, lines 419–464: acceptance status, owner hold and rollback; §9–10, lines 466–500: agent handoff and next gated step |
| `SRC-ABM-TAGGING-3` | `docs/ABM-CARD-TAGGING-GUIDE-3.md` | PF-0011 / SOURCE | lines 1–12: precedence over attachments and current operational scope; §0–2, lines 10–94: owner scope, hierarchy and project separation; §4, lines 125–264: board/project/tenant/assignee/phase/topic/idempotency/model fields; §6–9, lines 307–455: phase cards, UI limits and historical reconciliation; §11–14, lines 495–568: dispatch checklist, prohibitions and source list |
| `SRC-NAG-PLAN` | `NAGENTS-CONSOLIDATION-PLAN.md` | NOT_A_P4_PATH_FAMILY: this is the 8gent consolidation plan that defines the target package and ABM section matrix; P4 JSON is cited separately as the audit artifact. | §4.8, lines 169–179: target package and ABM-01..07 matrix; §5, lines 201–234: P4 group ledger and ABM group counts; §6.2–6.4, lines 247–277: variants, drift and owner gates; §7–8, lines 279–327: completeness and safe additive execution |
| `SRC-P4` | `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json` | P4_AUDIT_ARTIFACT: this JSON is the classification input itself, not a product-contract path family. | top-level keys: inventory, path_family_classifications, logical_group_rollups, current_local_readback_drift, quality_checks; inventory: 254 path families / 833 records / 272 unique SHA-256 values; ABM logical groups and selected path-family rows used for this package |

Zachowane reguły:

- Cron jest serwerowym, read-only generatorem dyspozycji (`no_agent=true`), a nie workerem produktu; czyta pełny board i nie ma sztucznego limitu kart.
- Przepływ: Cron co 5 min → profile-local spool → supervised receiver/helper → identity/hash/durable receipt → świeży readback boardu/grafu/rodziców/runów → natywny dispatcher kwalifikowanych kart `READY` → osobny worker.
- Helper nie uruchamia kart `TODO`, `TRIAGE`, `BLOCKED`, process-only, terminalnych ani z niespełnionym rodzicem; brak targetu/profilu/boardu/receiptu jest błędem fail-closed.
- Po `kanban_complete`/`kanban_block` wymagane są terminalny event i readback techniczny/kontekstowy; następca jest tworzony lub odzyskiwany idempotentnie.
- Defense powstaje wyłącznie przy konkretnych zarzutach Evaluatora; po Final Control pozostaje `INTEGRATION_REQUIRED`, a ten status nie jest zgodą na push/merge/deploy.
- P4 nie wybiera zwycięzcy wariantu relay; `ABM-RELAY` pozostaje `CONSOLIDATION_CANDIDATE` do exact live readbacku i canary.

Granica dowodu: ten staging package nie potwierdza `current Cron job/service state`, `current receiver process`, `server-only canary with Desktop closed`, `current eligible READY wave`. Każdy taki fakt wymaga świeżego, task-scoped readbacku; statyczny dokument, `queued`, `delivered`, UI lub raport nie zastępuje dowodu.

## ABM-04 — provider/model/effort/Fast i granica owner-chat

Status: `SOURCE; requested route is documented, actual runtime requires receipt/readback`
P4 groups: `ABM-OPS`, `ABM-RELAY`

Źródła, statusy i locatory:

| Source ID | Ścieżka | P4 status | Exact locator |
|---|---|---|---|
| `SRC-ABM-MODEL-POLICY` | `docs/AUTOBOT-MODEL-EFFORT-FAST-POLICY.md` | PF-0016 / SOURCE | §Canonical policy, lines 3–30: explicit model/provider/effort/tier and requested-vs-actual distinction; §Dispatch/receipt requirements, lines 32–65: required card and receipt fields; §Current implementation boundary, lines 67–79: prospective application and provider clamping; §Validation, lines 81–96: readback expectations |
| `SRC-ABM-KANBAN` | `AUTOBOT-KANBAN.md` | PF-0002 / CANONICAL; drift vs P2 87c426598b7d | §1–2, lines 44–90: source of truth, project/profile/board namespace and fallback prohibition; §3–4, lines 92–177: native_status/process_phase and dispatch preflight; §5, lines 179–338: phase graph, Cron → receiver → worker and owner target; §6–7, lines 340–490: receipts, readbacks, terminal events, watchdog and recovery; §10, lines 536–558: owner-chat bridge and selective delivery |
| `SRC-ABM-CRON-RUNBOOK` | `docs/ABM-CRON-HELPER-OPERATING-RUNBOOK.md` | PF-0012 / SOURCE | §1, lines 12–48: one profile and visible/canonical/worker chat distinction; §5, lines 211–320: Cron → spool → receiver/helper → worker contract; §6, lines 324–377: owner-chat delivery boundary and Desktop/server distinction; §7–8, lines 381–471: handoff, technical/contextual readback and canary; §9, lines 475–491: prohibited shortcuts; §11, lines 543–557: next step remains gated by visible-owner canary |
| `SRC-ABM-SAME-PROFILE` | `docs/ABM-SAME-PROFILE-CRON-MIGRATION.md` | PF-0015 / SOURCE | §1–2, lines 1–73: one-project/one-profile decision and no manual state.db migration; §5, lines 322–367: one canonical Cron, receiver and visible owner target; §6, lines 368–394: server-only cutover and replay checks; §7–8, lines 419–464: acceptance status, owner hold and rollback; §9–10, lines 466–500: agent handoff and next gated step |
| `SRC-ABM-TAGGING-3` | `docs/ABM-CARD-TAGGING-GUIDE-3.md` | PF-0011 / SOURCE | lines 1–12: precedence over attachments and current operational scope; §0–2, lines 10–94: owner scope, hierarchy and project separation; §4, lines 125–264: board/project/tenant/assignee/phase/topic/idempotency/model fields; §6–9, lines 307–455: phase cards, UI limits and historical reconciliation; §11–14, lines 495–568: dispatch checklist, prohibitions and source list |

Zachowane reguły:

- Dla nowych workerów: Operator/Evaluator/Defense = `provider=openai-codex`, `model=gpt-5.6-luna`, `reasoning_effort=max`, `service_tier=priority` (Fast). Final Control używa tego samego modelu i `reasoning_effort=ultra` + `priority`.
- `reasoning_effort` i Fast/service tier są niezależne. `Luna Ultra` nie jest osobnym ID; to requested `gpt-5.6-luna` + `ultra`.
- Receipt musi rozdzielać requested i actual model/provider/effort/service tier; provider może znormalizować wire effort, ale nie wolno cicho zmienić route karty.
- Widoczny owner chat, ukryty canonical Bot Chat i worker session to trzy różne cele. `deliver: local`, `deliver: bot-chat` i `message_agent` nie dowodzą same przez się dostawy do widocznej sesji.
- Wiadomość do widocznego owner chatu wymaga exact profile, session_id, source i live ownership/lease oraz wspieranej ścieżki; nie wolno pisać równolegle do sesji Desktopu ani do bazy.

Granica dowodu: ten staging package nie potwierdza `actual provider response and wire effort`, `current visible owner session/lease`, `successful visible-owner delivery`. Każdy taki fakt wymaga świeżego, task-scoped readbacku; statyczny dokument, `queued`, `delivered`, UI lub raport nie zastępuje dowodu.

## ABM-05 — package, artifact, patch i release review

Status: `SOURCE + CONSOLIDATION_CANDIDATE; NO_LIVE_INSTALL`
P4 groups: `ABM-PACKAGE`, `ABM-INTEGRATION`

Źródła, statusy i locatory:

| Source ID | Ścieżka | P4 status | Exact locator |
|---|---|---|---|
| `SRC-ABM-AGENTS` | `AGENTS.md` | PF-0001 / CANONICAL | §Goal, lines 3–5: durable read-only monitor objective; §Architecture constraints, lines 30–39: server execution, independent roles and dispatch contract; §Safety, lines 50–61: read-only/security/no-live-install boundary; §Acceptance criteria D–E, lines 87–96: package and verification gates |
| `SRC-ABM-KANBAN` | `AUTOBOT-KANBAN.md` | PF-0002 / CANONICAL; drift vs P2 87c426598b7d | §1–2, lines 44–90: source of truth, project/profile/board namespace and fallback prohibition; §3–4, lines 92–177: native_status/process_phase and dispatch preflight; §5, lines 179–338: phase graph, Cron → receiver → worker and owner target; §6–7, lines 340–490: receipts, readbacks, terminal events, watchdog and recovery; §10, lines 536–558: owner-chat bridge and selective delivery |
| `SRC-ABM-CRON-RUNBOOK` | `docs/ABM-CRON-HELPER-OPERATING-RUNBOOK.md` | PF-0012 / SOURCE | §1, lines 12–48: one profile and visible/canonical/worker chat distinction; §5, lines 211–320: Cron → spool → receiver/helper → worker contract; §6, lines 324–377: owner-chat delivery boundary and Desktop/server distinction; §7–8, lines 381–471: handoff, technical/contextual readback and canary; §9, lines 475–491: prohibited shortcuts; §11, lines 543–557: next step remains gated by visible-owner canary |
| `SRC-NAG-PLAN` | `NAGENTS-CONSOLIDATION-PLAN.md` | NOT_A_P4_PATH_FAMILY: this is the 8gent consolidation plan that defines the target package and ABM section matrix; P4 JSON is cited separately as the audit artifact. | §4.8, lines 169–179: target package and ABM-01..07 matrix; §5, lines 201–234: P4 group ledger and ABM group counts; §6.2–6.4, lines 247–277: variants, drift and owner gates; §7–8, lines 279–327: completeness and safe additive execution |
| `SRC-P4` | `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json` | P4_AUDIT_ARTIFACT: this JSON is the classification input itself, not a product-contract path family. | top-level keys: inventory, path_family_classifications, logical_group_rollups, current_local_readback_drift, quality_checks; inventory: 254 path families / 833 records / 272 unique SHA-256 values; ABM logical groups and selected path-family rows used for this package |

Zachowane reguły:

- Monitor v1 jest read-only: pane/status/progress, report/diff summary, sanitised logs i terminal snapshot; nie ma stop/kill/write/merge/push/deploy controls.
- Docelowy pakiet rozdziela server Python backend i native Desktop plugin; plugin, API i UI nie mogą rozszerzać dostępu ani ujawniać tokenów, env, arbitralnych ścieżek/komend lub niesanitizowanego terminala.
- Instalacja, restart gatewaya, publikacja i release wymagają osobnej zgody właściciela. Ten staging package nie jest instalacją, nie jest live contractem i nie nadaje zgody.
- P4 `ABM-PACKAGE` (1/2) pozostaje `CONSOLIDATION_CANDIDATE`, a `ABM-INTEGRATION` (1/1) pozostaje `SOURCE`; wariant lokalny/remote wymaga release review z manifestem, base SHA i testami.
- Nie zmieniać 8gent, The-Game, Hermes core ani repozytorium ABM w ramach tego pakietu; nie publikować z samego README lub raportu.

Granica dowodu: ten staging package nie potwierdza `package build/tests`, `upstream acceptance`, `live install/gateway restart`, `publication or merge`. Każdy taki fakt wymaga świeżego, task-scoped readbacku; statyczny dokument, `queued`, `delivered`, UI lub raport nie zastępuje dowodu.

## ABM-06 — recovery, replay, receipts i auditability

Status: `CONSOLIDATION_CANDIDATE; owner-gated relay/canary and immutable evidence`
P4 groups: `ABM-RELAY`, `ABM-EVIDENCE`, `ABM-REPORTS`, `ABM-CONTRACT`

Źródła, statusy i locatory:

| Source ID | Ścieżka | P4 status | Exact locator |
|---|---|---|---|
| `SRC-ABM-KANBAN` | `AUTOBOT-KANBAN.md` | PF-0002 / CANONICAL; drift vs P2 87c426598b7d | §1–2, lines 44–90: source of truth, project/profile/board namespace and fallback prohibition; §3–4, lines 92–177: native_status/process_phase and dispatch preflight; §5, lines 179–338: phase graph, Cron → receiver → worker and owner target; §6–7, lines 340–490: receipts, readbacks, terminal events, watchdog and recovery; §10, lines 536–558: owner-chat bridge and selective delivery |
| `SRC-ABM-CRON-RUNBOOK` | `docs/ABM-CRON-HELPER-OPERATING-RUNBOOK.md` | PF-0012 / SOURCE | §1, lines 12–48: one profile and visible/canonical/worker chat distinction; §5, lines 211–320: Cron → spool → receiver/helper → worker contract; §6, lines 324–377: owner-chat delivery boundary and Desktop/server distinction; §7–8, lines 381–471: handoff, technical/contextual readback and canary; §9, lines 475–491: prohibited shortcuts; §11, lines 543–557: next step remains gated by visible-owner canary |
| `SRC-ABM-SAME-PROFILE` | `docs/ABM-SAME-PROFILE-CRON-MIGRATION.md` | PF-0015 / SOURCE | §1–2, lines 1–73: one-project/one-profile decision and no manual state.db migration; §5, lines 322–367: one canonical Cron, receiver and visible owner target; §6, lines 368–394: server-only cutover and replay checks; §7–8, lines 419–464: acceptance status, owner hold and rollback; §9–10, lines 466–500: agent handoff and next gated step |
| `SRC-ABM-CARD-INDEX` | `docs/ABM-CARD-INDEX.md` | NOT_INDEXED_IN_P4: current -3-era index was read after the P4 snapshot; no P4 row is invented. | lines 1–22: precedence, map-not-copy rule and readback boundary; §1–2, lines 26–66: ABM scope, project identity and required documents; §4–5, lines 137–184: no-touch classes, safe reconciliation and readback; §6, lines 195–202: source-of-truth and live-status rule |
| `SRC-NAG-PLAN` | `NAGENTS-CONSOLIDATION-PLAN.md` | NOT_A_P4_PATH_FAMILY: this is the 8gent consolidation plan that defines the target package and ABM section matrix; P4 JSON is cited separately as the audit artifact. | §4.8, lines 169–179: target package and ABM-01..07 matrix; §5, lines 201–234: P4 group ledger and ABM group counts; §6.2–6.4, lines 247–277: variants, drift and owner gates; §7–8, lines 279–327: completeness and safe additive execution |
| `SRC-P4` | `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json` | P4_AUDIT_ARTIFACT: this JSON is the classification input itself, not a product-contract path family. | top-level keys: inventory, path_family_classifications, logical_group_rollups, current_local_readback_drift, quality_checks; inventory: 254 path families / 833 records / 272 unique SHA-256 values; ABM logical groups and selected path-family rows used for this package |

Zachowane reguły:

- Receipt wiąże source run/task, raport i evidence SHA-256, contract/policy SHA-256, child task, idempotency key; brak receiptu/eventu/raportu/wymaganych pól daje `INFRA/UNKNOWN`.
- Fazę kończy wyłącznie `kanban_complete` albo `kanban_block` i terminalny event; plik raportu, exit procesu, heartbeat, UI lub słowo „gotowe” nie wystarczają.
- Idempotency key/digest chroni przed drugim workerem, następcą i wiadomością; przed create/reuse i po nich wymagany jest readback.
- Recovery rozróżnia `STALLED`, `INFRA`, `ROUTING_ERROR` i `FAIL`; nie wolno ukrywać błędu zmianą klucza, ręcznym SQL, cichym fallbackiem lub retry po `queued` bez native receipt.
- Canary owner-delivery wymaga `queued → claimed → settled`, dokładnie jednego inbound owner turnu, replayu tego samego digestu i braku drugiej wiadomości/lifecycle call; do tego czasu receiver/Cron pozostaje owner-gated.
- Runy, eventy, raporty, hashe, receipts i inne evidence pozostają osobne i niekasowalne; podobny szablon nie jest podstawą do scalenia różnych ID.

Granica dowodu: ten staging package nie potwierdza `fresh relay canary`, `current native receipt/replay`, `current owner inbound message`, `any cleanup or archival mutation`. Każdy taki fakt wymaga świeżego, task-scoped readbacku; statyczny dokument, `queued`, `delivered`, UI lub raport nie zastępuje dowodu.


## 4. P4 readback i statusy rodzin ABM

P4 ma status `PASS_WITH_EXPLICIT_OWNER_GATES`. Globalny audyt obejmuje 254
rodziny ścieżek, 833 rekordy i 272 unikalne SHA-256; AutoBot Monitor stanowi
169 rodzin i 226 rekordów. Exact hashes, source IDs, warianty, drift,
`unique_content`, konflikty, ryzyko i rekomendacje pozostają w
`coverage.json` jako maszynowym readbacku bieżącego wycinka ABM, a pełny audyt
pozostaje w `P4-classification.json`.

| P4 group | Status | Rodziny / rekordy | Target package | Relacja |
|---|---|---:|---|---|
| `ABM-CONTRACT` | `CANONICAL` | 3 / 5 | AUTOBOT-PROJECT.md as future package; AUTOBOT-KANBAN.md remains canonical | hierarchia kontraktu: AUTOBOT-KANBAN > AGENTS/supporting run ledger |
| `ABM-EVIDENCE` | `EVIDENCE` | 136 / 180 | docs/ABM-HISTORY.md index; runs remain separate | immutable run/task evidence; similar templates are not logical duplicates |
| `ABM-HANDOFF` | `HISTORY` | 1 / 1 | AUTOBOT-PROJECT.md or docs/ABM-HISTORY.md | dated owner handoff, not canonical runtime truth |
| `ABM-HISTORY` | `HISTORY` | 5 / 9 | docs/ABM-HISTORY.md | old plans, requirements and ledgers superseded by current contract/evidence |
| `ABM-INTEGRATION` | `SOURCE` | 1 / 1 | AUTOBOT-PROJECT.md / docs/ABM-LIFECYCLE.md | Hermes patch/provenance material, not 8gent documentation |
| `ABM-LIFECYCLE` | `SOURCE` | 4 / 7 | docs/ABM-LIFECYCLE.md | install/upgrade/uninstall/backup policy separate from runtime evidence |
| `ABM-OPS` | `SOURCE` | 7 / 7 | AUTOBOT-PROJECT.md / docs/ABM-LIFECYCLE.md | warstwowe runbooki operacyjne; część jest dynamicznym readbackiem |
| `ABM-PACKAGE` | `CONSOLIDATION_CANDIDATE` | 1 / 2 | AUTOBOT-PROJECT.md / package/README.md after release review | same path present local/remote with content divergence |
| `ABM-RELAY` | `CONSOLIDATION_CANDIDATE` | 9 / 10 | AUTOBOT-PROJECT.md / docs/ABM-LIFECYCLE.md after owner-gated canary | local-only active relay snapshot vs checkout Cron document; divergent same-path content |
| `ABM-REPORTS` | `EVIDENCE` | 2 / 4 | docs/ABM-HISTORY.md | final reports with acceptance mapping; historical snapshots |

Interpretacja bram:

- `CANONICAL` oznacza rangę źródła, nie zgodę na publikację pakietu.
- `SOURCE` oznacza read-only materiał wymagający świeżego odczytu przy użyciu.
- `CONSOLIDATION_CANDIDATE` oznacza brak rozstrzygnięcia wariantu; nie wolno
  wybrać zwycięzcy przez nazwę, długość, mtime ani samą obecność remote.
- `EVIDENCE` oznacza zachowanie exact task/run/event/artifact; podobne raporty
  nie są scalane, a `PASS` raportu nie dowodzi live install.
- Drift `AUTOBOT-KANBAN.md` i historyczny alias przewodnika jest jawny; bieżący
  hash zastępuje P2 tylko dla readbacku, nie dla cichego nadpisania źródła.

## 5. Bramy właścicielskie i brakujące dowody

Następujące działania pozostają poza tym tematem i nie są wykonywane po cichu:

1. live readback projektu, boardu, profilu, kart, runów, Crona i receivera;
2. owner-chat visible-target canary z `queued → claimed → settled` i replayem;
3. wybór wariantu relay/package po exact readbacku, manifest/base SHA/testach;
4. instalacja, restart gatewaya, publikacja, push, merge, deploy oraz cleanup;
5. ewentualna zmiana rangi źródła lub usunięcie/przeniesienie dokumentu po
   osobnej allowliście, Evaluatorze, Final Control i readbacku.

Do czasu spełnienia bram prawidłowy wynik live to `OWNER_HOLD`, `PENDING` albo
`INFRA` zależnie od konkretnego readbacku — nigdy domyślne `PASS`.

## 6. Bezpieczeństwo i rollback

- Monitor jest read-only; nie ma operacji stop/kill/write w pakiecie.
- Nie używać ręcznego SQL, globalnego assignee, profilu `default` jako cichego
  fallbacku, cross-board routingu ani ręcznej kopii `state.db`.
- Przy błędzie zachować graph, runy, eventy, receipt i hashe; zatrzymać tylko
  wadliwą ścieżkę i odtwarzać przez wspierany `hermes backup`/`hermes import`,
  nigdy przez ręczne kopiowanie prywatnego stanu.
- Ten plik nie nadaje uprawnienia do zmian w ABM ani do integracji z Hermesem.

## 7. Związek z następnymi fazami

Operator dostarcza ten staging package. Niezależny Evaluator powinien sprawdzić
`coverage.json`, exact hashes i kompletność locatorów. Defense może powstać tylko
przy numerowanych zarzutach. Final Control rozstrzyga `NAPRAW | ODDAL | DO
DECYZJI CZŁOWIEKA`; dopiero po jego `PASS`, owner gate i osobnym readbacku można
rozważyć publikację lub integrację.

PUSH/DEPLOY: NIE WYKONANO
