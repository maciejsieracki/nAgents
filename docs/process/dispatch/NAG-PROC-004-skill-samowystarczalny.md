# Dispatch — NAG-PROC-004-skill-samowystarczalny

```text
TEMAT:   NAG-PROC-004-skill-samowystarczalny
DOMENA:  PROCES
DATA:    2026-08-23
RUNDA:   1 z 3

WYZWALACZ
Decyzja właściciela wydana w rozmowie 2026-08-23: „W jednym pliku, który
przekażemy nowemu agentowi, muszą być zawarte wszystkie informacje, co ma sobie
stworzyć i jak ma wyglądać cały obiekt, wszystkie zasady, a nie będę mu włączał
kilku plików." Obecny skill opisuje proces, ale odsyła do plików w `docs/`,
których nowy agent nie dostanie.

GOAL
SKILL.md jest jedynym plikiem potrzebnym, by agent wchodzący w pusty katalog
odtworzył repozytorium nAgents: strukturę, zawartość merytoryczną i zasady pracy.

KRYTERIA KOŃCA
- W SKILL.md znajduje się pełne drzewo plików do stworzenia, z przeznaczeniem
  każdego pliku i jego szkieletem.
- W SKILL.md znajduje się minimum merytoryczne projektu: pięć warstw, model
  danych, topologia, cztery etapy, decyzje D-001…D-011, scenariusze.
- Test odcięcia: czytelnik mający wyłącznie SKILL.md odtwarza repozytorium bez
  pytania o żaden inny plik.
- Żadne odesłanie w SKILL.md nie jest jedynym nośnikiem informacji koniecznej
  do pracy — odesłania wolno zostawić jako pogłębienie, nie jako warunek.

ZAKRES
W zakresie:      .claude/skills/nagents-autobot/SKILL.md, README.md skilla,
                 docs/process/tematy.md (wpis o temacie)
Poza zakresem:   zmiana treści docs/spec/**, zmiana decyzji, zmiana CLAUDE.md

ALLOWLISTA
- .claude/skills/nagents-autobot/SKILL.md
- .claude/skills/nagents-autobot/README.md
- docs/process/tematy.md
- docs/process/dispatch/NAG-PROC-004-skill-samowystarczalny.md
Zakazane bezwzględnie: .env*, docs/spec/decisions.md, .git/**

IZOLACJA
Temat dokumentacyjny, bez kodu. Praca bezpośrednio na gałęzi roboczej
`claude/git-connection-9sz6dg`, bez worktree — nie ma testów do odpalenia
ani ryzyka konfliktu z równoległym tematem.

PLAN TESTÓW
1. Test odcięcia — czytelnik z samym SKILL.md wylicza pliki do stworzenia.
2. Sprawdzenie, czy żadna zasada z rozmowy nie wypadła: siedem barier,
   ECHO-001, ECHO-002, przydział modeli, limit współbieżności, brama triage.
3. Sprawdzenie spójności: brak sprzecznych liczb między sekcjami.

ZALEŻNOŚCI
Zależy od:    brak
Blokuje:      przekazanie projektu innemu agentowi
Decyzje:      ECHO-001, ECHO-002

BARIERY DOTKNIĘTE PRZEZ TEN TEMAT
Bariera 1 — dokument będzie zawierał wzorce konfiguracji. Żaden wzorzec nie może
zawierać wartości sekretu, wyłącznie `vault_ref`.
Bariera 7 — push wyłącznie na `claude/git-connection-9sz6dg`.
```
