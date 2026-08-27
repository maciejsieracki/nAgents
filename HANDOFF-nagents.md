# HANDOFF — projekt nAgents

**Stan na:** 2026-08-25 · **HEAD:** `3e0ad2b` · **Gałąź:** `claude/git-connection-9sz6dg`
**Repozytorium:** `maciejsieracki/nAgents` · **Etap:** przed MVP1 — dokumentacja gotowa, **kod nie istnieje**

Ten plik zastępuje czytanie transkryptu. Każde zdanie niesie fakt, decyzję albo
ostrzeżenie. Po przeczytaniu wykonaj komendę z sekcji 6, zanim cokolwiek zmienisz.

---

## 1. Kim jestem i jak ze mną pracować

Właściciel firmy NASTER — branża energetyczna, około dwudziestu osób, **jedna osoba
techniczna**. Nie programuje. Prowadzi firmę i podejmuje decyzje o koszcie, danych
i ryzyku. Kontakt: `claude@naster.pl`.

**Język: wyłącznie polski.** Dokumenty, commity, raporty, rozmowa. Commity bez polskich
znaków diakrytycznych (patrz ostrzeżenie 5.3).

**Rejestr wypowiedzi wobec właściciela.** Zero terminów technicznych bez wyjaśnienia
w tym samym zdaniu. Zakazane bez tłumaczenia: API, protokół, konektor, warstwa,
backend, endpoint, token, kontekst, instancja, orkiestracja, middleware, SDK.
Zdanie najwyżej dwadzieścia kilka słów. **To nie jest preferencja stylistyczna —
właściciel raz odrzucił cały zestaw dwudziestu siedmiu pytań słowami: „zadałeś je
technicznym językiem, że ja w ogóle nie wiem, o co chodzi".** Praca poszła do kosza.

**Format pytania do właściciela — sześć części, w tej kolejności, na jego wyraźne
polecenie:** tytuł po ludzku → sytuacja (jak jest dziś, konkretnie) → problem (co
z tego wynika i kiedy uderzy) → możliwe rozwiązania (warianty z „co zyskujesz",
„co tracisz", kosztem oznaczonym jako szacunek, ryzykiem) → pytanie w jednym
zdaniu → rekomendacja z zaznaczeniem, że **rekomendacja nie jest decyzją**.
Odpowiada literami: `1B, 2A, 3C`.

**Czego oczekuje.** Rzeczowości bez ozdobników. Uczciwości o kosztach i o tym, czego
nie wiemy. Przyznania się do błędu wprost, nie w przypisie. Nie znosi deklaracji
bez pokrycia — kilkakrotnie pytał „czy to już zrobiłeś", i miał rację pytając.

**Czego nie robić.** Nie rozstrzygać za niego spraw dotyczących kosztu, danych,
dostępu i odwracalności. Nie zmieniać `docs/spec/decisions.md` — dziennik decyzji
wypełnia wyłącznie on. Nie pytać co sesję o zgodę na pracę wieloagentową (patrz 8.).

---

## 2. Infrastruktura

| Element | Wartość | Uwaga |
|---|---|---|
| Sesja | `session_01CwJz5HdU24wKUTFptVT6uD` | tytuł „nAgents Orkiestrator" |
| Środowisko | `env_01NzRrXW7ZawpN6a29M5kL4z` | nazwa: adres repozytorium; **to je edytować**, nie `Default` |
| Rodzaj środowiska | `anthropic_cloud` | kontener ulotny, po bezczynności odzyskiwany |
| Katalog roboczy | `/home/user/nAgents` | |
| Gałąź robocza | `claude/git-connection-9sz6dg` | **jedyna dozwolona do push** |
| `main` | `9522836` — tylko pierwszy commit | cała praca na gałęzi roboczej |
| Model orkiestratora | `claude-opus-5`, wysiłek `high` | |
| Model wykonawców | Sonnet 5, wysiłek `high` | wszystkie trzy role, z ECHO-001 |
| Rdzenie | 4 | |
| **Limit współbieżności** | **2** | `min(16, rdzenie − 2)`; nie zależy od obciążenia |
| Dostęp sieciowy | `Trusted` | **blokuje appto.ai, youtube, wikipedia, archive.org** |
| Proxy | `127.0.0.1:35001`, CA `/root/.ccr/ca-bundle.crt` | odmowy 403 zgłaszać, nie obchodzić |
| Koszt sesji | około 146 USD (pomiar w trakcie) | limit tygodniowy: ostrzeżenie |
| Skill | `.claude/skills/nagents-autobot/SKILL.md` | 2925 linii, ~160 000 znaków |

**Stos technologiczny — rozstrzygnięty, nie otwierać ponownie.** Python 3.12,
FastAPI, Jinja2, HTMX, PostgreSQL 16, Alembic, Authlib OIDC, Caddy, Docker Compose,
OpenTelemetry. Silnik agenta: **Hermes** (obcy, otwarty). Brama modeli: **LiteLLM**
z naszymi kluczami. Tożsamość: **Microsoft Entra ID**.

---

## 3. Co ZROBIONE — z dowodami

Czterdzieści jeden commitów, wszystkie wypchnięte. Dowodem jest plik w repozytorium
i identyfikator commita, nie zdanie w tym dokumencie.

| Rzecz | Dowód | Weryfikacja |
|---|---|---|
| Dokumentacja techniczna: architektura pięciowarstwowa, model danych SQL, cztery etapy MVP1–MVP4 | `b0df182` → `docs/spec/` (7 plików, 1198 linii) | `wc -l docs/spec/*.md` |
| Jedenaście decyzji architektonicznych | `docs/spec/decisions.md` (175 linii) | D-001…D-009 przyjęte, D-010 odroczona, D-011 **blokująca** |
| Trzydzieści pięć scenariuszy jako źródło testów | `docs/spec/scenarios.md` | grupy A/R/W/K/P/C z przypisaniem do etapów |
| Proces AutoBot związany z projektem, potem uniezależniony od dziedziny | `57a078a` → SKILL.md 942 → **2925 linii**, 22 sekcje | kontrola końcowa: `READY_FOR_DEPLOY`, cztery podstawienia dziedzin |
| Dokument dla pracowników o zasadzie AutoBot | `325ccdb` → `docs/proces-dla-pracownikow.md`, 915 słów | kontrola: zero zdań wymagających dopytania |
| Research konkurenta appto — sześć stron jako źródła pierwotne | `54afa60`, `29eceda` → `docs/process/zrodla/appto-*.md` | 790 linii zapisu źródeł |
| Katalog 58 funkcji appto zestawiony z naszą specyfikacją | `39b2bc8`, `4c17286` → `docs/nota-07`, 612 linii | 26 w planie, 25 luk, 7 odrzuconych; arytmetyka sprawdzona przez kontrolę |
| Analiza dokumentów prawnych appto | `4c17286` → `docs/nota-06`, 552 linie | 10 twierdzeń zweryfikowanych paragraf po paragrafie |
| Osiem pytań otwartych dla właściciela | `3e0ad2b` → `docs/process/pytania/2026-08-25-wybory.md`, 743 linie | zero „NIEZROZUMIAŁYCH", zakazane terminy sprawdzone `grep`em |
| Trzy zapisy ECHO | `docs/process/echo.md` | ECHO-001, ECHO-002, ECHO-003 |

**Czego nie ma:** ani jednej linii kodu produkcyjnego. Ani jednego testu
automatycznego. Żadnego uruchomionego środowiska.

---

## 4. Co DO ZROBIENIA

**Najpierw — czeka na właściciela, nie na wykonawcę.**

1. **Odpowiedź na osiem pytań** z `docs/process/pytania/2026-08-25-wybory.md`.
   Trzy oznaczone `BLOKUJE TERAZ`: gdzie stoi serwer, czy prawnik przed testem na
   prawdziwych danych, jak długo trzymamy zapisy rozmów. Bez pierwszej odpowiedzi
   `NAG-MVP1-009` nie ma gdzie się wykonać.
2. **Poprawka uzasadnienia D-001.** Zapisane uzasadnienie („rozliczenie za tokeny
   odbiera swobodę wyboru modelu") jest **nieprawdziwe** — patrz 7.13. Decyzja
   o budowie stoi z woli właściciela z 2026-08-25. Propozycja nowego brzmienia
   w `docs/nota-06` §5, cztery warianty.
3. **D-011 — rezydencja wspólnej pamięci.** Otwarta, blokuje MVP3.

**Potem — kolejność pracy z `docs/process/tematy.md`, sekcja „Do rozpoczęcia".**

`NAG-MVP1-001-szkielet` → `002-logowanie` → `003-uprawnienia` → `004-rejestr` →
`005-rozmowa` → `006-proxy-hermes` → `007-brama-modeli` → `008-audyt` →
`009-wdrozenie` → `010-pilot-rozliczenia`.

**`NAG-MVP1-010` jest kryterium biznesowym całego etapu:** trzy zamknięte miesiące
przeliczone przez agenta zgadzają się co do złotówki z liczeniem ręcznym. Reszta
to warunki techniczne.

**Zależności zewnętrzne — załatwić, zanim zablokują.**

| Zależność | Kiedy blokuje | Kto załatwia |
|---|---|---|
| Rejestracja aplikacji w Entra ID | od dnia trzeciego MVP1 | administrator dzierżawy, nie my |
| Umowa powierzenia z dostawcą modelu | od MVP1 — brama woła zewnętrznego dostawcę od pierwszego dnia | właściciel + prawnik |
| Zawiadomienie pracowników o monitorowaniu | zegar ustawowy ~2 tygodnie | właściciel |
| Podstawa prawna wobec klientów | przed pilotem na prawdziwych danych | prawnik |

**Dług techniczny, nic nie blokuje.** Skill ma 160 tys. znaków przy limicie Hermesa
100 tys. Rozbicie sekcji 17–19 do `references/` zdejmuje 70 tys. i mieści się w limicie.
**Ale** właściciel wcześniej wymagał, żeby skill był **jednym plikiem do wklejenia**.
Te dwa wymagania są sprzeczne — rozstrzygnąć z nim przed cięciem.

---

## 5. Ostrzeżenia — kosztowne lekcje

**5.1. Streszczenie wyszukiwarki nie jest źródłem.** Dwa razy przyjąłem twierdzenie
ze streszczenia jako fakt o produkcie i dwa razy było fałszywe. Raz **zmieniło to
wynik porównania silników**. Zasada w §12 skilla: fakt o zewnętrznym narzędziu
sprawdza się w źródle wyższego rzędu, nie w cudzym podsumowaniu.

**5.2. Materiał sprzedażowy mówi, co firma twierdzi, nie jak jest.** Przy appto pięć
twierdzeń ze strony rozjechało się z regulaminem i polityką prywatności. Gdy się
rozchodzą, **wiąże dokument prawny**.

**5.3. Polski cudzysłów zamykający psuje `git commit -m`.** Commit z treścią
zawierającą `"` przerwał argument i git potraktował resztę jako ścieżki:
`error: pathspec 'byla' did not match any file(s)`. **W commitach: bez polskich
cudzysłowów, bez znaków diakrytycznych.** Ten sam mechanizm psuje heredoc w Pythonie.

**5.4. Nie obchodź odmowy polityki sieciowej.** 403 na bramce wyjściowej to decyzja
organizacji. Dokumentacja proxy mówi wprost: zgłaszać, nie routować dookoła.
Rozwiązaniem było poproszenie właściciela o wklejenie treści.

**5.5. Ośmiu agentów nie pracuje jednocześnie.** Limit to 2. Fan-out szerszy niż limit
nie przyspiesza — ustawia kolejkę i wydłuża pętlę. Sprawdź `nproc` przed
zaprojektowaniem grafu.

**5.6. Deklaracja wykonawcy nie jest dowodem wykonania.** Kontrola końcowa musi
otwierać pliki i źródła sama. W jednym przebiegu wykonawca zgłosił pracę wykonaną,
a kontrola wykryła **realny ubytek treści** przeniesionej między sekcjami, którego
raport nie wspominał.

**5.7. Brama po ocenie, zawsze.** Bieg bez warunku po Evaluatorze przepuścił werdykt
`FAIL` i przepisywanie ruszyło mimo to. Wzorzec poprawny w 9.3.

**5.8. Nie stawiaj w checkliście Evaluatora rzeczy, które powstają dopiero po nim.**
Jeden Evaluator dał `FAIL` za brak plików, których na jego etapie nie miało być.
Fałszywy alarm z winy projektu grafu, nie wykonawcy.

**5.9. Nigdy `git add -A` ani `git add .`.** Integracja wyłącznie po jawnej liście
ścieżek. To bariera 5, jej naruszenie to `FAIL` niezależnie od jakości reszty.

**5.10. Push wyłącznie na `claude/git-connection-9sz6dg`.** Bariera 7.

---

## 6. Pierwsze zadanie po przejęciu

Uruchom **przed** jakąkolwiek zmianą. Sprawdza, czy przejmujesz stan, jaki opisuje
ten dokument.

```bash
echo "== HEAD (oczekiwane 3e0ad2b) ==" && git log -1 --oneline && \
echo "== GAŁĄŹ (oczekiwane claude/git-connection-9sz6dg) ==" && git branch --show-current && \
echo "== CZYSTO? (oczekiwane: pusto) ==" && git status --short && \
echo "== SYNCHRONIZACJA Z ORIGIN (oczekiwane: pusto) ==" && \
  git fetch origin claude/git-connection-9sz6dg -q && \
  git diff --stat HEAD origin/claude/git-connection-9sz6dg && \
echo "== PLIKI KRYTYCZNE ==" && \
  wc -l CLAUDE.md .claude/skills/nagents-autobot/SKILL.md \
        docs/spec/decisions.md docs/process/tematy.md \
        docs/process/pytania/2026-08-25-wybory.md && \
echo "== DECYZJE OTWARTE (oczekiwane 2: D-010, D-011) ==" && \
  grep -c "otwarta" docs/spec/decisions.md && \
echo "== ZGODA NA ORKIESTRACJĘ (oczekiwane: ECHO-001, 003) ==" && \
  grep -n "^### ECHO" docs/process/echo.md && \
echo "== RDZENIE (limit współbieżności = nproc-2) ==" && nproc && \
echo "== SIEĆ: czy appto nadal blokowane ==" && \
  (curl -sS -o /dev/null -w "%{http_code}\n" --max-time 10 https://www.appto.ai/pl/ || echo "zablokowane")
```

**Oczekiwany wynik:** HEAD `3e0ad2b`, drzewo czyste, brak różnicy wobec origin,
SKILL.md 2925 linii, dwie decyzje otwarte, trzy zapisy ECHO, `nproc` = 4.

Potem przeczytaj w tej kolejności: `CLAUDE.md` → `docs/process/tematy.md` →
`docs/process/echo.md` → `.claude/skills/nagents-autobot/SKILL.md` §0 i §17–21.

---

## 7. Dziennik problemów

Format: **zgłoszenie → co się okazało → jak rozwiązano → status.**

**7.1** „Hermes ma panel administracyjny" → oficjalna dokumentacja nie zna takiego
panelu, jest tylko `managed_scope` → sprostowane w Nocie 02 → **zamknięte.**

**7.2** „Eve nie ma kanału do Teams" → dokumentacja kanałów wymienia Teams jako
wbudowany; wziąłem to z przykładu w README → **zmieniło wynik porównania**: przewaga
Hermesa spadła z dwóch punktów do jednego → sprostowane w Notach 02 i 04 → **zamknięte.**

**7.3** „Ośmiu agentów pracuje nad tematami równolegle" → limit współbieżności to 2
przy czterech rdzeniach; nie sprawdziłem przed obietnicą → §11.3.1 skilla, dobieranie
szerokości do limitu → **zamknięte.**

**7.4** „Rejestracja w Entra ID to jedyna zależność zewnętrzna MVP1" → są trzy, w tym
zawiadomienie pracowników z zegarem ustawowym ~2 tygodnie → poprawiony handoff →
**zamknięte.**

**7.5** Powiedziałem właścicielowi, że **zleciłem** uzupełnienie o trzeci wariant —
nie zleciłem, wykonałem tylko audyt → właściciel tego nie wyłapał, sprostowałem sam
w następnej turze → reguła przeciw samooszukiwaniu w §11.3.3 → **zamknięte.**

**7.6** Schemat wymuszał `minItems: 2` na wariantach → siedem pytań wyszło z A/B
zamiast A/B/C → bieg dorabiający warianty C; **cztery z nich zmieniły rekomendację**
→ **zamknięte.**

**7.7** Wstawka pytania rozjechała numerację: dwa numery „26", brak „27" → poprawione
przez podmianę drugiego wystąpienia → **zamknięte.**

**7.8** `git commit` odrzucony: `pathspec 'byla' did not match` → polski cudzysłów
zamykający w treści przerwał argument `-m` → commit bez znaków diakrytycznych →
**zamknięte, patrz 5.3.**

**7.9** Właściciel: „zadałeś je technicznym językiem, że ja w ogóle nie wiem, o co
chodzi" → dwadzieścia siedem pytań nie do użytku → podział decyzji właściciel–
orkiestrator (§8.1), test zrozumiałości, rejestr językowy (§14.2), przepisanie
sześciu pytań → **zamknięte.**

**7.10** Bramka wyjściowa: `EGRESS_BLOCKED` na `appto.ai` → blokada środowiska, nie
konkurenta; te same 403 na `wikipedia.org` i `archive.org` → pięciu agentów spaliło
bieg na twierdzeniach `[D]` ze streszczeń → właściciel wkleił sześć stron jako źródła
pierwotne → **zamknięte obejściem, sama blokada trwa.**

**7.11** Redakcja wpisała temat do „Zamknięte" **przed** werdyktem kontroli końcowej →
treść była poprawna, nic nie cofnięto → odnotowane jako wyprzedzenie kompetencji →
**otwarte: dopilnować przy następnym dispatchu.**

**7.12** Nota twierdziła, że 2FA i zgodność z DSA „są w specyfikacji" → `grep` po
`docs/spec/` daje zero trafień dla obu → sprostowane, 2FA dopisane do luk →
**zamknięte.**

### Hipotezy, które okazały się błędne

**7.13** *„appto rozlicza za tokeny u dostawcy, co odbiera swobodę wyboru modelu"* —
**przesłanka decyzji D-001.** Cennik: jednostką jest kredyt, **wszystkie modele
dostępne bez dopłat**, przełączane przez użytkownika. Swoboda wyboru w ich katalogu
istnieje; nie ma własnego klucza do dostawcy. **Decyzja o budowie stoi, uzasadnienie
do poprawienia** — czeka na właściciela.

**7.14** *„appto ma tylko logowanie Google, więc nie spełnia naszego wymagania
o Microsofcie"* — strona główna wymienia tylko Google, ale cennik mówi: „Google,
Microsoft albo własne SSO". Luka nie istniała.

**7.15** *„Umowy powierzenia nie ma, polityka prywatności jej nie zastępuje"* —
**moje twierdzenie, nieprawdziwe.** Umowa powierzenia jest **załącznikiem numer 1
do regulaminu**, w trybie art. 28 RODO. Wyciągnąłem wniosek z niepełnych źródeł
zamiast napisać „nie wiem". Sprostowanie widoczne w Nocie 06.

**7.16** *„DSA prawdopodobnie nikogo tu nie dotyczy"* — dotyczy appto: ich polityka
powołuje art. 16 i 20 DSA, a regulamin wyznacza punkt kontaktowy z art. 11–12.
Wobec naszego narzędzia wewnętrznego nadal prawdopodobnie nie stosuje się.

**7.17** *„Warstwa integracji to nasz problem do rozwiązania"* — architektura
przypisuje wykonywanie operacji w programach **Hermesowi**, nie nam. Nasza część to
przechowywanie odwołań do haseł, decyzja co podłączamy i sprawdzanie uprawnień.
**To zmniejszyło zakres pracy o rząd wielkości.**

**7.18** *„Program rozliczeniowy to system z logowaniem"* — scenariusze R1–R4
opisują dzisiejszą pracę na plikach Excel. Cztery warianty integracji odpowiadały na
nieistniejący problem; wyłapane przed wysyłką do właściciela.

---

## 8. Decyzje strategiczne

| Pytanie | Decyzja | Uzasadnienie |
|---|---|---|
| Kupić gotową platformę czy zbudować? | **Budujemy** (D-001) | Właściciel 2026-08-25: „osiągnąć to samo, ale jako własna platforma". **Uwaga: pierwotne uzasadnienie obalone, patrz 7.13** |
| Kto jest silnikiem agenta? | **Hermes**, obcy i otwarty | Nie budujemy agenta. Budujemy warstwę: kto to jest, do czego ma prawo, ile wolno mu wydać, co zostawił |
| Kto trzyma klucze do modeli? | **Nasza brama LiteLLM** (D-002) | Budżet egzekwowalny w jednym punkcie, koszt przypisywalny, zmiana dostawcy bez ruszania agentów. Świadomy koszt: pojedynczy punkt awarii |
| Czy agent stanowiskowy ma własne hasła? | **Nie** (D-003) | Dwadzieścia profili z kluczami to dwadzieścia miejsc wycieku. Koszt: dodatkowy skok przy każdym pytaniu o dane |
| Co zwraca brak dostępu? | **404, nie 403** (D-004) | Komunikat o odmowie ujawnia istnienie zasobu |
| Gdzie mieszka wiedza agentów? | **W repozytorium, nie w bazie** (D-005) | Wersjonowanie, przegląd zmian, cofnięcie za darmo |
| Wielonajemność — od kiedy? | **Od pierwszego dnia** w modelu danych (D-007) | Dołożenie później oznacza przepisanie wszystkich zapytań |
| Frontend osobny czy szablony? | **Szablony serwerowe** (D-008) | Jedna osoba techniczna nie utrzyma dwóch aplikacji |
| Topologia: 27 profili czy hierarchia? | **Odroczona** (D-010) | Rozstrzygnięcie po MVP1, na danych z użycia. Rejestr obsługuje oba warianty |
| Gdzie leży wspólna pamięć? | **OTWARTE** (D-011) | **Blokuje MVP3.** MVP1 i MVP2 działają bez tego |
| Jak agent łączy się z programami? | **OTWARTE** — brak wpisu w rejestrze | Rekomendacja: mieszanka. **Odradzony wprost wariant płatnego pośrednika** — odtwarza zależność od jednej firmy, przez którą odrzucono appto |
| Czy praca idzie do subagentów? | **Tak, bezterminowo** (ECHO-001, potwierdzone ECHO-003) | Właściciel: „w tym czacie nie pracujesz nad niczym sam" |
| Jak zlecać? | **Wyłącznie przez workflow** (ECHO-002) | Tylko tam da się ustawić poziom wysiłku. Zwykłe wywołanie subagenta przyjmuje sam model |
| Czy proces jest tylko dla informatyki? | **Nie** | Zasady stałe, liczby to parametry, pojęcia odwzorowywane na dziedzinę. Wykonawcą bywa człowiek albo program |

---

## 9. Wzorce pracy, które się sprawdziły

**9.1. Zapis zlecenia przed startem wykonawcy.** Nie po. Zawiera wyzwalacz (dlaczego
teraz), cel, kryteria końca, zakres z jawnym „poza zakresem", listę dozwolonych
ścieżek, plan sprawdzenia, zależności i dotknięte bariery. **Wyzwalacz jest trzecim
obowiązkowym składnikiem obok zadania i kryterium — „bo była kolej" nie jest
wyzwalaczem.** Siedem zapisów w `docs/process/dispatch/`.

**9.2. Trzy role, nikt nie ocenia własnej pracy.** Operator wykonuje. Evaluator jest
adwokatem diabła — dostaje zadanie *obalić*, nie potwierdzić. Kontrola końcowa patrzy
na **wytwór na dysku**, nie na raport, i wykonuje własną próbę na **innych** pozycjach
niż Evaluator. Ten wzorzec wyłapał: realny ubytek treści, cztery fałszywe przypisania
do specyfikacji, rozmycie zasady w parametr i błędne założenie o systemie rozliczeń.

**9.3. Brama po ocenie.** W skrypcie: jeśli werdykt nie jest czysty, wchodzi domknięcie
**przed** kontrolą końcową. Bez bramy `FAIL` przelatuje i praca idzie dalej na złym
materiale.

```js
const w = String(ocena).trim().split('\n')[0].toUpperCase()
if (!w.includes('PASS') || w.includes('NOTES')) { /* domknięcie */ }
```

**9.4. `PASS-WITH-NOTES` nie zamyka tematu, jeśli uwagi dotyczą celu.** Kontrola
końcowa dwa razy odmówiła zamknięcia mimo pozytywnie brzmiącego werdyktu. Za każdym
razem słusznie.

**9.5. Znaczniki źródła przy każdym twierdzeniu o świecie zewnętrznym.**
`[F]` fakt z adresem i cytatem, `[W]` wniosek z przesłanką, `[D]` domysł z poziomem
pewności i informacją, co by go rozstrzygnęło. Przy blokadzie sieciowej dało to
uczciwy obraz zamiast pozornej wiedzy.

**9.6. Test podstawienia przy wszystkim, co ma być uniwersalne.** Przeczytać dokument
podstawiając biuro rachunkowe, kancelarię, marketing i zakład produkcyjny. Każde
zdanie tracące sens przy którymkolwiek podstawieniu jest błędem. Tak powstał skill
niezależny od dziedziny.

**9.7. Test odcięcia.** Czytelnik ma wyłącznie ten jeden plik — czy odtworzy z niego
całość? Kontrola końcowa wypisuje listę i porównuje z faktycznym drzewem.

**9.8. Ryzyko odwrotne ma własne zadanie.** Przy uniwersalizacji łatwo rozmyć proces
w zbiór sugestii. Evaluator dostał osobny punkt: *sprawdź, czy sparametryzowano coś,
co powinno zostać zasadą*. Wyłapał, że pytanie o niezależne sprawdzenie przyjmowało
odpowiedź „u nas nikt nie sprawdza" jako ustalenie.

**9.9. Podział na węzły ma cenę.** Rozbicie zadania na osobnych wykonawców kosztuje
wielokrotność wywołań wobec jednego przebiegu. **Najmniejszy skuteczny graf, nie
największy możliwy.** Brama triage przed każdym podziałem.

**9.10. Rejestry aktualizowane przy każdej zmianie statusu.** `docs/process/tematy.md`
jest źródłem prawdy o tym, co aktywne, zablokowane i zamknięte. Temat, którego cel
nie został osiągnięty, idzie do **zablokowanych**, nie do zamkniętych — nawet jeśli
powstał z niego dobry dokument.

---

**Koniec handoffu.** Przy sprzeczności między tym plikiem a stanem repozytorium
**wygrywa repozytorium** — ten dokument jest zdjęciem z 2026-08-25, nie źródłem prawdy.
