STATUS: PASS_WITH_EXPLICIT_OWNER_GATES
DOMAIN: INFORMACYJNY
TEMAT: NAG-DOCS-CONSOLIDATION-Q1 / P4
CEL: przygotować kandydatów do P5/P6 bez usuwania, przenoszenia, scalania ani publikacji.
P2 OBSERVED_AT: 2026-09-14T13:56:47+00:00 UTC
P4 READBACK_AT: 2026-09-14T14:47:54+00:00 UTC

# P4 — kandydaci do konsolidacji

## Zasada decyzji

To jest lista relacji i pakietów docelowych, nie lista plików do kasowania. Status `CONSOLIDATION_CANDIDATE` oznacza materiał do macierzy pokrycia i allowlisty. `CANONICAL`, `SOURCE`, `EVIDENCE`, `HISTORY`, `STALE` i `OWNER_DECISION_REQUIRED` są zachowane tam, gdzie konsolidacja byłaby zmianą znaczenia albo zakresu.

Każdy rekord ma w `P4-classification.json` własny `status`, `relation` i `reason`. Każda z 254 rodzin ścieżek ma `primary_status`, `unique_content`, `contradictions`, `risk`, `target_package` i `recommendation`. Poniższe sekcje opisują agregaty; pełne członkostwo jest w JSON.

## Macierz agregatów

| group_id | status | projekt | rodzin ścieżek | rekordów | target package |
|---|---|---|---:|---:|---|
| NAG-ENTRY | CANONICAL | nAgents | 2 | 25 | CLAUDE.md pozostaje; NAGENTS-PROJECT.md jest kandydatem P5 |
| NAG-INDEX | CONSOLIDATION_CANDIDATE | nAgents | 3 | 4 | NAGENTS-PROJECT.md; docelowo lekkie odnośniki do NAGENTS-HANDOFF.md |
| NAG-PROCESS | CANONICAL | nAgents | 7 | 73 | NAGENTS-PROCESS.md jako przyszły pakiet; SKILL.md i źródło uniwersalne pozostają osobno |
| NAG-SPEC | CANONICAL | nAgents | 8 | 96 | docs/spec/ pozostaje; przyszły NAGENTS-SPEC.md tylko po macierzy pokrycia P6 |
| NAG-DECISIONS | CANONICAL | nAgents | 1 | 12 | NAGENTS-DECISIONS.md jako przyszły pakiet; aktualne pliki pozostają źródłami |
| NAG-HANDOFF | CANONICAL | nAgents | 1 | 12 | NAGENTS-HANDOFF.md jako pakiet treści; docs/process/handoff.md pozostaje formatem wejściowym |
| NAG-RESEARCH | HISTORY | nAgents | 11 | 88 | NAGENTS-RESEARCH.md; decyzje tylko po wpisie w decisions/ECHO |
| NAG-INTEGRATIONS | OWNER_DECISION_REQUIRED | nAgents | 4 | 18 | NAGENTS-INTEGRATIONS.md po decyzji właściciela |
| NAG-APPT0 | SOURCE | nAgents | 8 | 96 | NAGENTS-RESEARCH.md; ewentualne integracje tylko po decyzji |
| NAG-USER | CONSOLIDATION_CANDIDATE | nAgents | 1 | 12 | NAGENTS-USER-GUIDE.md |
| NAG-RBAC | CONSOLIDATION_CANDIDATE | nAgents | 1 | 1 | NAGENTS-SPEC.md / NAGENTS-USER-GUIDE.md zależnie od odbiorcy |
| NAG-LEGACY-SPEC | STALE | nAgents | 4 | 4 | NAGENTS-SPEC.md i NAGENTS-DECISIONS.md tylko dla unikalnych sekcji |
| NAG-SOURCES | SOURCE | nAgents | 5 | 5 | NAGENTS-INTEGRATIONS.md / NAGENTS-RESEARCH.md jako cytowane odnośniki |
| NAG-EVIDENCE | EVIDENCE | nAgents | 7 | 84 | NAGENTS-PROCESS.md / docs/process/audit jako indeks dowodów |
| NAG-AUDIT | SOURCE | nAgents | 2 | 2 | NAGENTS-PROCESS.md / audit artifacts remain separate |
| NAG-HISTORY | HISTORY | nAgents | 20 | 75 | NAGENTS-HANDOFF.md / NAGENTS-DECISIONS.md / docs/ABM-HISTORY.md only by coverage matrix |
| ABM-CONTRACT | CANONICAL | AutoBot Monitor | 3 | 5 | AUTOBOT-PROJECT.md as future package; AUTOBOT-KANBAN.md remains canonical |
| ABM-OPS | SOURCE | AutoBot Monitor | 7 | 7 | AUTOBOT-PROJECT.md / docs/ABM-LIFECYCLE.md |
| ABM-RELAY | CONSOLIDATION_CANDIDATE | AutoBot Monitor | 9 | 10 | AUTOBOT-PROJECT.md / docs/ABM-LIFECYCLE.md after owner-gated canary |
| ABM-LIFECYCLE | SOURCE | AutoBot Monitor | 4 | 7 | docs/ABM-LIFECYCLE.md |
| ABM-PACKAGE | CONSOLIDATION_CANDIDATE | AutoBot Monitor | 1 | 2 | AUTOBOT-PROJECT.md / package/README.md after release review |
| ABM-HANDOFF | HISTORY | AutoBot Monitor | 1 | 1 | AUTOBOT-PROJECT.md or docs/ABM-HISTORY.md |
| ABM-EVIDENCE | EVIDENCE | AutoBot Monitor | 136 | 180 | docs/ABM-HISTORY.md index; runs remain separate |
| ABM-REPORTS | EVIDENCE | AutoBot Monitor | 2 | 4 | docs/ABM-HISTORY.md |
| ABM-HISTORY | HISTORY | AutoBot Monitor | 5 | 9 | docs/ABM-HISTORY.md |
| ABM-INTEGRATION | SOURCE | AutoBot Monitor | 1 | 1 | AUTOBOT-PROJECT.md / docs/ABM-LIFECYCLE.md |

## Szczegółowe kandydatury i decyzje

### NAG-ENTRY — nAgents
status: CANONICAL
relacja: warstwowy punkt wejścia; nie jest mechanicznym duplikatem indeksów
uzasadnienie: P5 może zaktualizować wyłącznie NAGENTS-PROJECT.md po readbacku. Nie zmieniać CLAUDE.md ani nie usuwać indeksów w P4.
source of truth: CLAUDE.md (P3 kat. 1)
target package: CLAUDE.md pozostaje; NAGENTS-PROJECT.md jest kandydatem P5
unikalna treść do zachowania: Twarde bariery, kolejność czytania i granica web-first/serwer/Desktop w bieżącym checkoutcie; README.md jest tylko markerem repozytorium.
sprzeczności / rozjazdy: Stare indeksy i handoffy kierują do wcześniejszych stanów; NAGENTS-PROJECT.md proponuje nowy lekki indeks, ale P3 nie nadaje mu jeszcze pierwszeństwa.
ryzyko: Agent może potraktować datowany indeks jako aktualny routing albo uznać lokalny dirty checkout za publikację.
rekomendacja: P5 może zaktualizować wyłącznie NAGENTS-PROJECT.md po readbacku. Nie zmieniać CLAUDE.md ani nie usuwać indeksów w P4.
zakres: 2 rodzin ścieżek, 25 rekordów; przykłady:
  - CLAUDE.md
  - README.md

### NAG-INDEX — nAgents
status: CONSOLIDATION_CANDIDATE
relacja: logicznie nakładające się indeksy i archiwa przekazania
uzasadnienie: P5: ustalić jeden lekki indeks. Zachować stare indeksy jako HISTORY/read-only do czasu macierzy pokrycia P6.
source of truth: CLAUDE.md + docs/process/handoff.md (P3)
target package: NAGENTS-PROJECT.md; docelowo lekkie odnośniki do NAGENTS-HANDOFF.md
unikalna treść do zachowania: NAGENTS-PROJECT.md mapuje pytanie → źródło → readback i granice projektów; starsze AUTOBOOT/NAGENTS-INDEKS zawierają kontekst historyczny, którego nie ma w CLAUDE.
sprzeczności / rozjazdy: Nazwy „aktualny indeks” i stare kolejności czytania nie są dowodem live state; część starych wpisów opisuje inne fazy i profile.
ryzyko: Duplikat punktu startowego może skierować pracę do nieistniejącego lub nieaktualnego stanu.
rekomendacja: P5: ustalić jeden lekki indeks. Zachować stare indeksy jako HISTORY/read-only do czasu macierzy pokrycia P6.
zakres: 3 rodzin ścieżek, 4 rekordów; przykłady:
  - AUTOBOOT-INDEKS.md
  - NAGENTS-INDEKS.md
  - NAGENTS-PROJECT.md

### NAG-PROCESS — nAgents
status: CANONICAL
relacja: warstwy procesu: skill projektowy, szkielety źródłowe, reguły i pomocnicze materiały
uzasadnienie: Nie scalać skillu w P4. P6 ma wskazać sekcje do NAGENTS-PROCESS.md, zachowując SKILL.md jako wejście procesu i źródła pierwotne.
source of truth: .claude/skills/nagents-autobot/SKILL.md + CLAUDE.md (P3 kat. 4)
target package: NAGENTS-PROCESS.md jako przyszły pakiet; SKILL.md i źródło uniwersalne pozostają osobno
unikalna treść do zachowania: Projektowy skill zawiera szczegółową pętlę, bariery, ABC/ECHO, watchdog i raportowanie; źródło uniwersalne oddziela reguły przenośne od wiązań nAgents; SCHEMAT opisuje bezpieczny podział.
sprzeczności / rozjazdy: Szkielet uniwersalny nie jest normą nAgents; pracownikowy opis jest uproszczeniem, a dispatchy są snapshotami zadań.
ryzyko: Mechaniczne scalenie może usunąć wiązania projektu albo nadać materiałowi referencyjnemu rangę normy.
rekomendacja: Nie scalać skillu w P4. P6 ma wskazać sekcje do NAGENTS-PROCESS.md, zachowując SKILL.md jako wejście procesu i źródła pierwotne.
zakres: 7 rodzin ścieżek, 73 rekordów; przykłady:
  - .claude/skills/nagents-autobot/README.md
  - .claude/skills/nagents-autobot/SKILL.md
  - SCHEMAT-PODZIALU-AUTOBOT.md
  - docs/process/echo.md
  - docs/process/tematy.md
  - docs/process/zmiana-procesu.md
  - docs/process/zrodla/autobots-szkielet-uniwersalny.md

### NAG-SPEC — nAgents
status: CANONICAL
relacja: kanoniczny pakiet specyfikacji; dokumenty uzupełniają się sekcjami
uzasadnienie: Traktować checkout jako bieżący, ale nieopublikowany. P5/P6 muszą jawnie rozdzielić lokalny diff od publikacji i nie przepisywać specyfikacji automatycznie.
source of truth: docs/spec/README.md, 00-architektura.md, decisions.md, scenarios.md (P3)
target package: docs/spec/ pozostaje; przyszły NAGENTS-SPEC.md tylko po macierzy pokrycia P6
unikalna treść do zachowania: Architektura, MVP1–MVP4, dziennik decyzji i scenariusze odbioru mają rozdzielone role; lokalny checkout wnosi D-012/D-013 i sekcje web-first/helper.
sprzeczności / rozjazdy: Dziewięć ścieżek ma lokalne zmiany względem nazwanego refu; zdalny snapshot nie zawiera tych nowych sekcji. `01-mvp1.md` ma lokalną korektę tabeli bez zmiany nagłówków.
ryzyko: Kopiowanie zdalnego refu lub starej specyfikacji może cofnąć decyzje i zakres.
rekomendacja: Traktować checkout jako bieżący, ale nieopublikowany. P5/P6 muszą jawnie rozdzielić lokalny diff od publikacji i nie przepisywać specyfikacji automatycznie.
zakres: 8 rodzin ścieżek, 96 rekordów; przykłady:
  - docs/spec/00-architektura.md
  - docs/spec/01-mvp1.md
  - docs/spec/02-mvp2.md
  - docs/spec/03-mvp3.md
  - docs/spec/04-mvp4.md
  - docs/spec/README.md
  - docs/spec/decisions.md
  - docs/spec/scenarios.md

### NAG-DECISIONS — nAgents
status: CANONICAL
relacja: decyzje, ECHO i aktualny pakiet pytań; stara numeracja jest kontekstem
uzasadnienie: P6 ma zrobić macierz decyzja → źródło → status. Nie usuwać ani nie scalać starych pakietów przed ECHO/readbackiem.
source of truth: docs/spec/decisions.md + docs/process/echo.md + docs/process/pytania/2026-08-25-wybory.md (P3)
target package: NAGENTS-DECISIONS.md jako przyszły pakiet; aktualne pliki pozostają źródłami
unikalna treść do zachowania: Literalne decyzje D-001…D-013, ECHO oraz jawne pytania właścicielskie; stare zestawy pytań zawierają warianty i odpowiedzi, których nie wolno przepisać bez rekonsyliacji.
sprzeczności / rozjazdy: Lokalne decisions/echo mają nowe wpisy względem refu; stare pytania mogą wyglądać na otwarte mimo późniejszych decyzji.
ryzyko: Powtórzenie rozstrzygniętego pytania lub cicha zmiana decyzji właściciela.
rekomendacja: P6 ma zrobić macierz decyzja → źródło → status. Nie usuwać ani nie scalać starych pakietów przed ECHO/readbackiem.
zakres: 1 rodzin ścieżek, 12 rekordów; przykłady:
  - docs/process/pytania/2026-08-25-wybory.md

### NAG-HANDOFF — nAgents
status: CANONICAL
relacja: bieżący format handoffu kontra datowane snapshoty
uzasadnienie: P5/P6 mają wskazać tylko bieżący format i jawnie oznaczyć pozostałe pliki HISTORY. Nie kopiować danych osobowych ani sekretów.
source of truth: docs/process/handoff.md (P3 kat. 7)
target package: NAGENTS-HANDOFF.md jako pakiet treści; docs/process/handoff.md pozostaje formatem wejściowym
unikalna treść do zachowania: Krótki format bieżącego handoffu; root HANDOFF-* i archiwum nagents-2026-09-10 przechowują kontekst, dowody i ostrzeżenia z wcześniejszych sesji.
sprzeczności / rozjazdy: Handoff jest snapshotem i może być sprzeczny z live readbackiem; `HANDOFF-nagents.md` ma lokalny dodatek web-first/helper względem remote.
ryzyko: Stary stan infrastruktury lub status zadania zostanie odczytany jako bieżący.
rekomendacja: P5/P6 mają wskazać tylko bieżący format i jawnie oznaczyć pozostałe pliki HISTORY. Nie kopiować danych osobowych ani sekretów.
zakres: 1 rodzin ścieżek, 12 rekordów; przykłady:
  - docs/process/handoff.md

### NAG-RESEARCH — nAgents
status: HISTORY
relacja: noty decyzyjne i materiały rozeznania; logiczny overlap nie oznacza duplikatu
uzasadnienie: Zachować jako HISTORY/EVIDENCE z datą i źródłem. P6 może wyciągnąć tylko unikalne wnioski do NAGENTS-RESEARCH.md; nie usuwać not.
source of truth: docs/process/pamiec.md oraz właściwe źródła pierwotne; nie jest źródłem bieżącego routingu
target package: NAGENTS-RESEARCH.md; decyzje tylko po wpisie w decisions/ECHO
unikalna treść do zachowania: Porównania dróg, topologii, dostawców serwerów i modeli oraz uzasadnienia odrzuceń; noty zachowują tok rozumowania.
sprzeczności / rozjazdy: Ceny, dostępne funkcje, topologia i status wdrożenia są zależne od daty; część hipotez została później skorygowana.
ryzyko: Stara rekomendacja może zostać użyta jako decyzja lub bieżący fakt.
rekomendacja: Zachować jako HISTORY/EVIDENCE z datą i źródłem. P6 może wyciągnąć tylko unikalne wnioski do NAGENTS-RESEARCH.md; nie usuwać not.
zakres: 11 rodzin ścieżek, 88 rekordów; przykłady:
  - LLM-OPEN-SOURCE-nAgents-HANDOFF.md
  - SERWERY-nAgents-ustalenia.md
  - WLASNY-LLM-nAgents-porownanie.md
  - ZALACZNIK-B-serwer-OVH.md
  - docs/nota-01-trzy-drogi-do-agenta.md
  - docs/nota-02-uprzaz-dla-agentow.md
  - docs/nota-02a-aneks-profile-i-zakres-v1.md
  - docs/nota-03-topologia-27-agentow.md
  - docs/nota-04-korekty-i-nowe-materialy.md
  - docs/nota-05-kupic-czy-zbudowac.md
  - docs/nota-08-wybory-otwarte.md

### NAG-INTEGRATIONS — nAgents
status: OWNER_DECISION_REQUIRED
relacja: materiał techniczny + instrukcje + wymagany wybór właściciela
uzasadnienie: Nie wybierać mechanizmu w P4. Najpierw decyzja właściciela, potem readback/test; zachować noty i handoff jako źródła pomocnicze.
source of truth: P3 kat. 11: OWNER_DECISION_REQUIRED; nota 09/10 i handoff M365 są pomocnicze
target package: NAGENTS-INTEGRATIONS.md po decyzji właściciela
unikalna treść do zachowania: Nota Hermesa rozdziela fakty lokalnej instalacji od protokołu; nota Entra i M365 opisują kroki administracyjne, uprawnienia i test dowodu.
sprzeczności / rozjazdy: Opis możliwości nie jest dowodem runtime; handoff M365 oznacza integrację jako nieudowodnioną. Nie wybrano głównego punktu styku.
ryzyko: Zbyt wczesne przepisanie instrukcji może rozszerzyć dostęp lub utrwalić niezweryfikowany endpoint.
rekomendacja: Nie wybierać mechanizmu w P4. Najpierw decyzja właściciela, potem readback/test; zachować noty i handoff jako źródła pomocnicze.
zakres: 4 rodzin ścieżek, 18 rekordów; przykłady:
  - INTEGRACJA-MICROSOFT365.md
  - docs/nota-09-interfejs-hermesa.md
  - docs/nota-10-entra-instrukcja-dla-administratora.md
  - nagents-2026-09-10/04-MICROSOFT365.md

### NAG-APPT0 — nAgents
status: SOURCE
relacja: opracowanie wtórne ↔ przechwycone źródła producenta ↔ dispatch/evidence
uzasadnienie: Zachować trzy warstwy osobno. P6 ma wskazać cytat/sekcję i datę, bez mechanicznego scalania ani traktowania ceny jako aktualnej.
source of truth: P3 kat. 13/history + jawne źródła appto w docs/process/zrodla/
target package: NAGENTS-RESEARCH.md; ewentualne integracje tylko po decyzji
unikalna treść do zachowania: Nota 06/07 syntetyzuje katalog, ceny, prywatność, regulamin i mapowanie appto; sześć plików zrodla zachowuje treść źródłową i link proweniencji.
sprzeczności / rozjazdy: Materiały marketingowe, prawne i katalog integracji zawierają rozbieżności; źródło producenta nie staje się decyzją nAgents.
ryzyko: Utrata rozróżnienia między deklaracją dostawcy, analizą i decyzją właściciela.
rekomendacja: Zachować trzy warstwy osobno. P6 ma wskazać cytat/sekcję i datę, bez mechanicznego scalania ani traktowania ceny jako aktualnej.
zakres: 8 rodzin ścieżek, 96 rekordów; przykłady:
  - docs/nota-06-appto-research.md
  - docs/nota-07-katalog-funkcji.md
  - docs/process/zrodla/appto-cennik-pl.md
  - docs/process/zrodla/appto-integracje-pl.md
  - docs/process/zrodla/appto-polityka-prywatnosci-pl.md
  - docs/process/zrodla/appto-regulamin-pl.md
  - docs/process/zrodla/appto-strona-glowna-pl.md
  - docs/process/zrodla/appto-wdrozenie-kohortowe-pl.md

### NAG-USER — nAgents
status: CONSOLIDATION_CANDIDATE
relacja: upraszczający przewodnik pracownika kontra techniczny proces AutoBot
uzasadnienie: Kandydat do NAGENTS-USER-GUIDE.md po przeglądzie właściciela; zachować techniczne ograniczenia jako odnośniki, nie mieszać ról.
source of truth: docs/proces-dla-pracownikow.md jako kandydat; proces normatywny pozostaje w skillu
target package: NAGENTS-USER-GUIDE.md
unikalna treść do zachowania: Język nietechniczny, pięć sytuacji pracy i minimalna zasada przekazania/odbioru.
sprzeczności / rozjazdy: Nie zawiera pełnych bram bezpieczeństwa, statusów i live readbacków; dispatch NAG-PROC-006 jest dowodem procesu, nie instrukcją dla użytkownika.
ryzyko: Pracownik otrzyma zbyt uproszczone reguły albo agent zastosuje przewodnik jako normę techniczną.
rekomendacja: Kandydat do NAGENTS-USER-GUIDE.md po przeglądzie właściciela; zachować techniczne ograniczenia jako odnośniki, nie mieszać ról.
zakres: 1 rodzin ścieżek, 12 rekordów; przykłady:
  - docs/proces-dla-pracownikow.md

### NAG-RBAC — nAgents
status: CONSOLIDATION_CANDIDATE
relacja: historyczny opis ról/uprawnień z overlapem do architektury i MVP2
uzasadnienie: P6: pokryć każdą operację wskazaną sekcją bieżącej architektury/decisions; nie przenosić bez owner/readback.
source of truth: docs/spec/00-architektura.md + docs/spec/02-mvp2.md; stary plik jako materiał
target package: NAGENTS-SPEC.md / NAGENTS-USER-GUIDE.md zależnie od odbiorcy
unikalna treść do zachowania: Macierz ról, akceptacji i operacji w języku właściciela; uzupełnia skrótową specyfikację.
sprzeczności / rozjazdy: Stary dokument może nie zawierać późniejszych D-012/D-013 i aktualnych granic web-first; statusy wymagają porównania z decisions.
ryzyko: Nadanie uprawnień na podstawie starej macierzy.
rekomendacja: P6: pokryć każdą operację wskazaną sekcją bieżącej architektury/decisions; nie przenosić bez owner/readback.
zakres: 1 rodzin ścieżek, 1 rekordów; przykłady:
  - ROLE-I-UPRAWNIENIA-nAgents.md

### NAG-LEGACY-SPEC — nAgents
status: STALE
relacja: historyczne dokumenty planu/specyfikacji o wysokim podobieństwie semantycznym do docs/spec
uzasadnienie: Oznaczyć HISTORY/STALE; P6 ma wykazać pokrycie unikalnych sekcji przed jakimkolwiek usunięciem.
source of truth: docs/spec/ (P3); legacy files are snapshots
target package: NAGENTS-SPEC.md i NAGENTS-DECISIONS.md tylko dla unikalnych sekcji
unikalna treść do zachowania: Starsza narracja biznesowa, plan etapów, moduły MVP1 i raport struktury; zawiera rationale oraz luki niewidoczne w skrócie.
sprzeczności / rozjazdy: Zakresy MVP, infrastruktura i statusy pochodzą z wcześniejszych dat; lokalna specyfikacja ma późniejsze decyzje i web-first.
ryzyko: Cicha regresja zakresu albo potraktowanie raportu jako dowodu zakończenia.
rekomendacja: Oznaczyć HISTORY/STALE; P6 ma wykazać pokrycie unikalnych sekcji przed jakimkolwiek usunięciem.
zakres: 4 rodzin ścieżek, 4 rekordów; przykłady:
  - DOKUMENTACJA-MVP1.md
  - PLAN-WDROZENIA-nAgents.md
  - RAPORT-nAgents-scenariusz-i-plan.md
  - SPECYFIKACJA-nAgents.md

### NAG-SOURCES — nAgents
status: SOURCE
relacja: zewnętrzne źródła techniczne zachowane jako snapshot
uzasadnienie: Zachować proweniencję i link; walidować aktualny kontrakt osobnym readbackiem. Nie scalać źródeł z analizą.
source of truth: źródła pierwotne z datą, nie bieżący routing nAgents
target package: NAGENTS-INTEGRATIONS.md / NAGENTS-RESEARCH.md jako cytowane odnośniki
unikalna treść do zachowania: Dokumentacja OAuth/Entra/Graph/Hermes zachowana w surowym, datowanym snapshotcie; `sources/8.md` jest pustym artefaktem.
sprzeczności / rozjazdy: Snapshot nie dowodzi aktualności API ani konfiguracji; pusty plik nie dostarcza treści.
ryzyko: Stare API lub brakujący materiał zostanie uznany za potwierdzenie.
rekomendacja: Zachować proweniencję i link; walidować aktualny kontrakt osobnym readbackiem. Nie scalać źródeł z analizą.
zakres: 5 rodzin ścieżek, 5 rekordów; przykłady:
  - nagents-2026-09-10/sources/10.md
  - nagents-2026-09-10/sources/6.md
  - nagents-2026-09-10/sources/7.md
  - nagents-2026-09-10/sources/8.md
  - nagents-2026-09-10/sources/9.md

### NAG-EVIDENCE — nAgents
status: EVIDENCE
relacja: dispatch/report snapshot, nie norma i nie bieżący stan
uzasadnienie: Zachować immutable evidence; indeksować po ID. Nie scalać z normatywnym procesem i nie usuwać.
source of truth: exact run/task/event readback; statyczny plik tylko dowód pomocniczy
target package: NAGENTS-PROCESS.md / docs/process/audit jako indeks dowodów
unikalna treść do zachowania: GOAL, allowlista, wynik i ograniczenia konkretnych dispatchy; rozróżnia zamknięte tematy od planów.
sprzeczności / rozjazdy: Status pliku lub `PASS` nie zastępuje terminalnego eventu; stare dispatchy mogą wskazywać inne decyzje.
ryzyko: Raport zostanie użyty jako dowód bez eventu/artefaktu/readbacku.
rekomendacja: Zachować immutable evidence; indeksować po ID. Nie scalać z normatywnym procesem i nie usuwać.
zakres: 7 rodzin ścieżek, 84 rekordów; przykłady:
  - docs/process/dispatch/NAG-DEC-001-wybory-otwarte.md
  - docs/process/dispatch/NAG-INFO-001-appto-research.md
  - docs/process/dispatch/NAG-INFO-002-katalog-funkcji.md
  - docs/process/dispatch/NAG-PROC-004-skill-samowystarczalny.md
  - docs/process/dispatch/NAG-PROC-005-skill-uniwersalny.md
  - docs/process/dispatch/NAG-PROC-006-ulotka-dla-pracownikow.md
  - docs/process/dispatch/SZABLON.md

### NAG-AUDIT — nAgents
status: SOURCE
relacja: aktywny plan/audit dispatch; steruje fazą, ale nie jest źródłem projektu
uzasadnienie: Zachować jako playbook oraz artefakt fazy; P4 nie zmienia grafu ani nie uruchamia P5.
source of truth: kanban + P1/P2/P3/P4 artifacts; plan jest playbookiem
target package: NAGENTS-PROCESS.md / audit artifacts remain separate
unikalna treść do zachowania: Granice P1–P7, statusy, allowlisty i konkretna dispatch specyfikacja pomocnika serwerowego.
sprzeczności / rozjazdy: Plan nie dowodzi wykonania; live board i eventy mają pierwszeństwo.
ryzyko: Rozpoczęcie następnej fazy na podstawie samego Markdownu.
rekomendacja: Zachować jako playbook oraz artefakt fazy; P4 nie zmienia grafu ani nie uruchamia P5.
zakres: 2 rodzin ścieżek, 2 rekordów; przykłady:
  - docs/process/NAGENTS-DOCS-CONSOLIDATION-PLAN.md
  - docs/process/dispatch/NAG-INFRA-002-pomocnik-serwerowy.md

### NAG-HISTORY — nAgents
status: HISTORY
relacja: datowane handoffy, pamięć, pytania i migracyjne snapshoty
uzasadnienie: Oznaczyć HISTORY/STALE według lifecycle, zachować hash/proweniencję; przenosić tylko po P6 i bez kasowania w P4.
source of truth: bieżące CLAUDE/spec/handoff + live readback; pliki tylko historia
target package: NAGENTS-HANDOFF.md / NAGENTS-DECISIONS.md / docs/ABM-HISTORY.md only by coverage matrix
unikalna treść do zachowania: Chronologia, błędne hipotezy, wyniki migracji, odpowiedzi i ostrzeżenia nieobecne w jednym krótkim źródle.
sprzeczności / rozjazdy: Daty, statusy serwera i odpowiedzi mogą być nieaktualne; część plików jest kopią z `/home/ubuntu/handoffs`.
ryzyko: Historia zostanie pomylona z bieżącym routingiem lub decyzją.
rekomendacja: Oznaczyć HISTORY/STALE według lifecycle, zachować hash/proweniencję; przenosić tylko po P6 i bez kasowania w P4.
zakres: 20 rodzin ścieżek, 75 rekordów; przykłady:
  - HANDOFF-centrum-projektow.md
  - HANDOFF-kolejne-kroki.md
  - HANDOFF-nagents.md
  - HANDOFF-serwer.md
  - HANDOFF-zespol-agentow.md
  - MIGRACJA-OVH-STATUS.md
  - PYTANIA-DO-ODPOWIEDZI-28-08.md
  - PYTANIA-I-ODPOWIEDZI-nAgents.md
  - RAPORTY-Z-HETZNERA-README.md
  - UZUPELNIENIE-MIGRACJI.md
  - docs/process/pamiec.md
  - docs/process/pytania/2026-08-22-kontrola.md
  - … 8 kolejnych; pełna lista w P4-classification.json

### ABM-CONTRACT — AutoBot Monitor
status: CANONICAL
relacja: hierarchia kontraktu: AUTOBOT-KANBAN > AGENTS/supporting run ledger
uzasadnienie: Keep AUTOBOT-KANBAN as first contract. Reconcile AGENTS in a separate owner-gated publication; no overwrite in P4.
source of truth: ABM_CHECKOUT :: AUTOBOT-KANBAN.md (P3 kat. 15)
target package: AUTOBOT-PROJECT.md as future package; AUTOBOT-KANBAN.md remains canonical
unikalna treść do zachowania: Jednoprofilowy routing, status native/process, preflight, idempotency, readback and server Cron boundaries; AGENTS gives product/safety acceptance context.
sprzeczności / rozjazdy: AGENTS remote snapshot is older than local checkout; `runs/README.md` is only run format and cannot override contract.
ryzyko: Agent reads remote AGENTS or report as newer than local canonical contract.
rekomendacja: Keep AUTOBOT-KANBAN as first contract. Reconcile AGENTS in a separate owner-gated publication; no overwrite in P4.
zakres: 3 rodzin ścieżek, 5 rekordów; przykłady:
  - AGENTS.md
  - AUTOBOT-KANBAN.md
  - runs/README.md

### ABM-OPS — AutoBot Monitor
status: SOURCE
relacja: warstwowe runbooki operacyjne; część jest dynamicznym readbackiem
uzasadnienie: Retain docs as sources; each use requires live readback. Reconcile overlapping instructions in P6, not by deleting one.
source of truth: AUTOBOT-KANBAN + current ops docs + live profile/board/service readback
target package: AUTOBOT-PROJECT.md / docs/ABM-LIFECYCLE.md
unikalna treść do zachowania: Cron/helper, card tagging, routing audit, model/effort/Fast, remote Desktop and backup policy; instructions encode fail-closed boundaries.
sprzeczności / rozjazdy: Dynamiczne liczby, project IDs, service state and owner target may age; a guide changed after P2.
ryzyko: Static runbook treated as live state or an old project ID is reused.
rekomendacja: Retain docs as sources; each use requires live readback. Reconcile overlapping instructions in P6, not by deleting one.
zakres: 7 rodzin ścieżek, 7 rekordów; przykłady:
  - docs/ABM-CARD-ROUTING-AUDIT.md
  - docs/ABM-CARD-TAGGING-GUIDE.md
  - docs/ABM-CRON-HELPER-OPERATING-RUNBOOK.md
  - docs/ABM-MODEL-REPAIR-PLAN.md
  - docs/ABM-REMOTE-DESKTOP-SERVER-PLAN.md
  - docs/ABM-SAME-PROFILE-CRON-MIGRATION.md
  - docs/AUTOBOT-MODEL-EFFORT-FAST-POLICY.md

### ABM-RELAY — AutoBot Monitor
status: CONSOLIDATION_CANDIDATE
relacja: local-only active relay snapshot vs checkout Cron document; divergent same-path content
uzasadnienie: Do not choose a winner in P4. Preserve both snapshots, run exact server readback/canary under the ABM gate, then P6 maps sections.
source of truth: active server relay readback + AUTOBOT-KANBAN; no static winner selected
target package: AUTOBOT-PROJECT.md / docs/ABM-LIFECYCLE.md after owner-gated canary
unikalna treść do zachowania: Receiver/owner-chat boundary, spool/receipt/replay details and relay run evidence; this content is absent from named GitHub main.
sprzeczności / rozjazdy: Two docs/CRON-DIRECTIVE-LOOP.md variants differ in length/hash and may reflect different worktree stages; current visible owner target requires live canary.
ryzyko: A stale relay contract can deliver to wrong profile/chat or dispatch duplicate work.
rekomendacja: Do not choose a winner in P4. Preserve both snapshots, run exact server readback/canary under the ABM gate, then P6 maps sections.
zakres: 9 rodzin ścieżek, 10 rekordów; przykłady:
  - docs/CRON-DIRECTIVE-LOOP.md
  - docs/OWNER-CHAT-RELAY.md
  - runs/ABM-OWNER-CHAT-RELAY-001/00-dispatch.md
  - runs/ABM-OWNER-CHAT-RELAY-001/01-operator.md
  - runs/ABM-OWNER-CHAT-RELAY-001/02-evaluator-dispatch.md
  - runs/ABM-OWNER-CHAT-RELAY-001/02-evaluator.md
  - runs/ABM-OWNER-CHAT-RELAY-001/03-defense.md
  - runs/ABM-OWNER-CHAT-RELAY-001/03-evaluator-r2-dispatch.md
  - runs/ABM-OWNER-CHAT-RELAY-001/04-evaluator-r2.md

### ABM-LIFECYCLE — AutoBot Monitor
status: SOURCE
relacja: install/upgrade/uninstall/backup policy separate from runtime evidence
uzasadnienie: Keep as separate lifecycle package; reconcile with package README and current manifest during P6, never install in P4.
source of truth: ABM checkout lifecycle docs; live install still owner-gated
target package: docs/ABM-LIFECYCLE.md
unikalna treść do zachowania: Manual safe install/uninstall, rollback and backup/GitHub boundaries with no live mutation.
sprzeczności / rozjazdy: Instructions in old reports/package docs may describe different package revisions; no live install proof is present.
ryzyko: Automatic install/restart or accidental overwrite of private state.
rekomendacja: Keep as separate lifecycle package; reconcile with package README and current manifest during P6, never install in P4.
zakres: 4 rodzin ścieżek, 7 rekordów; przykłady:
  - docs/INSTALL.md
  - docs/UNINSTALL.md
  - docs/UPGRADE-BACKUP-AND-GITHUB-POLICY.md
  - docs/V2-PACKAGE.md

### ABM-PACKAGE — AutoBot Monitor
status: CONSOLIDATION_CANDIDATE
relacja: same path present local/remote with content divergence
uzasadnienie: Compare package source/manifest/tests in release review; keep both hashes and do not publish/merge in P4.
source of truth: local package README only as uncommitted/current candidate; remote is baseline
target package: AUTOBOT-PROJECT.md / package/README.md after release review
unikalna treść do zachowania: Local 84-line README adds native namespace artifact r4, router/auth/run-root and redaction boundaries over remote 40-line baseline.
sprzeczności / rozjazdy: Local package is tracked_modified and remote main lacks 44 added lines; neither hash is a published post-review winner.
ryzyko: Package consumers receive a README inconsistent with manifest/artifact or claim unsupported live install.
rekomendacja: Compare package source/manifest/tests in release review; keep both hashes and do not publish/merge in P4.
zakres: 1 rodzin ścieżek, 2 rekordów; przykłady:
  - package/README.md

### ABM-HANDOFF — AutoBot Monitor
status: HISTORY
relacja: dated owner handoff, not canonical runtime truth
uzasadnienie: Preserve as HISTORY; extract only stable rules after independent verification.
source of truth: live ABM readback + AUTOBOT-KANBAN; handoff is context
target package: AUTOBOT-PROJECT.md or docs/ABM-HISTORY.md
unikalna treść do zachowania: Migration rationale, owner target distinction and operational warnings for one profile.
sprzeczności / rozjazdy: Handoff may be stale as soon as profile/receiver/board changes.
ryzyko: Replaying a handoff command without readback.
rekomendacja: Preserve as HISTORY; extract only stable rules after independent verification.
zakres: 1 rodzin ścieżek, 1 rekordów; przykłady:
  - docs/handoffs/ABM-SAME-PROFILE-OWNER-HANDOFF.md

### ABM-EVIDENCE — AutoBot Monitor
status: EVIDENCE
relacja: immutable run/task evidence; similar templates are not logical duplicates
uzasadnienie: Keep every run directory and ID; only dedupe exact cross-snapshot records as provenance, never across different run IDs.
source of truth: exact run/event/artifact readback
target package: docs/ABM-HISTORY.md index; runs remain separate
unikalna treść do zachowania: Per-run dispatch, role reports, evaluator findings, defense and final-control records across A/B/C/D, V2, ASTRA and other topics.
sprzeczności / rozjazdy: `PASS` in a report does not prove current service state; repeated template headings hide distinct IDs/attempts.
ryzyko: Deleting/reusing run evidence destroys auditability or confuses attempts.
rekomendacja: Keep every run directory and ID; only dedupe exact cross-snapshot records as provenance, never across different run IDs.
zakres: 136 rodzin ścieżek, 180 rekordów; przykłady:
  - runs/ABM-A-001/00-dispatch.md
  - runs/ABM-A-001/01-operator.md
  - runs/ABM-A-001/02-evaluator.md
  - runs/ABM-A-001/03-final-control.md
  - runs/ABM-A-001/04-integration.md
  - runs/ABM-ARCH-001/00-dispatch.md
  - runs/ABM-ASTRA-001/00-dispatch.md
  - runs/ABM-ASTRA-002/00-defense-r3-dispatch.md
  - runs/ABM-ASTRA-002/00-dispatch.md
  - runs/ABM-ASTRA-002/00-luna-defense-r5-dispatch.md
  - runs/ABM-ASTRA-002/00-luna-final-control-r5-dispatch.md
  - runs/ABM-ASTRA-002/00-luna-repair-evaluator-r5-dispatch.md
  - … 124 kolejnych; pełna lista w P4-classification.json

### ABM-REPORTS — AutoBot Monitor
status: EVIDENCE
relacja: final reports with acceptance mapping; historical snapshots
uzasadnienie: Keep immutable and index by revision/run; no merge into current contract.
source of truth: exact artifacts/tests and Final Control, not report prose alone
target package: docs/ABM-HISTORY.md
unikalna treść do zachowania: V1/V2 acceptance mappings, test counts, known hardening notes and owner gates.
sprzeczności / rozjazdy: Reports are tied to a completed revision; local checkout may contain newer package/ops docs.
ryzyko: Treating historical PASS as live install approval.
rekomendacja: Keep immutable and index by revision/run; no merge into current contract.
zakres: 2 rodzin ścieżek, 4 rekordów; przykłady:
  - FINAL-REPORT-V2.md
  - FINAL-REPORT.md

### ABM-HISTORY — AutoBot Monitor
status: HISTORY
relacja: old plans, requirements and ledgers superseded by current contract/evidence
uzasadnienie: Mark HISTORY/STALE by lifecycle; preserve hashes and run IDs, extract only proven stable decisions in P6.
source of truth: AUTOBOT-KANBAN + current board/readback
target package: docs/ABM-HISTORY.md
unikalna treść do zachowania: Evolution of V1/V2 plan, provider requirements and dispatch ledger; useful rationale for future review.
sprzeczności / rozjazdy: Plan/requirements can conflict with current profile, provider or model policy.
ryzyko: Resurrecting a closed plan or dispatching from a ledger.
rekomendacja: Mark HISTORY/STALE by lifecycle; preserve hashes and run IDs, extract only proven stable decisions in P6.
zakres: 5 rodzin ścieżek, 9 rekordów; przykłady:
  - PLAN-AUTOBOT-PLUGIN.md
  - PLAN-V2.md
  - PLAN.md
  - V2-LEDGER.md
  - V2-REQUIREMENTS.md

### ABM-INTEGRATION — AutoBot Monitor
status: SOURCE
relacja: Hermes patch/provenance material, not nAgents documentation
uzasadnienie: Keep source and manifest; live application/publication remains owner-gated.
source of truth: upstream/base SHA + patch manifest and tests
target package: AUTOBOT-PROJECT.md / docs/ABM-LIFECYCLE.md
unikalna treść do zachowania: Local patch set and integration boundaries for the native plugin.
sprzeczności / rozjazdy: Patch README cannot prove upstream acceptance or live plugin enablement.
ryzyko: Applying/publishing a patch without base/hash/test verification.
rekomendacja: Keep source and manifest; live application/publication remains owner-gated.
zakres: 1 rodzin ścieżek, 1 rekordów; przykłady:
  - hermes-patches/2026-09-12/README.md


## Relacje logiczne, które wymagają osobnego readbacku

### LD-NAG-ENTRY-INDEX — CONSOLIDATION_CANDIDATE
- zakres: NAG-ENTRY, NAG-INDEX, NAG-HANDOFF
- relacja: semantic overlap in entry/current-state routing, not exact duplicate
- sygnał: CLAUDE.md, NAGENTS-PROJECT.md, old indexes and handoffs share start/read/next-state vocabulary
- uzasadnienie: Only CLAUDE.md and docs/process/handoff.md are current normative entry/format sources per P3; other indexes are dated snapshots or candidates.
- decyzja / gate: P5 chooses one lightweight index; P4 keeps every source.

### LD-NAG-SPEC-LEGACY — CONSOLIDATION_CANDIDATE
- zakres: NAG-SPEC, NAG-LEGACY-SPEC, NAG-RBAC
- relacja: logical duplicate/coverage overlap between current spec and legacy plans
- sygnał: highest nAgents text-overlap examples: PLAN-WDROZENIA ↔ SPECYFIKACJA 0.690; RAPORT ↔ SPECYFIKACJA 0.647
- uzasadnienie: Legacy files contain unique rationale and older scope; they cannot replace the current docs/spec package.
- decyzja / gate: P6 builds section-level coverage before any extraction; no deletion in P4.

### LD-NAG-DECISIONS — CONSOLIDATION_CANDIDATE
- zakres: NAG-DECISIONS, NAG-HISTORY
- relacja: question/answer snapshots overlap decisions and ECHO
- sygnał: old question sets repeat decision vocabulary and include variants; current decisions/ECHO add later literal entries
- uzasadnienie: A similar paragraph is not a new owner decision. Current decisions and literal ECHO win; history remains evidence.
- decyzja / gate: Reconcile question IDs/statuses before copying; do not ask or decide again from an archive.

### LD-NAG-PROCESS — CONSOLIDATION_CANDIDATE
- zakres: NAG-PROCESS, NAG-USER, NAG-EVIDENCE
- relacja: layered process docs and dispatch reports share workflow terms
- sygnał: skill, universal skeleton, dispatches and employee guide intentionally reuse process vocabulary
- uzasadnienie: The project skill is normative, universal skeleton is reference, employee guide is audience-specific, dispatches are evidence. Similarity is not permission to merge layers.
- decyzja / gate: Keep the layers and map sections into NAGENTS-PROCESS.md/NAGENTS-USER-GUIDE.md in P6.

### LD-NAG-RESEARCH — CONSOLIDATION_CANDIDATE
- zakres: NAG-APPT0, NAG-RESEARCH, NAG-SOURCES
- relacja: research synthesis overlaps captured primary sources
- sygnał: nota 06/07 summarize appto source captures; Microsoft source snapshots have high link density and dated headings
- uzasadnienie: Synthesis, primary source and owner decision have different evidentiary roles.
- decyzja / gate: Preserve source hashes/links and cite only dated, verified claims in NAGENTS-RESEARCH.md.

### LD-NAG-INTEGRATIONS — OWNER_DECISION_REQUIRED
- zakres: NAG-INTEGRATIONS, NAG-RESEARCH
- relacja: Hermes/Entra/Microsoft 365 integration documents overlap but do not select a mechanism
- sygnał: nota 09/10, M365 handoffs and older research describe adjacent paths
- uzasadnienie: P3 explicitly leaves the integration choice to the owner; protocol description is not runtime proof.
- decyzja / gate: No consolidation or access broadening until owner decision plus live readback/test.

### LD-ABM-CONTRACT — CONSOLIDATION_CANDIDATE
- zakres: ABM-CONTRACT, ABM-OPS, ABM-RELAY
- relacja: contract/runbooks/relay documents share routing and lifecycle terms
- sygnał: ABM contract says one profile/board; runbooks and relay snapshots detail implementation/readback
- uzasadnienie: AUTOBOT-KANBAN is the canonical contract; ops docs are supporting and relay content is local-only/current-candidate.
- decyzja / gate: Use a precedence table in AUTOBOT-PROJECT.md; do not let a static runbook override live readback.

### LD-ABM-PACKAGE — CONSOLIDATION_CANDIDATE
- zakres: ABM-PACKAGE, ABM-LIFECYCLE, ABM-REPORTS
- relacja: package README, lifecycle docs and reports describe overlapping deliverables
- sygnał: local package README adds 44 lines and a new r4 heading over remote baseline; reports map acceptance criteria
- uzasadnienie: Package source, installation policy and historical acceptance evidence must stay separately auditable.
- decyzja / gate: Release review reconciles hashes and manifest; no publication or install in P4.

### LD-ABM-RUN-TEMPLATES — EVIDENCE
- zakres: ABM-EVIDENCE
- relacja: semantic template repetition across distinct run IDs, not duplicates
- sygnał: ABM-V2-H-001 evaluator dispatch snapshots reach cosine similarity 0.935 while IDs/stages/parents differ
- uzasadnienie: Run IDs, role, round and evidence paths give each record independent audit meaning.
- decyzja / gate: Never collapse across run IDs; deduplicate only exact same SHA-256 copies of the same path.


## Allowlista dla przyszłego P5/P6

1. Zanim powstanie nowy pakiet, wskazać dokładnie sekcję źródłową, status, hash, link digest i decyzję właściciela.
2. Nie przenosić treści z `HISTORY`/`STALE` do normy bez literalnego porównania z aktualnym source of truth i aktualizacji `docs/spec/decisions.md`/ECHO, gdy zmienia znaczenie.
3. Nie traktować `LOCAL_ONLY` jako opublikowanego źródła. Najpierw readback, test/canary i jawna zgoda na publikację.
4. NAgents i AutoBot Monitor pozostają odrębnymi projektami. AutoBot Monitor nie staje się The-Game, a materiały The-Game nie trafiają do nAgents.
5. Zachować raw evidence/run IDs i provenance; deduplikować tylko identyczny snapshot tej samej ścieżki, bez kasowania źródła.
6. Po P4 nie ma merge. P5 może wybrać allowlistę, a P6 przygotować sekcja→źródło→target→test→rollback.

## Brakujące decyzje właściciela

- wybór i zakres lekkiego indeksu `NAGENTS-PROJECT.md`;
- integracja web/Hermes/Entra/Microsoft 365 i granica uprawnień;
- zwycięzca między lokalnymi wariantami AutoBot Monitor relay/package/contract po live readbacku;
- ewentualne docelowe pakiety `NAGENTS-*.md` / `AUTOBOT-PROJECT.md` bez łamania obecnego source-of-truth.

Dopóki te bramki nie są rozstrzygnięte, status kandydatów oznacza „do przeglądu”, nie „wykonaj”.
