# Raport redakcji danych wrażliwych

Zakres: dokumentacja przygotowywana do publikacji w publicznym repozytorium
`maciejsieracki/nAgents`.

Wartości adresów e-mail i adresów IPv4 znalezione w lokalnych plikach zostały
zastąpione literalnym znacznikiem `[REDACTED]`. Raport nie zawiera wartości
oryginalnych.

## Zmienione pliki

| Plik | Redakcje e-mail | Redakcje IPv4 |
|---|---:|---:|
| `HANDOFF-nagents.md` | 1 | 1 |
| `docs/nota-09-interfejs-hermesa.md` | 0 | 1 |
| `docs/process/pytania/2026-08-22-zestaw-1.md` | 1 | 0 |
| `docs/process/zrodla/appto-polityka-prywatnosci-pl.md` | 1 | 0 |
| `docs/process/zrodla/appto-regulamin-pl.md` | 2 | 0 |
| `docs/spec/01-mvp1.md` | 1 | 0 |
| `docs/spec/02-mvp2.md` | 3 | 0 |
| `registry/agents.yaml` | 2 | 0 |
| **Razem** | **11** | **2** |

## Kontrola

Po redakcji należy wykonać pełny skan publicznego zestawu. Wymagany wynik:

```text
private-key: 0
bearer: 0
JWT: 0
IPv4: 0
e-mail: 0
credential-assignment: 0
```

Redakcja nie jest dowodem poprawności runtime ani zgody na publikację. Jest
wyłącznie usunięciem danych, których nie wolno publikować.
