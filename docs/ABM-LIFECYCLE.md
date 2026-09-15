# ABM-LIFECYCLE.md — staging cyklu życia AutoBot Monitor

STATUS: PASS (pokrycie dokumentacyjne tematu; nie zgoda live)
PUBLICATION_STATUS: STAGING_ONLY
DOMAIN: INFORMACYJNY
TEMAT: NAG-CONSOLIDATE-ABM-LIFECYCLE-Q1
TASK_ID: t_8a205853
PARENT: t_f9660fa7
ROUND: 1
OBSERVED_AT_UTC: 2026-09-15T00:52:11Z
P4_STATUS: PASS_WITH_EXPLICIT_OWNER_GATES
SOURCE_OF_TRUTH: false

Ten plik jest addytywną, stagingową instrukcją cyklu życia AutoBot Monitor.
Nie zastępuje źródeł kanonicznych, manifestu wydania, bieżącego readbacku ani
zgody właściciela. Nie jest publikacją, instalacją, dowodem działania usługi,
zgodą na restart ani potwierdzeniem stanu profilu Hermesa.

W czasie tego tematu nie wykonano instalacji, odinstalowania, rollbacku,
backup pushu, publikacji, restartu, zmiany profilu, zmiany gatewaya, operacji
na Desktopie ani operacji live. Wszystkie komendy w dalszej części są
procedurą dla właściciela do wykonania dopiero po sprawdzeniu celu i bram.

## 0. Trzy rozdzielone warstwy działania

| Warstwa | Co obejmuje | Kto wykonuje | Stan w tym artefakcie |
|---|---|---|---|
| `OFFLINE_PACKAGE` | Przygotowanie pakietu poza żywym profilem, izolowany venv, testy na fixture, kontrola plików pluginu, patchy i manifestu | właściciel/administrator lub Orkiestrator w zatwierdzonym zakresie | OPISANE, NIE WYKONANE |
| `MANUAL_OWNER_ACTION` | Ręczne uruchomienie komend instalacji, testu demo, kopiowania pluginu, wyłączenia pluginu albo usunięcia offline venv | wyłącznie właściciel/administrator, po potwierdzeniu celu | OPISANE, NIE WYKONANE |
| `LIVE_OWNER_GATE` | Użycie wobec prawdziwego profilu Hermesa, serwera produkcyjnego, realnego katalogu `runs`, gatewaya, uprawnień, publikacji, pushu, merge albo deployu | właściciel po osobnej, jednoznacznej zgodzie i readbacku | NIE OTWARTE W TYM TEMACIE |

`OFFLINE_PACKAGE` nie oznacza „gotowe do użycia live”. `MANUAL_OWNER_ACTION`
nie przenosi zgody na inne środowisko. `LIVE_OWNER_GATE` wymaga dokładnego
celu, wersji, bazowego SHA, listy plików, manifestu, testów i readbacku.

## 1. Zasady nadrzędne i zakres

1. AutoBot Monitor jest read-only monitorem. Pakiet nie zatrzymuje, nie zabija,
   nie tworzy, nie wysyła, nie stosuje, nie scala, nie pushuje, nie wdraża,
   nie archiwizuje i nie anuluje pracy live.
2. Serwerowy backend oraz plugin Desktopu są dwiema częściami pakietu. Backend
   pracuje w izolowanym środowisku Python; plugin jest pojedynczym modułem ESM
   bez kroku budowania.
3. Instalacja offline pozostaje poza żywymi profilami Hermesa. Skonfigurowanie
   realnego katalogu runów, uruchomienie usługi, zmiana uprawnień, restart
   gatewaya i włączenie produkcyjnego pluginu wymagają osobnej zgody.
4. `AUTOBOT-KANBAN.md` pozostaje kanonicznym kontraktem AutoBot Monitor.
   `AGENTS.md` wyznacza granice bezpieczeństwa i akceptacji. Ta księga jest
   mapą procedury, nie nową normą ani drugim dispatcherem.
5. Statyczny dokument, raport `PASS`, nazwa worktree, `queued`, `delivered`,
   status UI, obecność pliku, lokalny hash lub wyjście procesu nie dowodzą
   instalacji, żywej usługi, dostarczenia ani poprawności runtime.
6. Źródła AutoBot Monitor i nAgents są osobnymi projektami. Nie przenosić
   kart, profili, danych, patchy ani runtime między nimi.

## 2. LIFE-01 — bezpieczna instalacja i wymagania

Status: `SOURCE + STAGING_ONLY`; live install: `OWNER_DECISION_REQUIRED`.

### Źródła, statusy i exact locatory

| Source ID | Źródło | Status źródła | Exact locator |
|---|---|---|---|
| `SRC-ABM-INSTALL` | `AutoBot-Monitor/docs/INSTALL.md` | P4 `PF-0018 / SOURCE`; `PRESENT_BOTH`, `FRESH/RECENT` | §1 lines 1–11: manualny charakter i brak live install/restart; §1 lines 13–59: server backend, venv, testy, demo i real-run boundary; §2 lines 62–111: plugin Desktopu, copy, opt-in, reload i verify; §3 lines 113–129: read-only tmux; §4 lines 131–143: działania wyłączone |
| `SRC-ABM-V2-PACKAGE` | `AutoBot-Monitor/docs/V2-PACKAGE.md` | P4 `PF-0022 / SOURCE`; `PRESENT_BOTH`, `FRESH/RECENT` | lines 1–11: read-only/offline server boundary; lines 13–19: Desktop plugin i owner approval; lines 21–23: uninstall/rollback; lines 25–31: capability i redaction boundary |
| `SRC-ABM-AGENTS` | `AutoBot-Monitor/AGENTS.md` | P4 `PF-0001 / CANONICAL` | §Safety lines 50–61: read-only, brak sekretów, allowlisted roots i `DECISION_REQUIRED` przed live install/restart; §Acceptance criteria D–E lines 87–96: pakiet i brak live install |
| `SRC-ABM-KANBAN` | `AutoBot-Monitor/AUTOBOT-KANBAN.md` | P4 `PF-0002 / CANONICAL` | lines 44–49: Kanban jako źródło routingu; lines 292–338: serwerowość, Desktop jako klient i live canary; lines 371–447: terminalny event i readback |
| `SRC-NAG-PLAN` | `nAgents/NAGENTS-CONSOLIDATION-PLAN.md` | `PLAN_ONLY / OWNER_HOLD_REQUIRED`; nie jest rodziną P4 | §4.10 lines 191–199: LIFE-01 i bramka live; §8 lines 310–327: addytywność i brak operacji live |
| `SRC-P4` | `nAgents/docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json` | `P4_AUDIT_ARTIFACT` | `#/logical_group_rollups[group_id=ABM-LIFECYCLE]`; `#/path_family_classifications[path_family_id=PF-0018]` i `PF-0022` |

### `OFFLINE_PACKAGE`

1. Ustal zatwierdzony katalog projektu pakietu i trzymaj go poza żywymi
   profilami Hermesa. Nie wykonuj instalacji systemowej ani nie zapisuj do
   systemowego `site-packages`.
2. Utwórz izolowane środowisko Python w katalogu projektu i zainstaluj
   zależności pakietu wyłącznie w tym środowisku. Procedura źródłowa używa:

   ```bash
   cd /root/projects/Hermes-AutoBot-Monitor
   python3 -m venv .venv
   .venv/bin/pip install -e ".[test]"
   .venv/bin/python -m pytest -q
   ```

   Ścieżka projektu jest przykładem z dokumentacji i przed użyciem musi zostać
   potwierdzona przez właściciela. Granicą jest `.venv`; nie używać globalnego
   interpretera do instalacji zależności.
3. Testy uruchamiaj przed poleganiem na pakiecie. Zielony wynik testów offline
   potwierdza tylko ten checkout i te fixture; nie potwierdza live profilu,
   gatewaya ani realnego katalogu runów.
4. Demo serwera, jeśli właściciel je uruchomi, musi być lokalne i odseparowane
   od prawdziwych runów:

   ```bash
   tmux new-session -d -s autobot-monitor-demo \
     '.venv/bin/python -m server.demo_server --host localhost --port 8765'
   curl -s http://localhost:8765/api/runs | python3 -m json.tool
   ```

   To jest test demo, nie instalacja produkcyjna. Nie zmieniać `--host`, nie
   podpinać realnego `runs/` rootu i nie traktować działającego demo jako
   dowodu integracji z Hermesem.
5. Plugin Desktopu pozostaje plikiem źródłowym `desktop_plugin/hermes-autobot-monitor/plugin.js`.
   Sprawdzenie plain ESM i brak kroku JSX/build jest kontrolą pakietu, nie
   rejestracją w działającym profilu.

### `MANUAL_OWNER_ACTION`

1. Właściciel potwierdza wersję pakietu, exact SHA plików, docelowy katalog oraz
   to, że katalog nie jest żywym profilem Hermesa.
2. Właściciel może skopiować cały katalog `hermes-autobot-monitor`, nie tylko
   `plugin.js`, do zatwierdzonego folderu unified pluginów Windows, np.:

   ```text
   %APPDATA%\Hermes\plugins\hermes-autobot-monitor\
   ```

   Dokładna ścieżka zależy od wersji i profilu Desktopu. Kopiowanie jest
   ręczną czynnością właściciela i nie jest wykonywane przez ten temat.
3. Po kopiowaniu właściciel może ręcznie włączyć plugin w ustawieniach
   rozszerzeń i wykonać reload pluginu. Reload dotyczy procesu UI Desktopu;
   nie jest restartem gatewaya.
4. Właściciel sprawdza, czy widać pane AutoBot Monitor, status chip i czy
   plugin nie oferuje stop/kill/write/merge/push/deploy. Przy braku backendu
   oczekiwany jest stan loading/empty, nigdy zmyślony stan runu.
5. Opcjonalny podgląd terminala musi używać odczytu tylko do odczytu:

   ```bash
   tmux attach-session -t <tmux-session-name> -r
   ```

   Flaga `-r` jest obowiązkowa. Odłączenie wykonuje się standardową sekwencją
   `Ctrl-b d`; API, collector i plugin nie uruchamiają tego polecenia.

### `LIVE_OWNER_GATE`

Przed użyciem wobec prawdziwego profilu, produkcyjnego serwera, realnego
`runs/` rootu albo działającego gatewaya właściciel musi osobno zatwierdzić
konkretny target, wersję, manifest, SHA, uprawnienia, sposób rollbacku i okno
zmiany. `AGENTS.md` nakazuje zatrzymać się jako `DECISION_REQUIRED` przed live
installation, gateway restart, permission broadening, publication i destructive
cleanup. Ten artefakt tej bramy nie otwiera.

## 3. LIFE-02 — upgrade, backup i polityka GitHub

Status: `SOURCE + STAGING_ONLY`; backup/push/publication: `OWNER_GATE`.

### Źródła, statusy i exact locatory

| Source ID | Źródło | Status źródła | Exact locator |
|---|---|---|---|
| `SRC-ABM-UPGRADE` | `AutoBot-Monitor/docs/UPGRADE-BACKUP-AND-GITHUB-POLICY.md` | P4 `PF-0021 / SOURCE`; `LOCAL_ONLY`, `FRESH`, `NO_REMOTE_PATH` | §Cel lines 1–19: zakres odtworzenia; §AutoBot Monitor lines 21–34: prywatne repo, jawna lista i zakaz hurtowego add; §Czego nigdy lines 52–67: sekrety, runtime i `hermes backup`/`hermes import`; §Manifest lines 69–107: wymagany inventory; §Procedura lines 109–134: kolejność przed/po aktualizacji; §Odtworzenie lines 136–145: patch i restore; §Bramy lines 147–154: podział odpowiedzialności |
| `SRC-ABM-KANBAN` | `AutoBot Monitor/AUTOBOT-KANBAN.md` | P4 `PF-0002 / CANONICAL` | lines 340–369: receipts i idempotencja; lines 371–484: readback/fail-closed/watchdog; lines 592–601: GitHub, patch i rozdzielenie push/merge/install/restart |
| `SRC-ABM-AGENTS` | `AutoBot Monitor/AGENTS.md` | P4 `PF-0001 / CANONICAL` | lines 41–48: backup i ciągłość GitHub; lines 50–61: zakazy bezpieczeństwa i owner gates |
| `SRC-NAG-PLAN` | `nAgents/NAGENTS-CONSOLIDATION-PLAN.md` | `PLAN_ONLY / OWNER_HOLD_REQUIRED` | §6.4 lines 264–277: owner gates; §8 lines 310–327: bezpieczna kolejność; §9 lines 329–336: rollback P6 i brak publikacji |
| `SRC-P4` | `nAgents/docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json` | `P4_AUDIT_ARTIFACT` | `#/logical_group_rollups[group_id=ABM-LIFECYCLE]`; `#/path_family_classifications[path_family_id=PF-0021]` |

### Co zawiera manifest

Manifest jest jawnie wybranym spisem odtworzeniowym, a nie kopią sekretów ani
pełnego stanu konta. Powinien rozdzielać co najmniej:

| Część | Wymagane bezpieczne dane |
|---|---|
| Stan wersji | `hermes --version`, upstream `git rev-parse HEAD`, `git remote -v` bez sekretów, wersja/SHA Desktopu, `git status --short`, worktree/procesy jako metadane, board/task IDs/profile/fazy |
| AutoBot Monitor | `AGENTS.md`, plany, backend, server, `desktop_plugin`, `package`, testy, docs, lockfile oraz zsanityzowane runy tylko po readbacku |
| Lokalne łaty Hermesa | jawne patch files, bazowy upstream SHA, pliki/testy potrzebne do potwierdzenia łaty, oczekiwany wynik testów |
| Dowód | SHA-256 wybranych plików, wersja, status, zakres allowlisty, wynik testów i ścieżka raportu |

Nie wpisywać do manifestu wartości środowiskowych, tokenów, haseł, kluczy,
pełnych baz sesji, surowych transcriptów ani niesanitizowanych snapshotów.
Lista procesów, worktree i kart ma być metadanymi, nie ich treścią.

### `OFFLINE_PACKAGE`

1. Przygotuj manifest dla dokładnie wybranych plików oraz aktualnego bazowego
   SHA. Nie obejmuj całego brudnego checkoutu.
2. Zapisz lokalną zmianę Hermesa jako patch file poza checkoutem Hermesa,
   w prywatnym repozytorium AutoBot Monitor. Patch musi wskazywać bazowy
   upstream SHA i oczekiwany wynik po nałożeniu.
3. Sprawdź źródło, manifest, package i patch na fixture. Uruchom przewidziane
   testy, `node --check`, `compileall` i `git diff --check`, jeśli należą do
   wybranego zakresu. Brak testu albo brak hasha jest luką, nie domyślnym PASS.

### `MANUAL_OWNER_ACTION`

Przed aktualizacją właściciel/Orkiestrator wykonuje w tej kolejności:

1. zatrzymuje nowe dispatchowanie albo zapisuje kontrolowany punkt backupu;
   nie przerywa aktywnego workera bez osobnej decyzji;
2. odczytuje boardy, aktywne taski, profile, worktree i gateway status;
3. skanuje sekrety tylko na dokładnie wybranych plikach;
4. buduje manifest wersji i diffu;
5. zapisuje patch Hermesa poza jego checkoutem;
6. wybiera i commituję tylko jawnie wskazane pliki do prywatnej gałęzi backupu;
7. po autoryzacji pushuje backup do prywatnego GitHuba i odczytuje z powrotem
   URL, branch, SHA, listę plików, status i prywatną widoczność;
8. dopiero po pozytywnym readbacku aktualizuje serwer lub Desktop.

W punkcie 6 forma działania pozostaje allowlist-only; nigdy `git add -A` ani
`git add .`. W tym staging tasku żaden z tych kroków nie został uruchomiony.

### `LIVE_OWNER_GATE`

Push backupu, publikacja, merge, deploy, aktualizacja live i restart gatewaya
są osobnymi operacjami. Właściciel zatwierdza każdą z nich osobno, z exact
branch/targetem i readbackiem. Commit lub push nie oznacza merge, instalacji
live ani restartu gatewaya.

## 4. LIFE-03 — odinstalowanie i odwrócenie

Status: `SOURCE + STAGING_ONLY`; prywatny runtime nie jest czyszczony domyślnie.

### Źródła, statusy i exact locatory

| Source ID | Źródło | Status źródła | Exact locator |
|---|---|---|---|
| `SRC-ABM-UNINSTALL` | `AutoBot-Monitor/docs/UNINSTALL.md` | P4 `PF-0020 / SOURCE`; `PRESENT_BOTH`, `FRESH/RECENT` | lines 1–5: ręczny zakres; §1 lines 7–26: Desktop off/reload/close/delete; §2 lines 28–49: jawne zatrzymanie demo, venv/run artifacts i pozostawienie źródeł; §3 lines 51–59: brak gateway/live cleanup |
| `SRC-ABM-V2-PACKAGE` | `AutoBot-Monitor/docs/V2-PACKAGE.md` | P4 `PF-0022 / SOURCE` | lines 21–23: offline package removal only, zachowanie evidence i zakaz destructive live cleanup |
| `SRC-ABM-INSTALL` | `AutoBot-Monitor/docs/INSTALL.md` | P4 `PF-0018 / SOURCE` | §2 lines 84–111: lifecycle toggle/reload/verify; §3 lines 113–129: read-only tmux; §4 lines 131–143: brak live side effects |
| `SRC-ABM-AGENTS` | `AutoBot-Monitor/AGENTS.md` | P4 `PF-0001 / CANONICAL` | lines 50–61: brak stop/kill/write i `DECISION_REQUIRED` przed destructive cleanup |
| `SRC-ABM-KANBAN` | `AutoBot-Monitor/AUTOBOT-KANBAN.md` | P4 `PF-0002 / CANONICAL` | lines 371–447: evidence/readback przed rozstrzygnięciem; lines 479–484: zakaz reset/stash/clean/pull i live install |
| `SRC-NAG-PLAN` | `nAgents/NAGENTS-CONSOLIDATION-PLAN.md` | `PLAN_ONLY / OWNER_HOLD_REQUIRED` | §9 lines 329–336: odwracalność, zachowanie oryginału i osobna zgoda na usunięcie |
| `SRC-P4` | `nAgents/docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json` | `P4_AUDIT_ARTIFACT` | `#/path_family_classifications[path_family_id=PF-0020]` |

### `OFFLINE_PACKAGE`

1. Najpierw zachowaj raporty, manifest i zsanityzowane evidence. Usunięcie
   venvu albo skopiowanego pakietu nie może usunąć jedynego dowodu.
2. Offline uninstall usuwa tylko skopiowany katalog pakietu i jego izolowane
   środowisko. Źródłowy checkout, `backend/`, `server/`, `desktop_plugin/`,
   `docs/` i `package/` pozostają nietknięte.
3. Nie usuwać repozytorium, historii Git, żywego katalogu runów, gateway state,
   profilu Hermesa ani chmurowych zasobów jako „sprzątania pakietu”.

### `MANUAL_OWNER_ACTION`

Dla offline Desktopu właściciel wykonuje kolejno:

1. otwiera Hermes Desktop;
2. przechodzi do ustawień pluginów/extensions;
3. wyłącza `hermes-autobot-monitor`;
4. przeładowuje ustawienia pluginu albo Desktop, aby unregister nastąpił;
5. zamyka Desktop i dopiero wtedy usuwa skopiowany folder:

   ```text
   %APPDATA%\Hermes\plugins\hermes-autobot-monitor\
   ```

Dla offline demo/backendu właściciel najpierw zatrzymuje proces, a nie zabija
sesję w ciemno:

```bash
tmux send-keys -t autobot-monitor-demo C-c
tmux kill-session -t autobot-monitor-demo
rm -rf /root/projects/Hermes-AutoBot-Monitor/.venv
rm -rf /root/projects/Hermes-AutoBot-Monitor/runs/demo
```

Przed `rm -rf` właściciel musi potwierdzić exact katalog i to, że jest offline.
Powyższe polecenia są tylko cytowaną procedurą ręczną; nie zostały wykonane.

### `LIVE_OWNER_GATE`

Odinstalowanie z żywego profilu Desktopu, usunięcie live backendu, zatrzymanie
usługi, dotknięcie gateway state, usunięcie realnych runów lub odwrócenie
uprawnień wymaga osobnej decyzji właściciela, zakresu allowlisty i readbacku.
Automatyczny worker nie wykonuje tych czynności.

## 5. LIFE-04 — package, patch, manifest i walidacja

Status: `SOURCE + CONSOLIDATION_CANDIDATE`; `NO_LIVE_INSTALL`.

### Źródła, statusy i exact locatory

| Source ID | Źródło | Status źródła | Exact locator |
|---|---|---|---|
| `SRC-ABM-V2-PACKAGE` | `AutoBot-Monitor/docs/V2-PACKAGE.md` | P4 `PF-0022 / SOURCE`; `PRESENT_BOTH`, `FRESH/RECENT` | lines 1–3: pakiet read-only i zakaz operacji; lines 5–11: server offline install; lines 13–19: plugin Desktopu i owner gate; lines 21–23: uninstall/rollback |
| `SRC-ABM-UPGRADE` | `AutoBot-Monitor/docs/UPGRADE-BACKUP-AND-GITHUB-POLICY.md` | P4 `PF-0021 / SOURCE` | lines 21–50: prywatny kod, jawne pliki i patch; lines 52–67: wyłączenia; lines 69–107: manifest; lines 122–145: po aktualizacji i restore |
| `SRC-ABM-AGENTS` | `AutoBot-Monitor/AGENTS.md` | P4 `PF-0001 / CANONICAL` | lines 25–28: rozdział backend/pluginu i brak live restart; lines 50–61: safety; lines 87–96: package/verification |
| `SRC-ABM-KANBAN` | `AutoBot-Monitor/AUTOBOT-KANBAN.md` | P4 `PF-0002 / CANONICAL` | lines 119–148: wymagane fields i allowlista; lines 283–290: integration gate; lines 592–601: patch/backup/push boundary |
| `SRC-NAG-PLAN` | `nAgents/NAGENTS-CONSOLIDATION-PLAN.md` | `PLAN_ONLY / OWNER_HOLD_REQUIRED` | §4.10 lines 191–199: LIFE-04 criteria; §5 lines 213–217: ABM-PACKAGE/ABM-INTEGRATION; §6.4 lines 271–277: owner gate; §8 lines 318–327: release/review sequence |
| `SRC-P4` | `nAgents/docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json` | `P4_AUDIT_ARTIFACT` | `#/logical_group_rollups[group_id=ABM-PACKAGE]`; `#/logical_group_rollups[group_id=ABM-INTEGRATION]`; `#/path_family_classifications[path_family_id=PF-0022]` |

### Rozdzielenie pojęć

| Pojęcie | Granica |
|---|---|
| `package` | Read-only bundle monitora: server Python backend oraz native Desktop plugin. Nie jest gatewayem, profilem Hermesa, dispatcherem ani narzędziem stop/kill/write/merge/push/deploy. README lub lokalny ZIP nie nadaje zgody live. |
| `patch` | Jawny plik lokalnej poprawki Hermesa przechowywany w prywatnym repozytorium AutoBot Monitor wraz z bazowym upstream SHA. Patch nie jest automatycznie nakładany, nie zastępuje manifestu i nie jest wysyłany do upstreamu Nous Research. |
| `manifest` | Kontrolowany inventory wersji, bazowego SHA, wybranych plików, hashy, testów, statusu i instrukcji odtworzenia. Manifest nie zawiera sekretów, pełnego state, sesji ani nie jest samą zgodą na zmianę. |

Package, patch i manifest są odrębnymi artefaktami. Package opisuje co można
przygotować offline; patch opisuje lokalną różnicę względem upstreamu; manifest
wiąże wersję, zakres i odtworzenie. Żaden z nich sam nie uruchamia profilu,
zmienia gatewaya ani nie otwiera bramy publikacji.

### `OFFLINE_PACKAGE`

Walidacja kandydata wydania:

1. odczytaj wersję i exact SHA pakietu oraz bazowy upstream SHA patcha;
2. porównaj manifest z rzeczywistymi bajtami wybranych plików;
3. zweryfikuj deklarowane zależności w izolowanym venvie;
4. sprawdź plain ESM pluginu bez JSX/build, a gdy należy do zakresu, wykonaj
   `node --check` i `compileall`;
5. uruchom testy i `git diff --check`;
6. zapisz sanitized evidence i wynik, nie surowy log lub środowisko.

### `MANUAL_OWNER_ACTION`

P4 klasyfikuje `ABM-PACKAGE` jako `CONSOLIDATION_CANDIDATE` (1 rodzina, 2
rekordy) oraz `ABM-INTEGRATION` jako `SOURCE` (1 rodzina, 1 rekord). Nie wolno
wybrać wariantu lokalny/remote przez nazwę, długość, mtime, samą obecność na
GitHubie ani README. Właściciel/release reviewer musi porównać exact pliki,
manifest, bazowy SHA i testy; dodatkowe README package/patch są wejściem
późniejszego release review, nie dowodem tego stagingu.

Po pozytywnym review można ręcznie przygotować offline bundle i manifest.
Przy konflikcie patcha trzeba zatrzymać się jako `DECISION_REQUIRED` albo
`INFRA`; nie używać `git reset --hard`, `git clean`, hurtowego `git add` ani
cichego fallbacku.

### `LIVE_OWNER_GATE`

Nałożenie patcha na aktywną instalację, wybór aktywnego bundle, publikacja,
push, merge, deploy, instalacja w profilu live lub restart gatewaya wymaga
osobnej zgody właściciela i osobnego readbacku. `INTEGRATION_REQUIRED` jest
bramą Orkiestratora, nie zgodą na push, merge, deploy ani live install.

## 6. LIFE-05 — live proof i recovery

Status: `OWNER_GATE + LIVE_READBACK_REQUIRED`; w tym temacie `NOT_PROVEN`.

### Źródła, statusy i exact locatory

| Source ID | Źródło | Status źródła | Exact locator |
|---|---|---|---|
| `SRC-NAG-PLAN` | `nAgents/NAGENTS-CONSOLIDATION-PLAN.md` | `PLAN_ONLY / OWNER_HOLD_REQUIRED` | §4.10 lines 199: osobny test i readback; §6.4 lines 271–277: live canary, manifest/hash/base/test gate; §7 lines 298–300: `LIVE_READBACK_REQUIRED`; §8 lines 312–327: live readback i osobne bramy |
| `SRC-ABM-AGENTS` | `AutoBot-Monitor/AGENTS.md` | P4 `PF-0001 / CANONICAL` | lines 50–61: live install/restart/publication/destructive cleanup jako owner gate; lines 87–96: verification and no-push/no-deploy |
| `SRC-ABM-KANBAN` | `AutoBot-Monitor/AUTOBOT-KANBAN.md` | P4 `PF-0002 / CANONICAL` | lines 371–447: terminal event + technical/contextual readback; lines 454–484: watchdog, fail-closed i zakaz live install/restart; lines 592–601: patch/push/install separation |
| `SRC-ABM-INSTALL` | `AutoBot-Monitor/docs/INSTALL.md` | P4 `PF-0018 / SOURCE` | §4 lines 131–143: brak live installation, restart, push/deploy/publication; §2 lines 100–111: verify i loading/empty boundary |
| `SRC-ABM-V2-PACKAGE` | `AutoBot-Monitor/docs/V2-PACKAGE.md` | P4 `PF-0022 / SOURCE` | lines 21–31: rollback/capability state and bounded provider evidence |
| `SRC-P4` | `nAgents/docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json` | `P4_AUDIT_ARTIFACT` | `#/logical_group_rollups[group_id=ABM-LIFECYCLE]`; `#/path_family_classifications[path_family_id=PF-0018]`, `PF-0020`, `PF-0021`, `PF-0022` |

### Wymagany, ale niewykonany live proof

Jeżeli właściciel później otworzy tę bramę, dowód musi rozdzielać procedurę od
wyniku i obejmować:

1. jednoznaczną zgodę na exact środowisko, target, wersję, package SHA, patch
   base SHA, manifest i zakres uprawnień;
2. świeży readback projektu, profilu, boardu, workspace, gatewaya, usługi,
   Desktopu i realnego katalogu runów; nie używać starego raportu jako stanu;
3. osobny readback instalacji/upgrade/uninstall/rollback, zamiast wnioskowania
   z samego procesu lub UI;
4. test backendu, pluginu, odczytu API, sanitizacji, path allowlist i
   ewentualnego read-only tmux na zatwierdzonym celu;
5. po aktualizacji: sprawdzenie upstream SHA, nałożenia patcha, wersji,
   aktywnego bundle i testów regresji;
6. po autoryzowanym pushu: readback remote branch i SHA; push nadal nie jest
   merge/deploy/install;
7. po porażce: zachowanie raportu, manifestu, patcha, hashy, receiptów i
   eventów; zatrzymanie wadliwej ścieżki zamiast cichego retry/fallbacku.

Wymagany test obejmuje także potwierdzenie, że zamknięcie klienta nie zatrzymuje
serwerowej pracy, jeżeli ta funkcja jest objęta danym live wdrożeniem. Samo
„połączono”, `last_status: ok`, `delivered` albo obecność pliku nie jest takim
dowodem.

### Recovery

1. Zachowaj evidence i nie nadpisuj wcześniejszego manifestu ani patcha.
2. Odczytaj manifest ostatniego zatwierdzonego backupu.
3. Odtwórz prywatne repozytorium AutoBot Monitor, następnie upstream Hermesa
   na zapisanym SHA.
4. Nałóż patch files w zapisanej kolejności, po sprawdzeniu bazowego SHA.
5. Uruchom testy, sprawdź wersję i bundle, a wynik zapisz jako sanitized
   evidence.
6. Stan Hermesa przywracaj wyłącznie przez wspierane `hermes backup`/
   `hermes import` z osobnego szyfrowanego backupu.
7. Nigdy nie kopiuj ręcznie `state.db`, `.env`, `auth.json`, sesji ani całych
   profili. Nie używaj `git reset --hard` ani `git clean` na brudnym checkoutcie.
8. Każdy konflikt, nieznany target, brak receiptu, brak eventu, zły profil,
   rozjazd hashy lub brak dowodu kończy się `INFRA`/`UNKNOWN` albo
   `DECISION_REQUIRED`, a nie domysłem.

## 7. P4, proweniencja i bieżący status

P4 ma status `PASS_WITH_EXPLICIT_OWNER_GATES`. Globalny audit obejmuje 254
rodziny ścieżek, 833 rekordy i 272 unikalne SHA-256. Rodzina logiczna
`ABM-LIFECYCLE` ma status `SOURCE`, relację „install/upgrade/uninstall/backup
policy separate from runtime evidence”, 4 rodziny ścieżek i 7 rekordów. Jej
unikalna treść P4 brzmi: „Manual safe install/uninstall, rollback and
backup/GitHub boundaries with no live mutation.” Ryzyko: „Automatic
install/restart or accidental overwrite of private state.” Rekomendacja P4:
„Keep as separate lifecycle package; reconcile with package README and current
manifest during P6, never install in P4.”

| Path family | Source ID / obecność | P4 status | Preferred SHA-256 | Bieżący readback |
|---|---|---|---|---|
| `PF-0018` `docs/INSTALL.md` | `ABM_CHECKOUT`, `GITHUB_ABM_MAIN_REF`; `PRESENT_BOTH` | `SOURCE` | `ae368ee27414642bdf95c4a007f7b15415e3cd6182dfc26f2615a7b8a360fd94` | ten sam SHA; 5241 B / 142 linii |
| `PF-0020` `docs/UNINSTALL.md` | `ABM_CHECKOUT`, `GITHUB_ABM_MAIN_REF`; `PRESENT_BOTH` | `SOURCE` | `05800e661d539f4a8b2f6927085aef49838826b2e9a0c03b054d92c2c9225f7c` | ten sam SHA; 2126 B / 59 linii |
| `PF-0021` `docs/UPGRADE-BACKUP-AND-GITHUB-POLICY.md` | `ABM_CHECKOUT`; `LOCAL_ONLY` | `SOURCE` | `c97b8ab439539939b5a2039482d7ccc325de3ca08be4810a438dddc8834a9eaf` | ten sam SHA; 6327 B / 154 linii |
| `PF-0022` `docs/V2-PACKAGE.md` | `ABM_CHECKOUT`, `GITHUB_ABM_MAIN_REF`; `PRESENT_BOTH` | `SOURCE` | `09acd9916d7eebd3ce6b9166f719b1e451fa0002709fcfad300457875bb15928` | ten sam SHA; 2559 B / 31 linii |

Szczegółowa maszyna P4, w tym `source_ids`, `record_count`, warianty hash,
`unique_content`, ryzyko, rekomendacja i locatory JSON, jest zachowana w
`coverage.json`. Ten tekst nie scala rekordów exact-hash i nie wybiera wariantu
lokalny/remote.

## 8. Granice tego stagingu i następna bramka

Wykonano wyłącznie addytywny zapis trzech plików w katalogu stagingowym tego
tematu. Nie zmieniono źródeł AutoBot Monitor, źródeł nAgents, kart, runów,
profili, boardu, Crona, receivera, gatewaya, Desktopu ani GitHuba.

Następna bramka: niezależny Evaluator ma odczytać księgę i `coverage.json`,
sprawdzić LIFE-01..05, exact hashy, locatory, P4 counts, rozdzielenie
`OFFLINE_PACKAGE`/`MANUAL_OWNER_ACTION`/`LIVE_OWNER_GATE`, zakaz sekretów i
`state.db`, oraz brak twierdzenia o wykonaniu live. Defense powstaje tylko przy
ponumerowanych zarzutach. Final Control rozstrzyga zarzuty; dopiero potem
możliwy jest lokalny P7/readback. Publikacja, integracja, push, merge, deploy,
instalacja, odinstalowanie i rollback live pozostają osobnymi bramami.

DEPLOY/PUSH: NIE WYKONANO
