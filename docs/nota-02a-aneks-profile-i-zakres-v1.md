# Aneks do noty 02 — Profile jako konta, zakres wersji pierwszej

**NASTER · projekt nAgents · 22 sierpnia 2026**

Aneks do [noty 02](nota-02-uprzaz-dla-agentow.md). Weryfikacja propozycji „wiele instancji
Hermesa, po jednej na pracownika" oraz doprecyzowanie zakresu wersji pierwszej.

---

## Propozycja: konto Hermesa na pracownika

### Co się potwierdziło (3 z 4 punktów na korzyść propozycji)

1. **Instancje są darmowe i naprawdę odizolowane.** Profil to osobny katalog domowy:
   własna konfiguracja, klucze, `SOUL.md`, pamięć, historia rozmów, skille i zadania
   cykliczne. Zero opłat licencyjnych za kolejny profil.

2. **Jedna bramka obsłuży wszystkie profile.** Nie trzeba procesu i portu na agenta —
   Hermes ma tryb zwielokrotniania, w którym wspólny nasłuch kieruje ruch po prefiksie
   adresu (`/p/<profil>/webhooks/`). Dokumentacja sama zaleca go przy wdrożeniu
   kontenerowym. **To obala moje wcześniejsze zastrzeżenie**, że wielu agentów wymaga
   napisania routera.

3. **Uprząż faktycznie się kurczy.** Hermes ma własne `hermes profile create`, eksport
   i import profilu jako paczki `.tar.gz` (z usuniętymi kluczami), a nawet publikowanie
   profilu jako repozytorium git. Generator jest cieńszy, niż zakładałem:
   **z dwóch tygodni robi się 6–8 dni roboczych.**

4. **Osobne instancje omijają nierozwiązany błąd.** W jednej instancji pamięć trwała jest
   *wspólna dla wszystkich rozmówców* — [zgłoszenie #11430](https://github.com/NousResearch/hermes-agent/issues/11430)
   o mieszaniu tożsamości użytkowników jest wciąż otwarte. Rozdzielenie na profile to dziś
   jedyne obejście.

### Co wymaga poprawki

1. **Profil nie jest piaskownicą.** Dokumentacja mówi wprost: profile kontrolują stan
   Hermesa, *nie dostęp do systemu plików*. Jeśli uznamy „profil = konto = izolacja",
   zbudujemy ochronę, której nie ma. Prawdziwe odgrodzenie daje osobne zaplecze
   uruchomieniowe na profil (Docker/SSH) oraz to, jakie klucze profil w ogóle posiada.

2. **Konto na pracownika dzieli po złej osi.** Wiedza fragmentuje się na tyle kopii, ilu
   jest ludzi: skill dopracowany przez księgową nie poprawia agenta jej kolegi. Poprawka
   do procesu ląduje w trzydziestu miejscach zamiast w trzech.

3. **Rozmowy i tak są już rozdzielone.** Sesje są kluczowane po użytkowniku
   (`group_sessions_per_user: true`, domyślnie włączone) — każdy ma własny wątek w obrębie
   jednej instancji. Osobne konto nic tu nie dokłada.

4. **Teams liczy boty, nie bramki.** Wspólny nasłuch nie zwalnia z rejestracji aplikacji
   dla każdego bota. Trzy domeny = trzy aplikacje do zatwierdzenia przez administratora;
   trzydzieści osób = trzydzieści. *(Odczyt z dokumentacji, do potwierdzenia w pierwszym tygodniu.)*

### Zasada porządkująca

> **Granica profilu ma pokrywać się z granicą uprawnień.**

Nie po pracowniku i nie po jednym agencie na firmę, tylko **po domenie** — marketing,
sprzedaż, księgowość. Wtedy wspólna pamięć profilu zawiera dokładnie tę wiedzę, do której
wszyscy jego użytkownicy i tak mają prawo, więc otwarte zgłoszenie #11430 przestaje być
zagrożeniem. Wiedza kumuluje się tam, gdzie jest używana, a poprawka trafia w jedno miejsce.

**Wyjątek, w którym konto na pracownika jest jedynym słusznym rozwiązaniem:** pracownik
pracujący na danych, których nikt inny widzieć nie może — zarząd, kadry, sprawy pracownicze.
Tam osobny profil to nie nadmiar, tylko wymóg.

Czyli: **domena domyślnie, osobny profil tam, gdzie dane tego wymagają.**
Trzy do sześciu profili, jedna wystawiona bramka, sesje rozdzielone po użytkowniku,
izolacja z zaplecza uruchomieniowego, a nie z samego profilu.

---

## Zakres wersji pierwszej

Założenie o Microsofcie potwierdzone: **odpada Copilot jako platforma agentowa,
Azure jako miejsce hostingu zostaje.**

| Priorytet | Co | Czym pokryte |
|---|---|---|
| **1** | Wersja chmurowa — działa na serwerze, nie na laptopie | Kontenery + jedna bramka; Azure naturalny, ale plan nieprzywiązany |
| **2** | Zarządzanie agentami | Pięć elementów uprzęży, 6–8 dni |
| **3** | Pozostałe wymagania — wybór modelu, dostępność per pracownik, gotowe narzędzia | Hermes + LiteLLM |
| *później* | Teams jako aplikacja z listy | Element dodatkowy |

### Sprostowanie: weryfikacja Microsoftu nie jest potrzebna

To dwie różne rzeczy i warto ich nie mylić:

- **Aplikacja wewnętrzna w waszej dzierżawie** — wgrywana bezpośrednio, zatwierdzana przez
  waszego administratora Teams. *Żadnej weryfikacji Microsoftu.* To dokładnie ta ścieżka,
  którą opisuje dokumentacja Hermesa: rejestracja bota z linii poleceń, bez portalu Azure.
  Dostępna od ręki.
- **Publikacja w publicznym sklepie Teams** — dopiero to wymaga weryfikacji Microsoftu.
  Ale nie jest wam potrzebna: sklep służy temu, żeby aplikację znaleźli ludzie z innych firm.
  Wasi pracownicy dostaną ją z wewnętrznego katalogu organizacji.

**Wniosek:** „pracownik znajduje na liście, instaluje i ma wszystko" jest osiągalne *bez*
weryfikacji — to wewnętrzny katalog aplikacji waszej dzierżawy. Zgodnie z decyzją zostaje
to etapem drugim, ale gdy przyjdzie moment, będzie to dzień pracy, a nie certyfikacja.

**Czy da się obejść przez API albo MCP?** Do czytania tak, do pisania nie. Konektor
Microsoft 365 pozwala przeszukiwać rozmowy w Teams, ale nie pozwala niczego wysłać.
Żeby agent odpowiadał w Teams, musi być zarejestrowany jako bot — innej drogi nie ma.

---

## Zaktualizowana wycena

| Element | Wycena |
|---|---|
| 1. Rejestr agentów (`agents.yaml`) | 2 dni |
| 2. Generator konfiguracji (cieńszy — opiera się na `hermes profile create` i eksporcie profilu) | 2 dni |
| 3. Synchronizacja z Entra ID (Microsoft Graph) | 2 dni |
| 4. Brama modeli — wdrożenie LiteLLM | 1–2 dni |
| 5. Wspólny ślad audytowy | 1 dzień |
| ~~6. Router w Teams~~ | **skreślony** — zwielokrotniona bramka Hermesa załatwia to natywnie |
| **Razem** | **6–8 dni roboczych** |

Ścieżka mieszana: **2–3 tygodnie** do działającego, zarządzalnego agenta w chmurze
(poprzednio szacowane na 3–4 tygodnie).

---

## Pytanie otwarte

**Ilu agentów na start?** Przy trzech (marketing, sprzedaż, księgowość) rejestr i generator
zwracają się od pierwszego dnia. Przy jednym — lepiej postawić najpierw sam profil
rozliczeniowy w chmurze, sprawdzić na żywym procesie, a uprząż dopisać, gdy będzie wiadomo,
co naprawdę ma konfigurować.

---

## Nowe źródła

1. [Hermes — profile](https://hermes-agent.nousresearch.com/docs/user-guide/profiles) —
   co profil izoluje, ostrzeżenie że profil nie jest piaskownicą, eksport i import.
2. [Hermes — wiele bramek naraz](https://hermes-agent.nousresearch.com/docs/user-guide/multi-profile-gateways) —
   tryb zwielokrotniania jednej bramki dla wszystkich profili.
3. [Zgłoszenie #11430](https://github.com/NousResearch/hermes-agent/issues/11430) —
   pamięć trwała wspólna dla wszystkich rozmówców, mieszanie tożsamości. Otwarte.
