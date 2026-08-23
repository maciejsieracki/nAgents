# Warianty C — uzupełnienie zestawu pytań nr 1

**Data:** 2026-08-22 · workflow `nagents-graph-loop`, faza Warianty, dwaj Operatorzy Sonnet 5 high

Siedem pytań miało tylko dwa warianty zamiast trzech. Przyczyna była po mojej stronie:
w schemacie dla workflow ustawiłem `minItems: 2`, więc Operatorzy dostali pozwolenie
na formę binarną. Szablon §8.2 wymaga trzech.

**Dla żadnego z siedmiu trzeci wariant nie okazał się sztuczny.** W czterech przypadkach
zmienia rekomendację — co jest dowodem, że binarne postawienie sprawy realnie zawężało wybór.

---

# Trzeci wariant dla Q-INFRA-2, Q-INFRA-3, Q-INFRA-4, Q-INFRA-5

Przeczytane: `docs/process/pytania/2026-08-22-zestaw-1.md`, `.claude/skills/nagents-autobot/SKILL.md` §7 (siedem barier) i §8.2 (szablon ABC). Dla wszystkich czterech pytań znalazłem realny, nieartystyczny trzeci wariant — żadnego nie trzeba było odrzucić jako sztuczny.

---

## Q-INFRA-2 — Baza danych: kontener obok aplikacji czy Azure Database for PostgreSQL

**Wariant C — Postgres zarządzany, ale u dostawcy niezależnego od Azure** (np. Aiven, Crunchy Bridge, Neon) — zwykły connection string, bez Azure IAM/private endpoint, zgodnie z zapisaną w `00-architektura.md` §7 zasadą „przenośne poza Azure".

**ZA**
1. Zachowuje zasadę przenośności całego stosu MVP1 poza Azure — B jest jedynym miejscem w architekturze, gdzie ta zasada zostałaby złamana.
2. Daje automatyczne kopie i PITR (korzyść B) bez dokładania zależności od Azure IAM/private endpoint tuż obok już blokującej rejestracji w Entra ID (główny PRZECIW B).

**PRZECIW**
1. Nowy dostawca w łańcuchu przetwarzania PPE to TRZECIA umowa powierzenia do wynegocjowania (obok dostawcy modelu z Q-DANE-3/Q-PRAWO-1 i ewentualnego Blob z Q-INFRA-4) — w oknie 10-12 dni więcej pracy prawnej, nie mniej.
2. Jedna osoba techniczna musi ocenić nieznanego wcześniej dostawcę (bezpieczeństwo, SLA) bez realnej due diligence pod presją terminu, zamiast korzystać z istniejącej relacji z Azure.

**Rekomendacja: bez zmian, zostaje A.** Ciężar dodatkowej DPA (dokładnie ten typ ryzyka, który Q-DANE-3 i Q-PRAWO-1 już nazywają najdroższym wąskim gardłem etapu) przeważa nad korzyścią portowalności. C jest realną opcją, ale przegrywa z A w tym konkretnym oknie czasowym.

---

## Q-INFRA-3 — Gałąź bazowa i model rozgałęzień dla integracji MVP1

**Wariant C — `main` jako baza od teraz (jak A), ale integracja tematów przez squash-merge: każdy temat NAG-\* ląduje na `main` jako JEDEN commit**, nie merge commit z pełną historią roboczą; obecny dorobek z `claude/git-connection-9sz6dg` trafia tam tym samym mechanizmem — zsquashowany do jednego commitu dokumentacyjnego.

**ZA**
1. Jeden commit na temat odpowiada dokładnie jednostce przeglądu Final Control (allowlist per plik/hunk) — historia `main` czyta się jak dziennik tematów NAG-\*, bez szumu roboczych poprawek.
2. Wzmacnia barierę 5 (integracja allowlist-only) — squash wymusza świadomy przegląd całej zmiany tematu jako jednego diffu przed wejściem na główną gałąź, eliminując ryzyko przypadkowego wciągnięcia commitów spoza allowlisty.

**PRZECIW**
1. Traci się dokładną historię iteracji wewnątrz tematu (poprawki po Evaluatorze) — przydatną przy audycie lub cofnięciu się do stanu pośredniego.
2. Dodatkowy krok i dyscyplina (`git merge --squash` + ręczny commit message) zamiast prostego `git merge` — kolejna rzecz do zapamiętania przy jednej osobie technicznej, szczególnie ryzykowna przy pierwszym, historycznym scaleniu.

**Rekomendacja: zmienia się na C.** C rozwiązuje dokładnie to, co dotychczasowa rekomendacja A zostawiała otwarte (PRZECIW A #1 — jak potraktować historię `claude/git-connection-9sz6dg`) i dodatkowo lepiej pasuje do już przyjętej dyscypliny integracji per-temat niż goły trunk-based merge. Nie łamie bariery 7 — push nadal wyłącznie na `main`, po jednorazowym, świadomym scaleniu.

---

## Q-INFRA-4 — Gdzie leżą kopie zapasowe i kto ćwiczy odtworzenie

**Wariant C — Kopia wypychana do Blob Storage (jak B), ale odtworzenie weryfikowane automatycznym zadaniem** (cron odtwarzający dump na tymczasowym zasobie i sprawdzający sumę kontrolną/liczbę wierszy), niezależnym od pamięci czy dostępności jedynej osoby technicznej — człowiek dostaje alert tylko przy niepowodzeniu.

**ZA**
1. Usuwa zależność od tego, że jedna osoba pamięta i faktycznie wykonuje ćwiczenie — test biegnie regularnie, nie jako jednorazowy dowód wykonany pod presją dnia 10-11.
2. Wykrywa regresję (np. zmianę schematu psującą odtworzenie) w dniu, w którym powstała, nie w dniu zamknięcia etapu.

**PRZECIW**
1. Wymaga zaprojektowania i utrzymania dodatkowego komponentu (tymczasowy zasób, logika porównania) — poza tym, co wycenia 01-mvp1.md na 10-12 dni.
2. Sprawdza tylko techniczną integralność kopii, nie merytoryczną — nie zastępuje ręcznego potwierdzenia scenariusza odbioru 9, więc nie eliminuje pracy B, tylko ją dokłada.

**Rekomendacja: bez zmian, zostaje B.** C jest wartościowym ulepszeniem, ale to dodatkowy komponent ponad budżet MVP1, nie substytut B. Warto zapisać jako kandydata do MVP2, gdy jest więcej czasu na automatyzację.

---

## Q-INFRA-5 — Procurement domeny i zgody na publiczny endpoint pod webhooki Teams (MVP3)

**Wariant C — Zgłosić TERAZ do IT/bezpieczeństwa wyłącznie ogólną zapowiedź potrzeby** (typ: publiczny endpoint HTTPS pod webhooki Teams, orientacyjny termin: po MVP1, zależny od D-011) bez rezerwacji konkretnej nazwy subdomeny — rezerwacja DNS następuje dopiero po rozstrzygnięciu D-010, gdy znana jest docelowa struktura (jeden endpoint czy per-agent).

**ZA**
1. Uruchamia zegar procesu akceptacyjnego IT/bezpieczeństwa (realne ryzyko nazwane w pytaniu, wzorzec identyczny jak Entra ID) bez wiązania się z nazwą, którą trzeba by zmienić, gdyby D-010 rozstrzygnęło na strukturę per-agent.
2. Angażuje właściciela w jedno krótkie zdanie zapowiedzi, nie w decyzję o konkretnej nazwie domeny — mniejszy koszt uwagi niż A, przy tej samej korzyści czasowej.

**PRZECIW**
1. Proces akceptacyjny IT może zażądać konkretów (nazwa, właściciel, zakres danych) już przy pierwszym zgłoszeniu, żeby w ogóle ruszyć — w praktyce różnica wobec A może zniknąć.
2. Dokłada nieformalny krok pośredni (zapowiedź vs wniosek właściwy) bez jasnego miejsca w `decisions.md`/`handoff.md` — ryzyko, że zostanie zapomniany i nikt nie wróci do złożenia właściwego wniosku po D-010.

**Rekomendacja: zmienia się na C.** C jest słabo dominujące nad A: w najgorszym razie degeneruje się do A (jeśli IT i tak zażąda konkretów), w najlepszym — oszczędza przedwczesne wiązanie się z nazwą przed D-010, którą sama SYTUACJA pytania już nazywa realnym ryzykiem A. Warunek: jawny wpis w `decisions.md` o statusie „zapowiedź złożona, wniosek właściwy czeka na D-010", żeby PRZECIW #2 nie zmaterializowało się jako ciche zapomnienie.

---

**Podsumowanie zmian rekomendacji:** Q-INFRA-3 A→C, Q-INFRA-5 A→C. Q-INFRA-2 i Q-INFRA-4 pozostają przy dotychczasowej rekomendacji (odpowiednio A i B) mimo realnego wariantu C. Żadna z czterech propozycji C nie narusza żadnej z siedmiu barier.

---

# Wariant C dla Q-MODEL-2, Q-MODEL-3, Q-MODEL-4

Przeczytałem `docs/process/pytania/2026-08-22-zestaw-1.md`, §7 (siedem barier) i §8.2 (szablon ABC) z `.claude/skills/nagents-autobot/SKILL.md`, oraz oparłem warianty C na faktach z `docs/spec/00-architektura.md` (D-002, §8 środowiska dev/staging/prod) i `docs/spec/01-mvp1.md` (harmonogram, §12 ryzyka, `model_fallback`, `budget_monthly_usd`).

Żaden z trzech wariantów C nie łamie żadnej z siedmiu barier.

---

## PYTANIE: NAG-MVP1-Q-MODEL-2 — Azure Foundry jako backend bramy modeli

**WARIANT C: Rozdzielić rozmowę organizacyjną od budowy technicznej — podjąć teraz (koszt bliski zeru) temat dostępności/ceny Azure Foundry przy administratorze dzierżawy, przy okazji już zaplanowanej rozmowy o rejestracji aplikacji w Entra ID (zależność zewnętrzna nazwana wprost w `01-mvp1.md` §12 i w `CLAUDE.md`), ale w MVP1 nie budować żadnej integracji z Azure — silnik LiteLLM zostaje jednobackendowy jak w A.**

**ZA:**
1. Wykorzystuje rozmowę z administratorem dzierżawy, która i tak musi się odbyć przed MVP1 (blokada od dnia trzeciego) — dopytanie o Azure Foundry to darmowy dodatek do niej, nie osobna bramka.
2. Daje realne dane (cena, dostępność Claude w Foundry, gotowość IT) do podjęcia decyzji B w MVP2 na faktach, zamiast zgadywania "po pilotażu" jak w czystym A — bez dokładania ani linii kodu w oknie 10–12 dni.

**PRZECIW:**
1. Ryzyko, że rozmowa organizacyjna bez zobowiązania inżynierskiego utknie w "temacie do rozważenia" — tak samo jak przy A, jeśli nikt nie wyznaczy właściciela follow-upu i terminu.
2. Miesza dwie różne prośby o uprawnienia w jednej rozmowie z administratorem (Entra ID dla tożsamości pracowników vs. Azure Foundry dla modeli) — ryzyko wydłużenia ścieżki, która już blokuje MVP1 od dnia trzeciego.

**Czy rekomendacja się zmienia:** TAK. C jest zasadniczo ulepszeniem A bez kosztu — zerowa dodatkowa praca inżynierska w oknie 10–12 dni, a zamiast biernego "odłożenia" dostajemy aktywne, tanie pozyskanie danych do przyszłej decyzji. Nowa rekomendacja: **C**.

---

## PYTANIE: NAG-MVP1-Q-MODEL-3 — startowy limit budżetu agenta księgowości

**WARIANT C: Nie jeden limit na cały etap, tylko dwa osobne profile/klucze wirtualne agenta księgowości w rejestrze — zgodnie z już istniejącym podziałem środowisk `dev`/`staging`/`prod` (§8 architektury): profil dev/staging z wysokim limitem obserwacyjnym na czas iteracji promptów (dni 1–10, wyłącznie dane syntetyczne), profil prod z docelową, niską wartością od pierwszego dnia wdrożenia na prod (dni 10–11), zanim ruszy scenariusz 8.**

**ZA:**
1. Rozdziela ryzyko zgodnie z podziałem, który projekt już ma i wymaga (bariera 6: żadnych prawdziwych danych osobowych poza `prod`) — środowisko z syntetykami dostaje swobodę do iteracji, środowisko z realnym rozliczeniem dostaje ochronę od razu, bez etapu „tymczasowo wysoko, potem obniżyć”.
2. Eliminuje ryzyko z wariantu B, że wysoka wartość „tymczasowa” zostanie na stałe — na `prod` nigdy nie było wysokiego limitu do cofnięcia, więc nie ma czego zapomnieć obniżyć.

**PRZECIW:**
1. Wymaga dwóch osobnych profili agenta księgowości w rejestrze już w MVP1 (dev i prod) zamiast jednego — dodatkowa złożoność konfiguracyjna w etapie, który sam siebie ostrzega przed rozrostem zakresu (§12: „kuszenie, żeby dołożyć funkcję z MVP2”).
2. Scenariusz 8 z definicji obejmuje „wielokrotne przeliczanie trzech zamkniętych miesięcy podczas poprawiania promptów” na `prod` (dni 11–12, warunek zamknięcia etapu wymaga sprawdzenia na `prod`) — jeśli docelowy limit prod jest low od dnia 1, to samo ryzyko zablokowania najważniejszego testu co w wariancie A wraca, tylko przeniesione z całego etapu na samo środowisko `prod`.

**Czy rekomendacja się zmienia:** TAK, częściowo. C rozwiązuje realny problem obu wariantów (A blokuje iterację na dev, B nie chroni realnych pieniędzy na prod) precyzyjniej niż jeden numer na cały etap — ale nie usuwa całkowicie ryzyka z PRZECIW #2, więc „docelowa” wartość dla `prod` musi być skalibrowana z zapasem na kilka przeliczeń, nie na pojedynczy przebieg. Nowa rekomendacja: **C**, pod warunkiem że wartość docelowa dla `prod` zostanie ustalona z uwzględnieniem realnej liczby powtórzeń scenariusza 8, nie jako gołe „200 USD z przykładu”.

---

## PYTANIE: NAG-MVP1-Q-MODEL-4 — eskalacja do `model_fallback`

**WARIANT C: Zamiast logiki przełączania w runtime (B) albo czysto reaktywnej ręcznej edycji rejestru dopiero po niepowodzeniu na `prod` (A) — przeprowadzić w dniach 8–9 (już zaplanowane jako okno konfiguracji bramy modeli) empiryczną kalibrację: uruchomić `model_default` i kandydata na `model_fallback` na tym samym, reprezentatywnym zestawie syntetycznych faktur/rozliczeń na `dev`/`staging`, i na tej podstawie wybrać, który model wchodzi jako `model_default` na `prod`, zanim w ogóle wystartuje scenariusz 8. Bez żadnej logiki przełączania w proxy — pole `model_fallback` zostaje rezerwą jak w A.**

**ZA:**
1. Zero dodatkowej logiki w proxy Hermesa i żadnego drugiego klucza wirtualnego w runtime (koszt jak w A) — mieści się w już zaplanowanym oknie dni 8–9, nie dokłada nowego zakresu do ciasnych 10–12 dni.
2. Przenosi moment odkrycia problemu „model tani za słaby” z dni 11–12 (środek scenariusza biznesowego, na `prod`, z realnymi danymi) na dni 8–9 (na `dev`/`staging`, z danymi syntetycznymi) — ryzyko z §12 przestaje być niespodzianką w najgorszym możliwym momencie etapu.

**PRZECIW:**
1. Kalibracja z góry nie eliminuje ryzyka całkowicie: model dobrze radzący sobie z syntetycznymi fakturami w dniach 8–9 może zawieść na specyficznym przypadku prawdziwego rozliczenia w dniach 11–12 — dane syntetyczne z natury nie odwzorowują pełnej różnorodności realnych PPE i umów.
2. Dokłada do dnia 8–9 zadanie przygotowania reprezentatywnego zestawu testowego i przeprowadzenia porównania — ktoś musi go przygotować, w tym samym oknie, które już obejmuje konfigurację klucza wirtualnego i budżetów; ryzyko przeciążenia tego konkretnego dnia.

**Czy rekomendacja się zmienia:** TAK. C daje istotnie więcej ochrony niż A (odkrycie problemu wcześniej, na tańszym i bezpieczniejszym środowisku) przy koszcie inżynierskim bliskim A, a nie B — bez budowania mechanizmu przełączania, którego kryterium „kiedy przełączyć” i tak jest nierozstrzygniętą decyzją produktową (słuszny PRZECIW z B). Nowa rekomendacja: **C**.

---
