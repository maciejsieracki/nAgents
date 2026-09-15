# 8gent — architektura systemu

**Wersja 0.2 · 15 września 2026 · aktualizacja D-014; dokument źródłowy dla wszystkich etapów**

---

## 1. Cel systemu

8gent to **warstwa zarządzania** nad agentami, workspace'ami i sesjami OpenClaw.
OpenClaw jest self-hosted Gatewayem i control plane dla kanałów, agentów,
narzędzi, sesji oraz zdarzeń; bieżący opis jego zakresu znajduje się w
[`docs/OPENCLAW-STRATEGY.md`](../OPENCLAW-STRATEGY.md).[1][2][3]

8gent odpowiada wyłącznie na cztery pytania, których sam runtime OpenClaw nie
rozstrzyga w domenie naszego produktu:

1. **Kto to jest** — tożsamość pracownika, pobrana z firmowego katalogu
2. **Do czego ma prawo** — którzy agenci są dla niego widoczni i uruchamialni
3. **Ile mu wolno wydać** — limity kosztowe egzekwowane, nie obserwowane
4. **Co po sobie zostawił** — ślad audytowy operacji dozwolonych i odrzuconych

OpenClaw dostarcza wykonanie agenta, kanały, sesje, narzędzia, automatyzacje i
powierzchnie operatorskie. Nie budujemy równoległego runtime'u, OpenRoutera ani
OpenMonitora. Wszystko, co ma być dodane ponad natywne możliwości OpenClaw,
musi mieć wskazaną lukę i osobną bramkę właściciela.

## 2. Zasada nadrzędna: OpenClaw jest runtime'em i control plane

Model, provider, sesja, kanał, automatyzacja i narzędzie są konfigurowane oraz
uruchamiane przez OpenClaw. OpenClaw ma własny wybór modelu, allowlistę,
primary model i fallbacks; 8gent nie wymaga osobnego proxy modelowego.[5][13]

Konsekwencje:

- nie budujemy LiteLLM jako obowiązkowej warstwy;
- nie budujemy ani nie dodajemy OpenRoutera jako wymaganej bramy lub fallbacku;
- 8gent egzekwuje RBAC, tenant, budżet, approval i audyt przed oraz po wywołaniu;
- bezpośredni dostawca i sposób auth są wybierane jawnie, bez ukrytego routingu;
- awaria lub brak funkcji OpenClaw nie jest naprawiany przez drugi, niejawny
  control plane;
- AutoBot Monitor może wejść wyłącznie jako opcjonalny plugin, jeżeli readback
  potwierdzi konkretną lukę.[6][7][8][11]

## 3. Słownik

| Pojęcie | Znaczenie |
|---|---|
| **Agent** | Wpis w rejestrze: nazwa, rodzaj, model, uprawnienia, budżet |
| **Agent OpenClaw** | Izolowany agent z własnym `agentId`, `agentDir` i workspace'em |
| **Sesja** | Sesja należąca do Gatewaya OpenClaw, identyfikowana przez session key |
| **Rodzaj agenta** | `zarzadzajacy` (uprawnienia, jeden), `projektowy` (domena, kontrolowane narzędzia), `stanowiskowy` (jedna osoba, bez własnych sekretów) |
| **Nadanie** (`grant`) | Powiązanie agenta 8gent z użytkownikiem lub grupą z katalogu |
| **Brama / runtime** | OpenClaw Gateway: kanały, sesje, narzędzia, automatyzacje i provider/model config |
| **Rejestr** | Źródło prawdy o agentach 8gent. Baza + eksport do gita |
| **Provisioner** | Proces synchronizujący agentów 8gent z deklarowanym agentem/workspace'em OpenClaw |

## 4. Warstwy

```
┌─ 1 · TOŻSAMOŚĆ ────────────────────────────────────────────┐
│  Microsoft Entra ID — konta, grupy, wyłączanie pracowników │
└────────────────────────────┬───────────────────────────────┘
                             │ OIDC / verified identity
┌─ 2 · 8GENT (nasz kod) ─────▼───────────────────────────────┐
│  rejestr agentów · uprawnienia · audyt · budżety            │
│  polityka · nadania · webowa warstwa domenowa               │
└────────────────────────────┬───────────────────────────────┘
                             │ controlled agent request
┌─ 3 · OPENCLAW GATEWAY ─────▼───────────────────────────────┐
│  agenci · agentDir · workspace'y · sesje · kanały           │
│  narzędzia · skills · Control UI · CLI · nodes              │
└────────────────────────────┬───────────────────────────────┘
                             │ native automation/control
┌─ 4 · OPENCLAW AUTOMATION ──▼───────────────────────────────┐
│  automations/cron · background tasks · Task Flow · events   │
│  opcjonalne pluginy: tools · hooks · services · CLI         │
└────────────────────────────┬───────────────────────────────┘
                             │ provider/model config
┌─ 5 · DOSTAWCY MODELI ──────▼───────────────────────────────┐
│  wybierani bezpośrednio w OpenClaw; OpenRouter nie jest     │
│  wymaganym elementem architektury 8gent                    │
└────────────────────────────────────────────────────────────┘
```

**Budujemy wyłącznie warstwę 2.** Warstwy 3–4 dostarcza OpenClaw; AutoBot
Monitor może zostać dołączony tylko jako plugin po potwierdzeniu konkretnej
luki. OpenClaw opisuje Gateway jako control plane, a pluginy jako rozszerzenia
bez zmiany core.[2][6][7]

## 5. Model danych

Postgres. Nazwy tabel w liczbie pojedynczej, klucze `uuid`.

```sql
-- Najemca. W MVP1 jeden wiersz. Istnieje od początku, żeby MVP4
-- był wdrożeniem, a nie przepisywaniem.
tenant(id, slug UNIQUE, nazwa, created_at)

-- Lustro katalogu. Nie jest źródłem prawdy o ludziach — Entra ID jest.
app_user(id, tenant_id, entra_object_id UNIQUE, upn, display_name,
         is_active, last_seen_at, created_at)

directory_group(id, tenant_id, entra_object_id UNIQUE, nazwa, synced_at)
user_group(user_id, group_id, PRIMARY KEY(user_id, group_id))

-- Rejestr agentów — źródło prawdy
agent(id, tenant_id, slug, nazwa, opis,
      rodzaj CHECK IN ('zarzadzajacy','projektowy','stanowiskowy'),
      parent_agent_id NULL REFERENCES agent(id),
      openclaw_agent_id,
      openclaw_workspace_ref,
      model_default, model_fallback NULL,
      budget_monthly_usd NUMERIC(10,2),
      is_active, created_at, updated_at,
      UNIQUE(tenant_id, slug))

-- Uprawnienia. Brak wiersza = brak dostępu (domyślna odmowa).
agent_grant(id, agent_id, principal_type CHECK IN ('user','group'),
            principal_id, rola CHECK IN ('user','owner'),
            granted_by, granted_at, revoked_at NULL)

-- Sekrety: wyłącznie odwołania. Wartości nigdy tu nie trafiają.
agent_secret_ref(agent_id, nazwa, vault_ref, PRIMARY KEY(agent_id, nazwa))

conversation(id, agent_id, user_id, openclaw_session_key, started_at, last_at)

-- Audyt: rejestruje także to, czego odmówiono
audit_event(id, tenant_id, at, actor_user_id NULL, actor_ip,
            action, target_type, target_id NULL,
            decision CHECK IN ('allow','deny','error'), reason NULL,
            request_id, meta JSONB)

-- Koszty, przypisywalne do agenta i człowieka
usage_event(id, at, agent_id, user_id NULL, model,
            tokens_in, tokens_out, cost_usd, gateway_key, request_id)
```

**Indeksy krytyczne:** `agent_grant(agent_id, principal_type, principal_id) WHERE revoked_at IS NULL`,
`audit_event(tenant_id, at DESC)`, `usage_event(agent_id, at DESC)`.

## 6. Model bezpieczeństwa

Sześć reguł obowiązujących we wszystkich etapach.

### 6.1 Domyślna odmowa
Brak nadania oznacza brak dostępu. Nie ma reguły „wszyscy mogą, chyba że".

### 6.2 Brak dostępu = nie istnieje
Odpowiedź na żądanie do agenta bez uprawnień to **404, nie 403**. Marketing nie ma
dowiadywać się, że istnieje agent kadrowy, z komunikatu o braku dostępu.

### 6.3 Klucze idą w dół hierarchii, nie w bok
Agent stanowiskowy **nie posiada żadnych poświadczeń** do systemów firmowych.
Dane pobiera przez agenta projektowego swojej domeny — ten jest jeden, pilnowany
i audytowany. Dwadzieścia profili z kluczami to dwadzieścia miejsc wycieku.

### 6.4 Egzekwowanie przy danych, nie w interfejsie
Ukrycie pozycji na liście nie jest kontrolą dostępu. Każdy punkt wejścia sprawdza
uprawnienia niezależnie od tego, co pokazał interfejs.

### 6.5 Operacja nieodwracalna wymaga człowieka
Wysłanie korespondencji, zmiana w rozliczeniu, modyfikacja danych w systemie
zewnętrznym — zawsze przez zatwierdzenie. OpenClaw ma własne mechanizmy
approval i polityki narzędzi; 8gent musi je wywołać oraz zweryfikować na
konkretnym kanale, a nie zakładać ich istnienia na podstawie samej konfiguracji.[12]

### 6.6 Sekrety nigdy w kodzie ani w bazie
W bazie wyłącznie odwołania. Wartości są przechowywane w zatwierdzonym
magazynie sekretów środowiska. Żaden dokument, plugin ani agent nie może
utrwalać wartości tokenu, hasła lub klucza.

### 6.7 Model zagrożeń — co zakładamy

| Zagrożenie | Odpowiedź |
|---|---|
| Pracownik próbuje dobić się do cudzego agenta z pominięciem UI | Sprawdzenie uprawnień przy każdym żądaniu; 404; wpis `deny` w audycie |
| Wyciek klucza agenta projektowego | Klucz w magazynie sekretów, rotowalny; zakres ograniczony do jednej domeny |
| Agent w pętli generuje rachunek | Twardy limit w bramie modeli — blokada, nie powiadomienie |
| Pracownik odchodzi, zostaje dostęp | Synchronizacja z katalogiem; wyłączenie konta w Entra odbiera dostęp |
| Wstrzyknięcie polecenia przez treść dokumentu | Zatwierdzenia przy operacjach nieodwracalnych; agent bez kluczy nie ma czym zaszkodzić |
| Dane osobowe w logach | Treść rozmów poza logami aplikacyjnymi; retencja konfigurowalna (MVP4) |

## 7. Stos technologiczny

| Warstwa | Wybór | Uzasadnienie |
|---|---|---|
| Uprząż | **Python 3.12 + FastAPI** | warstwa domenowa 8gent pozostaje własnym kodem |
| Interfejs | **OpenClaw Control UI + web 8gent** | korzystamy z natywnej powierzchni, a własny panel dodajemy tylko dla funkcji domenowych |
| Baza | **PostgreSQL 16** | standard przenośny wszędzie; JSONB do metadanych audytu |
| Migracje | **Alembic** | wersjonowanie schematu od pierwszego dnia |
| Tożsamość | **OIDC → Entra ID** (Authlib) | żadnych własnych haseł; zakres integracji pozostaje owner-gated |
| Sesja | sesja Gatewaya OpenClaw + jawny ref 8gent | bez własnego magazynu kopii rozmów |
| Runtime / brama | **OpenClaw Gateway** | kanały, agenci, sesje, narzędzia, automatyzacje i provider/model config |
| Plugin | **AutoBot Monitor jako opcjonalny plugin OpenClaw** | tylko po potwierdzeniu luki w tasks/Task Flow |
| Wdrożenie | **Docker Compose** lub środowisko zatwierdzone dla OpenClaw | wybór hosta pozostaje osobną decyzją |
| Testy | **pytest** + zestaw scenariuszy | scenariusze przed kodem, patrz `scenarios.md` |
| Telemetria | **OpenClaw tasks/events + OpenTelemetry, jeśli potwierdzone** | usage i audyt muszą mieć niezależny readback |

**Odrzucone świadomie:** osobny frontend SPA (koszt utrzymania bez korzyści przy pięciu
ekranach), Kubernetes w MVP1 (złożoność bez skali), własny system logowania (ryzyko bez
powodu), ORM-owa abstrakcja nad wieloma bazami (nigdy nie zmienimy Postgresa).

## 8. Środowiska

| Środowisko | Cel | Dane |
|---|---|---|
| `dev` | praca lokalna | wyłącznie dane syntetyczne |
| `staging` | testy przed wdrożeniem | dane syntetyczne lub zanonimizowane |
| `prod` | produkcja | dane firmowe, pełny audyt, kopie zapasowe |

**Zasada:** żadne prawdziwe dane osobowe poza `prod`. Zestaw danych testowych generowany
skryptem, nie kopiowany z produkcji.

## 9. Konwencje repozytorium

Cztery pliki trwałej prawdy, przyjęte jako obrona przed tym, że wiedza o projekcie
mieszka w jednej głowie:

| Plik | Zawartość |
|---|---|
| `docs/spec/00-architektura.md` | ten dokument — co budujemy i dlaczego tak |
| `docs/spec/decisions.md` | dziennik decyzji: opcje, wybór, uzasadnienie, konsekwencje |
| `docs/spec/scenarios.md` | scenariusze, które system ma obsłużyć — źródło testów |
| `CLAUDE.md` | instrukcja dla narzędzia budującego; krótka i zmienna |

**Reguła dla dziennika decyzji:** każdy wybór dotyczący kosztu, danych, dostępu,
wdrożenia lub odwracalności trafia do `decisions.md` z opcjami i uzasadnieniem —
zanim powstanie kod, który go realizuje.

## 10. Wymagania niefunkcjonalne

| Wymaganie | Cel MVP1 | Cel docelowy |
|---|---|---|
| Czas do pierwszego znaku odpowiedzi | < 3 s | < 1,5 s |
| Dostępność w godzinach pracy | 99% | 99,5% |
| Odtworzenie z kopii zapasowej | ręcznie, < 4 h | udokumentowane i przećwiczone, < 1 h |
| Liczba równoczesnych rozmów | 5 | 20 |
| Retencja audytu | bezterminowo | konfigurowalna per najemca |
| Czas dodania nowego agenta | < 5 min | < 1 min, przez panel |

## 11. Etapy

| Etap | Zakres | Dni |
|---|---|---|
| [MVP1 — Spięcie](01-mvp1.md) | logowanie, rejestr, uprawnienia, rozmowa, brama, audyt | 10–12 |
| [MVP2 — Zarządzalność](02-mvp2.md) | panel, synchronizacja katalogu, budżety, zatwierdzenia, kopie | 12–15 |
| [MVP3 — Wiedza](03-mvp3.md) | trzy poziomy kontekstu, indeksy, rutyny, testy agenta, Teams | 18–22 |
| [MVP4 — Skala](04-mvp4.md) | tokenizacja, pula instancji, router, konektory, wielonajemność | 20–25 |

Każdy etap kończy się czymś, co działa na produkcji. Żaden nie jest półproduktem
czekającym na następny.

## 12. Kolejność interfejsu i niezależność od klienta

### 12.1 Web jest podstawową powierzchnią 8gent

8gent najpierw dostarcza bezpieczny, prosty dostęp webowy. W pierwszym etapie
pracownik korzysta z OpenClaw Control UI, wybranego kanału albo webowej
warstwy domenowej 8gent; nie konfiguruje sam Gatewaya, serwera, modelu ani
poświadczeń. Control UI jest klientem Gatewaya, a nie osobnym runtime'em.[2][14]

Webowa powierzchnia pracownika pokazuje wyłącznie agentów wynikających z
`agent_grant`. Nie pokazuje ustawień modeli, poświadczeń, budżetów, provisionera,
agentDir ani routingu. Te funkcje są dostępne administratorowi przez jawnie
wybrane powierzchnie OpenClaw i warstwę administracyjną 8gent.

### 12.2 Gateway jest właścicielem pracy

Sesje, wywołania modeli, narzędzia, kanały, automatyzacje, background tasks,
Task Flow i audyt integracyjny muszą być utrzymywane przez nadzorowany
OpenClaw Gateway oraz warstwę 8gent. Zamknięcie przeglądarki, Control UI albo
kanału odłącza klienta, ale nie zatrzymuje Gatewaya ani niezależnego zadania.
OpenClaw opisuje sesje jako własność Gatewaya, a tasks jako rejestr pracy poza
sesją główną.[2][4][10]

Każdy wyjątek musi być ujawniony jako niespełnienie scenariusza ciągłości, nie
ukryty pod etykietą „połączenie działa". Sam status tasku, UI lub obecność
procesu nie zastępuje readbacku wykonania i dostarczenia.

### 12.3 Klient dodatkowy nie jest procesowym rodzicem

Aplikacja Desktop, przeglądarka, kanał, CLI i node są powierzchniami klienta.
Nie mogą być procesowym rodzicem 8gent, agenta, Task Flow, workera ani kolejki.
Możemy dodać własną warstwę webową tylko dla funkcji domenowych, których
Control UI nie pokrywa.

Nie projektujemy obiegu, w którym pracownik ręcznie wybiera Gateway, wpisuje
adres serwera, zakłada agentDir albo pilnuje, czy klient pozostaje otwarty.

### 12.4 Automatyzacja procesu

Wcześniejszy wariant D-013 z Cronem i pomocnikiem Hermes-era jest bazą
historyczną, nie bieżącą instrukcją. Obecna kolejność jest następująca:

```text
OpenClaw automation / event
→ OpenClaw task
→ Task Flow, jeżeli proces jest wieloetapowy
→ readback agenta, runu, wyniku i dostarczenia
→ 8gent policy/audit
→ opcjonalny AutoBot Monitor plugin tylko przy potwierdzonej luce
```

OpenClaw automations są schedulerem, tasks są ewidencją pracy, a Task Flow
koordynuje trwałe procesy wieloetapowe.[9][10][11] AutoBot Monitor nie może
powielać tych mechanizmów bez wykazania konkretnej luki. Po `PASS` procesu
pozostaje osobna bramka `INTEGRATION_REQUIRED`; żaden plugin nie otrzymuje
prawa do merge, push, deployu ani zmiany decyzji właściciela.

## Sources

[1] https://docs.openclaw.ai — OpenClaw official documentation overview
[2] https://docs.openclaw.ai/concepts/architecture — OpenClaw Gateway architecture
[3] https://docs.openclaw.ai/concepts/multi-agent — OpenClaw multi-agent routing
[4] https://docs.openclaw.ai/concepts/session — OpenClaw session management
[5] https://docs.openclaw.ai/gateway/configuration — OpenClaw Gateway configuration
[6] https://docs.openclaw.ai/docs/plugins — OpenClaw plugins
[7] https://docs.openclaw.ai/plugins/building-plugins — OpenClaw building plugins
[8] https://docs.openclaw.ai/plugins/hooks — OpenClaw plugin hooks
[9] https://docs.openclaw.ai/automation — OpenClaw automation overview
[10] https://docs.openclaw.ai/automation/tasks — OpenClaw background tasks
[11] https://docs.openclaw.ai/automation/taskflow — OpenClaw Task Flow
[12] https://docs.openclaw.ai/gateway/security — OpenClaw security
[13] https://docs.openclaw.ai/concepts/models — OpenClaw models CLI and model selection
[14] https://docs.openclaw.ai/web/control-ui — OpenClaw Control UI
