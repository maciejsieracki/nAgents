# N‑Agents — handoff rozdzielenia boardu i zadań

**Status:** bieżąca decyzja właściciela: wspólny board z rozdzieleniem przez
`project_id`, assignee, tenant i pola procesu; pierwotna propozycja osobnego
boardu pozostaje niżej jako wariant historyczny. Plik nie wykonuje migracji.

> **Instrukcja operacyjna dla agenta:** Przeczytaj cały załączony plik
> `NAGENTS-BOARD-SEPARATION-HANDOFF.md` i traktuj go jako instrukcję
> operacyjną. Nie wykonuj migracji, nie twórz boardu ani profilu bez
> potwierdzenia preflightu i zgody właściciela. Najpierw potwierdź odczyt
> boardu, projektu, profilu i kart, a następnie zgłoś, czego brakuje do
> bezpiecznego rozdzielenia.
>
> **Ważne:** sam plik nie tworzy jeszcze boardu ani profilu. Dla kart
> nAgents bieżąca decyzja właściciela wskazuje assignee `default`; nazwa
> `nagents-coder` z historycznego wariantu nie jest wymagana. Nie przypisuj
> kart do `autobotmonitor`.

## Bieżąca decyzja właściciela — wspólny board

N‑Agents może korzystać ze wspólnego boardu projektu `autobot-monitor`. Nowe
lub odtwarzane karty nAgents muszą mieć komplet:

```text
board:              autobot-monitor
project_id:         p_e90c30bc
project_name/slug:  nAgents Documentation / nagents-docs
tenant:             nagents-docs
assignee:           default
prefix kart:        NAG-
process_phase:      operator | evaluator | defense | final-control
topic:              stabilny temat
idempotency_key:    unikalny klucz karty
workspace:          /home/ubuntu/projects/nAgents-readonly
```

`default` jest tu świadomym assignee właściciela/orkiestratora zgodnie z jego
aktualną decyzją. `autobotmonitor` pozostaje assignee kart AutoBot Monitor
z prefiksem `ABM-`; nie używaj go dla nowych kart `NAG-`.

Jeżeli karta nAgents nie spełnia powyższych pól, zatrzymaj jej dispatch na
`INFRA/DECISION_REQUIRED`. Nie twórz boardu `nagents-docs` na podstawie
historycznego wariantu poniżej bez nowej zgody właściciela.

## Wariant historyczny — osobny board (nieaktywny)


Oddzielić dokumentację N‑Agents od wspólnego boardu wykonawczego
`autobot-monitor`. Po rozdzieleniu:

- AutoBot Monitor pracuje na boardzie `autobot-monitor`;
- N‑Agents Documentation pracuje na osobnym boardzie `nagents-docs`;
- żadna nowa karta N‑Agents nie powstaje na `autobot-monitor`;
- żadna karta nie jest przenoszona przez SQL, ręczną edycję `kanban.db` ani
  kopiowanie `state.db`;
- istniejące karty pozostają na starym boardzie jako historia do czasu
  utworzenia i odczytania odpowiedników na nowym boardzie.

## 2. Docelowe parametry N‑Agents

```text
board:              nagents-docs
board_name:         nAgents Documentation
project_slug:       nagents-docs
project_id:         p_cb0f9def  # history: anchor in profile autobotmonitor; inactive for default workers
primary_workspace:  /home/ubuntu/projects/nAgents-readonly
execution_tenant:   nagents-docs
```

Nie ustawiaj `autobotmonitor` jako docelowego właściciela N‑Agents. To jest
profil wykonawczy AutoBot Monitor, który historycznie wykonywał także część
zadań dokumentacyjnych na wspólnym boardzie.

## 3. Profil / assignee

Bieżący model właściciela używa profilu `default` jako assignee kart nAgents.
Anchor projektu w tym profilu to `nagents-docs / p_e90c30bc`; profil
`autobotmonitor` i wcześniejszy `p_cb0f9def` pozostają historycznym execution
path. `nagents-coder` jest nazwą historycznej rekomendacji, nie wymaganym
profilem i nie może być używany jako blokada bieżącego dispatchu.

Przed utworzeniem lub wznowieniem karty wykonaj:

```bash
hermes --profile default project show nagents-docs
hermes profile list
```

Wymagane pola:

```text
assignee: default
project_id: p_e90c30bc
tenant: nagents-docs
```

Jeżeli anchor `p_e90c30bc` lub workspace nie jest dostępny, zatrzymaj dispatch
z wynikiem `INFRA/ROUTING_ERROR`; nie używaj `autobotmonitor` jako zastępstwa.

## 4. Utworzenie boardu

Wykonaj po zgodzie właściciela i po potwierdzeniu profilu:

```bash
hermes kanban boards create nagents-docs \
  --name 'nAgents Documentation' \
  --description 'Documentation audit, source-of-truth consolidation and publication plan for nAgents' \
  --default-workdir /home/ubuntu/projects/nAgents-readonly

hermes project bind-board nagents-docs nagents-docs

hermes kanban boards list
hermes project show nagents-docs
```

Nie ustawiaj boardu jako aktywnego globalnie, jeśli nie jest to konieczne.
Każde następne polecenie musi zawierać jawne:

```bash
hermes kanban --board nagents-docs ...
```

## 5. Karty należące do N‑Agents

Poniższe karty mają temat `NAG-DOCS-CONSOLIDATION-Q1` i powinny być obsługiwane
przez N‑Agents, nie przez AutoBot Monitor:

| Stara karta | Temat/faza | Obecny status | Decyzja migracyjna |
|---|---|---:|---|
| `t_c730b27c` | plan register / documentation audit and consolidation | done | odtworzyć jako historyczny plan register albo zachować mapę referencyjną; nie uruchamiać ponownie |
| `t_0d39c010` | P2 — Markdown inventory | done | zachować jako historyczny rezultat; nie tworzyć aktywnego retry |
| `t_28d855e9` | P1 — scope | done | zachować jako historyczny rezultat; nie tworzyć aktywnego retry |
| `t_adf197a7` | P3 — sources of truth | done | zachować jako historyczny rezultat; nie tworzyć aktywnego retry |
| `t_f07cb6ae` | P4 — stale duplicates | done | zachować jako historyczny rezultat; nie tworzyć aktywnego retry |
| `t_e103545b` | P5 — NAGENTS-PROJECT index update | done (run 199) | zachować jako historyczny rezultat; nie tworzyć aktywnego retry |
| `t_5e123be9` | P6 — consolidation matrix | blocked | odtworzyć jako blocked/todo po readbacku P5 i źródeł prawdy |
| `t_872fe837` | P7 — publish package after OWNER_HOLD | blocked | odtworzyć jako blocked; wymaga decyzji właściciela, nie dispatchować automatycznie |

Karty `done` nie są przenoszone jako aktywne zadania. Ich raporty i runy są
historią. Aktywne lub zablokowane karty można odtworzyć dopiero po readbacku i
z mapą starego ID.

## 6. Parametry nowych kart

Dla zwykłej karty Operatora N‑Agents użyj jawnych pól:

```text
board:              nagents-docs
project:            nagents-docs / p_e90c30bc
tenant:             nagents-docs
assignee:           default
workspace:          dir:/home/ubuntu/projects/nAgents-readonly
provider:           openai-codex
model:              gpt-5.6-luna
reasoning:          max dla Operator/Evaluator/Defense
service-tier:       priority
phase:              operator | evaluator | defense | final-control
completion:         local-only
push/merge/deploy:  NOT PERFORMED
```

Dla bramki planu, OWNER_HOLD albo publikacji:

```text
assignee: none
worker:   none
status:   blocked
```

Nie przypisuj bramki procesowej do profilu tylko po to, żeby zobaczyć ruch.

## 7. Wzorzec utworzenia karty po migracji

Najpierw utwórz nową kartę w `nagents-docs`; nie zmieniaj starej karty w tej
samej operacji:

```bash
hermes kanban --board nagents-docs create \
  'NAG-DOCS-P4-STALE-DUPLICATES-Q1 — Operator — duplikaty i nieaktualność' \
  --project nagents-docs \
  --tenant nagents-docs \
  --assignee <REGISTERED_NAGENTS_PROFILE> \
  --workspace dir:/home/ubuntu/projects/nAgents-readonly \
  --idempotency-key 'NAGENTS-DOCS-CONSOLIDATION-Q1:P4-STALE-DUPLICATES:OPERATOR:r1' \
  --model gpt-5.6-luna \
  --provider openai-codex \
  --reasoning max \
  --service-tier priority \
  --phase operator \
  --completion-contract local-only \
  --body 'TOPIC: NAGENTS-DOCS-CONSOLIDATION-Q1
OLD_TASK_ID: t_f07cb6ae
BOARD: nagents-docs
PROJECT: nagents-docs / p_e90c30bc
TENANT: nagents-docs
WORKSPACE: /home/ubuntu/projects/nAgents-readonly
PROFILE: <REGISTERED_NAGENTS_PROFILE>
PUSH/DEPLOY: NOT PERFORMED'
```

Po utworzeniu wykonaj readback:

```bash
hermes kanban --board nagents-docs show <NEW_TASK_ID> --json
hermes kanban --board nagents-docs attachments <NEW_TASK_ID> --json
```

Nowy board nie może mieć natywnego parenta wskazującego starą kartę z
`autobot-monitor`. Stare ID zapisuj w body i w osobnej macierzy migracji.

## 8. Macierz migracji

Utwórz i utrzymuj plik:

```text
old_board: autobot-monitor
new_board: nagents-docs
old_task_id → new_task_id → status → evidence → decision
```

Minimalne wpisy:

```text
t_c730b27c → historical-only → done → old report/readback → no active retry
t_0d39c010 → historical-only → done → P2 report → no active retry
t_28d855e9 → historical-only → done → P1 report → no active retry
t_adf197a7 → historical-only → done → P3 report → no active retry
t_f07cb6ae → historical-only → done → old run terminal → no active retry
t_e103545b → historical-only → done → run 199 terminal → no active retry
t_5e123be9 → pending-new-board-card → blocked → parent P6 → recreate after preflight
t_872fe837 → pending-new-board-card → blocked/owner-hold → P7 → recreate only after decision
```

## 9. Zamknięcie starej ścieżki

Nie archiwizuj, nie blokuj i nie zamykaj starych kart automatycznie. Po:

1. utworzeniu nowego boardu;
2. potwierdzeniu profilu N‑Agents;
3. utworzeniu nowych kart aktywnych;
4. readbacku ich project/board/workspace/assignee/parents;
5. porównaniu evidence;
6. akceptacji właściciela;

można dodać do starych kart komentarz migracyjny i dopiero wtedy zastosować
ustaloną politykę historyczną. Nie kasuj starych runów, eventów, receiptów ani
attachmentów.

## 10. Stop conditions

Zatrzymaj pracę z `INFRA/DECISION_REQUIRED`, gdy:

- nie istnieje zarejestrowany profil N‑Agents;
- board `nagents-docs` nie jest utworzony lub projekt nie jest z nim związany;
- karta ma aktywny run/claim;
- nie można potwierdzić starego task ID i jego evidence;
- migracja wymaga zmiany `project_id`, `tenant` lub parentów starej karty;
- ktoś proponuje SQL, kopiowanie `state.db`, merge, push, deploy, instalację lub
  restart bez osobnej zgody.

**Decyzja właściciela wymagana przed:** rejestracją profilu N‑Agents, utworzeniem
boardu, odtworzeniem kart aktywnych i zamknięciem kart na starym boardzie.
