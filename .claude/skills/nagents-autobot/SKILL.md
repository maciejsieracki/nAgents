---
name: nagents-autobot
description: >-
  Wiązania procesu AutoBot dla projektu nAgents: format ID tematu, allowlisty,
  izolacja przez worktree, plan testów oparty na scenarios.md, modele i effort
  per rola, limit rund, watchdog, kontrakt raportu z domenami tego projektu,
  rejestr ECHO oraz twarde bariery wynikające z modelu bezpieczeństwa. Używaj
  razem ze skillem `autobots` (szkielet uniwersalny) przy rozpoczęciu pracy,
  przejęciu tematu, dispatchu, kontroli statusu i przygotowaniu integracji
  w repozytorium nAgents.
---

# nAgents — wiązania procesu AutoBot

Ten skill jest **towarzyszem** skilla `autobots`. Szkielet uniwersalny mówi
*jak* prowadzić proces; ten dokument mówi *czym dokładnie* są w nAgents ID,
allowlista, izolacja, testy, modele, bariery i rejestry.

**Kolejność:** przeczytaj `autobots`, potem ten dokument. Przy konflikcie
wygrywa ten dokument — ale konflikt zgłoś właścicielowi, nie rozstrzygaj po cichu.

---

## 1. Punkt startowy i kolejność czytania

Szkielet wymaga dokumentu wskazującego mapę źródeł. W nAgents jest nim
`CLAUDE.md` w katalogu głównym. Kolejność obowiązkowa:

| # | Plik | Po co |
|---|---|---|
| 1 | `CLAUDE.md` | mapa źródeł i norma procesu |
| 2 | `docs/process/tematy.md` | rejestr tematów — co jest aktywne, co zablokowane |
| 3 | `docs/process/handoff.md` | bieżący handoff — stan na teraz |
| 4 | `docs/spec/README.md` | skrót projektu i etapów |
| 5 | `docs/spec/decisions.md` | decyzje, w tym **otwarte i blokujące** |
| 6 | `docs/spec/scenarios.md` | scenariusze — źródło planów testów |
| 7 | `docs/spec/0X-mvpX.md` | specyfikacja bieżącego etapu |

**Nie zaczynaj** od starego handoffu, samego czatu, artefaktów zamkniętych
tematów ani od notatek decyzyjnych `docs/nota-*` — te ostatnie są historią
rozważań, nie aktywnym routingiem.

## 2. Zasada nadrzędna w tym projekcie

Cel to `READY_FOR_DEPLOY`. W nAgents oznacza to łącznie:

- zmiana wyłącznie w zatwierdzonej allowliście
- testy jednostkowe zielone
- **scenariusze z `scenarios.md` przypisane do tematu przechodzą ręcznie**,
  nie tylko automatycznie
- brak nowych sekretów w repozytorium
- wpis w `docs/process/tematy.md` zaktualizowany

Push pozostaje **osobną bramką**. Dodatkowa reguła tego repozytorium:
**push wyłącznie na gałąź wskazaną przez właściciela.** Nigdy na inną,
nawet gdy wygląda na właściwą.

## 3. Format ID tematu

```
NAG-<ETAP>-<NNN>-<slug>
```

Przykłady: `NAG-MVP1-003-uprawnienia`, `NAG-MVP2-011-sync-entra`,
`NAG-PROC-002-rejestr-tematow`.

- `ETAP` ∈ `MVP1` `MVP2` `MVP3` `MVP4` `PROC` (proces/narzędzia) `INFRA`
- `NNN` — trzycyfrowy, rosnący w obrębie etapu, nigdy nieużywany ponownie
- ID jest niezmienne przez wszystkie rundy tematu

Pytanie decyzyjne: `NAG-MVP1-003-uprawnienia-Q2`. Nigdy samo „Q2".

## 4. GOAL i kryteria końca

Każdy temat ma jednozdaniowy `GOAL` oraz kryteria końca **wskazujące numery
scenariuszy** z `docs/spec/scenarios.md`.

```text
GOAL: Pracownik spoza grupy nie dobija się do agenta ani przez interfejs,
      ani z jego pominięciem.
KONIEC: scenariusze A2 i A3 przechodzą; wpisy `deny` widoczne w audycie;
        testy `tests/test_permissions.py` zielone.
```

Kryterium bez numeru scenariusza jest niekompletne — dopisz scenariusz do
`scenarios.md` zanim zaczniesz temat.

## 5. Allowlisty

Allowlista jest jawna, per plik lub per katalog, ustalona **przed** dispatchem.

| Obszar | Typowa allowlista |
|---|---|
| Uprząż — logika | `app/**`, `tests/**` |
| Rejestr agentów | `registry/agents.yaml`, `app/registry/**` |
| Migracje bazy | `migrations/**`, `app/models/**` |
| Wiedza | `knowledge/**` |
| Dokumentacja | `docs/**` |
| Proces | `docs/process/**`, `.claude/skills/**` |

**Nigdy w allowliście, bez wyjątków:**

- `.env`, `.env.*`, dowolny plik z wartościami sekretów
- `docs/spec/decisions.md` — zmienia go wyłącznie orkiestrator po ECHO
- `.git/**`, `docker-compose.prod.yml` bez jawnej zgody właściciela

## 6. Izolacja

```
worktree:  ../nagents-<ID>
branch:    auto/<ID>
baza:      gałąź robocza wskazana przez właściciela (nie zakładaj `main`)
```

Jeden temat = jeden worktree = jeden aktywny przebieg Operatora.
Worktree usuwany po integracji albo po zamknięciu tematu.

## 7. Plan testów

Minimum dla każdego tematu dotykającego kodu:

```text
1. pytest -q                       # całość musi być zielona
2. pytest tests/<obszar> -v        # obszar tematu, szczegółowo
3. scenariusze z KONIEC            # ręcznie, na środowisku dev
4. przy zmianie uprawnień: A2 i A3 obowiązkowo, niezależnie od tematu
5. przy zmianie wiedzy: pełny zestaw testów agenta (od MVP3)
```

Punkt 4 jest bezwarunkowy. Zmiana, która dotyka uprawnień choćby pośrednio,
zawsze przechodzi scenariusze odmowy dostępu.

## 8. Modele i effort per rola

Szkielet zostawia to jako placeholder. Wiązanie dla nAgents:

| Rola | Effort | Uwaga |
|---|---|---|
| Operator | wysoki | pisze kod w izolacji |
| Evaluator | wysoki | adwokat diabła; nigdy ten sam przebieg co Operator |
| Final Control | wysoki | osobny subagent, nigdy główny agent |
| Orkiestrator | model sesji | integruje, wystawia `READY_FOR_DEPLOY` |

**Domyślnie orkiestracja wieloagentowa jest WYŁĄCZONA.** Zgodnie z punktem 3
szkieletu wymaga jawnej, opt-in zgody właściciela na daną sesję. Bez niej
role różnicujemy **wyłącznie treścią promptu**, w jednym wątku.

Właściciel włącza ją zdaniem w rodzaju „zgoda na orkiestrację wieloagentową
w tej sesji". Brak takiego zdania = brak zgody. Nie domniemywaj jej z tego,
że temat jest duży.

## 9. Limit rund i eskalacja

**Limit: 3 rundy na temat.** Szkielet podaje 5 jako przykład; w nAgents
przyjmujemy 3, bo przy jednej osobie technicznej wcześniejsza eskalacja jest
tańsza niż czwarta pętla.

Po trzeciej rundzie orkiestrator zatrzymuje temat i zgłasza właścicielowi:
co próbowano, co zawiodło, jakie są warianty. **Cichy reset licznika jest
naruszeniem procesu.**

## 10. ABC/ECHO — kiedy obowiązkowe w tym projekcie

Formalna decyzja jest **obowiązkowa**, gdy zmiana dotyka któregokolwiek z:

- modelu uprawnień lub sposobu ich egzekwowania
- zakresu danych wysyłanych do dostawcy modelu
- budżetów, limitów i tego, co dzieje się po przekroczeniu
- retencji, audytu lub realizacji praw osób
- topologii agentów (patrz otwarta decyzja D-010)
- rezydencji pamięci (patrz otwarta, blokująca decyzja D-011)
- wyboru silnika, bramy modeli lub bazy

Dla drobnej implementacji w ramach przyjętej decyzji — nie jest wymagana.

**Gdzie zapisujemy:**

| Artefakt | Plik | Zawartość |
|---|---|---|
| ECHO | `docs/process/echo.md` | literalna odpowiedź właściciela, np. `NAG-MVP1-003-Q2 = B`, data, kto |
| ADR | `docs/spec/decisions.md` | uzasadnienie, opcje, konsekwencje — gdy decyzja zmienia architekturę |

ECHO zapisuje się **dopiero po jednoznacznej odpowiedzi literą**.
„Chyba B", „raczej tak" i rekomendacja agenta **nie są** decyzją właściciela.

## 11. Kontrakt raportu — domeny i statusy nAgents

```text
STATUS: PASS | PASS-WITH-NOTES | FAIL | BLOCK | TIMEOUT | INFRA | DECISION_REQUIRED
DOMAIN: PRODUKT | PROCES | INFRA | INFORMACYJNY
TEMAT:  NAG-<ETAP>-<NNN>-<slug>
GOAL:   <jedno zdanie>
ZMIANY: <pliki z allowlisty + SHA commita albo „brak zmian">
TESTY:  <wynik pytest + numery scenariuszy + wynik ręczny>
BLOKADY: <lista albo „brak">
NASTĘPNY KROK: <kolejna bramka>
DEPLOY/PUSH: NIE WYKONANO
```

`DEPLOY/PUSH` domyślnie `NIE WYKONANO`. `WYKONANO` wpisuje wyłącznie
orkiestrator, po jawnym poleceniu właściciela i wyłącznie na wskazaną gałąź.

**Status nie zmienia się** na podstawie nazwy gałęzi, deklaracji agenta ani
tego, że „wygląda na zrobione". Brak artefaktu nie jest dowodem zakończenia.

## 12. Watchdog

| Parametr | Wartość dla nAgents |
|---|---|
| Brak ruchu → `ZWIS` | 20 minut |
| Aktywna pula tematów | **2 równolegle** |
| Po raporcie terminalnym | slot zwolniony natychmiast, następny etap uruchomiony |

Pula wynosi 2, bo wszystko przegląda jedna osoba. Trzeci równoległy temat
przekracza pojemność przeglądu i kończy się integracją bez realnej kontroli.

Przy `ZWIS`: sprawdź transcript, stan worktree i artefakty. **Nie restartuj
w ciemno** — orkiestrator przejmuje temat.

## 13. Twarde bariery projektu

Naruszenie którejkolwiek oznacza **natychmiastowy `FAIL`**, niezależnie od
jakości reszty pracy. Wynikają z modelu bezpieczeństwa w `docs/spec/00-architektura.md`.

1. **Żadnych wartości sekretów w repozytorium.** W rejestrze wyłącznie
   `vault_ref`. Sekret w diffie = `FAIL` i rotacja klucza.
2. **Agent rodzaju `stanowiskowy` nie ma własnych poświadczeń.** Niepusta
   lista `secrets` = `FAIL` na poziomie walidatora rejestru.
3. **Brak dostępu zwraca 404, nie 403.** Zmiana tego zachowania wymaga ECHO.
4. **Domyślna odmowa.** Kod dodający ścieżkę „wszyscy mogą, chyba że" = `FAIL`.
5. **Nigdy `git add -A` ani `git add .`** — integracja allowlist-only,
   per plik, w razie potrzeby per hunk.
6. **Żadnych prawdziwych danych osobowych poza `prod`.** Dane testowe
   generowane skryptem, nie kopiowane z produkcji.
7. **Push wyłącznie na gałąź wskazaną przez właściciela.**

## 14. Recon przed kodowaniem

Przed pierwszą zmianą w temacie:

```bash
git status && git branch --show-current      # czy drzewo czyste
grep -rn "<pojęcie z GOAL>" app/ docs/spec/  # czy to już istnieje
```

Plus lektura: `docs/process/tematy.md` (czy temat nie koliduje z aktywnym),
`docs/spec/decisions.md` (czy decyzja go nie przesądza), `scenarios.md`
(czy scenariusz istnieje).

Przed integracją przejrzyj diff **również pod kątem usunięć** i nakładania
się z drugim aktywnym tematem.

## 15. Meldunek startowy

```text
Przeczytałem: CLAUDE.md, rejestr tematów, handoff, README specyfikacji,
dziennik decyzji, scenariusze, specyfikację bieżącego etapu.

Stan: <etap, tematy aktywne z ID i statusem>
Blokady: <lista, w tym decyzje otwarte>
Następna bramka: <co dokładnie>
Orkiestracja wieloagentowa: wyłączona (brak zgody w tej sesji)

Nie zaczynam zmian przed potwierdzeniem ID, GOAL, allowlisty i decyzji
wymaganych od właściciela.
```
