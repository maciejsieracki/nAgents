# NAG-DOCS-CONSOLIDATION-Q1 — P2 inwentaryzacja Markdownów

STATUS: PASS
DOMAIN: INFORMACYJNY
TEMAT: NAG-DOCS-P2-INVENTORY-Q1
GOAL: Programowo zebrać metadane wszystkich objętych Markdownów lokalnych, zdalnych i handoffowych bez kopiowania treści.
OBSERVED_AT: 2026-09-14T13:56:47+00:00

## Podział artefaktów

- `P2-inventory-local.jsonl`: nAgents checkout/worktree oraz AutoBot Monitor checkout i aktywny worktree relayu.
- `P2-inventory-github.jsonl`: trzy nazwane refy GitHuba, odczytane z drzew commitów wskazanych w P1.
- `P2-inventory-external.jsonl`: jawnie wskazane handoffy nAgents z `/home/ubuntu/handoffs`; JSON handoffów nie należy do inwentaryzacji Markdownów.
- `P2-counts.json`: liczniki, walidacja, readback refów i pełne listy różnic.

## Metoda i definicje

- Jednostka: `source_id + source_root + ref + relative_path`; basename nie jest tożsamością.
- `line_count` to liczba bajtów LF, zgodna z `wc -l`; `sha256` liczony z surowych bajtów pliku.
- Nagłówki to nagłówki ATX oraz setext; `top_level_headings` zawiera tylko poziom 1.
- `freshness_status`: `FRESH` ≤7 dni, `RECENT` 8–30 dni, `STALE` >30 dni; lokalnie względem mtime, zdalnie względem ostatniego commita ścieżki.
- `LOCAL_ONLY`/`REMOTE_ONLY` porównują względne ścieżki w obrębie repozytorium, względem unii wszystkich nazwanych refów i lokalnych snapshotów.
- Dla archiwum handoffów porównanie GitHub nie ma zastosowania (`NOT_APPLICABLE_EXTERNAL_ARCHIVE`).

## Liczniki

- Rekordy lokalne: **695**.
- Rekordy GitHub: **104**.
- Rekordy zewnętrznego archiwum handoffów: **34**.
- Rekordy łącznie: **833**.
- `LOCAL_ONLY`: **132** rekordów.
- `REMOTE_ONLY`: **0** rekordów.

### Lokalne według source_id

| source_id | rekordy |
|---|---:|
| `ABM_ACTIVE_RELAY_WORKTREE` | 9 |
| `ABM_CHECKOUT` | 161 |
| `NAGENTS_AUX_WORKTREES` | 474 |
| `NAGENTS_CHECKOUT` | 51 |

### GitHub według refu

| source_id | rekordy |
|---|---:|
| `GITHUB_ABM_MAIN_REF` | 56 |
| `GITHUB_NAGENTS_MAIN_REF` | 1 |
| `GITHUB_NAGENTS_WORK_REF` | 47 |

### Handoffy według source_id

| source_id | rekordy |
|---|---:|
| `HANDOFFS_NAGENTS` | 34 |

## Różnice LOCAL_ONLY

Pełna lista jest także w `P2-counts.json`; poniżej każdy rekord lokalny:
- `ABM_ACTIVE_RELAY_WORKTREE` / `primary` / `docs/CRON-DIRECTIVE-LOOP.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/.worktrees/t_771608cd/docs/CRON-DIRECTIVE-LOOP.md`
- `ABM_ACTIVE_RELAY_WORKTREE` / `primary` / `docs/OWNER-CHAT-RELAY.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/.worktrees/t_771608cd/docs/OWNER-CHAT-RELAY.md`
- `ABM_ACTIVE_RELAY_WORKTREE` / `primary` / `runs/ABM-OWNER-CHAT-RELAY-001/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/.worktrees/t_771608cd/runs/ABM-OWNER-CHAT-RELAY-001/00-dispatch.md`
- `ABM_ACTIVE_RELAY_WORKTREE` / `primary` / `runs/ABM-OWNER-CHAT-RELAY-001/01-operator.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/.worktrees/t_771608cd/runs/ABM-OWNER-CHAT-RELAY-001/01-operator.md`
- `ABM_ACTIVE_RELAY_WORKTREE` / `primary` / `runs/ABM-OWNER-CHAT-RELAY-001/02-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/.worktrees/t_771608cd/runs/ABM-OWNER-CHAT-RELAY-001/02-evaluator-dispatch.md`
- `ABM_ACTIVE_RELAY_WORKTREE` / `primary` / `runs/ABM-OWNER-CHAT-RELAY-001/02-evaluator.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/.worktrees/t_771608cd/runs/ABM-OWNER-CHAT-RELAY-001/02-evaluator.md`
- `ABM_ACTIVE_RELAY_WORKTREE` / `primary` / `runs/ABM-OWNER-CHAT-RELAY-001/03-defense.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/.worktrees/t_771608cd/runs/ABM-OWNER-CHAT-RELAY-001/03-defense.md`
- `ABM_ACTIVE_RELAY_WORKTREE` / `primary` / `runs/ABM-OWNER-CHAT-RELAY-001/03-evaluator-r2-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/.worktrees/t_771608cd/runs/ABM-OWNER-CHAT-RELAY-001/03-evaluator-r2-dispatch.md`
- `ABM_ACTIVE_RELAY_WORKTREE` / `primary` / `runs/ABM-OWNER-CHAT-RELAY-001/04-evaluator-r2.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/.worktrees/t_771608cd/runs/ABM-OWNER-CHAT-RELAY-001/04-evaluator-r2.md`
- `ABM_CHECKOUT` / `primary` / `AUTOBOT-KANBAN.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/AUTOBOT-KANBAN.md`
- `ABM_CHECKOUT` / `primary` / `PLAN-AUTOBOT-PLUGIN.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/PLAN-AUTOBOT-PLUGIN.md`
- `ABM_CHECKOUT` / `primary` / `docs/ABM-CARD-ROUTING-AUDIT.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/docs/ABM-CARD-ROUTING-AUDIT.md`
- `ABM_CHECKOUT` / `primary` / `docs/ABM-CARD-TAGGING-GUIDE.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/docs/ABM-CARD-TAGGING-GUIDE.md`
- `ABM_CHECKOUT` / `primary` / `docs/ABM-CRON-HELPER-OPERATING-RUNBOOK.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/docs/ABM-CRON-HELPER-OPERATING-RUNBOOK.md`
- `ABM_CHECKOUT` / `primary` / `docs/ABM-MODEL-REPAIR-PLAN.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/docs/ABM-MODEL-REPAIR-PLAN.md`
- `ABM_CHECKOUT` / `primary` / `docs/ABM-REMOTE-DESKTOP-SERVER-PLAN.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/docs/ABM-REMOTE-DESKTOP-SERVER-PLAN.md`
- `ABM_CHECKOUT` / `primary` / `docs/ABM-SAME-PROFILE-CRON-MIGRATION.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/docs/ABM-SAME-PROFILE-CRON-MIGRATION.md`
- `ABM_CHECKOUT` / `primary` / `docs/AUTOBOT-MODEL-EFFORT-FAST-POLICY.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/docs/AUTOBOT-MODEL-EFFORT-FAST-POLICY.md`
- `ABM_CHECKOUT` / `primary` / `docs/CRON-DIRECTIVE-LOOP.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/docs/CRON-DIRECTIVE-LOOP.md`
- `ABM_CHECKOUT` / `primary` / `docs/UPGRADE-BACKUP-AND-GITHUB-POLICY.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/docs/UPGRADE-BACKUP-AND-GITHUB-POLICY.md`
- `ABM_CHECKOUT` / `primary` / `docs/handoffs/ABM-SAME-PROFILE-OWNER-HANDOFF.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/docs/handoffs/ABM-SAME-PROFILE-OWNER-HANDOFF.md`
- `ABM_CHECKOUT` / `primary` / `hermes-patches/2026-09-12/README.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/hermes-patches/2026-09-12/README.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ARCH-001/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ARCH-001/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-001/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-001/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/00-defense-r3-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/00-defense-r3-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/00-luna-defense-r5-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/00-luna-defense-r5-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/00-luna-final-control-r5-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/00-luna-final-control-r5-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/00-luna-repair-evaluator-r5-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/00-luna-repair-evaluator-r5-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/00-luna-repair-final-control-r5-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/00-luna-repair-final-control-r5-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/00-luna-repair-r5-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/00-luna-repair-r5-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/01-luna-operator-r5.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/01-luna-operator-r5.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/01-luna-repair-r5.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/01-luna-repair-r5.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/01-operator-r3.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/01-operator-r3.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/01-repair-operator-r4.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/01-repair-operator-r4.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/02-evaluator-r3-review.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/02-evaluator-r3-review.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/02-evaluator-r3.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/02-evaluator-r3.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/02-luna-evaluator-r5.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/02-luna-evaluator-r5.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/02-luna-repair-evaluator-r5.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/02-luna-repair-evaluator-r5.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/03-defense-r3.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/03-defense-r3.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/03-luna-defense-r5.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/03-luna-defense-r5.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/04-final-control-r3.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/04-final-control-r3.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/04-luna-final-control-r5.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/04-luna-final-control-r5.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-ASTRA-002/04-luna-repair-final-control-r5.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-ASTRA-002/04-luna-repair-final-control-r5.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-BRIDGE-001/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-BRIDGE-001/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-MODEL-001/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-MODEL-001/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-MONITOR-001/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-MONITOR-001/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-MONITOR-001/01-operator.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-MONITOR-001/01-operator.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-MONITOR-001/02-evaluator.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-MONITOR-001/02-evaluator.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-MONITOR-001/03-defense.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-MONITOR-001/03-defense.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-MONITOR-001/04-final-control.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-MONITOR-001/04-final-control.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-MONITOR-002/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-MONITOR-002/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-MONITOR-002/01-backend-operator.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-MONITOR-002/01-backend-operator.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-MONITOR-002/01-desktop-operator.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-MONITOR-002/01-desktop-operator.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-MONITOR-002/02-ABM-MONITOR-002:backend:operator:r1-evaluator.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-MONITOR-002/02-ABM-MONITOR-002:backend:operator:r1-evaluator.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-MONITOR-002/02-ABM-MONITOR-002:desktop:operator:r1-evaluator.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-MONITOR-002/02-ABM-MONITOR-002:desktop:operator:r1-evaluator.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-MONITOR-002/03-ABM-MONITOR-002:backend:operator:r1-final-control.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-MONITOR-002/03-ABM-MONITOR-002:backend:operator:r1-final-control.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-MONITOR-002/04-ABM-MONITOR-002:desktop:operator:r1-final-control.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-MONITOR-002/04-ABM-MONITOR-002:desktop:operator:r1-final-control.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-MONITOR-VISIBILITY-001/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-MONITOR-VISIBILITY-001/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-MONITOR-VISIBILITY-001/01-operator.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-MONITOR-VISIBILITY-001/01-operator.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-PLAN-001/00-watchdog-repair-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-PLAN-001/00-watchdog-repair-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-PLUGIN-001/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-PLUGIN-001/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-PROFILE-001/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-PROFILE-001/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/00-topic-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/00-topic-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/a/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/a/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/b/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/b/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/b/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/b/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/c/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/c/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/c/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/c/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/d/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/d/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/d/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/d/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/e/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/e/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/e/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/e/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/f/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/f/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/f/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/f/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/g/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/g/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/g/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/g/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/h/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/h/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/h/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/h/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/i/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/i/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/i/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/i/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/j/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/j/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/j/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/j/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/k/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/k/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/k/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/k/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/l/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/l/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/l/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/l/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/m/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/m/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/m/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/m/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/n/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/n/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/n/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/n/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/o/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/o/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/o/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/o/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/p/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/p/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/p/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/p/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/q/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/q/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/q/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/q/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/r/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/r/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/r/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/r/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/s/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/s/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/s/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/s/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/t/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/t/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/t/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/t/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/u/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/u/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/u/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/u/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/v/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/v/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/v/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/v/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/w/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/w/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/w/00-evaluator-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/w/00-evaluator-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-H-001/x/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-H-001/x/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-P0-EVAL-R3/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-P0-EVAL-R3/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-P0-R2/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-P0-R2/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-V2-P0-R3/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-V2-P0-R3/00-dispatch.md`
- `ABM_CHECKOUT` / `primary` / `runs/ABM-WEB-001/00-dispatch.md` — `maciejsieracki/Autoboot-Monitor` — `/home/ubuntu/projects/Autoboot-Monitor/runs/ABM-WEB-001/00-dispatch.md`
- `NAGENTS_AUX_WORKTREES` / `wt-NAG-INFRA-001-interfejs-hermesa` / `docs/nota-09-interfejs-hermesa.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/wt-NAG-INFRA-001-interfejs-hermesa/docs/nota-09-interfejs-hermesa.md`
- `NAGENTS_AUX_WORKTREES` / `wt-NAG-INFRA-002-entra-instrukcja` / `docs/nota-10-entra-instrukcja-dla-administratora.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/wt-NAG-INFRA-002-entra-instrukcja/docs/nota-10-entra-instrukcja-dla-administratora.md`
- `NAGENTS_AUX_WORKTREES` / `wt-NAG-MVP1-003-uprawnienia` / `docs/nota-09-interfejs-hermesa.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/wt-NAG-MVP1-003-uprawnienia/docs/nota-09-interfejs-hermesa.md`
- `NAGENTS_AUX_WORKTREES` / `wt-NAG-MVP1-003-uprawnienia` / `docs/nota-10-entra-instrukcja-dla-administratora.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/wt-NAG-MVP1-003-uprawnienia/docs/nota-10-entra-instrukcja-dla-administratora.md`
- `NAGENTS_AUX_WORKTREES` / `wt-NAG-MVP1-005-rozmowa` / `docs/nota-09-interfejs-hermesa.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/wt-NAG-MVP1-005-rozmowa/docs/nota-09-interfejs-hermesa.md`
- `NAGENTS_AUX_WORKTREES` / `wt-NAG-MVP1-005-rozmowa` / `docs/nota-10-entra-instrukcja-dla-administratora.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/wt-NAG-MVP1-005-rozmowa/docs/nota-10-entra-instrukcja-dla-administratora.md`
- `NAGENTS_AUX_WORKTREES` / `wt-NAG-MVP1-006-proxy-hermes` / `docs/nota-09-interfejs-hermesa.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/wt-NAG-MVP1-006-proxy-hermes/docs/nota-09-interfejs-hermesa.md`
- `NAGENTS_AUX_WORKTREES` / `wt-NAG-MVP1-006-proxy-hermes` / `docs/nota-10-entra-instrukcja-dla-administratora.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/wt-NAG-MVP1-006-proxy-hermes/docs/nota-10-entra-instrukcja-dla-administratora.md`
- `NAGENTS_AUX_WORKTREES` / `wt-NAG-MVP1-007-brama-modeli` / `docs/nota-09-interfejs-hermesa.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/wt-NAG-MVP1-007-brama-modeli/docs/nota-09-interfejs-hermesa.md`
- `NAGENTS_AUX_WORKTREES` / `wt-NAG-MVP1-007-brama-modeli` / `docs/nota-10-entra-instrukcja-dla-administratora.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/wt-NAG-MVP1-007-brama-modeli/docs/nota-10-entra-instrukcja-dla-administratora.md`
- `NAGENTS_AUX_WORKTREES` / `wt-NAG-MVP1-008-audyt` / `docs/nota-09-interfejs-hermesa.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/wt-NAG-MVP1-008-audyt/docs/nota-09-interfejs-hermesa.md`
- `NAGENTS_AUX_WORKTREES` / `wt-NAG-MVP1-008-audyt` / `docs/nota-10-entra-instrukcja-dla-administratora.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/wt-NAG-MVP1-008-audyt/docs/nota-10-entra-instrukcja-dla-administratora.md`
- `NAGENTS_AUX_WORKTREES` / `wt-NAG-MVP1-009-wdrozenie` / `docs/nota-09-interfejs-hermesa.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/wt-NAG-MVP1-009-wdrozenie/docs/nota-09-interfejs-hermesa.md`
- `NAGENTS_AUX_WORKTREES` / `wt-NAG-MVP1-009-wdrozenie` / `docs/nota-10-entra-instrukcja-dla-administratora.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/wt-NAG-MVP1-009-wdrozenie/docs/nota-10-entra-instrukcja-dla-administratora.md`
- `NAGENTS_CHECKOUT` / `primary` / `docs/nota-09-interfejs-hermesa.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/nAgents-readonly/docs/nota-09-interfejs-hermesa.md`
- `NAGENTS_CHECKOUT` / `primary` / `docs/nota-10-entra-instrukcja-dla-administratora.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/nAgents-readonly/docs/nota-10-entra-instrukcja-dla-administratora.md`
- `NAGENTS_CHECKOUT` / `primary` / `docs/process/NAGENTS-DOCS-CONSOLIDATION-PLAN.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/nAgents-readonly/docs/process/NAGENTS-DOCS-CONSOLIDATION-PLAN.md`
- `NAGENTS_CHECKOUT` / `primary` / `docs/process/dispatch/NAG-INFRA-002-pomocnik-serwerowy.md` — `maciejsieracki/nAgents` — `/home/ubuntu/projects/nAgents-readonly/docs/process/dispatch/NAG-INFRA-002-pomocnik-serwerowy.md`

## Różnice REMOTE_ONLY

Pełna lista jest także w `P2-counts.json`; poniżej każdy rekord zdalny:
- brak

## Readback i odstępstwa

- `GITHUB_NAGENTS_WORK_REF`: live `ls-remote` = `fad6f26c48d31ea7cb5a3e4aa8ad53b5da14ab81`, P1 SHA = `fad6f26c48d31ea7cb5a3e4aa8ad53b5da14ab81`, tree entries = 56, blob entries = 47, Markdown = 47; zgodność: `PASS`.
- `GITHUB_NAGENTS_MAIN_REF`: live `ls-remote` = `9522836d79679f5296ad23822c9a8efba9f949fb`, P1 SHA = `9522836d79679f5296ad23822c9a8efba9f949fb`, tree entries = 1, blob entries = 1, Markdown = 1; zgodność: `PASS`.
- `GITHUB_ABM_MAIN_REF`: live `ls-remote` = `6b057bad349ccda4c4b8ec5c26ae03eea81b635a`, P1 SHA = `6b057bad349ccda4c4b8ec5c26ae03eea81b635a`, tree entries = 228, blob entries = 193, Markdown = 56; zgodność: `PASS`.
- P1 structured source list declares **34** handoff Markdown files and all **34** are present. P1 prose reported 36; this inventory follows the structured allowlist (34).
- Live ABM checkout selection contains **161** Markdown records versus P1 baseline 160 (delta +1); current readback is retained, not silently clipped.
- P1 outputs, metadata-only ABM worktrees, The-Game and other external candidates were not opened as Markdown sources.

TESTY: independentny odczyt `git status`, `git ls-files`, `git ls-tree`, `git ls-remote`, stat, LF count, nagłówków i SHA-256; JSONL parse/readback; unikalność rekordów; pełne listy LOCAL_ONLY/REMOTE_ONLY w tym dokumencie i `P2-counts.json`.
BLOKADY: brak; źródła nieobjęte zakresem pozostają jawnie sklasyfikowane w `P2-counts.json`.
NASTĘPNY KROK: P3 — przypisać każdą kategorię pytań do jednego źródła prawdy na podstawie tego inwentarza.
DEPLOY/PUSH: NIE WYKONANO
