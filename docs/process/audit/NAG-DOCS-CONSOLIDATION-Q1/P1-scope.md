# NAG-DOCS-CONSOLIDATION-Q1 — P1 zakres audytu

STATUS: PASS
DOMAIN: INFORMACYJNY
TEMAT: NAG-DOCS-P1-SCOPE-Q1
GOAL: Jednoznacznie wskazać repozytoria, worktree, handoffy, refy GitHuba i wyłączenia dla konsolidacji dokumentacji nAgents.
OBSERVED_AT: 2026-09-14T13:39:16.409760+00:00

## Wynik

Zakres został podzielony na rozłączne granice logiczne: fizyczne korzenie,
wyjątkowo wyłączone podkorzenie/pliki oraz jednoznaczne refy zdalne. Pełny
maszynowy zapis źródeł, granic, statusów Git, worktree, refów i
liczników jest w `P1-sources.json`; wyłączenia i ich rola są w
`P1-exclusions.md`.

Jednostką audytu jest:

```text
source_id + physical_root albo (repository, ref) + relative_path + timestamp odczytu
```

Sama nazwa pliku nie nadaje mu proweniencji. Tam, gdzie fizyczny korzeń zawiera
wyłączone podkorzenie (checkout i `.worktrees`, handoffy i osobny handoff
The-Game, checkout i katalog wynikowy P1), wyłączenie rodzica jest jawne i
wygrywa z ogólnym wzorcem włączenia. Ten sam względny path w dwóch
worktree albo refach oznacza dwa różne snapshoty do porównania, nie dwa źródła
tego samego odczytu. Każdy plik w zakresie dostaje dokładnie jeden `source_id`:
korzeń główny wyłącza zagnieżdżone worktree, worktree są partycjonowane osobno,
a pliki GitHuba są kluczowane przez repozytorium i ref.

## Źródła objęte zakresem

| Source ID | Korzeń / ref | Granica | Rola |
|---|---|---|---|
| `NAGENTS_CHECKOUT` | `/home/ubuntu/projects/nAgents-readonly` | Markdown projektu; kod i konfiguracja wyłącznie jako kontekst | główny lokalny snapshot nAgents |
| `NAGENTS_AUX_WORKTREES` | 10 zarejestrowanych worktree nAgents | Markdown każdego wskazanego korzenia; bez scalania treści w P1 | proweniencja branchy i kandydaci do porównania |
| `HANDOFFS_NAGENTS` | `/home/ubuntu/handoffs` | bezpieczne handoffy i jawne metadane JSON; bez prywatnego runtime | historia i materiały referencyjne nAgents |
| `ABM_CHECKOUT` | `/home/ubuntu/projects/Autoboot-Monitor` | dokumentacja operacyjna, raporty i wybrane sanitizowane evidence; kod tylko jako kontekst | osobny projekt towarzyszący AutoBot Monitor |
| `ABM_ACTIVE_RELAY_WORKTREE` | `/home/ubuntu/projects/Autoboot-Monitor/.worktrees/t_771608cd` | dokumenty Cron/relay, raporty/evidence tematu relayu oraz wskazany kod/testy; tylko odczyt | istotny snapshot serwerowego pomocnika i granicy owner-chat |
| `ABM_AUX_WORKTREES_METADATA_ONLY` | pozostałe 35 pomocniczych worktree ABM | wyłącznie path, branch, HEAD, status i licznik Markdown | historia/proweniencja; treść nie wchodzi do P1 |
| `GITHUB_NAGENTS_WORK_REF` | `maciejsieracki/nAgents`, `refs/heads/claude/git-connection-9sz6dg` | dokładne drzewo zdalnego refu | zdalny snapshot gałęzi pracy |
| `GITHUB_NAGENTS_MAIN_REF` | `maciejsieracki/nAgents`, `refs/heads/main` | dokładne drzewo zdalnego refu | zdalny punkt porównania |
| `GITHUB_ABM_MAIN_REF` | `maciejsieracki/Autoboot-Monitor`, `refs/heads/main` | dokładne drzewo zdalnego refu | zdalny punkt porównania projektu towarzyszącego |
| `P1_OUTPUTS` | `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/` | dokładnie trzy artefakty tej fazy; nie są wejściem do własnego skanu | wynik P1 |

AutoBot Monitor pozostaje osobnym repozytorium i namespace'em. Jego dokumenty
nie stają się automatycznie źródłem prawdy nAgents. The-Game pozostaje osobnym
projektem i jest tylko odnotowany jako wyłączenie/separate project.

## Odczyt lokalny i zdalny

- Checkout nAgents: branch `claude/git-connection-9sz6dg`, HEAD
  `f5a91010f3c4655bd85e8a2c0c536922a937935b`; przed utworzeniem artefaktów
  `git status` wykazał 8 zmienionych/ustawionych plików i 3 untracked, 51
  Markdownów w drzewie (48 śledzonych, 3 lokalne).
- nAgents ma 11 zarejestrowanych worktree łącznie z checkoutem; 10 pomocniczych,
  z czego jeden (`wt-NAG-MVP1-009-wdrozenie`) był dirty. Wszystkie zostały tylko
  odczytane.
- Checkout AutoBot Monitor: branch `main`, HEAD
  `c44c381c2deaabccec15686494eb4c63bf54394d`; przed artefaktami 11 zmian
  śledzonych i 3266 untracked. Po wyłączeniu `.worktrees`, standardowych
  artefaktów i `runs/**/*-tmp` pozostaje 160 Markdownów dokumentacji/raportów;
  bez wyłączenia katalogów tymczasowych było 198. W zwykłej granicy odnotowano
  53 surowe pliki logów — ich treści nie wchodzą do audytu.
- AutoBot Monitor ma 37 zarejestrowanych worktree łącznie z checkoutem; 36
  pomocniczych. Tylko `t_771608cd` ma treść w zakresie P1; pozostałe są
  metadata-only. Aktywny snapshot relayu był dirty (14 zmian śledzonych,
  74 untracked) i nie był modyfikowany.
- `/home/ubuntu/handoffs/` zawiera 39 obsługiwanych plików (36 Markdownów i 3
  JSON): 37 ma status historii/referencji nAgents, jeden prywatnego runtime'u
  jest wyłączony, a `THE-GAME-INTEGRATION-HANDOFF.md` jest separate project.

Live GitHub readback bez fetch/pull:

| Repozytorium/ref | SHA | drzewo |
|---|---|---|
| nAgents `claude/git-connection-9sz6dg` | `fad6f26c48d31ea7cb5a3e4aa8ad53b5da14ab81` | 56 wpisów, 47 blobów, 47 Markdownów |
| nAgents `main` | `9522836d79679f5296ad23822c9a8efba9f949fb` | 1 wpis, 1 blob, 1 Markdown |
| AutoBot Monitor `main` | `6b057bad349ccda4c4b8ec5c26ae03eea81b635a` | 228 wpisów, 193 blobów, 56 Markdownów |

Dodatkowe zdalne heady AutoBot Monitor (`backup/autobot-working-tree-20260912`,
`backup/upgrade-policy-20260912`, `wt/abm-model-003-repair`) zostały
odnotowane jako metadata-only. Nie są wskazanymi refami konsolidacji. GitHub
nie zwrócił tagów w tym readbacku.

## Hierarchia rozstrzygania sprzeczności

1. Świeży odczyt Git/worktree albo drzewa GitHuba dla nazwanego refu.
2. Jednoznaczna decyzja właściciela w `docs/process/echo.md` lub aktualnym
   `docs/spec/decisions.md`.
3. Aktualna specyfikacja nAgents i `CLAUDE.md`.
4. `AUTOBOT-KANBAN.md` i aktualne dokumenty operacyjne AutoBot Monitor — tylko
   dla jego własnego projektu.
5. Handoffy, raporty i notatki jako datowany kontekst, nigdy samodzielny live
   routing.

## Nieznane zakresy

Nieprzypisany legacy clone `/home/ubuntu/projects/Hermes-AutoBot-Monitor`,
repozytorium `Hermes-AutoBot-Execution-Router`, nieznane katalogi, nieoznaczone
refy GitHuba oraz pliki bez rozstrzygalnej proweniencji mają status
`INFRA/DECISION_REQUIRED`. Nie wolno włączać ich do nAgents przez podobieństwo
nazwy. Live board, Cron, receiver, profile, service i runtime są osobnym
źródłem typu `LIVE_READBACK_REQUIRED`, a nie dokumentacją do konsolidacji.

## Weryfikacja i granica zmian

Zakres sporządzono read-only. Nie zmieniono istniejących źródeł, nie usunięto
plików, nie wykonano fetch/pull, `git add`, commit, push, merge, deployu ani
restartu usługi. Nie czytano credentiali, `state.db`, sesji ani surowych logów.

TESTY: `P1-sources.json` zawiera odtworzalne liczniki, statusy, SHA i granice;
walidacja JSON, rozłączności source roots, obecności ścieżek oraz końcowy
readback `git status` są wykonywane po zapisaniu trzech artefaktów.
BLOKADY: brak; nieznane korzenie i live state są jawnie sklasyfikowane jako
`INFRA/DECISION_REQUIRED` albo `LIVE_READBACK_REQUIRED`, więc nie należą do
zatwierdzonego wejścia tej fazy.

NASTĘPNY KROK: P2 — programowa inwentaryzacja wyłącznie source ID oznaczonych
`IN_SCOPE*`, z zachowaniem wyłączeń i jawnych statusów `LOCAL_ONLY`,
`REMOTE_ONLY`, `HISTORY`, `EVIDENCE`, `INFRA` i `DECISION_REQUIRED`.

DEPLOY/PUSH: NIE WYKONANO
