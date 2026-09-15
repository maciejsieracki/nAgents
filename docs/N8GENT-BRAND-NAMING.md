# 8gent — zasada nazewnictwa

## Marka widoczna dla ludzi

Bieżąca nazwa marki projektu to:

```text
8gent
```

Wymowa: **„eight-gent”**.

Używaj `8gent` w nowych nagłówkach, opisach, instrukcjach dla użytkownika i
tekstach promocyjno-organizacyjnych.

## Identyfikatory techniczne pozostają bez zmian

Nie zmieniaj mechanicznych identyfikatorów, bo są częścią routingu, historii lub
proweniencji:

- repozytorium `maciejsieracki/nAgents`;
- nazwy plików `NAGENTS-*`;
- prefiks kart `NAG-`;
- tenant `nagents-docs`;
- projekt Hermes `nagents-docs` / `p_e90c30bc`;
- board `autobot-monitor`;
- ścieżki, branche, task ID, run ID, event ID, hashy i klucze idempotencji;
- nazwy historycznych raportów i snapshotów;
- literalne komendy oraz fragmenty kodu.

Jeżeli dokument opisuje techniczny identyfikator, zachowaj go dokładnie i nie
zamieniaj go na nazwę marki.

## Historia i evidence

Nie przepisuj historycznych raportów, auditów, coverage ani receiptów wyłącznie
po to, aby zmienić nazwę. Muszą zachować treść, hashe i proweniencję. W razie
potrzeby użyj zdania:

> W starszych artefaktach nazwa `nAgents` oznacza obecną markę `8gent`.

## Reguła dla nowych dokumentów

- tekst dla człowieka: `8gent`;
- techniczny locator: literalny `nAgents`/`NAGENTS-*`;
- decyzja, status, routing lub live state: odczyt źródła technicznego, nie z samej
  marki.
