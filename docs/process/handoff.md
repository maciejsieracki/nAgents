# Handoff

Stan na teraz. Zastępowany w całości przy każdym przekazaniu — to nie jest log,
tylko zdjęcie bieżącej sytuacji.

**Ostatnia aktualizacja:** 2026-08-22

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

| Co | Skutek | Kto odblokuje |
|---|---|---|
| **D-011** — rezydencja wspólnej pamięci | MVP3 nie startuje | właściciel, po informacji o bazie |
| Rejestracja aplikacji w Entra ID | MVP1 staje w dniu trzecim | administrator dzierżawy |

Druga pozycja jest pilniejsza, choć wygląda mniej poważnie — jest jedyną
zależnością zewnętrzną etapu pierwszego i warto ją uruchomić przed startem kodu.

## Decyzje czekające na właściciela

| ID | Pytanie | Termin |
|---|---|---|
| **D-010** | Topologia: 27 profili czy sześć agentów plus poziomy kontekstu? | przed MVP3 |
| **D-011** | Gdzie fizycznie mieszka wspólna pamięć? Wariant (c) — rezygnacja z niej — jest realny | przed MVP3 |

## Następna bramka

Rozpoczęcie `NAG-MVP1-001-szkielet`. Wymaga od właściciela:

1. potwierdzenia gałęzi bazowej
2. potwierdzenia, czy orkiestracja wieloagentowa jest w tej sesji dozwolona
   (domyślnie **nie**)
3. uruchomienia rejestracji aplikacji w Entra ID — równolegle, nie blokuje dnia pierwszego

## Czego nie robić

- Nie zaczynać kodu przed potwierdzeniem gałęzi bazowej.
- Nie dokładać funkcji z MVP2 do MVP1 — rozjazd zakresu jest tu głównym ryzykiem.
- Nie traktować `docs/nota-*.md` jako routingu. To historia rozważań.
