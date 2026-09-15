STATUS: PASS
DOMAIN: INFORMACYJNY
TEMAT: NAG-CONSOLIDATE-SPEC-Q1
P4_STATUS: PASS_WITH_EXPLICIT_OWNER_GATES
GOAL: Zbudować addytywny staging pakietu NAGENTS-SPEC.md na podstawie zweryfikowanej macierzy P6, zachowując unikalną treść i proweniencję.

ZMIANY:
- Utworzono wyłącznie katalog `docs/process/staging/NAG-CONSOLIDATE-SPEC-Q1/`.
- `NAGENTS-SPEC.md` zawiera siedem oznaczonych sekcji SPEC-01..SPEC-07, macierz źródeł, bieżące zasady web-first/serwer, pełny kontrakt identyfikatorów scenariuszy, RBAC i oznaczone legacy.
- `coverage.json` zachowuje 13 rodzin i 101 rekordów P4 dla NAG-SPEC (8/96), NAG-RBAC (1/1) i NAG-LEGACY-SPEC (4/4), z hashami, statusami, konfliktami i śladem `unique_content`.

TESTY:
- 7/7 sekcji ma status, źródło, kryterium i locator; 46/46 identyfikatorów A/R/W/K/P/C/U zachowano w artefakcie.
- Bieżący readback 10 wskazanych lokalnych źródeł zgodny z zapisanymi SHA-256; P4 JSON i P6 plan parsują się poprawnie.
- Sprawdzono rekordy P4: 8/96 + 1/1 + 4/4, wszystkie z path/hash/status/relation; warianty hashów i drift `NAGENTS-PROJECT.md` oznaczono jawnie.
- `NAGENTS-SPEC.md`: 562 linii, SHA-256 `3c431da703b7b53c7f4d7e6b655d92ef5283558f0161f481754edea7a6ddf855`.
- `coverage.json`: 403585 bajtów, SHA-256 `d3c1b077b21dc00d095b67c4af83700386fa3e91e96da402097881e0ea172529`.

BLOKADY: Owner gates P6 pozostają otwarte: wybór zakresu/pakietu, legacy, usunięcia/przeniesienia i publikacja. Nie kopiowano raw legacy, sekretów, PII ani runtime.
NASTĘPNY KROK: Niezależny Evaluator dla tego samego topicu; Defense tylko przy numerowanych zarzutach, potem Final Control. Pakiet nie jest publikacją.
DEPLOY/PUSH: NIE WYKONANO
