# Rejestr ECHO

Literalny zapis decyzji właściciela. **Wpis powstaje dopiero po jednoznacznej
odpowiedzi literą.**

„Chyba B", „raczej tak", „brzmi sensownie" i rekomendacja agenta **nie są**
decyzją. Rekomendacja agenta nigdy nie staje się ECHO przez brak sprzeciwu.

## Format

```text
<ID pytania> = <litera>
data: <RRRR-MM-DD>
kto: <właściciel>
pytanie: <jedno zdanie, o co było pytane>
wariant: <co dokładnie oznacza wybrana litera>
skutek: <ADR w decisions.md, jeśli decyzja zmienia architekturę>
```

Po zapisaniu ECHO kontynuuj ten sam temat pod tym samym ID.

## Wpisy

*Brak. Dotychczasowe ustalenia zapadły w rozmowie przed wprowadzeniem procesu
i są udokumentowane jako decyzje D-001…D-009 w [`../spec/decisions.md`](../spec/decisions.md).*

*Pierwsze pytania, które będą wymagały ECHO: **D-010** (topologia agentów)
oraz **D-011** (rezydencja wspólnej pamięci).*

---

### ECHO-001 — delegowanie i przydział modeli

```text
ECHO-001 = przyjęte
data: 2026-08-22
kto:  właściciel
```

**Treść decyzji, zapisana literalnie:**

> „Zgodnie z zasadami Autobot w tym czacie zlecasz wszystkie prace zawsze do
> wykonania. Sonet 5 jako operator i działasz dalej za zasadą Autobot. (…)
> W tym czacie nie pracujesz nad niczym sam. Wszystko zlecasz do subagentów
> zgodnie z zasadą Autobot. Jeżeli jest więcej zadań zebranych, uruchamiasz
> jeszcze Agendic Workflow. Ustaliliśmy, że jest operator, jest walidator
> i finalna weryfikacja. Wszystkie lecą na Sonnet 5 High."

**Skutek:**

1. Orkiestracja wieloagentowa **włączona** — zgoda jawna, bezterminowa do odwołania.
2. Przydział: Operator, Evaluator i Final Control — **Sonnet 5, effort wysoki.**
3. Orkiestrator nie wykonuje pracy sam; wyjątki wyliczone w skillu §11.4.
4. Przy kilku zebranych tematach — workflow z fan-outem, nie kolejka wywołań.

**Zapisane w:** `.claude/skills/nagents-autobot/SKILL.md` §11.
