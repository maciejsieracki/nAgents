# NAG-DOCS-CONSOLIDATION-Q1 — P1 wyłączenia audytu

STATUS: PASS
DOMAIN: INFORMACYJNY
TEMAT: NAG-DOCS-P1-SCOPE-Q1
GOAL: Wyłączyć z normalnej konsolidacji artefakty techniczne, dane runtime, sekrety, surowe logi i obce projekty, zachowując ich rolę w proweniencji oraz dowodach.
TESTY: sprawdzono kompletność klas wyłączeń, jawne statusy źródeł obcych,
zakaz kopiowania sekretów/PII oraz zgodność z granicami w `P1-sources.json`.
BLOKADY: brak; nieznane źródła pozostają jawnie oznaczone jako
`INFRA/DECISION_REQUIRED`.
NASTĘPNY KROK: P2 — inwentaryzacja tylko source ID `IN_SCOPE*`, z pominięciem
klas wyłączeń i bez czytania runtime, sekretów ani surowych logów.

## Zasada

Wyłączenie z konsolidacji nie oznacza usunięcia, ukrycia ani uznania za
nieistniejące. Oznacza tylko, że materiał nie jest normalnym wejściem do
katalogu dokumentacji i nie może stać się źródłem prawdy przez podobieństwo
nazwy. Jeżeli wyłączony element jest potrzebny do ustalenia proweniencji albo
dowodu, zapisujemy wyłącznie bezpieczne metadane i jego rolę.

Pliki objęte audytem mają dokładnie jedno źródło z `P1-sources.json`:

```text
source_id + fizyczny korzeń albo (repository, ref) + relative_path + timestamp odczytu
```

Katalog nadrzędny nie obejmuje zagnieżdżonego `.worktrees`; każdy worktree jest
osobnym korzeniem albo ma status `METADATA_ONLY`. Ref GitHuba nie jest lokalnym
checkoutem. Handoff z projektu The-Game nie jest handoffem nAgents. Artefakt
P1 nie jest wejściem własnego skanu.

## Wyłączenia techniczne i runtime

| Klasa | Wzorzec / granica | Status | Dlaczego poza normalnym audytem | Co wolno zachować |
|---|---|---|---|---|
| Git internals | `.git/**`, wewnętrzne pliki obiektów, indeksy, reflogi | `EXCLUDED_TECHNICAL` | nie jest dokumentacją; może zawierać historię, lokalne ślady i obiekty nieprzeznaczone do konsolidacji | repozytorium, branch, HEAD, status i bezpieczna informacja o refie |
| Zależności | `node_modules/**`, `vendor/**`, `third_party/**`, `.eggs/**`, `*.egg-info/**` | `EXCLUDED_DEPENDENCY` | kod dostawcy nie jest własną dokumentacją nAgents; powiela się i może być ogromny | nazwa katalogu i wpływ na granicę skanu |
| Cache | `.cache/**`, `.pytest_cache/**`, `.mypy_cache/**`, `__pycache__/**`, `*.pyc` | `EXCLUDED_GENERATED` | dane pochodne, nietrwałe i odtwarzalne | sam fakt wyłączenia oraz ewentualny licznik plików |
| Budowanie | `build/**`, `dist/**`, `target/**`, `.next/**`, `coverage/**` | `EXCLUDED_BUILD` | wynik kompilacji/testów, nie źródło; może zawierać zbundlowane lub powielone treści | typ artefaktu i jego wpływ na dowód, bez treści |
| Tymczasowe runy | `runs/**/*-tmp/**`, `.tmp/**`, katalogi tymczasowe | `EXCLUDED_TEMPORARY` | fixtures, eksperymenty i niezatwierdzone wyniki mogą wyglądać jak obowiązująca dokumentacja | nazwa runu, status i relacja do tematu, tylko jeśli bezpieczne |
| Stan procesu | `state.db`, pliki sesji, kolejki, sockety, locki, katalogi runtime | `EXCLUDED_RUNTIME` | stan zmienny nie jest źródłem dokumentacji; może zawierać dane rozmów, identyfikatory lub sekrety | typ stanu, właściciel procesu, status usługi i jawna ścieżka bez zawartości |
| Sekrety | `.env*`, `*.pem`, `*.key`, tokeny, hasła, credential files, vault values | `EXCLUDED_SECRET` | wartości tajne nie mogą trafić do repozytorium, raportu, hasha treści ani kontekstu | wyłącznie nazwa klasy, `vault_ref` albo `[REDACTED]`; nigdy wartość, próbka ani hash treści |
| Surowe logi | `*.log`, `*.out`, `logs/**`, raw event dumps | `EXCLUDED_RAW_LOG` | mogą zawierać PII, tokeny, treść rozmów i niekontrolowany rozmiar; nie są destylatem dowodu | sanitized receipt/evidence ID, timestamp, status i krótki wynik, po redakcji |
| Dane rzeczywiste | dane klientów/pracowników, PPE, eksporty produkcyjne poza `prod` | `EXCLUDED_PII` | granica nAgents zabrania prawdziwych danych osobowych poza `prod`; PII nie może być kopiowane do audytu | tylko syntetyczny identyfikator scenariusza i wynik bez danych osoby |
| Fixtures/generated | wygenerowane fixtures, snapshoty testowe, obrazy, paczki, eksporty | `EXCLUDED_GENERATED_EVIDENCE` | nie są stabilną dokumentacją i mogą zawierać sekrety albo PII; wynik musi być odtwarzalny | metadane dowodu, hash artefaktu wyłącznie gdy artefakt jest sanitizowany i jawnie potrzebny |

`raw logs`, `state.db`, sesje i credentiale nie były czytane przy P1. Ich
wystąpienie jest granicą skanu, nie dowodem zawartości.

## Wyłączenia obszarowe

| Obszar | Klasyfikacja | Zasada |
|---|---|---|
| P1 outputs: `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/**` | `OUTPUT_NOT_INPUT` | trzy artefakty P1 są wynikiem tej fazy; P2 może czytać ich jawne metadane, ale P1 nie skanuje ich jako źródeł |
| AutoBot Monitor `.worktrees/**` poza `t_771608cd` | `METADATA_ONLY` | zachować path, branch, HEAD, status i licznik Markdown; nie kopiować treści do P1 |
| AutoBot Monitor worktree `t_771608cd` | `IN_SCOPE_READ_ONLY` | tylko jawnie wskazane dokumenty relay/Cron/run oraz sanitizowane evidence; nie modyfikować dirty worktree |
| nAgents auxiliary worktrees | `IN_SCOPE_SNAPSHOT` | są osobnymi snapshotami proweniencji; w P1 nie scalać ani nie wybierać wersji kanonicznej |
| The-Game checkout/handoff | `SEPARATE_PROJECT` | nie jest źródłem prawdy nAgents; odnotować najwyżej metadane i granicę projektu |
| inne repozytoria podobne z nazwy | `INFRA/DECISION_REQUIRED` | nie włączać bez właściciela i kwalifikacji ścieżki, repozytorium oraz refu |
| live board, Cron, receiver, profile, service | `LIVE_READBACK_REQUIRED` | stan live sprawdzać osobną ścieżką; dokumentacja i handoff nie zastępują readbacku |
| nieoznaczone pliki bez proweniencji | `INFRA/DECISION_REQUIRED` | nie przypisywać na podstawie basename, treści podobnej do innego pliku ani lokalizacji „na oko" |

## Konkretnie odnotowane, ale niewłączone źródła

- `/home/ubuntu/projects/Hermes-AutoBot-Monitor` — legacy clone bez
  rozstrzygniętej roli; `INFRA/DECISION_REQUIRED`.
- `/home/ubuntu/projects/Hermes-AutoBot-Execution-Router` — osobny/niezweryfikowany
  kandydat; `INFRA/DECISION_REQUIRED`.
- `/home/ubuntu/projects/The-Game` i powiązane worktree — `SEPARATE_PROJECT`;
  nie mieszać z nAgents.
- `/home/ubuntu/autobot-loadtest-20260911` — runtime/loadtest, nie dokumentacja
  nAgents; `SEPARATE_PROJECT_OR_RUNTIME_EXCLUDED`.
- `/home/ubuntu/autobot-real24-20260911` oraz `/home/ubuntu/autobot-real24-20260913`
  — runtime/evidence historyczna do metadanych, nie normalne źródła dokumentacji;
  `SEPARATE_PROJECT_OR_RUNTIME_EXCLUDED`.
- Nieoznaczone branche GitHuba, tagi, forki i obce repozytoria — poza nazwanymi
  refami P1; `INFRA/DECISION_REQUIRED`.

## Bezpieczeństwo i odwracalność

P1 nie usuwa żadnego wyłączonego pliku, nie czyści worktree i nie zmienia
runtime. Nie wykonuje `git add -A`, `git add .`, resetu, stashu, clean, fetch,
pull, merge, push, deployu ani restartu. Nie zapisuje sekretów, PII, pełnych
logów, sesji ani wartości z bazy stanu. Wyłączenia są odwracalne: późniejsza
faza może zmienić kwalifikację wyłącznie przez jawne `source_id`, granicę i
readback, nigdy przez ciche rozszerzenie zakresu.

Jeżeli wyłączony materiał jest wymagany do scenariusza, P2/P3 tworzy
sanitized evidence z identyfikatorem i wynikiem, a nie kopię źródła. Jeżeli
sanitizacja nie daje pewności, status pozostaje `INFRA` albo
`DECISION_REQUIRED`.

## Następna bramka

P2 może rozpocząć inwentaryzację tylko dla source ID oznaczonych
`IN_SCOPE*` w `P1-sources.json`. Ma pominąć klasy wyłączeń powyżej i zapisać
każde odstępstwo jako jawny status, zamiast dopisywać plik do wygodniejszego
źródła. Nie wolno traktować `P1-exclusions.md` jako zgody na odczyt sekretów,
PII, runtime ani surowych logów.

DEPLOY/PUSH: NIE WYKONANO
