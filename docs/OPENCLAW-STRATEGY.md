# 8gent — strategiczna podstawa OpenClaw

**Status:** CURRENT / OWNER DECISION
**Data decyzji:** 2026-09-15
**Zakres:** architektura platformy, automatyzacje, sesje, modele i przyszła wtyczka AutoBot Monitor
**Nie jest:** zgodą na instalację, zmianę konfiguracji, migrację danych, integrację live ani deploy

Ten dokument jest aktualnym punktem odniesienia dla wyboru platformy. Zastępuje
wcześniejsze założenie, że 8gent ma być budowany na Hermesie i LiteLLM. Raporty,
noty i stagingi opisujące wcześniejszy wariant pozostają zachowane jako historia
oraz evidence; nie są dowodem bieżącego runtime.

## 0. Decyzja właściciela

> „Podjęliśmy strategiczną decyzję, że nie będziemy 8gent opierać o Hermesa,
> tylko o OpenClaw. W związku z tym sprawdź dokumentację, którą obecnie
> przygotowałeś, i uzupełnij ją o stosowne informacje pod kątem OpenClaw. Na pewno
> nie będziemy robić OpenRoutera i OpenMonitora, bo wszystko jest dostępne od razu
> w OpenClaw, ale nie jest powiedziane, że ta praca pójdzie na marne. Być może
> wykorzystamy to, co było zrobione, i dopniemy jako wtyczkę AutoBot Monitor.”

Z tej decyzji wynikają następujące reguły:

1. **OpenClaw jest podstawą runtime 8gent.** Nie budujemy 8gent jako warstwy
   nad Hermesem.
2. **Nie budujemy osobnego OpenRoutera** ani nie dodajemy go jako wymaganej
   bramy/proxy. Modele i dostawcy mają być konfigurowani bezpośrednio w
   OpenClaw; użycie konkretnego dostawcy wymaga osobnej decyzji i testu.
3. **Nie budujemy osobnego OpenMonitora.** Monitoring, automatyzacje, sesje,
   zadania i powierzchnia operatorska mają najpierw korzystać z natywnych
   powierzchni OpenClaw.
4. **AutoBot Monitor pozostaje kandydatem na wtyczkę OpenClaw**, a nie drugim
   gatewayem, drugim schedulerem ani równoległym centrum sterowania.
5. **Dotychczasowa praca nie jest usuwana.** Reużywalne są: model uprawnień,
   tenantów, budżetów i audytu; scenariusze; procedury readbacku; kontrakty
   Operator → Evaluator → Defense warunkowo → Final Control; oraz materiały
   AutoBot Monitor. Hermes-specific komendy i założenia wymagają adaptacji.
6. **Nie wykonano migracji live.** Ten dokument aktualizuje architekturę i plan;
   instalacja OpenClaw, podpięcie kanałów, konfiguracja modeli, migracja danych
   i uruchomienie pluginu pozostają osobnymi bramkami właściciela.

## 1. Co potwierdza dokumentacja OpenClaw

OpenClaw jest self-hosted Gatewayem, który łączy kanały komunikacyjne,
agentów, narzędzia, sesje i urządzenia; jeden Gateway może obsługiwać wiele
kanałów, a klienci łączą się z nim przez CLI, Control UI i inne powierzchnie.[1][2]
Gateway jest control plane dla sesji, routingu, połączeń kanałowych i zdarzeń;
nie należy utożsamiać Control UI, CLI, aplikacji mobilnej ani node'a z osobnym
runtime.[2][14]

OpenClaw ma natywny model wielu agentów: agent posiada własny workspace,
`agentDir`, rejestr modeli i magazyn sesji, a binding kieruje konto/kanał do
właściwego agenta.[3] Sesje są własnością Gatewaya; domyślnie DM-y mogą korzystać
ze wspólnej sesji, grupy i pokoje są izolowane, a uruchomienia automatyzacji mogą
mieć świeżą sesję na run.[4]

Konfiguracja jest przechowywana jako JSON5 w `~/.openclaw/openclaw.json`, a
schemat konfiguracji powinien być odczytywany przed zmianą. Control UI korzysta
z żywego schematu, a Gateway może stosować bezpieczne zmiany przez hot reload
lub wymagać restartu zależnie od ustawienia.[5]

Automatyzacje są natywnym schedulerem OpenClaw; CLI `openclaw automations` ma
alias `openclaw cron`. Background tasks są rejestrem pracy poza główną sesją,
a nie schedulerem.[9][10] Task Flow jest warstwą orkiestracji nad zadaniami:
trwały flow może koordynować wiele kroków, mieć stan, rewizję i powiązane taski,
a managed flow może być sterowany przez kod pluginu.[11]

OpenClaw ma system pluginów dla kanałów, dostawców modeli, narzędzi, hooków,
serwisów, CLI i innych capabilities.[6][7][8] Task Flow może być sterowany przez
kod pluginu, jeżeli ten zakres okaże się potrzebny.[11] Native plugin wymaga
`openclaw.plugin.json`; manifest jest czytany przed uruchomieniem kodu, a
plugin APIs są eksperymentalne. Host OpenClaw oraz zgodność wersji muszą być
pinowane i testowane.[7][8]

OpenClaw rozdziela wybór modelu od niskopoziomowego runtime agenta i pozwala
ustawiać primary model, fallbacks oraz allowlistę modeli w konfiguracji agenta.
Dla 8gent oznacza to: nie wprowadzamy osobnego proxy modelowego jako warstwy
obowiązkowej; bezpośrednie konfiguracje dostawców OpenClaw są punktem wyjścia.[13]

Domyślne zasady bezpieczeństwa OpenClaw obejmują między innymi nasłuch Gatewaya
na loopback, pairing nieznanych urządzeń, allowlisty kanałów i audyt
konfiguracji. Dokumentacja wyraźnie opisuje jedną granicę zaufania na Gateway;
OpenClaw nie jest sam z siebie granicą dla wzajemnie wrogich najemców na jednym
Gatewayu.[12]

## 2. Docelowy układ 8gent

```text
kanały / Control UI / CLI / nodes
                │
                ▼
       OpenClaw Gateway
       ├── OpenClaw agents
       │   ├── workspace + agentDir
       │   └── Gateway-owned sessions
       ├── native channels, tools, skills and device capabilities
       ├── automations / cron
       ├── background tasks
       ├── Task Flow for multi-step orchestration
       └── direct model/provider configuration
                │
                ▼
       8gent — warstwa produktu i polityki
       ├── tożsamość, tenant i RBAC
       ├── katalog agentów i nadania
       ├── budżety i egzekwowanie limitów
       ├── audyt decyzji allow/deny/error
       ├── scenariusze biznesowe i wiedza wersjonowana
       └── webowa warstwa domenowa, jeżeli Control UI nie wystarczy

       AutoBot Monitor — OPCJONALNA WTYCZKA, tylko po potwierdzeniu luki
       ├── hooki / tools / service / CLI / Task Flow controller
       ├── readback i evidence procesu
       └── bez drugiego gatewaya, schedulera, OpenMonitora i OpenRoutera
```

OpenClaw jest warstwą wykonawczą i control plane. 8gent nie kopiuje jego sesji,
modeli, kanałów ani schedulerów; przechowuje wyłącznie własne dane domenowe i
polityki, a do OpenClaw odwołuje się przez jawne identyfikatory agenta,
workspace'u, sesji i runu.

## 3. Mapowanie wcześniejszej dokumentacji

| Wcześniejszy element | Obecny odpowiednik | Status |
|---|---|---|
| Hermes Gateway | OpenClaw Gateway | **CURRENT** |
| Profil Hermesa | OpenClaw agent + `agentDir` + workspace | **CURRENT**, nazwy pól do potwierdzenia w implementacji |
| Sesja Hermesa | Gateway-owned OpenClaw session / session key | **CURRENT**, bez migracji live |
| Hermes Desktop/web | OpenClaw Control UI, kanał lub CLI | **CURRENT**, powierzchnię wybiera się po testach dostępu |
| Hermes Cron | OpenClaw automations / `openclaw cron` alias | **CURRENT** |
| Hermes helper/receiver | OpenClaw automations + tasks + Task Flow; ewentualnie service plugin | **ADAPT**, nie kopiować procesu 1:1 |
| Hermes Kanban | OpenClaw tasks/Task Flow plus ewentualna warstwa pluginu | **GAP TO VERIFY**; nie ma automatycznego założenia 1:1 |
| LiteLLM proxy | bezpośredni model/provider config OpenClaw | **SUPERSEDED** jako obowiązkowa warstwa |
| OpenRouter | brak w architekturze 8gent | **REJECTED BY OWNER** |
| OpenMonitor | brak w architekturze 8gent | **REJECTED BY OWNER** |
| AutoBot Monitor | opcjonalny native OpenClaw plugin | **RESEARCH / OWNER GATE** |
| Hermes profile/cron commands | komendy `openclaw ...` | **LEGACY REFERENCE**, wymagają nowego readbacku |

Tabela nie jest instrukcją migracji. Każdy wiersz oznaczony `CURRENT` wymaga
odczytu wersji OpenClaw i testu na środowisku docelowym przed implementacją.

## 4. Co zachowujemy z wykonanej pracy

| Zachowany materiał | Jak go wykorzystać |
|---|---|
| RBAC, `tenant_id`, agent grants i domyślna odmowa | warstwa domenowa 8gent; mapować nadania na OpenClaw agent/binding, nie przenosić sekretów |
| budżety, `usage_event` i audyt allow/deny/error | adapter telemetryczny do danych OpenClaw; najpierw ustalić, które zdarzenia i koszty są dostępne |
| scenariusze A/R/W/K/P/C/U | niezależny kontrakt akceptacyjny dla 8gent + OpenClaw |
| web-first i serwerowa własność pracy | sprawdzić przez OpenClaw Control UI/Gateway, bez zależności od Desktopu |
| Operator → Evaluator → Defense warunkowo → Final Control | zachować jako proces review; wykonanie przenieść na OpenClaw tasks/Task Flow dopiero po canary |
| readback, receipts, allowlisty i redakcje | zachować jako politykę 8gent i kryteria pluginu |
| klasyfikacja kart, relacje parent/dependency i historia ABM | wykorzystać jako materiał wejściowy do pluginu, nie przepisywać automatycznie do natywnego OpenClaw |
| AutoBot Monitor runbooki i dokumentacja lifecycle | adaptować do pluginu/service, bez osobnego OpenMonitor |
| Hermes-specific profile, Cron, receiver, gateway i Kanban commands | **HISTORY/LEGACY**; nie kopiować jako bieżącej architektury |
| historyczne hashe, raporty i stagingi | **EVIDENCE**; nie zmieniać dla samej migracji platformy |

## 5. Kandydat: wtyczka AutoBot Monitor dla OpenClaw

### 5.1 Cel

Wtyczka ma uzupełnić tylko lukę, której nie pokryją natywne automatyzacje,
tasks i Task Flow. Nie może stać się drugim runtime'em 8gent. Jej pierwsze
zadanie to read-only obserwacja i kontrolowane prowadzenie zatwierdzonego flow,
a nie instalacja, publikacja ani zmiana polityki właściciela.

### 5.2 Możliwy zakres native pluginu

Dokumentacja OpenClaw przewiduje pluginy rejestrujące narzędzia, hooki, serwisy
oraz komendy CLI.[6][7][8] Plugin może też współpracować z Task Flow, jeżeli
potrzebuje wieloetapowej kontroli.[11] Dla AutoBot Monitor rozważamy więc
następujący podział:

- **Task Flow controller:** przechowuje bounded state, bieżący krok, rewizję,
  parent/child task references i wynik przejścia;
- **typed hooks:** obserwują lifecycle agenta, tasków, narzędzi i dostarczenia,
  ale nie omijają autoryzacji ani approval policy;
- **read-only tools/CLI:** pokazują status flow, tasku, runu, artefaktu i
  readbacku bez tworzenia nowej pracy z samego odczytu;
- **service:** wykonuje tylko jawnie zatwierdzone, idempotentne przejście;
- **manifest/config:** definiuje capability ownership i konfigurację pluginu,
  a nie ukryty runtime ani sekrety.

To jest hipoteza architektoniczna, nie stwierdzenie, że plugin już istnieje.
OpenClaw wymaga manifestu `openclaw.plugin.json`, a API pluginów są
eksperymentalne; host version i zgodność trzeba przypiąć przed kodowaniem.[7][8]

### 5.3 Warunki akceptacji pluginu

Plugin nie przechodzi do implementacji, dopóki nie ma dowodu, że:

1. natywny Task Flow nie pokrywa całej potrzebnej kontroli zależności,
   evidence i warunków właścicielskich;
2. automatyzacja OpenClaw może wybudzić proces bez osobnego schedulera;
3. `tasks` i flow zachowują wystarczające `taskId`, `runId`, sesję, agenta,
   owner/requester context oraz stan terminalny;
4. można odróżnić wykonanie, dostarczenie wyniku i akceptację — status `queued`
   ani sam zapis tasku nie jest dowodem ukończenia;
5. instalacja i aktualizacja pluginu przechodzą przez politykę zaufania,
   pinowaną wersję i runtime inspection; `plugins inspect` bez `--runtime` nie
   dowodzi załadowania kodu do działającego Gatewaya.[6][7][8]
6. plugin nie przejmuje RBAC 8gent, nie zapisuje sekretów, nie tworzy drugiego
   model gatewaya i nie wykonuje merge/push/deploy bez osobnej zgody;
7. awaria pluginu jest fail-closed i nie blokuje podstawowego OpenClaw
   Gatewaya ani innych kanałów;
8. istnieje test upgrade/rollback, backup oraz usunięcia pluginu bez utraty
   evidence.

## 6. Automatyzacje i obserwowalność bez OpenMonitor

Pierwszym wyborem są natywne mechanizmy OpenClaw:

- scheduler: `automations` / alias `cron`;
- ewidencja pracy odłączonej: `tasks list`, `tasks show`, `tasks audit`;
- wieloetapowe procesy: `tasks flow` / Task Flow;
- sesje i routing: Gateway + agent bindings;
- diagnostyka: Gateway status, health, doctor, security audit;
- powierzchnia operatora: Control UI, CLI oraz jawnie wybrane kanały.

Nie tworzymy nazwy ani modułu „OpenMonitor”. Jeżeli po readbacku zabraknie
konkretnej funkcji — na przykład wymaganej relacji parent/dependency, polityki
review albo selektywnego dostarczenia — zapisujemy dokładną lukę i dopiero wtedy
rozważamy AutoBot Monitor jako plugin. Nie wolno zamienić ogólnego „monitoringu”
w drugi, nieudokumentowany control plane.

## 7. Modele i dostawcy

`OpenRouter` nie jest elementem wybranej architektury. OpenClaw ma własny model
wyboru provider/model, allowlistę, primary model i fallbacks.[13] W 8gent:

- nie budujemy proxy OpenRouter;
- nie dodajemy OpenRoutera jako ukrytego fallbacku;
- nie zapisujemy kluczy w repozytorium ani w danych 8gent;
- dostawcę, model, auth profile, koszty i retencję wybieramy jawnie przed
  uruchomieniem testu;
- koszt musi być przypisywalny do agenta/użytkownika, ale sposób pobrania
  usage z OpenClaw jest jeszcze `GAP TO VERIFY`.

## 8. Tożsamość, dane i granica bezpieczeństwa

8gent nadal odpowiada za własną domenę: użytkownika, tenant, nadanie, limit,
audyt i decyzję allow/deny. OpenClaw odpowiada za wykonanie agenta, kanał,
sesję, workspace i narzędzia. Granica nie oznacza, że OpenClaw sam rozwiązuje
RBAC 8gent — adapter musi sprawdzić uprawnienie przed przekazaniem żądania.

Jeden Gateway OpenClaw jest jedną granicą zaufania. Przy wielu niezależnych lub
wzajemnie wrogich najemcach nie wolno zakładać, że sam agent ID zapewnia
izolację; trzeba rozważyć osobny Gateway, host lub użytkownika systemowego,
zgodnie z modelem bezpieczeństwa OpenClaw.[12]

Prawidłowa kolejność przed każdym dostępem do danych:

```text
8gent identity/RBAC check
→ OpenClaw agent/binding/session selection
→ OpenClaw tool/approval policy
→ model/provider call
→ usage + audit readback
```

Nie zapisujemy w repozytorium wartości tokenów, haseł, kluczy, auth profiles,
realnych danych osobowych ani connection stringów. W dokumentacji używamy
`[REDACTED]`.

## 9. Plan dalszej pracy

| Faza | Zakres | Status / bramka |
|---|---|---|
| O0 | ten dokument, mapowanie Hermes → OpenClaw, lista luk i reuse | **DONE jako dokumentacja** |
| O1 | read-only inventory aktualnej wersji OpenClaw: Gateway, config, agents, sessions, automations, tasks, flows, plugins | **NEXT; bez instalacji live** |
| O2 | macierz 8gent RBAC/budżet/audyt → OpenClaw API i zdarzenia | **OWNER GATE**, wymaga decyzji o danych i auth |
| O3 | bounded canary jednego agenta, jednej sesji i jednej automatyzacji | **OWNER GATE**, osobny host/config/test |
| O4 | plugin spike AutoBot Monitor wyłącznie po potwierdzeniu luki | **RESEARCH / OWNER GATE** |
| O5 | test wieloagentowy, restart/rollback/backup i security audit | **OWNER GATE** |
| O6 | dopiero po PASS: integracja z repozytorium, publikacja, instalacja lub deploy | **osobna zgoda** |

Żadna faza od O0 do O2 nie instaluje OpenClaw, nie łączy kanałów, nie używa
poświadczeń i nie zmienia runtime.

## 10. Otwarte pytania, których nie rozstrzygam sam

1. Jaka wersja OpenClaw i jaki kanał wydań są dopuszczone na środowisku docelowym?
2. Czy OpenClaw ma być jednym Gatewayem dla wszystkich użytkowników, czy granice
   zaufania wymagają osobnych Gatewayów/hostów?
3. Który provider/model i jaka metoda auth są dopuszczone bez OpenRoutera?
4. Gdzie ma powstać źródło prawdy dla usage, kosztów i audytu 8gent?
5. Czy OpenClaw Task Flow pokrywa wymagany graf parent/dependency oraz evidence,
   czy tę konkretną lukę ma uzupełnić AutoBot Monitor plugin?
6. Które kanały, Control UI i nodes są wymagane w MVP1?
7. Czy istniejące decyzje o Entra/Graph pozostają takie same po zmianie runtime,
   czy trzeba zarejestrować ich nowy zakres i model dostępu?
8. Jaki jest plan migracji treści/modeli/sesji z wcześniejszego prototypu, jeżeli
   kiedykolwiek powstanie taki runtime? Nie zakładamy automatycznej migracji.

## 11. Ranga dokumentów po zmianie platformy

| Ranga | Dokument / źródło | Rola |
|---|---|---|
| 1 | `docs/OPENCLAW-STRATEGY.md` + D-014 | obecna decyzja platformowa i mapowanie |
| 2 | `docs/spec/00-architektura.md` | docelowa architektura 8gent po aktualizacji |
| 3 | `docs/spec/decisions.md` | decyzje właściciela i ich supersession |
| 4 | `NAGENTS-PROJECT.md` | indeks i kolejność czytania |
| 5 | `docs/OPENCLAW-*` / aktualne specyfikacje | uzupełnienia i plany bez live proof |
| 6 | `docs/nota-*`, `HANDOFF-nagents.md`, stare dispatchy i stagingi | historyczny Hermes baseline / evidence |

`CLAUDE.md` i `.claude/skills/nagents-autobot/SKILL.md` zawierają chronione
instrukcje procesu oraz fragmenty Hermes-era. Nie należy brać ich dawnych
założeń platformowych za bieżącą decyzję bez odczytania tego dokumentu i D-014.
Ich ewentualna zmiana jest osobną kontrolą plików instrukcyjnych.

## Sources

[1] https://docs.openclaw.ai — oficjalny przegląd OpenClaw
[2] https://docs.openclaw.ai/concepts/architecture — architektura Gatewaya
[3] https://docs.openclaw.ai/concepts/multi-agent — multi-agent routing
[4] https://docs.openclaw.ai/concepts/session — zarządzanie sesjami
[5] https://docs.openclaw.ai/gateway/configuration — konfiguracja Gatewaya
[6] https://docs.openclaw.ai/docs/plugins — system pluginów
[7] https://docs.openclaw.ai/plugins/building-plugins — budowanie pluginów
[8] https://docs.openclaw.ai/plugins/hooks — hooki pluginów
[9] https://docs.openclaw.ai/automation — automatyzacje
[10] https://docs.openclaw.ai/automation/tasks — background tasks
[11] https://docs.openclaw.ai/automation/taskflow — Task Flow
[12] https://docs.openclaw.ai/gateway/security — bezpieczeństwo
[13] https://docs.openclaw.ai/concepts/models — modele i wybór provider/model
[14] https://docs.openclaw.ai/web/control-ui — Control UI
