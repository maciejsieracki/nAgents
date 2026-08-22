# Dokumentacja techniczna nAgents

Warstwa zarządzania nad flotą instancji Hermesa.

## Kolejność czytania

1. **[00-architektura.md](00-architektura.md)** — czym jest system, model danych,
   model bezpieczeństwa, stos technologiczny. Dokument źródłowy dla wszystkich etapów.
2. **[decisions.md](decisions.md)** — dziennik decyzji. Każdy wybór dotyczący kosztu,
   danych, dostępu lub odwracalności, z uzasadnieniem. **Dwie pozycje otwarte.**
3. **[scenarios.md](scenarios.md)** — sytuacje, które system ma obsłużyć. Źródło testów.
4. Etapy: **[MVP1](01-mvp1.md)** → **[MVP2](02-mvp2.md)** →
   **[MVP3](03-mvp3.md)** → **[MVP4](04-mvp4.md)**

## Skrót

| Etap | Cel | Dni |
|---|---|---|
| MVP1 · Spięcie | logowanie, rejestr, uprawnienia, rozmowa, brama modeli, audyt | 10–12 |
| MVP2 · Zarządzalność | panel, synchronizacja katalogu, budżety, zatwierdzenia, kopie | 12–15 |
| MVP3 · Wiedza | trzy poziomy kontekstu, indeksy, rutyny, testy agenta, Teams | 18–22 |
| MVP4 · Skala | tokenizacja, pula instancji, router, wielonajemność, retencja | 20–25 |

**Razem: 60–74 dni robocze.** Każdy etap kończy się czymś, co działa na produkcji.

## Trzy rzeczy do zapamiętania

1. **Nie budujemy agenta.** Hermes nim jest. Budujemy warstwę, która odpowiada na
   pytania: kto to jest, do czego ma prawo, ile mu wolno wydać, co po sobie zostawił.
2. **Brama modeli należy do nas.** To fundament, nie detal — z niego wynika swoboda
   wyboru modelu i egzekwowalny budżet.
3. **Nie gonimy parytetu z gotowymi platformami.** Budujemy pod pięć wymagań.
   Gonienie parytetu zamienia projekt na trzy miesiące w projekt na rok.

## Blokada

**D-011: rezydencja wspólnej pamięci.** MVP1 i MVP2 działają bez niej.
MVP3 nie startuje bez odpowiedzi. Szczegóły w dzienniku decyzji.
