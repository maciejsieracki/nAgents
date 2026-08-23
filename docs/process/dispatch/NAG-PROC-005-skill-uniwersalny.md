# Dispatch — NAG-PROC-005-skill-uniwersalny

**Wersja druga.** Pierwsza wersja tego zapisu zakładała uniwersalność wobec
projektów przy zachowaniu dziedziny wytwarzania oprogramowania. Właściciel
poszerzył zakres 2026-08-23. Poprzedni bieg zatrzymany przed wytworzeniem
czegokolwiek — nie ma pracy do odzyskania.

```text
TEMAT:   NAG-PROC-005-skill-uniwersalny
DOMENA:  PROCES
DATA:    2026-08-23
RUNDA:   1 z 3

WYZWALACZ
Korekta właściciela, 2026-08-23: „Skill musi być uniwersalny, niezależnie od
tego, czym zajmuje się dana firma i jaki projekt — czy to programowanie,
finanse, księgowość, czy kwestie prawne. Zasady muszą być wszędzie uniwersalne,
a jedynie skill ma wskazać, na co zwrócić uwagę agent i o co użytkownik powinien
dopytać, aby dopasować się do konkretnego typu projektu."

GOAL
Proces daje się zastosować w dowolnej dziedzinie pracy, a wszystko, co od
dziedziny lub projektu zależy, jest nazwane, wypytane u właściciela i zapisane
— nigdy założone po cichu.

TRZY WARSTWY, KTÓRE TEN TEMAT ROZDZIELA

  ZASADA — niezmienna wszędzie. Nikt nie ocenia własnej pracy. Zapis zlecenia
    powstaje przed startem wykonawcy. Deklaracja wykonawcy nie jest dowodem
    wykonania. Naruszenie twardej bariery to FAIL niezależnie od reszty.
    Odpowiedź niejednoznaczna nie jest decyzją. Brak sprzeciwu nie jest zgodą.

  PARAMETR — liczba lub nazwa, którą projekt ustawia u siebie. Ile rund, jaki
    model do której roli, ilu sprawdzających, ilu wykonawców naraz, po jakim
    czasie milczenia uznajemy zawieszenie.

  ODWZOROWANIE DZIEDZINY — pojęcie procesu, które w każdej dziedzinie ma inną
    postać fizyczną. Proces mówi „dowód wykonania"; w programowaniu jest nim
    zielony zestaw testów, w księgowości zgodność przeliczenia z liczeniem
    ręcznym, w kancelarii druga lektura przez osobę, która nie pisała pisma.
    Proces nazywa funkcję. Postać ustala się pytaniem.

KRYTERIA KOŃCA
- Żadne zdanie zasady nie zakłada kodu, repozytorium, testów automatycznych,
  wdrożenia ani gałęzi. Pojęcia procesu są nazwane funkcjonalnie.
- Istnieje tabela odwzorowań: pojęcie procesu wobec co najmniej pięciu dziedzin
  — wytwarzanie oprogramowania, księgowość i finanse, obsługa prawna,
  marketing i sprzedaż, operacje i produkcja.
- Istnieje zamknięta lista pytań rozpoznających dziedzinę: co jest wytworem,
  co jest dowodem skończenia, co jest zapisem zmian, na czym polega izolacja
  pracy, czego nie wolno naruszyć nigdy, kto zatwierdza, co narzuca prawo.
- Istnieje zamknięta lista pytań kalibrujących parametry, z ceną i ryzykiem
  każdego wariantu, koszt podany jako krotność.
- Wszystkie pytania przechodzą test zrozumiałości: osoba nietechniczna
  odpowiada bez dopytywania.
- Wypełnienie dla nAgents nie znika — wydzielony dodatek na końcu.
- Test odcięcia zdany: jeden plik wystarcza, by założyć strukturę i ruszyć.

ZAKRES
W zakresie:      .claude/skills/nagents-autobot/SKILL.md — całość,
                 .claude/skills/nagents-autobot/README.md,
                 docs/process/tematy.md
Poza zakresem:   docs/spec/** — merytoryka nAgents zostaje na miejscu.
                 CLAUDE.md. Zmiana nazwy katalogu skilla.

ALLOWLISTA
- .claude/skills/nagents-autobot/SKILL.md
- .claude/skills/nagents-autobot/README.md
- docs/process/tematy.md
- docs/process/dispatch/NAG-PROC-005-skill-uniwersalny.md
Zakazane bezwzględnie: .env*, docs/spec/**, CLAUDE.md, .git/**

IZOLACJA
Temat dokumentacyjny. Praca na gałęzi `claude/git-connection-9sz6dg`, bez
osobnej kopii katalogu — brak kodu, brak równoległego tematu na tych plikach.

PLAN TESTÓW
1. Test podstawienia dziedziny. Przeczytać plik cztery razy, podstawiając:
   biuro rachunkowe zamykające miesiąc; kancelaria prowadząca rejestr umów;
   dział marketingu przygotowujący kampanię; zakład planujący przeglądy.
   Każde zdanie, które przy którymkolwiek podstawieniu przestaje mieć sens,
   jest błędem.
2. Test zrozumiałości pytań z pozycji osoby nietechnicznej.
3. Test nienaruszalności — czy zasady nie zostały rozmyte w parametry.
4. Test odcięcia — czy z samego pliku da się założyć strukturę pracy.

ZALEŻNOŚCI
Zależy od:    brak
Blokuje:      przekazanie procesu poza ten projekt
Decyzje:      ECHO-001, ECHO-002 — po tej zmianie są wypełnieniem parametrów
              dla nAgents, nie normą uniwersalną

BARIERY DOTKNIĘTE PRZEZ TEN TEMAT
Bariera 1 — szkielety nie mogą zawierać wartości sekretów.
Bariera 5 — integracja wyłącznie po allowliście.
Bariera 7 — push wyłącznie na `claude/git-connection-9sz6dg`.
Uwaga: sama lista siedmiu barier jest wypełnieniem dla nAgents. Uniwersalnie
obowiązkowe jest istnienie takiej listy i skutek jej naruszenia, nie treść.
```
