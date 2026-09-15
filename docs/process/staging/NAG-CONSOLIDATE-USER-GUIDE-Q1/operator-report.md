STATUS: `PASS`
DOMAIN: `INFORMACYJNY`
TEMAT: `NAG-CONSOLIDATE-USER-GUIDE-Q1`
TASK_ID: `t_9e8f68de`
PHASE: `operator`
ROUND: `1`
WORKSPACE: `/home/ubuntu/projects/nAgents-readonly`
MODEL: `gpt-5.6-luna` (task override)
PROVIDER: `openai-codex` (task override)
REASONING_EFFORT: `max` (task event readback)
SERVICE_TIER: `priority` (task event readback)
RUN_ID: `227`
TOKENS: `N/D`
TOOL_CALLS: `N/D`
COST: `N/D`
JOURNAL_PATH: `N/D` — trzy pliki stagingowe są trwałym dowodem Operatora

GOAL: Zbudować addytywny staging pakietu NAGENTS-USER-GUIDE.md dla pracownika,
zachowując język skutków, pięć sytuacji pracy, granice pracownik–administrator
i zasady odbioru.

ZMIANY:
- Utworzono wyłącznie katalog
  `docs/process/staging/NAG-CONSOLIDATE-USER-GUIDE-Q1/`.
- Utworzono dokładnie trzy pliki: `NAGENTS-USER-GUIDE.md`, `coverage.json`
  oraz ten raport.
- `USER-01` opisuje firmowe logowanie, gotowy profil, przydzielony czat,
  web-first i brak obowiązku konfiguracji zaplecza przez pracownika.
- `USER-02` zachowuje wszystkie pięć sytuacji pracy, skutki zaniedbań,
  cztery czynności minimalne, koszt 20–30 minut i przypadki, w których zasady
  nie stosować.
- `USER-03` zachowuje odbiór przez inną osobę, ciągłość po zamknięciu
  przeglądarki, zatrzymanie niejasności, brak nowego zakresu/pustego sprawdzania
  i brak duplikatu po ponowieniu tej samej dyspozycji.
- `USER-04` rozdziela korzystanie z przydzielonego czatu od administracji;
  nie nadaje ról ani uprawnień. Bieżąca architektura i MVP2 mają pierwszeństwo
  przed historyczną macierzą RBAC.
- P4 `NAG-USER` obejmuje 1 rodzinę / 12 rekordów; P4 `NAG-RBAC` obejmuje
  1 rodzinę / 1 rekord. Zewnętrznego pliku RBAC nie odczytano ani nie kopiowano;
  użyto wyłącznie jego bezpiecznej metadany P4.
- Nie zmieniono źródeł kanonicznych, decyzji, istniejących stagingów, raportów,
  routingu, integracji ani deploymentu.

READBACK ŹRÓDEŁ:
- `docs/proces-dla-pracownikow.md`: SHA-256
  `09f3a374b861e6ca2171bdded8a6af909f703c66f1c04f57610fbc7a233d6f20`
  (6307 B, 130 linii), P4 `PF-0209`, `CONSOLIDATION_CANDIDATE`.
- `docs/spec/scenarios.md`: SHA-256
  `8b8be2a4e631fa92306dca1779a858d0c95803bc6623b0fc60a84c5b55dd570d`
  (5324 B, 92 linii), P4 `PF-0242`, `CANONICAL`.
- `CLAUDE.md`: SHA-256
  `8de583cc4fece98c55c49b5fac9f1562669040204f571ee2daacc9905e9390d2`
  (6521 B, 121 linii), P4 `PF-0173`, `CANONICAL`.
- `NAGENTS-PROJECT.md`: SHA-256
  `84cc8a89c0e7bd53268fc78826938bec062f9223340aa2e0934d64719764a2fb`
  (53421 B, 819 linii), P4 `PF-0184`, świeży lokalny readback.
- `NAGENTS-CONSOLIDATION-PLAN.md`: SHA-256
  `b282d9e49bf77994a290fbfab71803217d02c9565ab8809e6fbf33501ca4b15c`
  (130539 B, 665 linii), `PLAN_ONLY / OWNER_HOLD_REQUIRED`.
- `docs/spec/decisions.md`: SHA-256
  `57264a14bcf38b38811d42b025aba31359470db6c4e33dbc7abe9cad03681d49`
  (10789 B, 264 linii), P4 `PF-0241`, `CANONICAL`.
- `docs/spec/00-architektura.md`: SHA-256
  `322a6c2a76dc4c6bee53c7c1569c306767eb29cd05420bf298e34672aca133cc`
  (15112 B, 283 linii), P4 `PF-0235`, `CANONICAL`.
- `docs/spec/02-mvp2.md`: SHA-256
  `10cbba06ac5179dee6ff97107fb9f1a109f91419cca7d803412eba25faaddd48`
  (6915 B, 153 linii), P4 `PF-0237`, `CANONICAL`.
- `P4-classification.json`: SHA-256
  `95d2a06fc80e4cf1c27d59959d84b8631e70acbb3e0b879b83df95c38bf382b8`
  (2655394 B, 54556 linii), `PASS_WITH_EXPLICIT_OWNER_GATES`.

WERYFIKACJA:
- `NAGENTS-USER-GUIDE.md`: 228 linii, SHA-256
  `67a1e3bbaa77244147886992aadf60df6c1139535a517679c36d8c41bb5480fe`.
- `coverage.json`: poprawny JSON, 4/4 sekcje `COVERED`, 9/9 scenariuszy
  `U1`, `U2`, `U3`, `U4`, `U7`, `U8`, `U9`, `U10`, `U11`; SHA-256
  `6a75a1f8942aba66e097f05e626bb66045d4dc40d2debae27048bbd5e617ff39`
  (31297 B, 693 linii).
- 13/13 wybranych rekordów P4 ma zgodne `record_id`, SHA-256, status,
  obecność, status treści i świeżość; agregat wybrany to 2 rodziny / 13 rekordów.
- Każda sekcja `USER-01..04` ma status wejścia, źródła, locator P6,
  locator artefaktu, kryterium pokrycia i wynik `COVERED`.
- Hashy bieżącego checkoutu źródeł i hashy P6/P4 sprawdzono programowo;
  rozjazd `NAGENTS-PROJECT.md` względem snapshotu P4 pozostaje jawny i nie
  został rozstrzygnięty przez nadpisanie źródła.
- Pakiet nie zawiera NUL ani końcowych białych znaków; struktura JSON,
  granica katalogu i linki względne zostały sprawdzone.
- `git diff --check` przechodzi. Testy aplikacji: `N/D` — faza tworzy
  dokumentację, nie kod.

BLOKADY / OWNER GATES:
- Brak blokady dla samego stagingu.
- Przegląd właściciela języka i zakresu przewodnika pozostaje wymagany.
- P6, publikacja, zastąpienie źródeł, integracja, usunięcia, merge, push i deploy
  pozostają poza allowlistą i nie zostały wykonane.
- Przewodnik nie jest dowodem live runtime; scenariusze ciągłości i pomocnika
  wymagają osobnego testu w odpowiedniej fazie.

NASTĘPNY KROK: Niezależny Evaluator ma odczytać te same trzy pliki i powtórzyć
kontrolę `USER-01..04`, pięciu sytuacji, scenariuszy, hashy, locatorów, granicy
pracownik–administrator oraz rozdzielenia staging/source. Defense uruchomić
wyłącznie przy niepustej, numerowanej liście zarzutów; następnie Final Control
i lokalny P7 readback. Pakiet nie jest publikacją.

DEPLOY/PUSH: `NIE WYKONANO`
