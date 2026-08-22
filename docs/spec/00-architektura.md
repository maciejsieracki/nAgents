# nAgents — architektura systemu

**Wersja 0.1 · 22 sierpnia 2026 · dokument źródłowy dla wszystkich etapów**

---

## 1. Cel systemu

nAgents to **warstwa zarządzania** nad flotą instancji Hermesa. Nie jest silnikiem agenta —
Hermes nim jest, jest otwarty i wykonuje całą pracę: rozmowę, narzędzia, piaskownicę,
pamięć, kanały, wybór modelu.

nAgents odpowiada wyłącznie na cztery pytania, na które Hermes nie odpowiada:

1. **Kto to jest** — tożsamość pracownika, pobrana z firmowego katalogu
2. **Do czego ma prawo** — którzy agenci są dla niego widoczni i uruchamialni
3. **Ile mu wolno wydać** — limity kosztowe egzekwowane, nie obserwowane
4. **Co po sobie zostawił** — ślad audytowy operacji dozwolonych i odrzuconych

Wszystko poza tym jest cudze, otwarte i wymienne.

## 2. Zasada nadrzędna: brama modeli należy do nas

Rozliczenie za tokeny u dostawcy platformy odbiera wybór modelu, więc odpada.
**Wszystkie wywołania modeli przechodzą przez naszą bramę, z naszymi kluczami.**

To nie jest detal wdrożeniowy, tylko fundament architektury. Konsekwencje:

- zmiana dostawcy modelu nie dotyka ani jednej konfiguracji agenta
- budżet jest egzekwowany w jednym punkcie, a nie w dwudziestu siedmiu
- koszt jest przypisywalny do agenta i do człowieka
- żaden komponent poza bramą nie zna kluczy dostawców

## 3. Słownik

| Pojęcie | Znaczenie |
|---|---|
| **Uprząż** | Nasze oprogramowanie. Warstwa zarządzania, nie runtime |
| **Agent** | Wpis w rejestrze: nazwa, rodzaj, model, uprawnienia, budżet |
| **Profil** | Instancja Hermesa realizująca agenta. Osobny katalog domowy, własna konfiguracja, pamięć, skille, sesje |
| **Rodzaj agenta** | `zarzadzajacy` (uprawnienia, jeden), `projektowy` (domena, klucze do systemów), `stanowiskowy` (jedna osoba, bez kluczy) |
| **Nadanie** (`grant`) | Powiązanie agenta z użytkownikiem lub grupą z katalogu |
| **Brama modeli** | LiteLLM. Jedyne wyjście do dostawców modeli |
| **Rejestr** | Źródło prawdy o agentach. Baza + eksport do gita |
| **Provisioner** | Proces wystawiający profile Hermesa na podstawie rejestru |

## 4. Warstwy

```
┌─ 1 · TOŻSAMOŚĆ ────────────────────────────────────────────┐
│  Microsoft Entra ID — konta, grupy, wyłączanie pracowników │
└────────────────────────────┬───────────────────────────────┘
                             │ OIDC
┌─ 2 · UPRZĄŻ (nasz kod) ────▼───────────────────────────────┐
│  logowanie · rejestr agentów · uprawnienia · audyt         │
│  budżety · provisioner · strona rozmowy                    │
└──────────┬─────────────────────────────────┬───────────────┘
           │ wystawia i odbiera              │ proxy rozmowy
┌─ 3 · FLOTA HERMESÓW ──────▼─────────────────▼──────────────┐
│  profil: księgowość · sprzedaż · marketing · …             │
│  skille, pamięć, narzędzia, piaskownica, zatwierdzenia     │
└────────────────────────────┬───────────────────────────────┘
                             │ wszystkie wywołania modeli
┌─ 4 · BRAMA MODELI ─────────▼───────────────────────────────┐
│  LiteLLM — klucze wirtualne, budżety, limity, routing      │
└────────────────────────────┬───────────────────────────────┘
                             │
┌─ 5 · DOSTAWCY MODELI ──────▼───────────────────────────────┐
│  dowolni, wymienni bez ruszania warstw 2 i 3               │
└────────────────────────────────────────────────────────────┘
```

**Budujemy wyłącznie warstwę 2.** Warstwy 3 i 4 wdrażamy jako gotowe otwarte oprogramowanie.

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
      hermes_profile, hermes_endpoint,
      model_default, model_fallback NULL,
      budget_monthly_usd NUMERIC(10,2),
      is_active, created_at, updated_at,
      UNIQUE(tenant_id, slug))

-- Uprawnienia. Brak wiersza = brak dostępu (domyślna odmowa).
agent_grant(id, agent_id, principal_type CHECK IN ('user','group'),
            principal_id, rola CHECK IN ('user','owner'),
            granted_by, granted_at, revoked_at NULL)

-- Sekrety: wyłącznie odwołania. Wartości nigdy nie trafiają do bazy.
agent_secret_ref(agent_id, nazwa, vault_ref, PRIMARY KEY(agent_id, nazwa))

conversation(id, agent_id, user_id, hermes_session_id, started_at, last_at)

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
zewnętrznym — zawsze przez zatwierdzenie. Hermes ma ten mechanizm wbudowany.

### 6.6 Sekrety nigdy w kodzie ani w bazie
W bazie wyłącznie odwołania. Wartości w magazynie sekretów środowiska.
Brama modeli jest jedynym komponentem znającym klucze dostawców.

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
| Uprząż | **Python 3.12 + FastAPI** | ten sam język co brama modeli; jedna osoba utrzymuje jeden stos |
| Interfejs | **Jinja2 + HTMX** | bez osobnego procesu budowania frontendu; w MVP1 pięć ekranów |
| Baza | **PostgreSQL 16** | standard przenośny wszędzie; JSONB do metadanych audytu |
| Migracje | **Alembic** | wersjonowanie schematu od pierwszego dnia |
| Tożsamość | **OIDC → Entra ID** (Authlib) | żadnych własnych haseł |
| Sesja | ciasteczko podpisane, `HttpOnly`, `Secure`, `SameSite=Lax` | bez własnego magazynu tokenów |
| Brama modeli | **LiteLLM proxy** | klucze wirtualne, budżety na czterech poziomach, otwarte |
| Silnik agenta | **Hermes** | otwarty, 200+ backendów, profil jako jednostka izolacji |
| Reverse proxy / TLS | **Caddy** | certyfikaty automatycznie; wymagane pod webhooki Teams w MVP3 |
| Wdrożenie | **Docker Compose** → pula kontenerów | MVP1 na jednym serwerze; przenośne poza Azure |
| Testy | **pytest** + zestaw scenariuszy | scenariusze przed kodem, patrz `scenarios.md` |
| Telemetria | **OpenTelemetry** | Hermes eksportuje natywnie; wspólny zbiór od MVP2 |

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
