# Dispatch — NAG-PROC-005-skill-uniwersalny

```text
TEMAT:   NAG-PROC-005-skill-uniwersalny
DOMENA:  PROCES
DATA:    2026-08-23
RUNDA:   1 z 3

WYZWALACZ
Korekta właściciela wydana w rozmowie 2026-08-23, po zatrzymaniu NAG-PROC-004:
„27 agentów to jest dla naszego projektu, to nie jest uniwersalne ustalenie.
Ustalenie modeli językowych powinno być dokładnie określone: nowy agent ma
odpytać użytkownika, jakie modele chce ustalić dla danych ról, ile ma być pętli,
ile weryfikacji, ile prób. Parametry dla każdego projektu mogą być trochę inne,
zasady są takie same. Wszystko trzeba jeszcze raz przepisać w wersji uniwersalnej."

GOAL
SKILL.md jest procesem uniwersalnym: zasady obowiązują wszędzie, a każda liczba
i nazwa zależna od projektu jest nazwanym parametrem, o który nowy agent pyta
właściciela prostym językiem, zanim zacznie pracę.

KRYTERIA KOŃCA
- Żadna wartość zależna od projektu nie stoi w treści zasady. Każda ma nazwę
  parametru, wartość domyślną i miejsce, w którym zapisuje się wybór właściciela.
- Istnieje zamknięta lista pytań kalibracyjnych. Każde pytanie ma: wyjaśnienie
  po co pytamy, samo pytanie, warianty z ceną i ryzykiem każdego, wartość
  domyślną przy braku odpowiedzi.
- Pytania przechodzą test zrozumiałości: osoba nietechniczna odpowiada bez
  dopytywania. Żadnego terminu technicznego bez wyjaśnienia w tym samym zdaniu.
- Koszt jest podany jako krotność przy każdym wariancie zwiększającym nakład.
- Wypełnienie dla nAgents nie znika — trafia do wydzielonego dodatku na końcu.
- Test odcięcia z NAG-PROC-004 nadal zdany: jeden plik wystarcza do odtworzenia
  struktury repozytorium i zasad pracy.

ZAKRES
W zakresie:      .claude/skills/nagents-autobot/SKILL.md — całość,
                 .claude/skills/nagents-autobot/README.md,
                 docs/process/tematy.md
Poza zakresem:   docs/spec/** — merytoryka projektu zostaje tam, gdzie jest.
                 Nie przenosimy jej do skilla. Zmiana nazwy katalogu skilla.

ALLOWLISTA
- .claude/skills/nagents-autobot/SKILL.md
- .claude/skills/nagents-autobot/README.md
- docs/process/tematy.md
- docs/process/dispatch/NAG-PROC-005-skill-uniwersalny.md
Zakazane bezwzględnie: .env*, docs/spec/**, CLAUDE.md, .git/**

IZOLACJA
Temat dokumentacyjny. Praca na gałęzi `claude/git-connection-9sz6dg`, bez
worktree — brak kodu, brak testów automatycznych, brak równoległego tematu.

PLAN TESTÓW
1. Test uniwersalności: przeczytać plik, podstawiając inny projekt. Każde
   zdanie, które przestaje mieć sens, jest błędem.
2. Test zrozumiałości pytań: każde pytanie ocenić z pozycji osoby nietechnicznej.
3. Test odcięcia: czy z samego pliku da się odtworzyć strukturę repozytorium.
4. Test kompletności parametrów: czy każda liczba w pliku ma nazwę parametru.

ZALEŻNOŚCI
Zależy od:    brak
Blokuje:      przekazanie procesu jakiemukolwiek innemu projektowi
Decyzje:      ECHO-001, ECHO-002 — stają się wypełnieniem parametrów,
              nie normą uniwersalną

BARIERY DOTKNIĘTE PRZEZ TEN TEMAT
Bariera 1 — szkielety konfiguracji nie mogą zawierać wartości sekretów.
Bariera 5 — integracja wyłącznie po allowliście, nigdy `git add -A`.
Bariera 7 — push wyłącznie na `claude/git-connection-9sz6dg`.
Uwaga osobna: siedem barier nAgents to wypełnienie parametru, nie norma
uniwersalna. W wersji uniwersalnej obowiązkowe jest *istnienie* listy barier
i to, że jej naruszenie oznacza FAIL — nie jej konkretna treść.
```
