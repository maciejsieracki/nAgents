# Jak bezpiecznie zmieniać sam proces

Szkielet AutoBot (punkt 2) wymaga osobnego dokumentu na wypadek, gdy modyfikujemy
**mechanizm procesu**, a nie kod produktu. To jest ten dokument.

**Przeczytaj go przed dotknięciem czegokolwiek z tej listy:**

```
.claude/skills/**
CLAUDE.md
docs/process/**
```

---

## 1. Dlaczego to jest osobny tryb

Zmiana w kodzie produktu psuje funkcję. **Zmiana w procesie psuje zdolność
wykrywania, że coś jest zepsute.** Błąd w `app/permissions.py` wyłapie Evaluator.
Błąd w definicji roli Evaluatora nie wyłapie nikt — bo właśnie zepsuliśmy to,
co miało go wyłapać.

Dlatego zmiany procesu mają ostrzejszy tryb niż zmiany produktu, mimo że są
mniejsze i wyglądają niewinnie.

## 2. Domena i ID

Każda zmiana procesu to temat w domenie `PROCES`, z ID `NAG-PROC-<NNN>-<slug>`.
Nie „przy okazji" innego tematu. **Zmiana procesu nigdy nie jedzie w allowliście
tematu produktowego** — nawet jednolinijkowa.

## 3. Cztery reguły bezwzględne

### 3.1 Nigdy nie zmieniaj procesu w trakcie otwartego tematu produktowego
Najpierw zamknij albo świadomie zawieś tematy, których zmiana dotknie.
Zmiana reguł w połowie rundy sprawia, że werdykt Evaluatora odnosi się
do nieistniejącej już normy.

### 3.2 Zmiana bariery wymaga ECHO
Siedem twardych barier z `nagents-autobot` §9 to nie jest konwencja stylistyczna.
**Osłabienie, usunięcie albo dodanie wyjątku do którejkolwiek wymaga formalnego
pytania ABC i odpowiedzi literą.** Nie wolno tego zrobić „bo przeszkadza".

Dotyczy również: limitu rund, wielkości puli, progu ZWIS i listy ścieżek
zakazanych w allowliście.

### 3.3 Zmiana skilla wymaga uzasadnienia z przypadku, nie z przeczucia
Do zmiany dołącz konkretny przebieg, w którym obecna reguła zawiodła:
ID tematu, co się stało, dlaczego reguła nie pomogła. **„Wydaje mi się, że
tak będzie lepiej" nie jest podstawą.** Proces zmieniany na podstawie
przeczuć rozjedzie się w kwartał.

### 3.4 Zmiana wchodzi z datą i powodem
Każda zmiana w `.claude/skills/**` i `CLAUDE.md` musi mieć w commicie:
ID tematu, przypadek, który ją wywołał, i co konkretnie się zmienia.
Historia gita jest tu jedynym rejestrem — nie ma osobnego dziennika procesu.

## 4. Ścieżka zmiany

```
1. Zauważony przypadek   → zapis w docs/process/tematy.md jako NAG-PROC-<NNN>
2. Czy dotyka bariery?
   ├─ tak → pytanie ABC (szablon w nagents-autobot §6) → ECHO → dalej
   └─ nie → dalej
3. Zapis dispatchu w docs/process/dispatch/<ID>.md
4. Zmiana w izolacji, allowlista: wyłącznie ścieżki procesu
5. Evaluator sprawdza szczególnie:
   ├─ czy zmiana nie osłabia bariery po cichu
   ├─ czy nie tworzy furtki „w wyjątkowych wypadkach"
   └─ czy mapa zgodności w §0 skilla nadal się zgadza
6. Final Control: czy uzasadnienie z przypadku istnieje i jest prawdziwe
7. Integracja + READY_FOR_DEPLOY
8. Push jak zwykle — osobna bramka, wskazana gałąź
```

## 5. Czego nie wolno nigdy

| Zakaz | Powód |
|---|---|
| Usunąć regułę, bo utrudniła bieżący temat | To jest dokładnie moment, w którym działała |
| Dodać wyjątek „tylko na teraz" | Wyjątki nie znikają; zostają i rosną |
| Zmienić skill w tej samej rundzie, w której się go łamie | Konflikt interesu w czystej postaci |
| Poszerzyć allowlistę tematu produktowego o ścieżki procesu | Obejście trybu opisanego w tym dokumencie |
| Zresetować licznik rund przez przenumerowanie tematu | Licznik ma boleć; to jego funkcja |
| Skrócić szkielet uniwersalny `autobots` | To materiał właściciela — zmienia go tylko on |

## 6. Szkielet uniwersalny jest cudzy

`.claude/skills/autobots/SKILL.md` pochodzi od właściciela i był pisany dla innego
projektu. **Nie modyfikujemy go.** Wszystkie różnice, zawężenia i wartości
konkretne idą do `nagents-autobot` — z odnotowaniem w mapie zgodności (§0),
gdzie i jak dana zasada została związana.

Jeśli zasada ze szkieletu wydaje się nie pasować do nAgents — **nie jest to powód
do jej pominięcia.** Zgłoś właścicielowi jako pytanie ABC. Domyślnie stosujemy
wszystko, bez wyłączeń.

## 7. Przegląd okresowy

Raz na etap (po MVP1, MVP2, MVP3) orkiestrator przegląda proces i odpowiada
właścicielowi na trzy pytania:

1. Która reguła zadziałała — złapała coś, co inaczej by przeszło?
2. Która przeszkadzała bez korzyści — i jaki konkretny przypadek to pokazuje?
3. Czego zabrakło — jaki błąd przeszedł mimo procesu?

Przegląd jest tematem `NAG-PROC-<NNN>-przeglad-<etap>`, nie luźną rozmową.
