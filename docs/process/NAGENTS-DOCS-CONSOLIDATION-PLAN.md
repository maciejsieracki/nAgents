# Plan porządkowania i konsolidacji dokumentacji 8gent

**Status:** `ACTIVE / AUTONOMOUS_EXECUTION`

**Właściciel:** zatwierdził pełne wykonanie planu w głównym wątku.

**Cel:** przeanalizować, uporządkować i skonsolidować dokumentację 8gent oraz
powiązanego AutoBot Monitor, zachowując treść, proweniencję i dowody. Plan ma
być także stałym playbookiem dla przyszłych porządków dokumentacji.

> Ten plik opisuje zakres, graf Kanbana, bramki jakości i reguły sprzątania.
> Nie jest zamiennikiem źródeł prawdy projektu ani raportów fazowych.

---

## 1. Granice projektu

### Projekt główny

```text
nAgents
/home/ubuntu/projects/nAgents-readonly
```

### Projekt wykonawczy i towarzyszący

```text
AutoBot Monitor
/home/ubuntu/projects/Autoboot-Monitor
```

AutoBot Monitor dostarcza Kanban, dispatcher, receiver, watchdog i kontrakt
procesu. Nie wolno mieszać jego kodu z dokumentacją 8gent bez wskazania
konkretnej ścieżki i uzasadnienia.

### Projekt odrębny

```text
The-Game
```

The-Game nie jest scalany z 8gent. Można odnotować jego dokumenty i handoffy
jako `SEPARATE_PROJECT`, ale nie wolno ustanawiać ich źródłem prawdy 8gent.

### Źródła objęte audytem

- checkout 8gent na OVH;
- wszystkie istotne worktree 8gent;
- handoffy i artefakty używane przez 8gent;
- dokumentacja AutoBot Monitor związana z routingiem, Kanbanem, Cronem,
  receiverem i procesem;
- wskazane branche GitHuba 8gent i AutoBot Monitor;
- dokumenty publikacyjne, plany, decyzje, scenariusze, dispatchy i evidence.

### Wyłączenia

Nie traktować jako normalnej dokumentacji:

```text
.git/
node_modules/
target/
dist/
build/
cache/
.tmp/
raw runtime state
state.db
sesje
surowe logi
fixtures i wygenerowane artefakty testowe
```

Wyłączenie nie oznacza ukrycia. Katalog zostaje odnotowany jako artefakt
techniczny, jeśli wpływa na proweniencję lub dowód.

---

## 2. Kanoniczny obieg pracy

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

Każda faza ma osobną kartę Kanbana, run, idempotency key, workspace, artefakt,
Operatora, niezależnego Evaluatora i Final Control. Defense powstaje wyłącznie
przy rzeczywistych, numerowanych zarzutach Evaluatora.

### Stabilne ID tematów

```text
NAG-DOCS-CONSOLIDATION-Q1
NAG-DOCS-P1-SCOPE-Q1
NAG-DOCS-P2-INVENTORY-Q1
NAG-DOCS-P3-SOURCES-OF-TRUTH-Q1
NAG-DOCS-P4-STALE-DUPLICATES-Q1
NAG-DOCS-P5-INDEX-UPDATE-Q1
NAG-DOCS-P6-CONSOLIDATION-PLAN-Q1
NAG-DOCS-P7-PUBLISH-Q1
```

Task ID nadaje Kanban. Stable ID nie zmienia się między rundami.

### Bieżąca rejestracja Kanbana

| Element | Task/job ID | Stan przy ostatnim readbacku |
|---|---|---|
| parent / plan register | `t_c730b27c` | `DONE`; najnowszy załącznik planu jest odczytywany z karty parent |
| P1 scope | `t_28d855e9` | `DONE`, run `195` |
| P2 inventory | `t_0d39c010` | `DONE`, run `196` |
| P3 sources | `t_adf197a7` | `DONE`, run `197` |
| P4 duplicates/stale | `t_f07cb6ae` | `DONE`, run `198` |
| P5 index | `t_e103545b` | `DONE`, run `199`; no active retry |
| P6 consolidation | `t_5e123be9` | `DONE`, run `201`; `PLAN_ONLY / OWNER_HOLD_REQUIRED` |
| P7 publish | `t_872fe837` | `BLOCKED`, parent P6 and owner gate |
| server Kanban watchdog | `6911e5eac7d3` | active, `every 5m`, `no_agent`, receiver active |
| current-chat reminder | `c02f6f601e09` | active, `every 5m`, `deliver=origin`; manual run delivered successfully to this conversation |

Nowe karty 8gent będą używać wspólnego boardu `autobot-monitor` z aktualnym
`project_id: p_e90c30bc`, slugiem `nagents-docs`, tenantem `nagents-docs`,
assignee `default`, prefiksem `NAG-`, jawnym `process_phase`, stabilnym `topic`
i unikalnym `idempotency_key`. Karty P1–P6 powyżej pozostają historycznymi
terminalnymi runami; nie zmieniaj ich statusów i nie twórz ich duplikatów.

### Wykonanie tematyczne po P6/P7

| Temat | Operator | Evaluator | Final Control | P7 | Stan |
|---|---|---|---|---|---|
| `NAG-CONSOLIDATE-SPEC-Q1` | `t_d391b30f` / run `205` | `t_5604bc68` / run `206` | `t_9fc85b12` / run `207` | `t_872fe837` / run `208` | `PASS`, staging-only; 5 plików, bez publikacji |
| `NAG-CONSOLIDATE-DECISIONS-Q1` | `t_c09403d3` / run `210` | `t_7953af39` / run `211` | `t_e663b85b` / run `212` | `t_132ffdd7` / run `214` | `PASS`, staging-only; SRC-06 reconciled |
| `NAG-CONSOLIDATE-PROCESS-Q1` | `t_95e6cb34` / run `215` | `t_144ba28b` / run `216` | `t_b9e82e32` / run `217` | `t_b8a1e9b2` / run `218` | `PASS`, staging-only |
| `NAG-CONSOLIDATE-HANDOFF-Q1` | `t_a173dc9e` / run `219` | `t_3433734a` / run `220` | `t_8bc3e0be` / run `221` | `t_3594d8f0` / run `222` | `PASS`, staging-only |
| `NAG-CONSOLIDATE-RESEARCH-Q1` | `t_75595cef` / runs `223→226` | review recovery w tym samym task ID / run `226` PASS | — | `t_0b5d6edf` / run `237` | `PASS`, staging-only; 4 pliki |
| `NAG-CONSOLIDATE-USER-GUIDE-Q1` | `t_9e8f68de` / run `227` | `t_e299cf8e` / run `228` | `t_05455749` / run `229` | `t_d7c0c06d` / run `230` | `PASS`, staging-only |
| `NAG-CONSOLIDATE-ABM-HISTORY-Q1` | `t_f026c7e9` / run `231` | `t_72bb1fa4` / run `232`; R2 `t_79f6bd5b` / run `234` | `t_9d6256bd` / run `235` | `t_475b464c` / run `236` | `PASS`, staging-only; R1 2 zarzuty przyjęte i zamknięte |
| `NAG-CONSOLIDATE-AUTOBOT-PROJECT-Q1` | `t_8c84c659` / run `238` | `t_89297d53` / run `239` | `t_adc3f027` / run `240` | `t_f9660fa7` / run `241` | `PASS`, staging-only |
| `NAG-CONSOLIDATE-ABM-LIFECYCLE-Q1` | `t_8a205853` / run `242` (routing block; completion receipt `243`) | `t_0ccffd4e` / run `244` | `t_a349126e` / run `245` | `t_0bd4ccd5` / run `246` | `PASS`, manual-only/staging-only |
| `NAG-CONSOLIDATE-INTEGRATIONS-Q1` | `t_24edebcc` owner gate | — | — | — | `OWNER_HOLD`; wybór A: research-only, bez integracji/live worker, bez assignee |

Pierwszy pakiet tematyczny pozostaje wyłącznie w
`docs/process/staging/NAG-CONSOLIDATE-SPEC-Q1/`. Nie zastępuje źródeł
kanonicznych i nie jest publikacją. Kolejne pakiety tworzyć sekwencyjnie w
osobnych tematach `NAG-`, ponieważ checkout jest współdzielony.

### Minimalny graf kart

```text
plan register / parent
  └── P1
      └── P2
          └── P3
              └── P4
                  └── P5
                      └── P6
                          └── OWNER_HOLD
                              └── P7
```

Parent jest kartą rejestru planu, nie workerem produktowym. Faza nie może być
uruchomiona tylko dlatego, że istnieje w tym pliku; musi mieć zgodną kartę,
rodzica, allowlistę i readback.

---

## 3. Role i bramki

### Operator

Wykonuje wyłącznie zakres jednej fazy w izolowanym workspace. Zapisuje raport,
artefakty maszynowe, hashe, liczniki, ograniczenia i dokładny następny krok.
Nie ocenia własnego wyniku, nie usuwa źródeł i nie publikuje.

### Evaluator

Niezależnie odczytuje rzeczywisty artefakt, powtarza obliczenia i kontroluje
zakres, liczniki, hashe, linki, klasyfikację i kompletność. Każde odstępstwo
jest osobnym numerowanym zarzutem. Evaluator nie naprawia po cichu.

### Defense

Uruchamiana tylko przy niepustej liście zarzutów. Odpowiada na każdy numer
`PRZYJMUJĘ` albo `ODRZUCAM` z dowodem. Nie może rozszerzyć zakresu ani usunąć
pliku.

### Final Control

Sprawdza artefakt bez polegania na deklaracji Operatora. Dla każdego zarzutu
wydaje `NAPRAW`, `ODDAL` albo `DO DECYZJI CZŁOWIEKA`. `PASS` nie oznacza jeszcze
publikacji.

### Orchestrator staging/readback

Wykonuje techniczny i kontekstowy readback, sprawdza hash, diff, rodziców,
allowlistę i idempotency key. Przygotowuje następną fazę. Nie wykonuje merge,
push, deployu ani usuwania przed właściwą bramką.

---

## 4. Specyfikacja faz

### P1 — zakres audytu

**ID:** `NAG-DOCS-P1-SCOPE-Q1`

**Cel:** jednoznacznie określić repozytoria, katalogi, branche, worktree,
handoffy i wyłączenia.

**Artefakty:**

```text
P1-scope.md
P1-sources.json
P1-exclusions.md
```

**Kryterium PASS:** każdy późniejszy plik ma przypisane źródło: 8gent,
AutoBot Monitor, The-Game, handoff, GitHub, runtime albo evidence.

### P2 — inwentaryzacja Markdownów

**ID:** `NAG-DOCS-P2-INVENTORY-Q1`

**Cel:** programowo zebrać listę plików bez ładowania całej treści do kontekstu.

**Pola:**

```text
path
source_root
repository
branch/ref
tracked/untracked/modified
size_bytes
line_count
first_heading
top_level_headings
sha256
mtime, jeśli dostępne
presence_local
presence_github
category
freshness_status
```

**Artefakty:**

```text
P2-inventory-local.jsonl
P2-inventory-github.jsonl
P2-inventory-external.jsonl
P2-inventory-summary.md
P2-counts.json
```

**Kryterium PASS:** liczniki są odtwarzalne, a różnice `LOCAL_ONLY` i
`REMOTE_ONLY` są jawnie wymienione.

### P3 — źródła prawdy

**ID:** `NAG-DOCS-P3-SOURCES-OF-TRUTH-Q1`

**Cel:** przypisać każdą kategorię pytań do jednego źródła kanonicznego.

**Kategorie:** start agenta, architektura, MVP, proces, routing, bieżący stan,
handoff, decyzje, pytania otwarte, scenariusze, integracje, dowody, historia,
publikacja, AutoBot Monitor i The-Game jako projekt odrębny.

**Artefakty:**

```text
P3-sources-of-truth.md
P3-source-map.json
```

**Kryterium PASS:** każda kategoria ma jedno źródło prawdy albo jawny status
`OWNER_DECISION_REQUIRED` lub `LIVE_READBACK_REQUIRED`.

### P4 — duplikaty i nieaktualność

**ID:** `NAG-DOCS-P4-STALE-DUPLICATES-Q1`

**Cel:** sklasyfikować relacje między dokumentami bez ich usuwania.

**Wynik:**

```text
P4-duplicates-stale.md
P4-classification.json
P4-consolidation-candidates.md
```

**Statusy:**

```text
CANONICAL
SOURCE
EVIDENCE
HISTORY
STALE
LOCAL_ONLY
REMOTE_ONLY
DUPLICATE
CONSOLIDATION_CANDIDATE
PRIVATE_RUNTIME
SEPARATE_PROJECT
OWNER_DECISION_REQUIRED
```

Każda grupa musi wskazywać unikalną treść, sprzeczności, kandydata na źródło
docelowe, ryzyko i zalecenie. W P4 nie wolno usuwać plików.

### P5 — aktualizacja `NAGENTS-PROJECT.md`

**ID:** `NAG-DOCS-P5-INDEX-UPDATE-Q1`

**Domyślna allowlista:**

```text
NAGENTS-PROJECT.md
```

Indeks ma wskazywać kolejność czytania, pytanie → pakiet → sekcję, lokalne i
zdalne różnice, statusy, granicę projektów, reguły ograniczania kontekstu,
readback i artefakty P1–P4. Nie kopiuje całej treści źródeł.

### P6 — plan konsolidacji treści

**ID:** `NAG-DOCS-P6-CONSOLIDATION-PLAN-Q1`

**Artefakt:**

```text
NAGENTS-CONSOLIDATION-PLAN.md
```

P6 przygotowuje macierz:

```text
źródło/grupa
status
źródło prawdy
pakiet docelowy
unikalna treść do zachowania
różnice i konflikty
ryzyko
wymaga zgody
kryterium pokrycia
```

P6 nie scala, nie przenosi, nie usuwa, nie zmienia routingu, nie wykonuje
pushu i nie wykonuje merge do `main`.

### P7 — weryfikacja i publikacja

**ID:** `NAG-DOCS-P7-PUBLISH-Q1`

P7 uruchamia się dopiero po `OWNER_HOLD` i jawnej decyzji zakresu publikacji.
Paczka obejmuje tylko allowlistę zatwierdzoną przez właściciela. Przed
publikacją wymagane są: `git diff --check`, kontrola linków, skan sekretów,
porównanie hashy, kontrola usunięć, commit jawnych plików, push na wskazaną
gałąź i zdalny readback SHA. Merge do `main` jest osobną decyzją.

---

## 5. Docelowe pakiety treści

Konsolidacja ma zachować możliwie dużo informacji i zmniejszyć liczbę miejsc,
w których agent szuka odpowiedzi.

```text
NAGENTS-SPEC.md
  architektura, zakres, MVP1–MVP4, kryteria techniczne

NAGENTS-DECISIONS.md
  decyzje, ECHO, pytania otwarte, historia rozstrzygnięć

NAGENTS-PROCESS.md
  proces, tematy, handoff rules, dispatch i raportowanie

NAGENTS-HANDOFF.md
  bieżący stan i przejęcie projektu

NAGENTS-RESEARCH.md
  noty projektowe i research, z zachowaniem źródeł pierwotnych

NAGENTS-INTEGRATIONS.md
  Hermes, Entra ID, Microsoft 365 i granice integracji

NAGENTS-USER-GUIDE.md
  dokument dla pracownika, bez technicznego żargonu

AUTOBOT-PROJECT.md
  kontrakt AutoBot Monitor, Kanban, Cron, routing i helper

docs/ABM-HISTORY.md
  stare raporty, plany i zamknięte ustalenia AutoBot

docs/ABM-LIFECYCLE.md
  instalacja, aktualizacja, backup, restore i dezinstalacja
```

Pakiety są kandydatami, nie zgodą na mechaniczne połączenie. Macierz P6 musi
wskazać, gdzie trafia każda unikalna sekcja.

### Pozostają osobno

- `NAGENTS-PROJECT.md` jako lekki indeks;
- źródła pierwotne i materiały zewnętrzne;
- `decisions.md` do czasu zweryfikowania migracji decyzji;
- scenariusze jako kontrakt testowy;
- aktywne dispatchy i evidence;
- raporty aktywnych workerów;
- prywatne handoffy i runtime;
- The-Game;
- skill Hermesa `SKILL.md`, jeśli jest używany jako plik wejściowy procesu.

---

## 6. Reguły usuwania i zachowania historii

Nie usuwać dokumentu tylko dlatego, że jest stary, podobnie nazwany albo ma
niższy hash. Usunięcie jest dozwolone dopiero, gdy:

1. treść została przeniesiona lub zachowana jako historia;
2. macierz pokrycia wskazuje pakiet i sekcję docelową;
3. działają odnośniki i przekierowania kompatybilności;
4. nie znikają dowody, decyzje ani proweniencja;
5. niezależny Evaluator potwierdził brak utraty;
6. Final Control dał `PASS`;
7. właściciel zatwierdził konkretną allowlistę usunięć;
8. readback po zmianie potwierdził stan lokalny i zdalny.

Stare pliki, których nie można bezpiecznie usunąć, oznaczać nagłówkiem/statusom
`HISTORY` albo `STALE`, bez przepisywania ich treści.

---

## 7. Kanban i runtime

```text
board: nagents-docs? NO — use shared board autobot-monitor
project slug: nagents-docs
project_id: p_e90c30bc
authoring/workspace: /home/ubuntu/projects/nAgents-readonly
assignee:           default
prefix kart:        NAG-
tenant:             nagents-docs
process_phase:      operator | evaluator | defense | final-control
topic:              stable NAG-DOCS-CONSOLIDATION-Q1
idempotency_key:    unikalny dla task/phase/round
```

`autobotmonitor` i project `p_ffb5c6ad` dotyczą historycznego execution path.
Aktywny P5/run 199 pozostaje nietknięty do terminalnego readbacku; nie tworzyć
jego duplikatu. Nowe lub odtwarzane karty 8gent mają używać wspólnego boardu,
ale aktualnego `project_id: p_e90c30bc` i zarejestrowanego assignee `default`.

Nie tworzyć nowego profilu ani boardu bez decyzji i preflightu. Nie używać
The-Game jako fallbacku. Każda nowa karta ma jawny model, provider, effort,
service tier, workspace, branch, allowlistę, rodzica, procesową fazę, topic,
idempotency key i completion contract.

### Przypomnienie i watchdog

Używany jest jeden kanoniczny job:

```text
job: 6911e5eac7d3
schedule: every 5m
no_agent: true
deliver: local
script: autobot_monitor_directive.py
receiver: autobot-monitor-cron-receiver.service
```

Job jest read-only i nie wykonuje pracy produktu. Sprawdza cały board, zapisuje
dyrektywę do serwerowego spoola, a receiver wykonuje readback i obsługuje
kwalifikowane przejścia. Nie tworzyć drugiego joba co pięć minut.

`deliver=local` oznacza trwałe wejście do receivera, nie dowód wiadomości w
konkretnym Desktopowym czacie. Każdy raport musi rozdzielać scheduler, receiver,
worker i owner-chat delivery.

---

## 8. Kryterium zakończenia całego programu

Program dokumentacyjny jest zakończony dopiero, gdy:

- audyt obejmuje uzgodniony zakres OVH, GitHuba, worktree i handoffów;
- znamy liczbę plików lokalnych, zdalnych, lokalnych-only i zdalnych-only;
- każdy dokument ma status i kategorię;
- każda kategoria ma źródło prawdy;
- duplikaty logiczne mają macierz pokrycia;
- treść pakietów docelowych jest opisana;
- dokumenty historyczne i nieaktualne są oznaczone;
- `NAGENTS-PROJECT.md` kieruje agenta do właściwych pakietów;
- plan konsolidacji ma niezależny PASS;
- po decyzji właściciela wykonano wyłącznie zatwierdzoną paczkę;
- linki, hashe, diff, skan sekretów i zdalny readback przechodzą;
- Kanban ma terminalne readbacki wszystkich faz i nie ma nieopisanych sierot;
- plan pozostaje odtwarzalny jako wzorzec dla następnego porządkowania.

Jeśli wystąpi `FAIL`, `BLOCK`, `INFRA`, `TIMEOUT`, `UNKNOWN` albo
`DECISION_REQUIRED`, następna faza nie rusza. Recovery zachowuje ten sam temat,
rundę, dowody i idempotency key; nie ukrywa błędu przez nową nazwę.

---

## 9. Dziennik zmian planu

| Data | Zmiana | Dowód |
|---|---|---|
| 2026-09-14 | Zapisano plan wykonawczy i playbook przyszłych audytów | główny wątek właściciela; karta parent w Kanbanie |
| 2026-09-14 | Utworzono anchor projektu `nagents-docs` w profilu `default` | Hermes project `p_e90c30bc`; wcześniejszy anchor `p_cb0f9def` pozostaje historycznym anchoru profilu `autobotmonitor` |
| 2026-09-14 | Potwierdzono użycie istniejącego watchdogu co 5 minut | Cron `83e4098a9f87`, receiver aktywny |

Dalsze wpisy dodaje Orchestrator po terminalnych readbackach faz. Nie zmieniaj
historii wpisów; korekty dopisuj jako nowe wiersze.
