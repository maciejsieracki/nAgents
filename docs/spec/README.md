# Dokumentacja techniczna 8gent

Warstwa zarządzania nad agentami, workspace'ami i sesjami OpenClaw. Aktualna
decyzja platformowa, mapowanie wcześniejszej pracy i plan pluginu są w
[`../OPENCLAW-STRATEGY.md`](../OPENCLAW-STRATEGY.md).

## Kolejność czytania

1. **[00-architektura.md](00-architektura.md)** — czym jest system, model danych,
   model bezpieczeństwa, stos technologiczny i relacja z OpenClaw. Dokument źródłowy dla wszystkich etapów.
2. **[../OPENCLAW-STRATEGY.md](../OPENCLAW-STRATEGY.md)** — aktualna decyzja
   platformowa, mapowanie Hermes → OpenClaw i granica pluginu AutoBot Monitor.
3. **[decisions.md](decisions.md)** — dziennik decyzji. Każdy wybór dotyczący kosztu,
   danych, dostępu lub odwracalności, z uzasadnieniem. **Dwie pozycje otwarte.**
4. **[scenarios.md](scenarios.md)** — sytuacje, które system ma obsłużyć. Źródło testów.
5. Etapy: **[MVP1](01-mvp1.md)** → **[MVP2](02-mvp2.md)** →
   **[MVP3](03-mvp3.md)** → **[MVP4](04-mvp4.md)**

## Skrót

| Etap | Cel | Dni |
|---|---|---|
| MVP1 · Spięcie | logowanie, rejestr, uprawnienia, rozmowa, brama modeli, audyt | 10–12 |
| MVP2 · Zarządzalność | panel, synchronizacja katalogu, budżety, zatwierdzenia, kopie | 12–15 |
| MVP3 · Wiedza | trzy poziomy kontekstu, indeksy, rutyny, testy agenta, Teams | 18–22 |
| MVP4 · Skala | tokenizacja, pula instancji, router, wielonajemność, retencja | 20–25 |

**Razem: 60–74 dni robocze.** Każdy etap kończy się czymś, co działa na produkcji.

## Priorytet dostarczenia interfejsu — web-first

Kolejność produktu jest nadrzędna wobec wygody konkretnego klienta:

1. **Najpierw web.** Potwierdzamy bezpieczny i prosty dostęp przez OpenClaw
   Control UI, wybrany kanał albo webową powierzchnię 8gent. Pracownik dostaje
   gotowego agenta i sesję; nie konfiguruje Gatewaya ani serwera. Praca musi
   pozostać na serwerze po zamknięciu przeglądarki.
2. **Później dodatkowe klienty.** Desktop, mobile, node i własny panel są
   klientami dodatkowymi. Nie mogą być właścicielem cyklu życia sesji, workerów,
   kolejki ani agenta.

Zaawansowane ustawienia pozostają dla administratora i są dostępne przez
Control UI/CLI OpenClaw albo webową warstwę administracyjną 8gent. Pracownik
widzi tylko przydzielonego agenta i jego sesję.

## Trzy rzeczy do zapamiętania

1. **Nie budujemy agenta ani drugiego Gatewaya.** OpenClaw dostarcza runtime,
   kanały, sesje, narzędzia i automatyzacje. Budujemy warstwę 8gent odpowiadającą
   na pytania: kto to jest, do czego ma prawo, ile mu wolno wydać i co po sobie
   zostawił.
2. **Nie budujemy obowiązkowego proxy modeli.** OpenClaw konfiguruje provider,
   primary model, fallbacks i allowlistę. OpenRouter nie jest wymaganym elementem
   tej architektury.
3. **Nie budujemy OpenMonitora.** Najpierw używamy Control UI, tasks,
   automations i Task Flow OpenClaw. AutoBot Monitor pozostaje opcjonalnym
   pluginem wyłącznie dla potwierdzonej luki.

## Blokada

**D-011: rezydencja wspólnej pamięci.** MVP1 i MVP2 działają bez niej.
MVP3 nie startuje bez odpowiedzi. Szczegóły w dzienniku decyzji.
