# AutoBot Monitor — historia i evidence (staging)

STATUS: `STAGING_ONLY`
TEMAT: `NAG-CONSOLIDATE-ABM-HISTORY-Q1`
TASK_ID: `t_f026c7e9`
PHASE: `operator`
RUNDA: `1`
GENERATED_AT_UTC: `2026-09-14T22:20:52Z`

> To jest addytywny indeks historii. Nie jest kontraktem routingu, źródłem live state,
> zgodą na dispatch, instalację, publikację ani usunięcie/przeniesienie źródeł.

Zakres odczytu obejmuje bezpieczne metadane źródeł ABM, P4 oraz read-only snapshot
Kanbana. Nie kopiowano body kart, surowych runów, logów, transcriptów, payloadów
eventów, credentiali, sekretów, PII ani prywatnego runtime. `AUTOBOT-KANBAN.md`
i świeży readback boardu/profilu pozostają nadrzędne dla bieżącego routingu.

## Kontrakt statusów

- `EVIDENCE` oznacza zachowany dowód historyczny; `DUPLICATE` oznacza drugi snapshot tej samej treści, nie usunięty rekord.
- `HISTORY`/`STALE` oznacza kontekst datowany. Nie wolno z niego tworzyć nowego dispatchu ani wniosku o live state.
- `LIVE_READBACK_REQUIRED` jest bramką dla każdego twierdzenia o bieżącym boardzie, profilu, receiverze, usłudze lub instalacji.
- `STAGING_ONLY` oznacza, że ten katalog nie zastępuje źródeł i nie nadaje zgody na publikację, merge, push ani deploy.
- Exact path-family IDs, record IDs, task IDs, run IDs, event IDs i artifact IDs są w `coverage.json`; tutaj podano ludzką mapę.

## Sekcje HISTORY-01..05 — source/status/locator

| ID | Zakres | Source/status | Locator | Wynik |
|---|---|---|---|---|
| HISTORY-01 | Katalog per-run/per-task evidence; podobne szablony pozostają osobnymi próbami. | EVIDENCE, DUPLICATE, PRESENT_BOTH, LOCAL_ONLY | NAGENTS-CONSOLIDATION-PLAN.md §4.9 lines 183–185 | COVERED |
| HISTORY-02 | Final reports and acceptance mappings as immutable historical evidence. | EVIDENCE, DUPLICATE, PRESENT_BOTH | NAGENTS-CONSOLIDATION-PLAN.md §4.9 lines 183–186 | COVERED |
| HISTORY-03 | Evolution of plans, requirements and ledgers; historical context only. | HISTORY, DUPLICATE, PRESENT_BOTH, LOCAL_ONLY | NAGENTS-CONSOLIDATION-PLAN.md §4.9 lines 183–187 | COVERED |
| HISTORY-04 | Dated owner handoff, migration rationale and operational warnings. | HISTORY, LOCAL_ONLY | NAGENTS-CONSOLIDATION-PLAN.md §4.9 lines 183–188 | COVERED |
| HISTORY-05 | Archive boundary and exclusions: The-Game, runtime, credentials, raw logs and unrelated/test cards. | SEPARATE_PROJECT, PRIVATE_RUNTIME, LOCAL_ONLY, SOURCE, OTHER_UNKNOWN | NAGENTS-CONSOLIDATION-PLAN.md §4.9 lines 183–189; P4 #/excluded_scope | COVERED |

Reguła wspólna: `source/status/locator` nie jest dowodem aktualności. Aktualny stan
wymaga osobnego readbacku; historyczne `PASS` nie awansuje do live approval.

## HISTORY-01 — katalog per-run/per-task evidence

`ABM-EVIDENCE`: P4 `EVIDENCE`, 136 path families / 180 records. Każdy run pozostaje
osobnym wpisem. Exact duplicates między lokalnym checkoutem i nazwanym remote są
zachowane jako proweniencja (`DUPLICATE`), a podobne szablony nie są łączone.
Źródło: `NAGENTS-CONSOLIDATION-PLAN.md §4.9 lines 183–185`; P4 group rollup;
locator artefaktu: `coverage.json.run_index`, `p4_selected_families`, `p4_selected_records`.

### Mapa katalogów runów

| Run key | PF | Records | Mentioned source task IDs | Live task IDs | Live run IDs | Live events (count) | Artifact IDs (count) | Mapping |
|---|---:|---:|---|---|---|---:|---:|---|
| ABM-A-001 | 5 | 10 | — | — | — | 0 | 0 | NO_TASK_ID_IN_SAFE_SOURCE_METADATA |
| ABM-ARCH-001 | 1 | 1 | — | t_36a8740b, t_4b32acdf, t_7a40116c, t_c9d8cae2, t_fd54eab5 | 78, 79, 81, 82, 83 | 55 | 0 | LIVE_KEY_MATCH_NO_SOURCE_TASK_ID |
| ABM-ASTRA-001 | 1 | 1 | t_5caea696 | t_12f9e589, t_30686b70, t_a7991db2, t_cf678b6f, t_d5fec02f, t_d81d914e, t_f18114c5 | 30, 33, 34, 36, 37, 40, 43, 44 | 178 | 0 | PARTIAL_LIVE_READBACK_MENTIONED_IDS |
| ABM-ASTRA-002 | 20 | 20 | t_00015016, t_08cb82c4, t_18f508b7, t_1b3e1a8c, t_656c573b, t_8072d3a6, t_9236336b, t_a56cba0f, t_ac0e0045, t_dae115ec, t_f08b39ee, t_f52b33f2 | t_00015016, t_08cb82c4, t_18f508b7, t_5374161f, t_58821d98, t_656c573b, t_8072d3a6, t_9236336b, t_9ad95e44, t_a56cba0f, t_ac0e0045, t_dae115ec, t_f08b39ee, t_f52b33f2 | 31, 32, 35, 38, 39, 41, 42, 45, 46, 47, 48, 49, 50, 51, 52 | 276 | 0 | PARTIAL_LIVE_READBACK_MENTIONED_IDS |
| ABM-B-001 | 4 | 8 | — | — | — | 0 | 0 | NO_TASK_ID_IN_SAFE_SOURCE_METADATA |
| ABM-BRIDGE-001 | 1 | 1 | — | t_5d772b97 | — | 20 | 0 | LIVE_KEY_MATCH_NO_SOURCE_TASK_ID |
| ABM-C-001 | 4 | 8 | — | — | — | 0 | 0 | NO_TASK_ID_IN_SAFE_SOURCE_METADATA |
| ABM-D-001 | 5 | 10 | — | — | — | 0 | 0 | NO_TASK_ID_IN_SAFE_SOURCE_METADATA |
| ABM-MODEL-001 | 1 | 1 | — | t_eb87e396 | 96 | 12 | 0 | LIVE_KEY_MATCH_NO_SOURCE_TASK_ID |
| ABM-MONITOR-001 | 5 | 5 | t_34841413, t_594bac06, t_93b0639f, t_dd59ddcc, t_e6b89439 | t_34841413, t_594bac06, t_dd59ddcc | 1, 5, 6 | 68 | 0 | PARTIAL_LIVE_READBACK_MENTIONED_IDS |
| ABM-MONITOR-002 | 7 | 7 | t_00aab3d8, t_07cebaff, t_6a3194e5, t_d3b9cf0e | t_00aab3d8, t_07cebaff, t_4f4bcf82, t_6583f39f, t_d3b9cf0e | 9, 10, 11, 12, 13 | 115 | 0 | PARTIAL_LIVE_READBACK_MENTIONED_IDS |
| ABM-MONITOR-VISIBILITY-001 | 2 | 2 | t_4b16b6e6, t_9841dd31, t_b0ae1323 | — | — | 0 | 0 | SOURCE_METADATA_ONLY_NO_CURRENT_BOARD |
| ABM-PLAN-001 | 1 | 1 | t_e9584658 | t_13038d43, t_2a981677, t_574e692b, t_59cb06ed, t_9998fc59, t_9dde4be9, t_a0abcb14, t_b1f3ccf0, t_bae32721, t_c7f3bd50, t_e96beb85 | 58, 59, 60, 61, 62, 63, 64, 65, 66, 67, 68, 69, 70, 71, 72, 73, 74, 75, 76, 77 | 312 | 0 | PARTIAL_LIVE_READBACK_MENTIONED_IDS |
| ABM-PLUGIN-001 | 1 | 1 | t_11b10d7c, t_26396295, t_cbc3e9cd | t_1008ceae, t_11b10d7c, t_1b3e1a8c, t_26396295, t_5caea696, t_6307913f, t_7000fc6e, t_a37670bd, t_b5edca95, t_cbc3e9cd, t_e4f5a255, t_e7b6a1da, t_ea81374e, t_eebe686c, t_f794b99a, t_f87129dc | 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29 | 369 | 0 | PARTIAL_LIVE_READBACK_MENTIONED_IDS |
| ABM-PROFILE-001 | 1 | 1 | — | t_04785789 | — | 20 | 0 | LIVE_KEY_MATCH_NO_SOURCE_TASK_ID |
| ABM-V2-A | 5 | 10 | — | — | — | 0 | 0 | NO_TASK_ID_IN_SAFE_SOURCE_METADATA |
| ABM-V2-B | 5 | 10 | — | — | — | 0 | 0 | NO_TASK_ID_IN_SAFE_SOURCE_METADATA |
| ABM-V2-C | 5 | 10 | — | — | — | 0 | 0 | NO_TASK_ID_IN_SAFE_SOURCE_METADATA |
| ABM-V2-D | 5 | 10 | — | — | — | 0 | 0 | NO_TASK_ID_IN_SAFE_SOURCE_METADATA |
| ABM-V2-E | 5 | 10 | — | — | — | 0 | 0 | NO_TASK_ID_IN_SAFE_SOURCE_METADATA |
| ABM-V2-H-001 | 47 | 47 | t_0e4e8e51, t_17390e69, t_32c1397d, t_3687d4b8, t_3ad5a3dc, t_3d5fda13, t_3dd33fba, t_4eef730a, t_50ab73a0, t_529bf671, t_540d308b, t_59203800, t_59ebd2f9, t_60845a51, t_618f177c, t_61a756b4, t_627ae22b, t_67e3831d, t_6bc29093, t_6e181e6e, t_72363a51, t_7429e354, t_74e4c71e, t_75dddb50, t_7a063b6e, t_7bbcfc53, t_8883c787, t_8a487022, t_9df58370, t_a14bc74f, t_a27d8db2, t_a7d13c77, t_a995a560, t_b35d7a95, t_bb54113c, t_c8592caa, t_d3ce5f9a, t_db44041c, t_e15537a2, t_e7a2bc18, t_ea17552f, t_ebf0b660, t_f684df3b, t_fd051f56 | — | — | 0 | 0 | SOURCE_METADATA_ONLY_NO_CURRENT_BOARD |
| ABM-V2-P0-EVAL-R3 | 1 | 1 | — | — | — | 0 | 0 | NO_TASK_ID_IN_SAFE_SOURCE_METADATA |
| ABM-V2-P0-R2 | 1 | 1 | — | — | — | 0 | 0 | NO_TASK_ID_IN_SAFE_SOURCE_METADATA |
| ABM-V2-P0-R3 | 1 | 1 | — | — | — | 0 | 0 | NO_TASK_ID_IN_SAFE_SOURCE_METADATA |
| ABM-V21-GPU-001 | 1 | 2 | — | — | — | 0 | 0 | NO_TASK_ID_IN_SAFE_SOURCE_METADATA |
| ABM-WEB-001 | 1 | 1 | — | t_30042897, t_56cb64e9, t_dd1ecfab, t_ee0b1416 | 87, 90, 93, 94 | 60 | 0 | LIVE_KEY_MATCH_NO_SOURCE_TASK_ID |

Exact live event and artifact IDs are retained in `coverage.json.run_index` and
`coverage.json.card_snapshot.card_ledger`; the Markdown view shows counts to avoid
turning a human index into a raw event dump. Source task IDs are metadata mentions only,
not ownership assertions.

### Path-family locator index

| PF | Path | P4 status | Records | Source IDs | P4 locator |
|---|---|---|---:|---|---|
| PF-0026 | runs/ABM-A-001/00-dispatch.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0026] |
| PF-0027 | runs/ABM-A-001/01-operator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0027] |
| PF-0028 | runs/ABM-A-001/02-evaluator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0028] |
| PF-0029 | runs/ABM-A-001/03-final-control.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0029] |
| PF-0030 | runs/ABM-A-001/04-integration.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0030] |
| PF-0031 | runs/ABM-ARCH-001/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0031] |
| PF-0032 | runs/ABM-ASTRA-001/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0032] |
| PF-0033 | runs/ABM-ASTRA-002/00-defense-r3-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0033] |
| PF-0034 | runs/ABM-ASTRA-002/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0034] |
| PF-0035 | runs/ABM-ASTRA-002/00-luna-defense-r5-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0035] |
| PF-0036 | runs/ABM-ASTRA-002/00-luna-final-control-r5-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0036] |
| PF-0037 | runs/ABM-ASTRA-002/00-luna-repair-evaluator-r5-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0037] |
| PF-0038 | runs/ABM-ASTRA-002/00-luna-repair-final-control-r5-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0038] |
| PF-0039 | runs/ABM-ASTRA-002/00-luna-repair-r5-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0039] |
| PF-0040 | runs/ABM-ASTRA-002/01-luna-operator-r5.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0040] |
| PF-0041 | runs/ABM-ASTRA-002/01-luna-repair-r5.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0041] |
| PF-0042 | runs/ABM-ASTRA-002/01-operator-r3.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0042] |
| PF-0043 | runs/ABM-ASTRA-002/01-repair-operator-r4.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0043] |
| PF-0044 | runs/ABM-ASTRA-002/02-evaluator-r3-review.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0044] |
| PF-0045 | runs/ABM-ASTRA-002/02-evaluator-r3.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0045] |
| PF-0046 | runs/ABM-ASTRA-002/02-luna-evaluator-r5.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0046] |
| PF-0047 | runs/ABM-ASTRA-002/02-luna-repair-evaluator-r5.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0047] |
| PF-0048 | runs/ABM-ASTRA-002/03-defense-r3.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0048] |
| PF-0049 | runs/ABM-ASTRA-002/03-luna-defense-r5.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0049] |
| PF-0050 | runs/ABM-ASTRA-002/04-final-control-r3.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0050] |
| PF-0051 | runs/ABM-ASTRA-002/04-luna-final-control-r5.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0051] |
| PF-0052 | runs/ABM-ASTRA-002/04-luna-repair-final-control-r5.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0052] |
| PF-0053 | runs/ABM-B-001/00-dispatch.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0053] |
| PF-0054 | runs/ABM-B-001/01-operator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0054] |
| PF-0055 | runs/ABM-B-001/02-evaluator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0055] |
| PF-0056 | runs/ABM-B-001/03-final-control.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0056] |
| PF-0057 | runs/ABM-BRIDGE-001/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0057] |
| PF-0058 | runs/ABM-C-001/00-dispatch.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0058] |
| PF-0059 | runs/ABM-C-001/01-operator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0059] |
| PF-0060 | runs/ABM-C-001/02-evaluator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0060] |
| PF-0061 | runs/ABM-C-001/03-final-control.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0061] |
| PF-0062 | runs/ABM-D-001/00-dispatch.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0062] |
| PF-0063 | runs/ABM-D-001/01-operator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0063] |
| PF-0064 | runs/ABM-D-001/02-evaluator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0064] |
| PF-0065 | runs/ABM-D-001/03-final-control.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0065] |
| PF-0066 | runs/ABM-D-001/04-integration.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0066] |
| PF-0067 | runs/ABM-MODEL-001/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0067] |
| PF-0068 | runs/ABM-MONITOR-001/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0068] |
| PF-0069 | runs/ABM-MONITOR-001/01-operator.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0069] |
| PF-0070 | runs/ABM-MONITOR-001/02-evaluator.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0070] |
| PF-0071 | runs/ABM-MONITOR-001/03-defense.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0071] |
| PF-0072 | runs/ABM-MONITOR-001/04-final-control.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0072] |
| PF-0073 | runs/ABM-MONITOR-002/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0073] |
| PF-0074 | runs/ABM-MONITOR-002/01-backend-operator.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0074] |
| PF-0075 | runs/ABM-MONITOR-002/01-desktop-operator.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0075] |
| PF-0076 | runs/ABM-MONITOR-002/02-ABM-MONITOR-002:backend:operator:r1-evaluator.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0076] |
| PF-0077 | runs/ABM-MONITOR-002/02-ABM-MONITOR-002:desktop:operator:r1-evaluator.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0077] |
| PF-0078 | runs/ABM-MONITOR-002/03-ABM-MONITOR-002:backend:operator:r1-final-control.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0078] |
| PF-0079 | runs/ABM-MONITOR-002/04-ABM-MONITOR-002:desktop:operator:r1-final-control.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0079] |
| PF-0080 | runs/ABM-MONITOR-VISIBILITY-001/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0080] |
| PF-0081 | runs/ABM-MONITOR-VISIBILITY-001/01-operator.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0081] |
| PF-0089 | runs/ABM-PLAN-001/00-watchdog-repair-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0089] |
| PF-0090 | runs/ABM-PLUGIN-001/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0090] |
| PF-0091 | runs/ABM-PROFILE-001/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0091] |
| PF-0092 | runs/ABM-V2-A/00-dispatch.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0092] |
| PF-0093 | runs/ABM-V2-A/01-operator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0093] |
| PF-0094 | runs/ABM-V2-A/02-evaluator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0094] |
| PF-0095 | runs/ABM-V2-A/03-final-control.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0095] |
| PF-0096 | runs/ABM-V2-A/04-integration.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0096] |
| PF-0097 | runs/ABM-V2-B/00-dispatch.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0097] |
| PF-0098 | runs/ABM-V2-B/01-operator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0098] |
| PF-0099 | runs/ABM-V2-B/02-evaluator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0099] |
| PF-0100 | runs/ABM-V2-B/03-final-control.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0100] |
| PF-0101 | runs/ABM-V2-B/04-integration.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0101] |
| PF-0102 | runs/ABM-V2-C/00-dispatch.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0102] |
| PF-0103 | runs/ABM-V2-C/01-operator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0103] |
| PF-0104 | runs/ABM-V2-C/02-evaluator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0104] |
| PF-0105 | runs/ABM-V2-C/03-final-control.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0105] |
| PF-0106 | runs/ABM-V2-C/04-integration.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0106] |
| PF-0107 | runs/ABM-V2-D/00-dispatch.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0107] |
| PF-0108 | runs/ABM-V2-D/01-operator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0108] |
| PF-0109 | runs/ABM-V2-D/02-evaluator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0109] |
| PF-0110 | runs/ABM-V2-D/03-final-control.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0110] |
| PF-0111 | runs/ABM-V2-D/04-integration.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0111] |
| PF-0112 | runs/ABM-V2-E/00-dispatch.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0112] |
| PF-0113 | runs/ABM-V2-E/01-operator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0113] |
| PF-0114 | runs/ABM-V2-E/02-evaluator.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0114] |
| PF-0115 | runs/ABM-V2-E/03-final-control.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0115] |
| PF-0116 | runs/ABM-V2-E/04-integration.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0116] |
| PF-0117 | runs/ABM-V2-H-001/00-topic-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0117] |
| PF-0118 | runs/ABM-V2-H-001/a/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0118] |
| PF-0119 | runs/ABM-V2-H-001/b/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0119] |
| PF-0120 | runs/ABM-V2-H-001/b/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0120] |
| PF-0121 | runs/ABM-V2-H-001/c/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0121] |
| PF-0122 | runs/ABM-V2-H-001/c/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0122] |
| PF-0123 | runs/ABM-V2-H-001/d/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0123] |
| PF-0124 | runs/ABM-V2-H-001/d/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0124] |
| PF-0125 | runs/ABM-V2-H-001/e/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0125] |
| PF-0126 | runs/ABM-V2-H-001/e/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0126] |
| PF-0127 | runs/ABM-V2-H-001/f/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0127] |
| PF-0128 | runs/ABM-V2-H-001/f/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0128] |
| PF-0129 | runs/ABM-V2-H-001/g/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0129] |
| PF-0130 | runs/ABM-V2-H-001/g/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0130] |
| PF-0131 | runs/ABM-V2-H-001/h/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0131] |
| PF-0132 | runs/ABM-V2-H-001/h/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0132] |
| PF-0133 | runs/ABM-V2-H-001/i/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0133] |
| PF-0134 | runs/ABM-V2-H-001/i/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0134] |
| PF-0135 | runs/ABM-V2-H-001/j/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0135] |
| PF-0136 | runs/ABM-V2-H-001/j/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0136] |
| PF-0137 | runs/ABM-V2-H-001/k/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0137] |
| PF-0138 | runs/ABM-V2-H-001/k/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0138] |
| PF-0139 | runs/ABM-V2-H-001/l/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0139] |
| PF-0140 | runs/ABM-V2-H-001/l/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0140] |
| PF-0141 | runs/ABM-V2-H-001/m/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0141] |
| PF-0142 | runs/ABM-V2-H-001/m/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0142] |
| PF-0143 | runs/ABM-V2-H-001/n/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0143] |
| PF-0144 | runs/ABM-V2-H-001/n/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0144] |
| PF-0145 | runs/ABM-V2-H-001/o/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0145] |
| PF-0146 | runs/ABM-V2-H-001/o/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0146] |
| PF-0147 | runs/ABM-V2-H-001/p/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0147] |
| PF-0148 | runs/ABM-V2-H-001/p/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0148] |
| PF-0149 | runs/ABM-V2-H-001/q/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0149] |
| PF-0150 | runs/ABM-V2-H-001/q/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0150] |
| PF-0151 | runs/ABM-V2-H-001/r/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0151] |
| PF-0152 | runs/ABM-V2-H-001/r/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0152] |
| PF-0153 | runs/ABM-V2-H-001/s/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0153] |
| PF-0154 | runs/ABM-V2-H-001/s/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0154] |
| PF-0155 | runs/ABM-V2-H-001/t/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0155] |
| PF-0156 | runs/ABM-V2-H-001/t/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0156] |
| PF-0157 | runs/ABM-V2-H-001/u/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0157] |
| PF-0158 | runs/ABM-V2-H-001/u/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0158] |
| PF-0159 | runs/ABM-V2-H-001/v/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0159] |
| PF-0160 | runs/ABM-V2-H-001/v/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0160] |
| PF-0161 | runs/ABM-V2-H-001/w/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0161] |
| PF-0162 | runs/ABM-V2-H-001/w/00-evaluator-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0162] |
| PF-0163 | runs/ABM-V2-H-001/x/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0163] |
| PF-0164 | runs/ABM-V2-P0-EVAL-R3/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0164] |
| PF-0165 | runs/ABM-V2-P0-R2/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0165] |
| PF-0166 | runs/ABM-V2-P0-R3/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0166] |
| PF-0167 | runs/ABM-V21-GPU-001/00-dispatch.md | EVIDENCE | 2 | ABM_CHECKOUT, GITHUB_ABM_MAIN_REF | P4-classification.json#/path_family_classifications[path_family_id=PF-0167] |
| PF-0168 | runs/ABM-WEB-001/00-dispatch.md | EVIDENCE | 1 | ABM_CHECKOUT | P4-classification.json#/path_family_classifications[path_family_id=PF-0168] |

Pełne 136 rodzin / 180 rekordów, w tym exact `record_id`, revision, SHA-256,
presence/content/freshness status oraz bieżący lokalny readback, jest w machine-readable
`coverage.json`. Nie nastąpiło łączenie różnych runów ani usuwanie duplikatów snapshotów.

## HISTORY-02 — final reports

Source/status/locator: `ABM-REPORTS` = 2 path families / 4 records, P4 `EVIDENCE`.
Raporty są datowanym evidence; acceptance mapping nie zmienia bieżącego kontraktu.
P4 locator: `P4-classification.json#/path_family_classifications`; plan locator:
`NAGENTS-CONSOLIDATION-PLAN.md §4.9 lines 183–186`.

| PF | Path | Records | P4 status | P4 record IDs (exact) |
|---|---|---:|---|---|
| PF-0003 | FINAL-REPORT-V2.md | 2 | EVIDENCE | ABM_CHECKOUT\|/home/ubuntu/projects/Autoboot-Monitor\|refs/heads/main\|FINAL-REPORT-V2.md, GITHUB_ABM_MAIN_REF\|github://maciejsieracki/Autoboot-Monitor/refs/heads/main\|refs/heads/main\|FINAL-REPORT-V2.md |
| PF-0004 | FINAL-REPORT.md | 2 | EVIDENCE | ABM_CHECKOUT\|/home/ubuntu/projects/Autoboot-Monitor\|refs/heads/main\|FINAL-REPORT.md, GITHUB_ABM_MAIN_REF\|github://maciejsieracki/Autoboot-Monitor/refs/heads/main\|refs/heads/main\|FINAL-REPORT.md |

## HISTORY-03 — plany i ewolucja

Source/status/locator: `ABM-HISTORY` = 5 path families / 9 records, P4 `HISTORY`/`STALE`.
To jest kontekst datowany, nie źródło dispatchu. Aktualny routing/status wymaga
readbacku `AUTOBOT-KANBAN.md` i boardu. Plan locator: `NAGENTS-CONSOLIDATION-PLAN.md §4.9 lines 183–187`.

| PF | Path | Records | P4 status | P4 record IDs (exact) |
|---|---|---:|---|---|
| PF-0005 | PLAN-AUTOBOT-PLUGIN.md | 1 | HISTORY | ABM_CHECKOUT\|/home/ubuntu/projects/Autoboot-Monitor\|refs/heads/main\|PLAN-AUTOBOT-PLUGIN.md |
| PF-0006 | PLAN-V2.md | 2 | HISTORY | ABM_CHECKOUT\|/home/ubuntu/projects/Autoboot-Monitor\|refs/heads/main\|PLAN-V2.md, GITHUB_ABM_MAIN_REF\|github://maciejsieracki/Autoboot-Monitor/refs/heads/main\|refs/heads/main\|PLAN-V2.md |
| PF-0007 | PLAN.md | 2 | HISTORY | ABM_CHECKOUT\|/home/ubuntu/projects/Autoboot-Monitor\|refs/heads/main\|PLAN.md, GITHUB_ABM_MAIN_REF\|github://maciejsieracki/Autoboot-Monitor/refs/heads/main\|refs/heads/main\|PLAN.md |
| PF-0008 | V2-LEDGER.md | 2 | HISTORY | ABM_CHECKOUT\|/home/ubuntu/projects/Autoboot-Monitor\|refs/heads/main\|V2-LEDGER.md, GITHUB_ABM_MAIN_REF\|github://maciejsieracki/Autoboot-Monitor/refs/heads/main\|refs/heads/main\|V2-LEDGER.md |
| PF-0009 | V2-REQUIREMENTS.md | 2 | HISTORY | ABM_CHECKOUT\|/home/ubuntu/projects/Autoboot-Monitor\|refs/heads/main\|V2-REQUIREMENTS.md, GITHUB_ABM_MAIN_REF\|github://maciejsieracki/Autoboot-Monitor/refs/heads/main\|refs/heads/main\|V2-REQUIREMENTS.md |

## HISTORY-04 — owner handoff

Source/status/locator: `ABM-HANDOFF` = 1 path family / 1 record, P4 `HISTORY`.
Handoff nie jest zgodą ani live ownership readbackiem; ostrzeżenia i receiver/profile
wymagają potwierdzenia z bieżącego boardu/profilu przed akcją. Plan locator:
`NAGENTS-CONSOLIDATION-PLAN.md §4.9 lines 183–188`.

| PF | Path | Records | P4 status | P4 record ID (exact) |
|---|---|---:|---|---|
| PF-0023 | docs/handoffs/ABM-SAME-PROFILE-OWNER-HANDOFF.md | 1 | HISTORY | ABM_CHECKOUT\|/home/ubuntu/projects/Autoboot-Monitor\|refs/heads/main\|docs/handoffs/ABM-SAME-PROFILE-OWNER-HANDOFF.md |

## HISTORY-05 — granice archiwum i wyłączenia

Source/status/locator: P4 `excluded_scope`, `ABM-CARD-AUDIT.json` snapshot oraz
`AUTOBOT-KANBAN.md`/`ABM-CARD-INDEX.md`. Plan locator: `NAGENTS-CONSOLIDATION-PLAN.md §4.9 lines 183–189`.

- The-Game jest `SEPARATE_PROJECT`: 0 rekordów P4 w tym pakiecie; nie klasyfikowano go jako ABM.
- Private runtime, credentials, raw logs/transcripts i payloady eventów są wyłączone; zachowano tylko bezpieczne metadane/locator/hash.
- `OTHER_UNKNOWN`/testowe karty nie są dopisywane do historii ABM; exact IDs są w coverage, poniżej podano tabelę kontroli.

### Snapshot kart

| Snapshot | Liczba | Status | Locator |
|---|---:|---|---|
| ABM-CARD-AUDIT.json | 158 | total | coverage.json.card_snapshot.counts |
| ABM-CARD-AUDIT.json | {'done': 89, 'blocked': 22, 'todo': 13, 'archived': 27, 'triage': 5, 'ready': 1, 'running': 1} | by_status | coverage.json.card_snapshot.counts |
| ABM-CARD-AUDIT.json | {'ABM_CONFIRMED': 34, 'ABM_INFERRED': 64, 'OTHER_UNKNOWN': 28, 'ABM_LEGACY_PROJECT': 24, 'NAGENTS': 8} | by_classification | coverage.json.card_snapshot.counts |
| ABM-CARD-AUDIT.json | {'HISTORY_NO_TOUCH': 109, 'REVIEW_BY_PROJECT_GUIDE': 21, 'HOLD_DECISION_REQUIRED': 12, 'PROCESS_GATE_NO_TOUCH': 7, 'OWNER_ROUTED_NAG': 8, 'ACTIVE_NO_TOUCH': 1} | by_mutation_policy | coverage.json.card_snapshot.counts |
| ABM-CARD-AUDIT.json | {'ABM-': 122, 'OTHER': 28, 'NAG-': 8} | by_prefix | coverage.json.card_snapshot.counts |

`ABM-CARD-AUDIT.json` zachowuje 158 exact task IDs i bezpieczne pola kart. `OTHER_UNKNOWN` = 28 kart poza historią ABM:

| Task ID | Title | Status | Classification |
|---|---|---|---|
| t_e9584658 | Verify event-driven routing and watchdog behavior | done | OTHER_UNKNOWN |
| t_d085b037 | operator | done | OTHER_UNKNOWN |
| t_22ec7d94 | normal fixture | blocked | OTHER_UNKNOWN |
| t_3235d4f3 | dispatch | blocked | OTHER_UNKNOWN |
| t_3305aa8b | canary operator | done | OTHER_UNKNOWN |
| t_5787d91d | canary evaluator | done | OTHER_UNKNOWN |
| t_612d45ea | canary defense | done | OTHER_UNKNOWN |
| t_f3c65bda | canary final-control | done | OTHER_UNKNOWN |
| t_02b1c48e | normal fixture | blocked | OTHER_UNKNOWN |
| t_cb7e0d89 | canary operator | done | OTHER_UNKNOWN |
| t_f3a40121 | canary evaluator | done | OTHER_UNKNOWN |
| t_ff47255f | canary defense | done | OTHER_UNKNOWN |
| t_70658c56 | canary final-control | done | OTHER_UNKNOWN |
| t_dbd1ad4d | normal fixture | blocked | OTHER_UNKNOWN |
| t_905af04c | explicit | blocked | OTHER_UNKNOWN |
| t_7a55d20f | explicit | blocked | OTHER_UNKNOWN |
| t_9fdbf5ee | explicit | done | OTHER_UNKNOWN |
| t_bd877eee | canary operator | done | OTHER_UNKNOWN |
| t_24d114cb | canary evaluator | done | OTHER_UNKNOWN |
| t_963c72d0 | canary defense | done | OTHER_UNKNOWN |
| t_27022270 | canary final-control | done | OTHER_UNKNOWN |
| t_f78c8bca | normal fixture | blocked | OTHER_UNKNOWN |
| t_ce24d086 | canary operator | done | OTHER_UNKNOWN |
| t_557d3249 | canary evaluator | done | OTHER_UNKNOWN |
| t_52fafe25 | canary defense | done | OTHER_UNKNOWN |
| t_464a20a6 | canary final-control | done | OTHER_UNKNOWN |
| t_eca6b3ff | normal fixture | blocked | OTHER_UNKNOWN |
| t_d209d048 | legacy route | blocked | OTHER_UNKNOWN |

### Zasada separacji

`coverage.json.card_snapshot.card_ledger` zawiera task/run/event/artifact IDs ze snapshotu
read-only. Event payloads są jawnie pominięte, a artefakty są reprezentowane wyłącznie
przez ID, nazwę, rozmiar i typ. Brak live mappingu oznacza `UNRESOLVED`/`NOT_IN_CURRENT_BOARD`,
nie brak historycznego zdarzenia.

## Proweniencja i kontrola

Źródła mają bieżący SHA-256, rozmiar, liczbę linii i status Git w `coverage.json.source_ledger`.
P4 pozostaje audytem klasyfikacyjnym; nie jest źródłem live state. Nowy katalog zawiera
wyłącznie `ABM-HISTORY.md`, `coverage.json` i `operator-report.md`.

Status pakietu: `PASS_WITH_EXPLICIT_HISTORY_BOUNDARIES`; publikacja/push/merge/deploy:
`NOT_PERFORMED`.
