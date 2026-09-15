# NAG-CONSOLIDATE-DECISIONS-Q1 — package readback

STATUS: PASS
DOMAIN: INFORMACYJNY
TEMAT: NAG-CONSOLIDATE-DECISIONS-Q1
PHASE: local-package/readback (P7)
READBACK_AT_UTC: 2026-09-14T18:44:07Z

## Zakres i pliki

Przed utworzeniem tego raportu katalog stagingu zawierał dokładnie sześć
regularnych plików allowlisty. Ich hashe SHA-256, rozmiary i liczba linii:

| Plik | SHA-256 | Bajty | Linie |
|---|---|---:|---:|
| `NAGENTS-DECISIONS.md` | `cec183df199f7cbded295779cf0358e60a0db7df288b86a3d3894e5e13e329df` | 103527 | 2000 |
| `coverage.json` | `f55be285767bd445dab75b8af61ae9548d02fa9d266528af7db2375049dc2f35` | 69307 | 1761 |
| `operator-report.md` | `722a8ce3a8a0bcc6dd9b4e175880afbddbed4b1388d7f7b3f35ecfab9c9b46b3` | 5618 | 65 |
| `evaluator-report.md` | `c6259f84ed70fc0b32c31ec88d2023a6e8e743ec5cb84939a30bcd085d11c814` | 2140 | 20 |
| `defense-report.md` | `c7cbcb222dee910f755ccc753832efe20ad33ac6ee4daefa7c828759f730f56f` | 3353 | 41 |
| `final-control-report.md` | `8126653a5e64cf5c2a01dc634c2d87b46362e7ae14b737baf6a12ff6372c2177` | 2012 | 23 |

`package-report.md` jest wyłącznie raportem wyjściowym P7, a nie siódmym
plikiem wejściowej allowlisty.

## Wyniki niezależnego readbacku

- `coverage.json` parsuje się; `topic_id` i status odpowiadają tematowi.
- Źródła w ledgerze: 7/7 ma zgodny z bieżącym checkoutem SHA-256, rozmiar i
  liczbę linii. Potwierdzone hashe: `SRC-01` `57264a14bcf38b38811d42b025aba31359470db6c4e33dbc7abe9cad03681d49`,
  `SRC-02` `e631fb90812066690a1c1a927abb885472157c2607f8c0ba4f2e6ad56996bbde`,
  `SRC-03` `3e0b66a6157ed159fc5c95a8a65e8b3ec006ad465b69c164573249286d9398e7`,
  `SRC-04` `b05ce062506e6ac5d50309c504a6bf0a32f5257e925c1b78ad27d3ccb86625d9`,
  `SRC-05` `b282d9e49bf77994a290fbfab71803217d02c9565ab8809e6fbf33501ca4b15c`,
  `SRC-06` `84cc8a89c0e7bd53268fc78826938bec062f9223340aa2e0934d64719764a2fb`,
  `SRC-07` `95d2a06fc80e4cf1c27d59959d84b8631e70acbb3e0b879b83df95c38bf382b8`.
- Literalne zakresy: 28/28 markerów początku i końca oraz 28/28 zgodności
  bajtowej z bieżącymi zakresami źródeł (hash liczony bez końcowego LF):
  13 decyzji + 5 ECHO + 8 pytań + 1 status poza pytaniami + 1 pełny zakres
  historii.
- Decyzje: 13/13, dokładna sekwencja `D-001`…`D-013`. `D-010` pozostaje
  otwarta i odroczona; `D-011` pozostaje otwarta i blokująca MVP3.
- ECHO: 5/5: `NAG-MVP1-008-audyt-Q1`, `ECHO-001`, `ECHO-002`, `ECHO-003`,
  `NAG-INFRA-002-pomocnik-serwerowy-Q1`. Odpowiedzi, daty, autorzy, statusy
  źródeł i locatory są obecne.
- Pytania: 8/8, `Q-01`…`Q-08`, każde `OPEN_OWNER_DECISION` i
  `NOT_A_DECISION`; `Q-08` wskazuje `D-011`; nie ma `Q-09`.
- Historia: 12/12, `H-01`…`H-12`; wpisy są `HISTORY_ONLY` i nienormatywne.
- Linki względne: 14/14 celów istnieje. Siedem unikalnych celów występuje w
  pakiecie w dwóch odwołaniach: `docs/spec/decisions.md`,
  `docs/process/echo.md`, `docs/process/pytania/2026-08-25-wybory.md`,
  `docs/nota-08-wybory-otwarte.md`, `NAGENTS-CONSOLIDATION-PLAN.md`,
  `NAGENTS-PROJECT.md` oraz audyt P4.

## SRC-06 — rekonsyliacja

Status `RECONCILED` został potwierdzony bez modyfikowania źródła.

- Bieżący `NAGENTS-PROJECT.md`: SHA-256
  `84cc8a89c0e7bd53268fc78826938bec062f9223340aa2e0934d64719764a2fb`,
  53421 B, 819 linii.
- Snapshot: SHA-256
  `bab1666d8528622b055b1a3f0b9e21a918be5efabc5be0af2ac06064ae00236a`,
  53403 B, 819 linii.
- Drift: `NAGENTS-PROJECT.md:165`, różnica `+18` B. Zastąpienie bieżącej
  linii wersją snapshotu odtwarza dokładny hash i rozmiar snapshotu.
- Skutek jest nawigacyjny: anchor używa właściwego profilu i projektu.
  Decyzje, ECHO, pytania, historia, linki i liczniki pakietu pozostają bez
  zmian.

## Bariery i stan gita

- Skan sześciu plików allowlisty: private key, bearer, JWT, IPv4, email i
  secret assignment — `0/0/0/0/0/0`.
- Usunięcia i przeniesienia: `0`; bieżący tracked diff nie zawiera wpisu `D`
  ani `R`. Nie usuwano ani nie przenoszono źródeł.
- `git diff --check`: PASS, exit `0`.
- Checkout miał już zmiany i nieśledzone materiały przed P7. Zostały
  zachowane; P7 nie zmienił źródeł kanonicznych, `NAGENTS-PROJECT.md` ani
  żadnego z sześciu plików wejściowych.
- Nie wykonano `git add`, commit, push, merge, publikacji, deployu ani
  restartu. Owner gates `OWNER-01`, `OWNER-02`, `OWNER-04` i `OWNER-05`
  pozostają otwarte.
- Brak `AUTOBOT-KANBAN.md` pozostaje jawnie zarejestrowaną, nieblokującą luką
  infrastrukturalną; nie użyto cichego fallbacku.

## Następny krok

Lokalny P7 jest zakończony jako PASS. Ewentualna integracja, publikacja,
push, merge lub deploy wymaga osobnej jawnej decyzji właściciela.
