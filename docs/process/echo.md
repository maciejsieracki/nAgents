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
