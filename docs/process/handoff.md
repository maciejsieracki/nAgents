# Handoff

Stan na teraz. Zastępowany w całości przy każdym przekazaniu — to nie jest log,
tylko zdjęcie bieżącej sytuacji.

**Ostatnia aktualizacja:** 2026-08-22 (po zestawie pytań nr 1)

---

## Gdzie jesteśmy

Projekt jest **przed MVP1**. Dokumentacja techniczna kompletna, kod nie istnieje.
Repozytorium zawiera specyfikację czterech etapów, dziennik decyzji, scenariusze
oraz proces AutoBot związany z projektem.

Gałąź robocza: `claude/git-connection-9sz6dg`. Drzewo czyste.

## Co zostało ustalone

- Budujemy własną warstwę zarządzania na Hermesie (**D-001**). Gotowa platforma
  odpadła, bo rozliczenie za tokeny u dostawcy odbiera swobodę wyboru modelu.
- Brama modeli należy do nas (**D-002**) — to fundament, nie detal.
- Cztery etapy: MVP1 spięcie (10–12 dni), MVP2 zarządzalność (12–15),
  MVP3 wiedza (18–22), MVP4 skala (20–25).

## Co blokuje

| Co | Skutek | Kto odblokuje | Zegar |
|---|---|---|---|
| Rejestracja aplikacji w Entra ID | MVP1 staje w dniu trzecim | administrator dzierżawy | nieznany |
| **Umowa powierzenia z dostawcą modelu** | scenariusz 8 nie ruszy — dni 11–12 | prawnik + dostawca | **nieznany, proces prawny** |
| **Zawiadomienie pracowników o monitoringu** | pilot nie ruszy legalnie | właściciel + kadry | **ustawowe ~2 tygodnie** |
| **D-011** — rezydencja wspólnej pamięci | MVP3 nie startuje | właściciel, po informacji o bazie | — |

**Korekta z 2026-08-22.** Poprzednia wersja tego dokumentu nazywała rejestrację
w Entra ID „jedyną zależnością zewnętrzną" etapu pierwszego. **To była nieprawda.**
Final Control wykazał dwie kolejne zależności tego samego rodzaju — procesy poza
kontrolą zespołu technicznego, o nieznanym czasie trwania, na krytycznej ścieżce.

Zawiadomienie pracowników jest z nich najgroźniejsze: jego zegar jest **dłuższy
niż większość etapu MVP1**. Jeśli nie ruszy w dniu pierwszym, to ono, a nie kod,
zatrzyma pilota w dniu dziesiątym.

**Wszystkie trzy uruchomić równolegle, przed pierwszą linijką kodu.**

## Decyzje czekające na właściciela

| ID | Pytanie | Termin |
|---|---|---|
| **D-010** | Topologia: 27 profili czy sześć agentów plus poziomy kontekstu? | przed MVP3 |
| **D-011** | Gdzie fizycznie mieszka wspólna pamięć? Wariant (c) — rezygnacja z niej — jest realny | przed MVP3 |

## Następna bramka

**Odpowiedzi na zestaw pytań nr 1** — [`docs/process/pytania/2026-08-22-zestaw-1.md`](pytania/2026-08-22-zestaw-1.md),
25 pytań w kolejności wyznaczonej przez Final Control.

Bezwzględnie przed `NAG-MVP1-001-szkielet`:

1. **Q-INFRA-3** — gałąź bazowa. Dosłowna blokada startu.
2. **Q-ZAKRES-2, Q-INNE-1, Q-MODEL-1, Q-DANE-3** — cztery pytania uruchamiające
   zależności zewnętrzne. Odpowiedzi na nie **nie czekają na kod** — uruchamiają
   zegary, które biegną równolegle.
3. Rejestracja aplikacji w Entra ID — uruchomić natychmiast.

Orkiestracja wieloagentowa: **włączona** decyzją ECHO-001, Sonnet 5 high dla
Operatora, Evaluatora i Final Control. Dispatch wyłącznie przez workflow (ECHO-002).

## Czego nie robić

- Nie zaczynać kodu przed potwierdzeniem gałęzi bazowej.
- Nie dokładać funkcji z MVP2 do MVP1 — rozjazd zakresu jest tu głównym ryzykiem.
- Nie traktować `docs/nota-*.md` jako routingu. To historia rozważań.
