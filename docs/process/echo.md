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

---

### ECHO-002 — dispatch wyłącznie przez workflow

```text
ECHO-002 = przyjęte
data: 2026-08-22
kto:  właściciel
```

**Treść decyzji, zapisana literalnie:**

> „Jeżeli przydzielasz subagentów do pracy, dawaj zawsze w agencie workflow,
> ponieważ wtedy możesz wyznaczyć afort. Jeśli przydzielasz zwykłych subagentów
> bez workflow, nie możesz tego zrobić."

**Skutek:**

1. **Każde** zlecenie pracy subagentowi idzie przez narzędzie workflow — także
   pojedyncze, drobne zadanie.
2. Powód techniczny: zwykłe wywołanie subagenta przyjmuje wyłącznie model;
   **effort jest ustawialny tylko w workflow**. Bez workflow przydział
   „Sonnet 5, effort wysoki" z ECHO-001 jest niewykonalny.
3. Każde wywołanie `agent()` musi mieć **jawnie** podane `model` i `effort`.

**Zapisane w:** `.claude/skills/nagents-autobot/SKILL.md` §11.3.

---

### ECHO-003 — potwierdzenie ECHO-001 przy sprzeczności zapisów

```text
ECHO-003 = A
data: 2026-08-23
kto:  właściciel
```

**Skąd wzięło się pytanie.** Właściciel zapisał poza tym repozytorium: „ten
proces obowiązuje w projekcie nAgents, orkiestracja wieloagentowa domyślnie OFF
do jawnej zgody". To stało w sprzeczności z **ECHO-001**, gdzie zgoda została
udzielona bezterminowo, oraz z całą pracą wykonaną 2026-08-23, która na tej
zgodzie się opierała. Orkiestrator nie rozstrzygnął sprzeczności sam — zadał
pytanie w głównym wątku, zgodnie z zasadą, że przy konflikcie zapisów decyzję
podejmuje właściciel.

**Pytanie:** ECHO-001 dał bezterminową zgodę na pracę przez subagentów, a
nowszy zapis mówi o zgodzie na daną sesję. Co obowiązuje?

**Wybrany wariant A:** ECHO-001 zostaje w mocy. Zgoda jest bezterminowa.
Zlecający nie pyta o nią przy każdym zadaniu ani na starcie sesji.

**Skutek:**

1. **ECHO-001 obowiązuje bez zmian.** Zgoda na pracę wielu wykonawców naraz
   jest bezterminowa, do jawnego odwołania.
2. Reguła „domyślnie wyłączona, wymaga zgody na daną sesję" z `CLAUDE.md`
   opisuje stan wyjściowy projektu, w którym takie ECHO nie zapadło. Tutaj
   zapadło. `CLAUDE.md` uzupełniony o to zastrzeżenie, żeby następny agent nie
   trafił na tę samą sprzeczność.
3. Sposób odwołania zgody nie zmienia się — opisuje go §11.5 skilla.

**Zapisane w:** `CLAUDE.md` (zasady pracy z właścicielem),
`.claude/skills/nagents-autobot/SKILL.md` §21.
