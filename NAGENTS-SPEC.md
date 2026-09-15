# NAGENTS-SPEC.md — pakiet stagingowy

STATUS: STAGING_ONLY
DOMAIN: INFORMACYJNY
TEMAT: NAG-CONSOLIDATE-SPEC-Q1
P4_STATUS: PASS_WITH_EXPLICIT_OWNER_GATES

Ten plik jest addytywną ekstrakcją do przeglądu. Nie jest nowym źródłem prawdy,
nie zastępuje `docs/spec/`, `CLAUDE.md`, `docs/spec/decisions.md` ani
`docs/spec/scenarios.md` i nie oznacza publikacji. Usunięcie całego katalogu
stagingowego odtwarza stan źródeł; żaden plik kanoniczny nie jest przez ten
pakiet zmieniany, przenoszony ani usuwany.

P4 OBSERVED_AT_UTC: 2026-09-14T14:47:54+00:00
P4 CLASSIFICATION SHA-256: `95d2a06fc80e4cf1c27d59959d84b8631e70acbb3e0b879b83df95c38bf382b8`
P6 PLAN SHA-256: `b282d9e49bf77994a290fbfab71803217d02c9565ab8809e6fbf33501ca4b15c`

**Aktualizacja platformy:** bieżącą podstawą jest `docs/OPENCLAW-STRATEGY.md`
i decyzja D-014. Hashe źródeł w ledgerach P4 poniżej są historycznymi snapshotami;
nie są bieżącym hashem zmodyfikowanego źródła. Przed implementacją wykonaj świeży
odczyt i użyj aktualnego manifestu.
## 0. Zasada proweniencji i rangi

Kolejność rozstrzygania treści:

1. bieżący odczyt wskazanego checkoutu i jego rzeczywiste bajty;
2. literalna decyzja właściciela w `docs/spec/decisions.md` oraz ECHO;
3. bieżąca specyfikacja `docs/spec/` i `CLAUDE.md`;
4. historyczne źródła oraz macierz P4 — wyłącznie jako oznaczony kontekst;
5. raporty i snapshoty — nigdy jako samodzielny routing ani dowód wdrożenia.

Wszystkie fragmenty poniżej mają ślad ścieżki, statusu P4 i SHA-256 w sekcji
`Source ledger`. Hash bieżący oznacza bajty odczytane w checkoutcie podczas
przygotowania pakietu. Hash wariantu P4 oznacza odrębny snapshot z audytu; jego
obecność nie wybiera zwycięzcy przez długość, nazwę, mtime ani zdalną obecność.

## 1. Macierz pokrycia pakietu

| Sekcja | Status wejścia | Źródło prawdy | Rodziny P4 | Co zachowuje staging |
|---|---|---|---|---|
| `SPEC-01` | `CANONICAL` | `docs/spec/00-architektura.md` §1, `CLAUDE.md`, indeks P5 | `PF-0235`, `PF-0173`, `PF-0184` | tożsamość, zakres, granice, siedem barier i granica projektu |
| `SPEC-02` | `CANONICAL` | `docs/spec/00-architektura.md` §2–§10 | `PF-0235` | warstwy, słownik, model danych, bezpieczeństwo, stos, środowiska, NFR |
| `SPEC-03` | `CANONICAL` | `docs/spec/README.md`, `01-mvp1.md`–`04-mvp4.md` | `PF-0236`–`PF-0240` | roadmapa, zakresy, zależności, kryteria i ryzyka MVP1–MVP4 |
| `SPEC-04` | `CANONICAL` | `docs/spec/scenarios.md` i tabele odbioru etapów | `PF-0242`, `PF-0236`–`PF-0239` | pełny kontrakt identyfikatorów A/R/W/K/P/C/U |
| `SPEC-05` | `CANONICAL` | `docs/spec/00-architektura.md` §12, `README.md`, `CLAUDE.md` | `PF-0235`, `PF-0240`, `PF-0173` | web-first, serwerową własność pracy, Desktop jako klienta i pomocnika |
| `SPEC-06` | `CONSOLIDATION_CANDIDATE` z bieżącym źródłem nadrzędnym | architektura §3/§6, `02-mvp2.md`; stara macierz tylko historia | `PF-0191`, `PF-0235`, `PF-0237` | techniczną macierz ról, nadawań, kontroli i zatwierdzeń |
| `SPEC-07` | `STALE` / `HISTORY` | P4 dla legacy; `docs/spec/` rozstrzyga normę | `PF-0174`, `PF-0185`, `PF-0188`, `PF-0194` | unikalny opis legacy jako oznaczony kontekst, nie jako norma |

Decyzje D-001–D-013 pozostają literalnie nadrzędne w
`docs/spec/decisions.md`; ten pakiet pokazuje ich konsekwencje tylko w zakresie
potrzebnym do czytania specyfikacji. Nie tworzy ani nie aktualizuje decyzji.

## SPEC-01 — Tożsamość, zakres i granice

STATUS: `CANONICAL_INPUT` → ekstrakcja `STAGING_ONLY`
SOURCE_OF_TRUTH: `docs/spec/00-architektura.md` §1; `CLAUDE.md`; `NAGENTS-PROJECT.md`
P4: `NAG-SPEC/CANONICAL`, `NAG-ENTRY/CANONICAL`, `NAG-INDEX/CONSOLIDATION_CANDIDATE`

### Czym jest 8gent

8gent to warstwa zarządzania nad agentami, workspace'ami i sesjami OpenClaw.
OpenClaw dostarcza self-hosted Gateway, runtime agenta, kanały, narzędzia,
sesje, automatyzacje i control-plane surfaces. Szczegółowa decyzja, mapowanie
oraz lista luk są w `docs/OPENCLAW-STRATEGY.md`.

8gent odpowiada za cztery pytania, których sam OpenClaw nie rozstrzyga w naszej
domenie:

1. kto to jest — tożsamość pracownika z firmowego katalogu;
2. do czego ma prawo — agenci widoczni i uruchamialni dla tej osoby;
3. ile wolno wydać — limity kosztowe egzekwowane, nie tylko obserwowane;
4. co zostało po operacji — audyt operacji dozwolonych i odrzuconych.

Nie budujemy drugiego runtime'u, OpenRoutera ani OpenMonitora. OpenClaw jest
platformą wykonawczą; własny kod 8gent obejmuje politykę domenową. AutoBot
Monitor jest wyłącznie kandydatem na opcjonalny plugin OpenClaw. The-Game jest
osobnym projektem i nie należy do tej specyfikacji.

### Granice nienaruszalne

| ID | Reguła | Źródło |
|---|---|---|
| B-01 | W repozytorium, bazie i raportach nie ma wartości sekretów; zapisujemy wyłącznie `vault_ref`. | `CLAUDE.md`; architektura §6.6 |
| B-02 | Agent `stanowiskowy` nie ma własnych poświadczeń do systemów firmowych. | `CLAUDE.md`; architektura §6.3; `02-mvp2.md` |
| B-03 | Brak dostępu ujawnia możliwie najmniej: odpowiedź to 404, nie 403. | `CLAUDE.md`; decyzja D-004 |
| B-04 | Obowiązuje domyślna odmowa; brak nadania nie daje dostępu. | `CLAUDE.md`; architektura §6.1 |
| B-05 | Włączanie zmian jest jawne i allowlistowane; nie wolno używać `git add -A` ani `git add .`. | `CLAUDE.md`; norma procesu |
| B-06 | Poza `prod` nie używa się prawdziwych danych osobowych. | `CLAUDE.md`; architektura §8 |
| B-07 | Przekazanie na zewnątrz odbywa się wyłącznie do miejsca wskazanego przez właściciela. | `CLAUDE.md`; norma procesu |

Naruszenie którejkolwiek bariery jest `FAIL`, niezależnie od jakości reszty
pakietu. Zmiana bariery wymaga osobnej decyzji/ECHO, a nie edycji tego stagingu.

### Decyzje, które wpływają na odczyt specyfikacji

| ID | Status | Konsekwencja dla specyfikacji |
|---|---|---|
| D-001 | przyjęta; platforma superseded przez D-014 | budujemy własną warstwę zarządzania; aktualnym runtime'em jest OpenClaw, nie Hermes |
| D-002 | superseded przez D-014 | nie budujemy LiteLLM jako obowiązkowej bramy; provider/model wybiera bezpośrednio OpenClaw |
| D-003 | przyjęta | agent stanowiskowy pobiera dane przez domenę i nie ma własnych kluczy |
| D-004 | przyjęta | brak dostępu to 404 i równoległy wpis `deny` w audycie |
| D-005 | przyjęta | wiedza firmowa i zespołowa jest wersjonowana w plikach repozytorium |
| D-006 | przyjęta | agent procesu krytycznego nie czyta prywatnego poziomu kontekstu |
| D-007 | przyjęta | `tenant_id` istnieje od MVP1 |
| D-008 | przyjęta | FastAPI + Jinja2 + HTMX, bez osobnego SPA |
| D-009 | przyjęta | tokenizacja ogranicza szkodę, ale nie zastępuje umowy powierzenia |
| D-010 | otwarta | topologia agentów wymaga ponownego testu na izolacji OpenClaw |
| D-011 | otwarta, blokująca MVP3 | rezydencja wspólnej pamięci nie jest rozstrzygnięta; OpenClaw nie zmienia tej bramki |
| D-012 | przyjęta | web-first przez OpenClaw Control UI/kanał/web 8gent; klient nie jest właścicielem pracy |
| D-013 | przyjęta; implementacja Hermes-era superseded przez D-014 | zachować readback/fail-closed; oprzeć automatyzację na OpenClaw tasks/Task Flow |
| D-014 | przyjęta | OpenClaw jako runtime i control plane; bez osobnego OpenRoutera/OpenMonitora; AutoBot Monitor tylko jako kandydat pluginu |

Źródło normatywne i pełne uzasadnienie: `docs/spec/decisions.md`, hash
`57264a14bcf38b38811d42b025aba31359470db6c4e33dbc7abe9cad03681d49`.

## SPEC-02 — Architektura, dane, bezpieczeństwo i środowiska

STATUS: `CANONICAL_INPUT` → ekstrakcja `STAGING_ONLY`
SOURCE_OF_TRUTH: `docs/spec/00-architektura.md` §2–§10
P4: `NAG-SPEC/CANONICAL`, rodzina `PF-0235`

### Warstwy systemu

1. **Tożsamość:** Microsoft Entra ID — konta, grupy i wyłączanie pracowników.
2. **Uprząż:** logowanie, rejestr agentów, uprawnienia, audyt, budżety,
   provisioner i strona rozmowy.
3. **Flota Hermesów:** profile z pamięcią, skillami, narzędziami, piaskownicą
   i zatwierdzeniami.
4. **Brama modeli:** LiteLLM — klucze wirtualne, budżety, limity i routing.
5. **Dostawcy modeli:** wymienni bez zmiany warstw uprzęży i floty.

8gent buduje warstwę 2; warstwy 3–4 są wdrażanymi komponentami otwartymi.

### Słownik

| Pojęcie | Znaczenie |
|---|---|
| Uprząż | własne oprogramowanie zarządzające, nie runtime agenta |
| Agent | wpis w rejestrze: nazwa, rodzaj, model, uprawnienia i budżet |
| Profil | instancja Hermesa z osobnym katalogiem domowym, konfiguracją, pamięcią i sesjami |
| `zarzadzajacy` | agent zarządzający uprawnieniami, jeden w projekcie |
| `projektowy` | agent domenowy z kontrolowanymi kluczami systemowymi |
| `stanowiskowy` | agent jednej osoby, bez własnych kluczy, powiązany z agentem projektowym |
| Nadanie (`grant`) | powiązanie agenta z użytkownikiem albo grupą katalogową |
| Rejestr | źródło prawdy o agentach: baza oraz eksport do gita |
| Provisioner | proces wystawiający profile Hermesa z rejestru |

### Model danych

Baza to PostgreSQL 16, nazwy tabel są pojedyncze, klucze są typu `uuid`.
Każda tabela ma `tenant_id` tam, gdzie wymaga tego model domeny.

| Tabela | Istotne pola i reguły |
|---|---|
| `tenant` | `id`, unikalny `slug`, nazwa, `created_at`; w MVP1 jeden wiersz |
| `app_user` | `tenant_id`, unikalne `entra_object_id`, UPN, nazwa, `is_active`, daty |
| `directory_group` / `user_group` | lustro katalogu i członkostwo; Entra pozostaje źródłem prawdy |
| `agent` | slug, rodzaj, `parent_agent_id`, profil/end-point Hermesa, modele, budżet, aktywność |
| `agent_grant` | agent, principal `user|group`, rola `user|owner`, nadanie/cofnięcie |
| `agent_secret_ref` | `(agent_id, nazwa, vault_ref)`; wartości sekretów nigdy tu nie trafiają |
| `conversation` | agent, użytkownik, `hermes_session_id`, czas rozpoczęcia i ostatniej aktywności |
| `audit_event` | actor, akcja, cel, decyzja `allow|deny|error`, powód, request ID, metadane |
| `usage_event` | agent, użytkownik, model, tokeny wejścia/wyjścia, koszt, klucz bramy, request ID |

Krytyczne indeksy to aktywne nadania, audyt po najemcy/czasie oraz zużycie po
agencie/czasie. Brak wiersza aktywnego nadania oznacza brak dostępu.

### Bezpieczeństwo i zagrożenia

- Uprawnienia sprawdza się przy każdym wejściu, nie tylko w interfejsie.
- Żądanie bez nadania zwraca 404, a prawdziwy powód pozostaje w audycie.
- Klucze schodzą w dół hierarchii; agent stanowiskowy nie ma sekretów.
- Operacja nieodwracalna wymaga zatwierdzenia człowieka.
- Brama modeli jest jedynym komponentem znającym klucze dostawców.
- Treść rozmów nie trafia do zwykłych logów aplikacyjnych; retencja jest
  konfigurowalna w MVP4.

| Zagrożenie | Odpowiedź systemu |
|---|---|
| Próba wejścia do cudzego agenta z pominięciem UI | ponowne sprawdzenie, 404 i `deny` w audycie |
| Wyciek klucza agenta projektowego | magazyn sekretów, rotacja i zakres jednej domeny |
| Agent generuje nadmierny rachunek | twardy limit w bramie, blokada zamiast samego ostrzeżenia |
| Odejście pracownika | synchronizacja katalogu i unieważnienie dostępu/sesji |
| Prompt injection w dokumencie | zatwierdzenia operacji nieodwracalnych i brak kluczy u agenta stanowiskowego |
| Dane osobowe w logach | brak treści rozmów w logach, kontrolowana retencja |

### Stos, środowiska i NFR

| Obszar | Ustalenie |
|---|---|
| Uprząż | Python 3.12 + FastAPI |
| Interfejs | Jinja2 + HTMX, bez SPA |
| Baza/migracje | PostgreSQL 16 + Alembic |
| Tożsamość/sesja | OIDC → Entra ID; podpisane ciasteczko `HttpOnly`, `Secure`, `SameSite=Lax` |
| Brama/silnik | LiteLLM proxy; Hermes |
| TLS/wdrożenie | Caddy; Docker Compose |
| Testy/telemetria | pytest + scenariusze; OpenTelemetry |

`dev` używa wyłącznie danych syntetycznych, `staging` syntetycznych lub
zanonimizowanych, a `prod` danych firmowych i pełnego audytu. Cele MVP1 to
pierwszy znak odpowiedzi poniżej 3 s, 99% dostępności w godzinach pracy,
manualne odtworzenie poniżej 4 h, pięć równoczesnych rozmów, bezterminowy audyt
i dodanie agenta poniżej 5 min.

## SPEC-03 — Roadmapa i zakres MVP1–MVP4

STATUS: `CANONICAL_INPUT` → ekstrakcja `STAGING_ONLY`
SOURCE_OF_TRUTH: `docs/spec/README.md`, `docs/spec/01-mvp1.md`,
`docs/spec/02-mvp2.md`, `docs/spec/03-mvp3.md`, `docs/spec/04-mvp4.md`
P4: `NAG-SPEC/CANONICAL`, rodziny `PF-0236`–`PF-0240`

### Roadmapa

| Etap | Cel | Wycena | Zależności/warunek |
|---|---|---:|---|
| MVP1 · Spięcie | logowanie, rejestr, uprawnienia, rozmowa, brama, audyt | 10–12 dni | zewnętrzne Entra; odbiór biznesowy R1–R4 |
| MVP2 · Zarządzalność | panel, katalog, budżety, zatwierdzenia, kopie | 12–15 dni | MVP1; Graph i role administratora |
| MVP3 · Wiedza | kontekst, indeksy, rutyny, testy, Teams | 18–22 dni | odpowiedź na D-011 przed startem |
| MVP4 · Skala | tokenizacja, pula, router, konektory, retencja | 20–25 dni | wyniki i potrzeby z wcześniejszych etapów |

Każdy etap ma kończyć się działającym stanem produkcyjnym, a nie półproduktem.

### MVP1 — Spięcie

STATUS: `CANONICAL`; SOURCE: `docs/spec/01-mvp1.md`, SHA-256
`e2dfb80d198052474450b08a5a4de0b740c1230021a7608e51d56f37e6405066`.

Zakres: szkielet FastAPI/Postgres/migracje, logowanie Entra przez OIDC,
rejestr YAML, funkcja `can_use`, lista agentów, rozmowa ze streamingiem,
proxy do profilu Hermesa, LiteLLM z kluczem wirtualnym i limitem, audyt,
`usage_event`, provisioner oraz Compose/TLS/kopia bazy. Poza zakresem pozostają
panel i synchronizacja grup, wspólna pamięć, Teams, rutyny, agenci
stanowiskowi i tokenizacja.

Punkty wejścia: `/login`, `/agents`, `/agents/{slug}`,
`/agents/{slug}/messages`, `/agents/{slug}/stream`, `/admin/audit`,
`/healthz` i `/readyz`. Każdy punkt z `{slug}` sprawdza uprawnienia niezależnie.
Przepływ wiadomości ponawia kontrolę po otwarciu ekranu, przekazuje użytkownika
i request ID do Hermesa, zapisuje koszt po zakończeniu, a przekroczenie budżetu
blokuje odpowiedź czytelnym komunikatem.

Odbiór obejmuje logowanie, 404 dla wejścia spoza grupy, odrzucenie wiadomości
po cofnięciu nadania, blokadę budżetu, dodanie agenta, zgodność rozliczenia,
odtworzenie bazy oraz przegląd audytu. Scenariusz biznesowy tego etapu to R1–R4
w aktualnym kontrakcie scenariuszy; historyczna tabela „1–10” z pliku etapu nie
zastępuje identyfikatorów `scenarios.md`.

### MVP2 — Zarządzalność

STATUS: `CANONICAL`; SOURCE: `docs/spec/02-mvp2.md`, SHA-256
`10cbba06ac5179dee6ff97107fb9f1a109f91419cca7d803412eba25faaddd48`.

Zakres: panel administracyjny, synchronizacja Microsoft Graph, obsługa odejść,
twarde budżety agenta/zespołu/najemcy, podgląd kosztów, zatwierdzenia, audyt z
odrzuceniami, agenci stanowiskowi, kopie i opcjonalny agent zarządzający. Poza
zakresem: wspólna pamięć, rutyny, Teams i tokenizacja.

Synchronizacja działa cyklicznie i ręcznie: pobiera wskazane grupy, porównuje
członkostwo i nadaje/odbiera tylko to, co wynika z rejestru. Wyłączenie konta
unieważnia dostępy i sesje, a zniknięcie z katalogu tworzy zgłoszenie obsady.
Budżety są egzekwowane w bramie; ostrzeżenia przy 70%/90% nie zastępują
blokady. Podniesienie limitu i każda zmiana panelu wymagają właściwej roli,
audytu i wersjonowania.

Agent stanowiskowy ma `parent`, jedną osobę/grant i `secrets: []`; walidator
odrzuca niepustą listę sekretów. Przejęcie stanowiska odbiera dostęp poprzedniej
osobie, nadaje następnej i zachowuje dorobek stanowiska.

### MVP3 — Wiedza

STATUS: `CANONICAL` z blokadą D-011; SOURCE: `docs/spec/03-mvp3.md`, SHA-256
`8d86f8ff83722b7e835d9f385dfe062960069982e55d512eeff2db3cc48b4047`.

Zakres: poziomy firma → zespół → prywatny, wersjonowana wiedza w repozytorium,
skrócone indeksy i pobieranie dokumentów na żądanie, rozdzielenie pamięci,
rutyny czasowe i zdarzeniowe, testy agenta, pętla propozycji poprawek oraz
kanał Teams. Agent procesu krytycznego nie czyta poziomu prywatnego.

Zmiana wiedzy uruchamia testy; zielony wynik pozwala na propagację, czerwony
zatrzymuje zmianę. Webhook ma własny sekret-ref, weryfikację podpisu,
rate-limit i audyt także dla odrzuceń. Agent proponuje poprawkę po trzech
negatywnych ocenach, ale człowiek ją przyjmuje, odrzuca albo poprawia.

Warunek wejścia: właściciel musi rozstrzygnąć fizyczną rezydencję wspólnej
pamięci. Bez tego MVP3 nie startuje; MVP1 i MVP2 są niezależne.

### MVP4 — Skala

STATUS: `CANONICAL`; SOURCE: `docs/spec/04-mvp4.md`, SHA-256
`cd52c8cf8a37cd31de8d34ca4a62a621a72a61fe8d37b3bdf5843fdf1f1fd7d6`.

Zakres: tokenizacja przed wysłaniem promptu, pula instancji z konsekwentnym
routingiem sesji, jeden router Teams, konektory, wielonajemność, retencja i
eksport oraz bramka akcji z domyślną odmową.

Tokenizacja podmienia identyfikatory przed wyjściem do modelu, a tablica mapowań
zostaje po stronie firmy. Jest pseudonimizacją, nie anonimizacją; dane nadal są
osobowe i wymagana umowa powierzenia nie znika. Pula zachowuje zasadę jednego
pisarza profilu i odtwarza sesje po awarii. Router przed przekazaniem wykonuje
`can_use`. Izolacja najemców jest wymuszana po stronie dostępu, profile i klucze
są odrębne, a instalacja drugiego klienta ma być konfiguracyjna.

Każda akcja narzędzia przechodzi przez regułę polityki: brak reguły oznacza
blokadę i audyt próby, reguła zezwalająca oznacza wykonanie i audyt. Retencja
rozmów, prywatnej pamięci i tablicy tokenizacji jest rozdzielona od dłuższej
retencji audytu.

## SPEC-04 — Kontrakt scenariuszy i mapowanie odbioru

STATUS: `CANONICAL_INPUT` → ekstrakcja `STAGING_ONLY`
SOURCE_OF_TRUTH: `docs/spec/scenarios.md`, SHA-256
`8b8be2a4e631fa92306dca1779a858d0c95803bc6623b0fc60a84c5b55dd570d`
P4: `PF-0242` oraz tabele odbioru `PF-0236`–`PF-0239`

Identyfikatory są zachowane literalnie. Pełny plik `docs/spec/scenarios.md`
pozostaje kontraktem testowym; poniższa lista jest kopią katalogu do stagingu.

### Dostęp i tożsamość

| ID | Scenariusz | Etap |
|---|---|---|
| A1 | Pracownik księgowości loguje się kontem firmowym i rozmawia ze swoim agentem | MVP1 |
| A2 | Pracownik marketingu nie widzi agenta księgowości ani nie dobija się do niego z pominięciem interfejsu | MVP1 |
| A3 | Konto wyłączone w katalogu — logowanie nieudane, trwające sesje unieważnione | MVP1 |
| A4 | Dodanie osoby do grupy w katalogu daje jej dostęp w ciągu minuty | MVP2 |
| A5 | Usunięcie z grupy odbiera dostęp do agentów tej grupy, pozostałe bez zmian | MVP2 |
| A6 | Zwolnienie dyscyplinarne — natychmiastowe odcięcie bez czekania na synchronizację | MVP2 |
| A7 | Przejęcie stanowiska: następca widzi dorobek, poprzednik traci dostęp | MVP2 |

### Rozliczenia

| ID | Scenariusz | Etap |
|---|---|---|
| R1 | Suma prowizji zgadza się co do grosza z kwotą docelową | MVP1 |
| R2 | Usunięte pozycje trafiają do osobnego arkusza z sumami kontrolnymi | MVP1 |
| R3 | Trzy zamknięte miesiące zgadzają się z liczeniem ręcznym | MVP1 |
| R4 | Pytanie o umowę spoza pliku skutkuje przyznaniem braku danych, bez liczby | MVP1 |
| R5 | Korekty są dopasowane po numerze umowy, PPE i dacie wejścia w życie | MVP2 |
| R6 | Zapis wyniku do systemu jest wstrzymany do zatwierdzenia człowieka | MVP2 |
| R7 | Nowa korekta wyzwala weryfikację automatycznie | MVP3 |

### Wiedza

| ID | Scenariusz | Etap |
|---|---|---|
| W1 | Reguła w wiedzy zespołu zmienia zachowanie wszystkich jego agentów | MVP3 |
| W2 | Zmiana psująca zachowanie nie wchodzi, bo testy ją zatrzymują | MVP3 |
| W3 | Cofnięcie złej zmiany przywraca poprzedni stan | MVP3 |
| W4 | To samo pytanie dwóch osób do agenta procesowego daje identyczną odpowiedź | MVP3 |
| W5 | Audyt pokazuje dokumenty użyte przy odpowiedzi | MVP3 |
| W6 | Trzy oceny negatywne rodzą rano propozycję poprawki | MVP3 |

### Koszty

| ID | Scenariusz | Etap |
|---|---|---|
| K1 | Każda rozmowa ma koszt przypisany do agenta i człowieka | MVP1 |
| K2 | Wyczerpanie budżetu zatrzymuje agenta z czytelnym komunikatem | MVP1 |
| K3 | Podniesienie limitu wymaga roli i trafia do audytu | MVP2 |
| K4 | Zużycie jest widoczne per agent, człowiek, model, dzień i miesiąc | MVP2 |
| K5 | Zmiana modelu na tańszy nie wymaga zmian w agentach | MVP1 |

### Proaktywność

| ID | Scenariusz | Etap |
|---|---|---|
| P1 | Poniedziałkowy raport metryk działa bez udziału człowieka | MVP3 |
| P2 | Podpisany webhook z systemu firmowego uruchamia rutynę | MVP3 |
| P3 | Webhook z błędnym podpisem jest odrzucony i zapisany w audycie | MVP3 |
| P4 | Nocna analiza używa modelu mocnego, dzienna rozmowa tańszego | MVP3 |

### Ciągłość i zgodność

| ID | Scenariusz | Etap |
|---|---|---|
| C1 | Odtworzenie bazy z kopii jest przećwiczone, nie zadeklarowane | MVP1 |
| C2 | Awaria instancji Hermesa przenosi rozmowy i odtwarza sesje | MVP4 |
| C3 | Żądanie usunięcia danych osoby działa, a audyt jest zanonimizowany | MVP4 |
| C4 | Eksport danych osoby jest tekstowy | MVP4 |
| C5 | Instalacja drugiego klienta wymaga tylko konfiguracji i jednego dnia | MVP4 |
| C6 | Do dostawcy modelu idą tokeny zamiast numerów PPE | MVP4 |

### Interfejs pracownika i administracja

| ID | Scenariusz | Etap |
|---|---|---|
| U1 | Pracownik loguje się do webowej powierzchni i widzi gotowy profil oraz przydzielone czaty | MVP1 |
| U2 | Pracownik otwiera czat bez znajomości gatewaya, serwera, modelu, poświadczeń i routingu | MVP1 |
| U3 | Zamknięcie przeglądarki odłącza widok, ale serwer utrzymuje zdrową pracę i ten sam stan | MVP1 |
| U4 | Pracownik nie ma dostępu do profili technicznych, budżetów, poświadczeń, routingu ani ustawień zaawansowanych | MVP1 |
| U5 | Administrator zarządza przez web administracyjny albo terminal, bez wymogu Desktopu | MVP2 |
| U6 | Nakładka Desktopu korzysta z obiegu serwerowego i nie zatrzymuje pracy po zamknięciu klienta | MVP2 |

### Serwerowy pomocnik procesu

| ID | Scenariusz | Etap |
|---|---|---|
| U7 | Cron na serwerze dostarcza pełną dyspozycję pomocnikowi bez otwartego Desktopu | MVP1 |
| U8 | Pomocnik po terminalnym evencie uruchamia dokładnie następny zatwierdzony etap grafu | MVP1 |
| U9 | Pomocnik nie tworzy nowego zakresu ani pustej Obrony | MVP1 |
| U10 | Niejasność, brak dowodu, obcy profil/projekt lub konflikt receipt zatrzymuje strumień i eskaluje | MVP1 |
| U11 | Restart lub replay tej samej dyspozycji nie tworzy drugiego runu, następcy ani dostarczenia | MVP1 |

## SPEC-05 — Web-first, serwerowa własność pracy i pomocnik

STATUS: `CANONICAL_INPUT` → ekstrakcja `STAGING_ONLY`
SOURCE_OF_TRUTH: `docs/spec/00-architektura.md` §12, `docs/spec/README.md`,
`CLAUDE.md`; decyzje D-012/D-013 pozostają w `decisions.md`
P4: `PF-0235`, `PF-0240`, `PF-0173`

### Kolejność interfejsu

Najpierw potwierdza się bezpieczny, prosty dostęp webowy do gotowego profilu i
czatu. Pracownik nie konfiguruje gatewaya, serwera, profilu technicznego,
modelu, poświadczeń ani routingu. Ustawienia zaawansowane są dla administratora
przez powierzchnię administracyjną albo terminal. Desktop jest późniejszym
klientem dodatkowym i nie jest warunkiem działania.

### Serwer jako właściciel pracy

Sesje, wywołania modeli, narzędzia, kolejka, workerzy, pamięć i audyt są
utrzymywane przez nadzorowane procesy serwerowe. Zamknięcie przeglądarki lub
Desktopu odłącza klienta, ale nie zatrzymuje backendu ani niezależnego zadania.
Ponowne wejście ma pokazać ten sam stan. Sam napis „połączono” nie jest
dowodem scenariusza U3.

### Pomocnik procesu

Wariant przyjęty w D-013 to niezależny serwerowy pomocnik. Cron jest
read-only generatorem dyspozycji. Pomocnik wykonuje świeży readback Kanbana,
rodziców, runów, eventów i receipts, a następnie prowadzi wyłącznie istniejące,
jednoznaczne przejścia:

`Operator → Evaluator → Obrona tylko przy konkretnych zarzutach → Final Control → INTEGRATION_REQUIRED`

Nie tworzy nowego zakresu, nie zmienia GOAL ani allowlisty, nie tworzy pustej
Obrony i nie integruje, nie pushuje, nie scala ani nie wdraża. Brak dowodu,
obcy profil/projekt, nieznany receipt, konflikt lub niejasność kończą się
fail-closed i eskalacją. Restart/replay jest idempotentny, a terminalny event
plus readback — nie status UI, raport ani `queued` — wyznacza zakończenie fazy.

Wymagania te są testowane przez U7–U11. Staging nie rozstrzyga ich live stanu;
`AUTOBOT-KANBAN.md` AutoBot Monitor i readback runtime pozostają odrębnym
kontraktem operacyjnym.

## SPEC-06 — Techniczna macierz ról, nadań i egzekwowania

STATUS: `CONSOLIDATION_CANDIDATE`; aktualna norma z architektury/MVP2 ma
pierwszeństwo przed historyczną macierzą.
SOURCE_OF_TRUTH: `docs/spec/00-architektura.md` §3, §6; `docs/spec/02-mvp2.md`
P4: `NAG-RBAC`, `PF-0191`, status historyczny; źródła bieżące `PF-0235`, `PF-0237`

### Rodzaje agentów

| Rodzaj | Zakres | Poświadczenia |
|---|---|---|
| `zarzadzajacy` | administracja uprawnieniami i operacjami zarządczymi | zgodnie z kontrolowanym rejestrem |
| `projektowy` | jedna domena, np. księgowość; dane systemowe przez własny zakres | kontrolowane sekrety przez `vault_ref` |
| `stanowiskowy` | jedna obsada stanowiska, dziedziczy wiedzę domenową | zawsze brak własnych poświadczeń; `secrets: []` |

### Macierz operacyjna

| Operacja | Kontrola obowiązkowa | Źródło | Status/scenariusz |
|---|---|---|---|
| Pokazanie listy agentów | filtr po aktywnym `agent_grant` dla zalogowanego użytkownika | architektura §6.1; MVP1 | norma bieżąca; A1/A2/U1 |
| Otwarcie agenta | ponowne `can_use`; brak nadania → 404 i `audit deny` | D-004; MVP1 §4–§6 | norma bieżąca; A2 |
| Wysłanie wiadomości | ponowne `can_use`, request ID przez warstwy, audyt/usage | MVP1 §5 | norma bieżąca; A1/K1 |
| Nadanie grupowe | wpis w rejestrze, potem synchronizacja tylko tej grupy | MVP2 §3 | norma bieżąca; A4/A5 |
| Wyłączenie konta | odebranie dostępów i unieważnienie sesji bez czekania na zwykły cykl | MVP2 §3 | norma bieżąca; A3/A6 |
| Podniesienie limitu | właściwa rola, zatwierdzenie i audyt | MVP2 §4, §7 | norma bieżąca; K3 |
| Zmiana w systemie zewnętrznym | wbudowane zatwierdzenie Hermesa z określeniem, kto może zatwierdzić | architektura §6.5; MVP2 §5 | norma bieżąca; R6 |
| Przejęcie stanowiska | odebranie starego grantu, nadanie nowego, dorobek bez zmian | MVP2 §6 | norma bieżąca; A7 |
| Użycie sekretu przez stanowiskowego | walidator odrzuca niepustą `secrets` | D-003; MVP2 §6 | twarda bariera; B-02 |
| Webhook/akcja | sekret-ref, podpis, rate-limit, polityka i audyt także odmowy | MVP3 §5; MVP4 §6 | przyszły zakres; P2/P3/C6 |

Historyczna `ROLE-I-UPRAWNIENIA-nAgents.md` jest wyłącznie materiałem
`HISTORY` (P4 `NAG-RBAC`, SHA-256
`1485fdfafd1441fa27a5706789da016c621bc1ce81835c7e302071a0712b607d`). Nie
nadaje ról, nie rozszerza dostępu i nie zastępuje decyzji ani bieżącej
architektury. Jej proweniencja i status są zachowane w SPEC-07 i `coverage.json`.

## SPEC-07 — Legacy, konflikty i unikalne uzasadnienie

STATUS: `HISTORY/STALE`; treść wyłącznie nie-normatywna
SOURCE_OF_TRUTH: `docs/spec/` dla bieżącego zakresu; P4 dla historii
P4: `NAG-LEGACY-SPEC`, rodziny `PF-0174`, `PF-0185`, `PF-0188`, `PF-0194`

Cztery poniższe materiały są archiwum poza checkoutem (`HANDOFFS_NAGENTS`). Nie
czytano ich ponownie w tym zadaniu; zachowany jest bezpieczny opis `unique_content`
z P4, ścieżka i hash. To celowe: historyczny plik nie może zostać cicho
podniesiony do normy, a brak raw readbacku nie jest powodem do zgadywania.

| Rodzina / źródło | P4 status | SHA-256 z P4 | Zachowana unikalna treść | Działanie |
|---|---|---|---|---|
| `DOKUMENTACJA-MVP1.md` (`PF-0174`) | `STALE`/`HISTORY` | `827d32ccafd4bd0a40c64f08ed17e14f7f57e09267e983d6350c8959e6b7b16a` | starsza narracja biznesowa, plan etapów, moduły MVP1, raport struktury, rationale i luki niewidoczne w skrócie | zachować jako kontekst; nie używać do zakresu |
| `PLAN-WDROZENIA-nAgents.md` (`PF-0185`) | `STALE`/`HISTORY` | `6baec5f0a9980e400c2b876039bb27cc229b630f3d36e0e30f209cac9e63291e` | starszy plan etapów i zadań oraz uzasadnienia historyczne | porównać literalnie dopiero przy osobnej bramce |
| `RAPORT-nAgents-scenariusz-i-plan.md` (`PF-0188`) | `STALE`/`HISTORY` | `31953e24a4f556c666e04635e23f707648a7538c4ab99ae67d8633aaf6ed5c7e` | starszy raport struktury, scenariuszy, planu i rationale | zachować hash/proweniencję; raport nie dowodzi wykonania |
| `SPECYFIKACJA-nAgents.md` (`PF-0194`) | `STALE`/`HISTORY` | `1cdc0ad813f1f3440c8422b9bf18968751e629b8627c7548225316562e25cd97` | starsza narracja techniczna, wymagania, infrastruktura i kwestie nierozstrzygnięte | nie przepisywać jako normy; bieżące `docs/spec/` ma pierwszeństwo |

Wspólny P4 `unique_content` dla tej grupy to: „starsza narracja biznesowa,
plan etapów, moduły MVP1 i raport struktury; zawiera rationale oraz luki
niewidoczne w skrócie”. Wspólny konflikt: zakresy MVP, infrastruktura i statusy
pochodzą z wcześniejszych dat, a lokalna specyfikacja ma późniejsze decyzje i
web-first. Wspólne ryzyko: cicha regresja zakresu albo potraktowanie raportu jako
dowodu zakończenia. Rekomendacja P4: oznaczyć `HISTORY/STALE` i nie usuwać ani
nie przenosić bez osobnego owner-approved allowlist.

### Konflikty bieżące a warianty P4

| Ścieżka | Bieżący wariant | Wariant historyczny / zdalny | Znaczenie |
|---|---|---|---|
| `docs/spec/00-architektura.md` | `322a6c2a76dc...`, 283 linii | `17318e9014ca...`, 233 linii | lokalne §12 web-first i pomocnik są nowszą treścią; nie nadpisywać |
| `docs/spec/01-mvp1.md` | `e2dfb80d198...`, 214 linii | `cc2687413e02...`, 214 linii | lokalna korekta tabeli, ten sam zestaw nagłówków; porównać bajty |
| `docs/spec/README.md` | `2f5719a2a41e...`, 54 linii | `d075b08f5f14...`, 38 linii | lokalna sekcja web-first jest późniejsza |
| `docs/spec/decisions.md` | `57264a14bcf3...`, 264 linii | `ee793c30d8a2...`, 175 linii | lokalne D-012/D-013 są nadrzędnym bieżącym zapisem |
| `docs/spec/scenarios.md` | `8b8be2a4e631...`, 92 linii | `7084dc99b657...`, 71 linii | lokalne U1–U11 są częścią aktualnego kontraktu |

Warianty są zachowane jako proweniencja, nie jako alternatywna norma. Zdalny
ref nie jest publikacją bieżącego dirty checkoutu. `NAGENTS-PROJECT.md` ma
status readbacku lokalnego indeksu, a nie źródła treści specyfikacji; jego
bieżący SHA to `bab1666d8528622b055b1a3f0b9e21a918be5efabc5be0af2ac06064ae00236a`.

## 8. Source ledger — ślad źródło → hash → sekcja

| Source path | Source ID / P4 family | Status P4 | Bieżący SHA-256 | P4 preferred / warianty |
|---|---|---|---|---|
| `docs/spec/00-architektura.md` | `NAGENTS_CHECKOUT`; `PF-0235` | `CANONICAL` | `322a6c2a76dc4c6bee53c7c1569c306767eb29cd05420bf298e34672aca133cc` | `322a...` current; `17318e...` history |
| `docs/spec/01-mvp1.md` | `NAGENTS_CHECKOUT`; `PF-0236` | `CANONICAL` | `e2dfb80d198052474450b08a5a4de0b740c1230021a7608e51d56f37e6405066` | `e2df...` current; `cc268...` history |
| `docs/spec/02-mvp2.md` | `NAGENTS_CHECKOUT`; `PF-0237` | `CANONICAL` | `10cbba06ac5179dee6ff97107fb9f1a109f91419cca7d803412eba25faaddd48` | one P4 hash |
| `docs/spec/03-mvp3.md` | `NAGENTS_CHECKOUT`; `PF-0238` | `CANONICAL` | `8d86f8ff83722b7e835d9f385dfe062960069982e55d512eeff2db3cc48b4047` | one P4 hash |
| `docs/spec/04-mvp4.md` | `NAGENTS_CHECKOUT`; `PF-0239` | `CANONICAL` | `cd52c8cf8a37cd31de8d34ca4a62a621a72a61fe8d37b3bdf5843fdf1f1fd7d6` | one P4 hash |
| `docs/spec/README.md` | `NAGENTS_CHECKOUT`; `PF-0240` | `CANONICAL` | `2f5719a2a41e0fb9f69404b66a0adfce6d90e3e8fabc96caaf8e5815d036b1f6` | `2f5719...` current; `d075...` history |
| `docs/spec/decisions.md` | `NAGENTS_CHECKOUT`; `PF-0241` | `CANONICAL` | `57264a14bcf38b38811d42b025aba31359470db6c4e33dbc7abe9cad03681d49` | `57264...` current; `ee793...` history |
| `docs/spec/scenarios.md` | `NAGENTS_CHECKOUT`; `PF-0242` | `CANONICAL` | `8b8be2a4e631fa92306dca1779a858d0c95803bc6623b0fc60a84c5b55dd570d` | `8b8be2...` current; `7084...` history |
| `CLAUDE.md` | `NAGENTS_CHECKOUT`; `PF-0173` | `CANONICAL` | `8de583cc4fece98c55c49b5fac9f1562669040204f571ee2daacc9905e9390d2` | P4 variant `8de583...` is current local input |
| `NAGENTS-PROJECT.md` | `NAGENTS_CHECKOUT`; `PF-0184` | `CONSOLIDATION_CANDIDATE` | `bab1666d8528622b055b1a3f0b9e21a918be5efabc5be0af2ac06064ae00236a` | live readback required; not a spec authority |
| `NAGENTS-CONSOLIDATION-PLAN.md` | P6 plan artifact | `PLAN_ONLY` | `b282d9e49bf77994a290fbfab71803217d02c9565ab8809e6fbf33501ca4b15c` | matrix source |
| `docs/process/audit/NAG-DOCS-CONSOLIDATION-Q1/P4-classification.json` | P4 classification | `PASS_WITH_EXPLICIT_OWNER_GATES` | `95d2a06fc80e4cf1c27d59959d84b8631e70acbb3e0b879b83df95c38bf382b8` | machine source; 254 families / 833 records |

The P4 group `NAG-SPEC` contains 8 path families and 96 records. Exact-hash
copies remain record-level provenance: they are not silently merged. The
relevant P4 group `NAG-RBAC` contains 1 family/record; `NAG-LEGACY-SPEC` 4
families/records. P4 globally records 254 path families, 833 records, 272
unique SHA-256 values, 105 exact-hash families, 12 same-path hash-variant
families and 3 drift files.

## 9. Granice stagingu i odwracalność

- Dozwolona zmiana obejmuje wyłącznie ten nowy katalog stagingowy.
- Nie zmieniono żadnego źródła kanonicznego, planu P6, raportu P1–P5,
  `CLAUDE.md`, rejestru tematów ani decyzji.
- Nie czytano ani nie kopiowano sekretów, `.env*`, `state.db`, sesji, surowych
  logów ani danych osobowych.
- Nie wykonano `git add`, commit, push, merge, deploy, fetch, pull, reset,
  stash, clean ani restartu.
- Pakiet nie przenosi plików historycznych; zachowuje tylko bezpieczne
  metadane P4 i unikalny opis źródłowy.
- Wycofanie polega na odrzuceniu katalogu stagingowego jako jednej jednostki;
  źródła pozostają w niezmienionym stanie.

## 10. Kryterium następnej bramki

Ten dokument i `coverage.json` muszą przejść niezależny Evaluator: odtworzenie
7 sekcji, 8 rodzin `NAG-SPEC`, 1 `NAG-RBAC`, 4 `NAG-LEGACY-SPEC`, wszystkich
identyfikatorów scenariuszy, hashy, statusów, konfliktów i granicy stagingu.
Przy zarzutach obrona odpowiada numerami. Dopiero po terminalnym PASS
Evaluator/Final Control i osobnej decyzji ownera można rozważać jakiekolwiek
włączenie lub publikację. Ten pakiet sam nie wystawia `READY_FOR_DEPLOY`.

DEPLOY/PUSH: NIE WYKONANO
