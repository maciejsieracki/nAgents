# Nota decyzyjna 03 — Topologia 27 agentów

**NASTER · projekt nAgents · 22 sierpnia 2026**

Jeden zarządzający, sześciu projektowych, dwudziestu stanowiskowych. Jak to złożyć
na Hermesie, którymi mechanizmami wiedza krąży między warstwami i co dokładnie dzieje się
w dniu, w którym odchodzi pracownik.

Wersja do czytania (ze schematami): https://claude.ai/code/artifact/b4a6c09d-16ca-41f5-9446-11cd30e77c4d

---

## Werdykt

**Topologia składa się na Hermesie w całości — wszystkie trzy mechanizmy, których wymaga,
są udokumentowane i gotowe.** Nie trzeba ich pisać, trzeba je poprawnie spiąć.

- Agent stanowiskowy korzystający z wiedzy projektowego → **wspólny workspace pamięci**
- Agent projektowy narzucający skille podwładnym → **dystrybucja profilu przez git**
- Przejęcie konta po odchodzącym → **eksport i import profilu** (a w wersji zalecanej
  nawet to nie jest potrzebne)

Jedna rzecz z opisu wymaga nazwania wprost: „każdy pracownik ma swoje stanowisko, swojego
agenta", a potem „inny pracownik przejmuje jego konto i całą wiedzę" znaczy, że
**profil jest stanowiskiem, a nie osobą.** To zdanie porządkuje całą resztę architektury.

---

## Architektura — dwa kanały między warstwami

Między agentem projektowym a stanowiskowymi biegną **dwie różne drogi** i mylenie ich to
najczęstszy sposób na zepsucie takiego układu:

- **Dystrybucja git (w dół, jednokierunkowo):** skille, SOUL, zadania cykliczne.
  Konfiguracja płynie z domeny do wszystkich jej stanowisk.
- **Workspace pamięci (wspólny, dwukierunkowo):** każdy w domenie czyta i dopisuje.
  Nikt spoza domeny nie sięga. To jest granica uprawnień, nie tylko rysunek.

Agent zarządzający wystawia i odbiera dostępy w obu warstwach.

## Warstwy

| Warstwa | Ilu | Kto ma dostęp | Model | Klucze do systemów |
|---|---|---|---|---|
| **Zarządzający** | 1 | Właściciel + administrator — dwa identyfikatory, nikt więcej | Mocny | Rejestr, Microsoft Graph, brama modeli. **Zero dostępu do danych operacyjnych** |
| **Projektowi** | 6 | Grupa działu w Entra ID | Dobrany do domeny — księgowość mocny, marketing tańszy | Systemy swojej domeny i tylko swojej |
| **Stanowiskowi** | 20 | Jedna osoba — ta, która aktualnie zajmuje stanowisko | Tani i szybki | **Żadnych własnych** — sięgają przez swoją domenę |

Ostatni wiersz to najważniejsza decyzja bezpieczeństwa w całym układzie.
**Agenci stanowiskowi nie trzymają żadnych kluczy.** Dwadzieścia profili z kluczami do
systemów firmowych to dwadzieścia miejsc, z których klucz może wyciec. Pytanie wymagające
danych wędruje do agenta domeny, który klucz ma — i który jest jeden, pilnowany i audytowany.

---

## Trzy mechanizmy, których nie musimy budować

### 1. Workspace pamięci — wiedza wspólna
Hermes ma osiem wtyczek zewnętrznej pamięci. Pamięć modelowana jest jako rozmówcy
wymieniający się wiadomościami we wspólnym obszarze: **użytkownik jest wspólny dla
wszystkich profili, a każdy agent ma w nim własną tożsamość.**

Handlowiec pyta o warunki umowy, dostaje wiedzę całej sprzedaży, a notatki o swoich
klientach trzyma u siebie.

### 2. Dystrybucja przez git — konfiguracja w dół
Profil można spakować jako repozytorium: osobowość, skille, zadania cykliczne, połączenia,
konfiguracja. Instalacja jedną komendą. **Klucze, pamięć i historia rozmów odbiorcy
zostają nietknięte.**

Kierownik sprzedaży poprawia skill ofertowania raz, wypycha wersję, dwudziestu handlowców
pobiera aktualizację i nikt nie traci własnej pamięci. To odpowiedź na obawę o fragmentację
wiedzy z aneksu do noty 02.

### 3. Eksport i import profilu — ciągłość
Jedna komenda pakuje profil do archiwum (skille, pamięć, osobowość, zadania, wtyczki,
ustawienia) **z usuniętymi kluczami**. Druga rozpakowuje pod dowolną nazwą.
Przy jednym serwerze nie będzie nawet potrzebne — wystarczy zmienić, kto ma dostęp.

---

## Co się dzieje w dniu odejścia pracownika

**Zmienia się identyfikator osoby, nie agent.** Ponieważ profil jest stanowiskiem, a nie
człowiekiem, przejęcie to jedna zmiana w rejestrze i ponowne wystawienie listy dostępu.
Następca pierwszego dnia wie o swoich klientach wszystko, co wiedział poprzednik.
Odebranie dostępu jest natychmiastowe i nie niszczy dorobku stanowiska.

### Warunek, żeby to nie było kłopotem prawnym

Skoro nowa osoba dziedziczy pamięć poprzedniej, trzeba przesądzić, **co do tej pamięci
w ogóle trafia.** Ustalenia z klientem — tak, to własność firmy. Prywatne notatki, oceny
współpracowników, sprawy kadrowe — nie, i agent stanowiskowy musi mieć to w instrukcji
jako zakaz. Inaczej przy trzecim przejęciu stanowiska ktoś odziedziczy rzeczy, których
nie powinien zobaczyć.

Zasada: **pamięć stanowiskowa dotyczy pracy, a nie ludzi.** Sprawy kadrowe idą do osobnego
profilu z dostępem wyłącznie dla zarządu — to ten wyjątek, w którym profil naprawdę jest
przypisany do osoby.

---

## Agent zarządzający — panel administracyjny w formie rozmowy

Nie budujemy panelu z ekranami i formularzami. **Budujemy narzędzia, a interfejsem jest czat.**

- „Dodaj Kowalskiego do agenta sprzedaży" → dopisanie do rejestru, generowanie konfiguracji,
  karta z podsumowaniem, czeka na kliknięcie
- „Pokaż, kto ma dostęp do agenta księgowości" → odczyt z rejestru
- „Nowak odchodzi, stanowisko przejmuje Wiśniewska" → jedna zmiana wpisu, dorobek zostaje
- „Ile kosztował nas w tym miesiącu agent marketingu" → odczyt z bramy modeli
- „Podnieś budżet księgowości o połowę na czas zamknięcia miesiąca" → z kartą zatwierdzenia

### Trzy zabezpieczenia, bez których tego nie uruchamiam

1. **Nic bez kliknięcia człowieka.** Każda operacja zmieniająca uprawnienia idzie przez
   kartę zatwierdzenia — Hermes ma to wbudowane, to konfiguracja, nie kod.
2. **Agent zarządzający nie widzi danych operacyjnych.** Brak kluczy do rozliczeń i CRM.
   Przejęcie go daje władzę nad konfiguracją, nie nad danymi — i widać to w audycie.
3. **Rejestr w repozytorium git.** Każda zmiana uprawnień to commit z autorem i datą.
   Ścieżka audytowa powstaje sama.

---

## Co trzeba zbudować — wycena dla 27 profili

| # | Element | Wycena |
|---|---|---|
| 1 | **Rejestr z hierarchią** — zarządzający, 6 domen, 20 stanowisk; powiązanie wiele-do-wielu (dopinanie ludzi do kolejnych domen) | 3 dni |
| 2 | **Generator profili** — instaluje dystrybucję domeny, ustawia model, podpina workspace, wpisuje listę dostępu. Dwudzieste stanowisko kosztuje tyle co pierwsze | 4 dni |
| 3 | **Synchronizacja z Entra ID i obsługa odejść** — grupy działów zasilają dostępy; wykrycie zniknięcia z katalogu, odebranie dostępu, zgłoszenie następcy | 3 dni |
| 4 | **Brama modeli z budżetem na profil** — przy 27 profilach limity to zabezpieczenie, nie wygoda | 2 dni |
| 5 | **Wspólna pamięć i sześć obszarów domenowych** — wdrożenie dostawcy, workspace na domenę, reguła co wolno zapisywać | 3 dni |
| 6 | **Narzędzia agenta zarządzającego** — opakowanie punktów 1–5 w polecenia + karty zatwierdzeń. Fasada, nie nowa logika | 3 dni |
| 7 | **Wspólny ślad audytowy** — telemetria z 27 profili, rozdzielona po agencie i po człowieku | 2 dni |
| | **Razem** | **20 dni** |

Około czterech tygodni pracy jednej osoby wspieranej przez Claude Code, plus czas na
wdrożenie i poprawki po pilocie.

---

## Dwa etapy

### Etap 1 — 2 tygodnie: agent zarządzający i rejestr
- Rejestr, generator, synchronizacja z Entra, brama modeli z budżetami
- Agent zarządzający dostępny wyłącznie dla właściciela i administratora
- Jeden agent projektowy jako dowód — **księgowość**, bo tam są gotowe skille i wynik
  sprawdzalny co do złotówki
- Dwa stanowiska pod nim, obsadzone realnymi ludźmi

**Kryterium wyjścia:** dodanie trzeciego stanowiska trwa minutę i odbywa się przez rozmowę
z agentem zarządzającym, a nie przez edycję plików.

### Etap 2 — 2–3 tygodnie: pozostałe domeny i stanowiska
- Pięć pozostałych agentów projektowych, każdy z dystrybucją i workspace'em
- Osiemnaście pozostałych stanowisk — generowanych, nie konfigurowanych
- Wspólny ślad audytowy i przegląd budżetów po pierwszym pełnym miesiącu
- Proces promocji wiedzy: co z pamięci stanowiskowej wędruje w górę do domeny

Dopiero po tym etapie ma sens rozmowa o aplikacji w wewnętrznym katalogu Teams.

---

## Trzy ryzyka

1. **Koszt.** Dwadzieścia stanowisk używanych codziennie to zupełnie inny rachunek niż
   trzech agentów. Największa zmiana względem poprzedniego planu. Dlatego brama modeli
   z limitem na profil wchodzi w etapie **pierwszym**, a warstwa stanowiskowa domyślnie
   chodzi na modelu tanim. Po pierwszym pełnym miesiącu będą prawdziwe liczby na profil.
2. **Dane osobowe — gdzie fizycznie mieszka wspólna pamięć.** Dostawcy pamięci to usługi
   zewnętrzne. Przy PPE i danych z umów trzeba przed pierwszym prawdziwym zapisem ustalić,
   czy hostujemy u siebie, czy podpisujemy umowę powierzenia. **Jedyny punkt planu
   niedomknięty bez sprawdzenia — do weryfikacji w pierwszym tygodniu.**
3. **Wiedza — dwadzieścia agentów uczących się osobno.** Dystrybucja git rozwiązuje ruch
   w dół, ale nie w górę. Jeśli handlowiec wypracuje lepszy sposób ofertowania, musi
   istnieć droga, żeby trafiło to do domeny i pozostałych dziewiętnastu. To nawyk, nie
   technika: comiesięczny przegląd, co zasługuje na awans do skilla domeny. Bez tego układ
   po pół roku się rozjedzie.

---

## Źródła

1. [Hermes — pamięć Honcho](https://hermes-agent.nousresearch.com/docs/user-guide/features/honcho)
   i [dostawcy pamięci](https://github.com/NousResearch/hermes-agent/blob/main/website/docs/user-guide/features/memory-providers.md)
2. [Hermes — dystrybucje profili](https://hermes-agent.nousresearch.com/docs/user-guide/profile-distributions)
3. [Hermes — komendy profili](https://hermes-agent.nousresearch.com/docs/reference/profile-commands)
4. [Hermes — wiele bramek naraz](https://hermes-agent.nousresearch.com/docs/user-guide/multi-profile-gateways)
5. [Hermes — Microsoft Teams](https://github.com/nousresearch/hermes-agent/blob/main/website/docs/user-guide/messaging/teams.md)
6. [LiteLLM — budżety i limity](https://docs.litellm.ai/docs/proxy/users)
