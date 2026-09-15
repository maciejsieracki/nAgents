# Rejestr tematów

Źródło prawdy o tym, co jest aktywne, zablokowane i zamknięte.
Aktualizowany przez orkiestratora przy każdej zmianie statusu tematu.

Format ID: `NAG-<ETAP>-<NNN>-<slug>`. ID jest niezmienne i nigdy nieużywane ponownie.

---

## Aktywne

| ID | GOAL | Status |
|---|---|---|
| `NAG-INFRA-002-pomocnik-serwerowy` | Serwerowy pomocnik odbiera dyspozycje Crona i prowadzi kwalifikowane przejścia Kanbana bez Desktopu | wariant A przyjęty; implementacja i canary do wykonania |

Tematy MVP1-001, MVP1-003, MVP1-004, MVP1-005, MVP1-006, MVP1-007, MVP1-008 oraz zakresy infrastrukturalne zatwierdzone przez petle AutoBot sa zamkniete lokalnie; push wymaga osobnej zgody.

## Zablokowane

| ID | GOAL | Blokada | Odblokuje |
|---|---|---|---|
| `NAG-MVP1-009-wdrozenie` | Compose, TLS, backup i restore | DECISION_REQUIRED: serwer, domena/TLS, retencja i magazyn kopii, szyfrowanie, sekrety i przypiete obrazy | decyzja wlasciciela |
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
| `NAG-MVP1-001-szkielet` | Aplikacja startuje, `healthz` odpowiada, migracje dzialaja | 2026-09-11 | commit `6e74dd2`, Final Control PASS |
| `NAG-MVP1-003-uprawnienia` | Osoba spoza grupy nie dobija sie do agenta ani przez UI, ani z jego pominieciem | 2026-09-11 | commit `d7db7c8`, Final Control PASS; 43 testy |
| `NAG-MVP1-004-rejestr` | Rejestr w YAML wczytywany do bazy; `validate` i `apply` idempotentne | 2026-09-11 | commit `d57d10d`, Final Control PASS; 27 testow |
| `NAG-MVP1-005-rozmowa` | Ekran rozmowy ze strumieniowaniem odpowiedzi | 2026-09-11 | commit `dadc5de`, Final Control PASS; 45 testow |
| `NAG-MVP1-006-proxy-hermes` | Wiadomosc trafia do wlasciwego profilu, a identyfikator zadania przechodzi przez warstwy | 2026-09-11 | commit `378e540`, Final Control PASS; 47 testow |
| `NAG-MVP1-007-brama-modeli` | Wszystkie wywolania przechodza przez brame; wyczerpany budzet zatrzymuje agenta z czytelnym komunikatem | 2026-09-11 | commit `d768bb8`, Final Control PASS; 50 testow |
| `NAG-MVP1-008-audyt` | Operacje dozwolone i odrzucone zapisane; widok dziennika | 2026-09-11 | commit `6b93e25`, Final Control PASS; 55 testow; ECHO wariant B |
| `NAG-PROC-001-dokumentacja` | Architektura i cztery etapy opisane | 2026-08-22 | `docs/spec/` |
| `NAG-PROC-002-proces` | Proces AutoBot związany z projektem | 2026-08-22 | `CLAUDE.md`, `.claude/skills/` |
| `NAG-PROC-005-skill-uniwersalny` | Skill niezależny od dziedziny, sekcje 17 do 21 | 2026-08-23 | `.claude/skills/nagents-autobot/SKILL.md`, `README.md` |
| `NAG-PROC-006-ulotka-dla-pracownikow` | Dokument informacyjny o zasadzie AutoBot dla pracowników | 2026-08-23 | `docs/proces-dla-pracownikow.md` |
| `NAG-INFO-001-appto-research` | Wiemy, co appto faktycznie robi, jak zarządza dostępem i kosztem, na czym stoi | 2026-08-25 | Blokadę (403 na bramce wyjściowej) obeszło dostarczenie czterech stron appto przez właściciela wklejeniem treści — `docs/nota-06-appto-research.md` |
| `NAG-INFO-002-katalog-funkcji` | Katalog funkcji appto zestawiony z naszą specyfikacją, z korektą przesłanki D-001 do decyzji właściciela | 2026-08-25 | `docs/nota-06-appto-research.md`, `docs/nota-07-katalog-funkcji.md` |
| `NAG-INFO-003-dokumenty-prawne` | Polityka prywatności i regulamin appto (z umową powierzenia) zestawione z materiałem sprzedażowym; sprostowanie wcześniejszego błędnego twierdzenia o braku umowy powierzenia; ryzyka dostawcy i wnioski dla własnej budowy | 2026-08-25 | `docs/nota-06-appto-research.md`, `docs/nota-07-katalog-funkcji.md` |
| `NAG-DEC-001-wybory-otwarte` | Osiem otwartych wyborów technicznych (w tym integracje — dotąd bez wpisu w `decisions.md`, oraz D-011) zestawionych w pytania z wariantami, ceną i ryzykiem, językiem właściciela; czeka na odpowiedzi literą w `docs/spec/decisions.md` | 2026-08-25 | `docs/process/pytania/2026-08-25-wybory.md`, `docs/nota-08-wybory-otwarte.md` |
