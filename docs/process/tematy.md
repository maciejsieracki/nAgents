# Rejestr tematów

Źródło prawdy o tym, co jest aktywne, zablokowane i zamknięte.
Aktualizowany przez orkiestratora przy każdej zmianie statusu tematu.

Format ID: `NAG-<ETAP>-<NNN>-<slug>`. ID jest niezmienne i nigdy nieużywane ponownie.

---

## Aktywne

*Brak. Projekt jest przed MVP1 — dokumentacja gotowa, kod nie istnieje.*

## Zablokowane

| ID | GOAL | Blokada | Odblokuje |
|---|---|---|---|
| `NAG-MVP3-001-pamiec-wspolna` | Agenci domeny dzielą pamięć procesową | **D-011** — nierozstrzygnięta rezydencja danych | odpowiedź właściciela o bazie |

## Do rozpoczęcia — MVP1

Kolejność wynika z podziału pracy w [`docs/spec/01-mvp1.md`](../spec/01-mvp1.md).
Allowlisty i plany testów ustalane przy dispatchu, nie z góry.

| ID | GOAL | Scenariusze | Zależy od |
|---|---|---|---|
| `NAG-MVP1-001-szkielet` | Aplikacja startuje, `healthz` odpowiada, migracje działają | — | — |
| `NAG-MVP1-002-logowanie` | Pracownik loguje się kontem firmowym; wyłączone konto nie wchodzi | A3 | rejestracja w Entra ID |
| `NAG-MVP1-003-uprawnienia` | Osoba spoza grupy nie dobija się do agenta ani przez UI, ani z jego pominięciem | A1, A2 | 001 |
| `NAG-MVP1-004-rejestr` | Rejestr w YAML wczytywany do bazy; `validate` i `apply` idempotentne | — | 001 |
| `NAG-MVP1-005-rozmowa` | Ekran rozmowy ze strumieniowaniem odpowiedzi | A1 | 003 |
| `NAG-MVP1-006-proxy-hermes` | Wiadomość trafia do właściwego profilu; identyfikator żądania przechodzi przez warstwy | A1 | 005 |
| `NAG-MVP1-007-brama-modeli` | Wszystkie wywołania przez bramę; wyczerpany budżet zatrzymuje agenta | K1, K2, K5 | 006 |
| `NAG-MVP1-008-audyt` | Operacje dozwolone i odrzucone zapisane; widok dziennika | A2 | 003 |
| `NAG-MVP1-009-wdrozenie` | Compose na serwerze, TLS, kopia dobowa, **przećwiczone odtworzenie** | C1 | 007, 008 |
| `NAG-MVP1-010-pilot-rozliczenia` | Trzy zamknięte miesiące zgadzają się co do złotówki z liczeniem ręcznym | R1, R2, R3, R4 | 009 |

**`NAG-MVP1-010` jest kryterium biznesowym całego etapu.** Reszta to warunki techniczne.

## Zamknięte

| ID | GOAL | Zamknięty | Wynik |
|---|---|---|---|
| `NAG-PROC-001-dokumentacja` | Architektura i cztery etapy opisane | 2026-08-22 | `docs/spec/` |
| `NAG-PROC-002-proces` | Proces AutoBot związany z projektem | 2026-08-22 | `CLAUDE.md`, `.claude/skills/` |
| `NAG-PROC-005-skill-uniwersalny` | Skill niezależny od dziedziny, sekcje 17 do 21 | 2026-08-23 | `.claude/skills/nagents-autobot/SKILL.md`, `README.md` |
| `NAG-PROC-006-ulotka-dla-pracownikow` | Dokument informacyjny o zasadzie AutoBot dla pracowników | 2026-08-23 | `docs/proces-dla-pracownikow.md` |
| `NAG-INFO-001-appto-research` | Wiemy, co appto faktycznie robi, jak zarządza dostępem i kosztem, na czym stoi | 2026-08-25 | Blokadę (403 na bramce wyjściowej) obeszło dostarczenie czterech stron appto przez właściciela wklejeniem treści — `docs/nota-06-appto-research.md` |
| `NAG-INFO-002-katalog-funkcji` | Katalog funkcji appto zestawiony z naszą specyfikacją, z korektą przesłanki D-001 do decyzji właściciela | 2026-08-25 | `docs/nota-06-appto-research.md`, `docs/nota-07-katalog-funkcji.md` |
