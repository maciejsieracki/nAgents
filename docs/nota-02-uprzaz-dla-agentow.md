# Nota decyzyjna 02 — Uprząż dla agentów

**NASTER · projekt nAgents · 22 sierpnia 2026**

Eve, Hermes, Buzz, Grok Bot i Azure Foundry zmierzone pięcioma wymaganiami właściciela.
Co każde daje z pudełka, czego żadne nie daje — i ile kodu trzeba napisać, żeby to spiąć.

Wersja do czytania: https://claude.ai/code/artifact/3cc24314-36ae-49bc-b29b-dc6bedd463c9

---

## Rekomendacja

**Hermes jako silnik, LiteLLM jako brama modeli, cienka własna uprząż na wierzchu.**
Wszystko na Azure, uprawnienia z Entra ID, kanał Teams natywnie.

Hermes wygrywa nie dlatego, że jest lepiej zbudowany od Eve — inżyniersko Eve jest solidniejsza.
Wygrywa, bo ma dwie rzeczy będące wprost wymaganiami, a których Eve nie ma:
**natywny kanał do Microsoft Teams z listą dostępu po identyfikatorach Entra ID** oraz
**ponad 200 backendów modeli przełączanych w konfiguracji**.

Żadne z narzędzi nie daje uprzęży w komplecie. Różnica polega na tym, ile trzeba dopisać —
przy Hermesie to **około dwóch tygodni**, nie kwartał.

> **Kiedy zamiast tego wybrać Azure Foundry:** jeśli w ciągu roku dojdzie audyt zewnętrzny
> albo compliance wymagające pełnej ścieżki tożsamości „ten pracownik, przez tego agenta,
> sięgnął po ten zasób" — kup gotową uprząż od Microsoftu zamiast pisać własną.

---

## Pięć wymagań to cztery warstwy — jedna jest pusta

| Warstwa | Co obejmuje | Czym wypełniamy |
|---|---|---|
| **1. Modele** | Wybór modelu na agenta, budżety, limity, jeden punkt kosztowy | Hermes (200+ backendów) + LiteLLM (budżety, klucze) |
| **2. Wykonanie** | Skille, narzędzia, piaskownica, zatwierdzenia, pamięć | Hermes lub Eve — obie to mają |
| **3. Kanał** | Jak pracownik rozmawia z agentem | Hermes: natywnie. Eve: dopisać |
| **4. Zarządzanie** ⚠ | Katalog agentów, kto do którego ma dostęp, audyt, budżet na agenta | **Nikt nie daje — trzeba napisać** |

Wniosek: nie szukamy narzędzia, które ma wszystko, bo takiego nie ma. Szukamy takiego,
które wypełnia warstwy 1–3 najlepiej, żeby warstwa 4 była do dopisania w dwa tygodnie.

---

## Odpadają od razu

**Buzz (Block / Jack Dorsey)** — otwartoźródłowy workspace wydany 21.07.2026 (Apache 2.0).
Agenci są pełnoprawnymi członkami kanałów, z własnymi kluczami i uprawnieniami na protokole Nostr.
**Odpada, bo Buzz nie jest platformą agentową — Buzz *jest komunikatorem*,** konkurentem Slacka
i Teams. Wdrożenie = zmiana komunikatora firmowego. Autorzy sami piszą: „wczesny etap".

**Grok Bot (xAI)** — start 11.08.2026, zespół zawsze aktywnych agentów z własnym komputerem
w chmurze, logujących się do narzędzi firmy. **Odpada, bo działa wyłącznie na modelach Grok** —
łamie wymaganie o niezależności od jednego modelu w najostrzejszy sposób. Zamknięty,
rozliczany subskrypcjami SuperGrok/Cursor, klienci firmowi na liście oczekujących.

---

## Finaliści na liście wymagań

| Wymaganie | Eve (Vercel) | Hermes (Nous Research) | Azure Foundry |
|---|---|---|---|
| **Uprząż nad wieloma agentami** | Częściowo — subagenci, brak wspólnego katalogu | Częściowo — orkiestrator-pracownik, ale **1 agent na profil** | **Tak** — Agent Framework, agenci połączeni |
| **Dostępność per pracownik** | Pośrednio — tokeny per użytkownik przez Connect | **Tak** — `TEAMS_ALLOWED_USERS` (ID z Entra), `managed_scope` | **Tak, wzorcowo** — tożsamość agenta w Entra, role, delegacja OBO |
| **Wybór modelu** | Tak — bramka modeli | **Tak, najszerzej** — 200+ backendów, model na 4 poziomach | Tak — katalog 10 000+, Claude GA od lipca 2026 |
| **Teams** | **Nie** — trzeba napisać adapter | **Natywnie** — rejestracja przez CLI, karty zatwierdzeń | **Jednym kliknięciem** |
| **Gotowe, nie pisane** | Wymaga TypeScriptu, beta | **Konfiguracja YAML**, skille w markdownie | Gotowe, ale ciężkie |
| **Koszt i niezależność** | Darmowy, ciągnie ku Vercelowi | **Otwarty, rdzeń darmowy**, Docker gdziekolwiek | Uzależnienie od Azure |

---

## Korekta wcześniejszego ustalenia

Pierwsze źródła twierdziły, że Hermes ma panel administracyjny do kluczy, modeli i kanałów.
**Oficjalna dokumentacja tego nie potwierdza** — mówi wprost, że wbudowanego panelu nie ma,
a jedynym mechanizmem firmowym jest `managed_scope` (przypinanie wartości konfiguracji
i sekretów, których zwykły użytkownik nie nadpisze).

Osobno istnieje **Nous Portal** — usługa chmurowa producenta obiecująca agentów dla całej
organizacji z granularną kontrolą dostępu i wspólnym rozliczeniem. Brzmi jak nasza uprząż,
ale to hosting u dostawcy — dane wychodzą z firmy. Przy PPE i umowach wymaga najpierw umowy
powierzenia. **Planujemy tak, jakby panelu nie było.**

---

## Hermes — bilans

**Z pudełka**
- Kanał Teams z kontrolą dostępu (lista ID z Entra na agenta)
- Zatwierdzenia w czacie: pozwól raz / w sesji / zawsze / odmów
- Wybór modelu na 4 poziomach: agent, podagent, zadania poboczne, wywołanie
- Izolacja: lokalnie, Docker, SSH, piaskownice chmurowe
- Pamięć trwała (3 poziomy) i system skilli w markdownie
- 20+ kanałów; jeden agent może odpowiadać w kilku naraz

**Czego nie ma**
- **Jeden agent na profil** — marketing/sprzedaż/księgowość to trzy osobne profile
- Brak katalogu agentów i ekranu „kto ma do czego dostęp"
- Listy dostępu wpisywane ręcznie — rozjadą się przy rotacji pracowników
- Teams wymaga publicznego adresu z prawdziwym certyfikatem
- Audyt rozproszony po profilach

---

## Co trzeba napisać — sześć elementów uprzęży

Wycena w tygodniach pracy jednej osoby wspieranej przez Claude Code.

| # | Element | Po co | Wycena |
|---|---|---|---|
| 1 | **Rejestr agentów** (`agents.yaml`) | Serce uprzęży: nazwa, profil, model, skille, narzędzia, grupa Entra, budżet. Jedyne miejsce, w którym cokolwiek się zmienia | 3–4 dni |
| 2 | **Generator konfiguracji** | Z rejestru robi config każdego profilu + listę dozwolonych użytkowników + opis wdrożenia na Azure. Nowy agent = 5 linijek | 3–4 dni |
| 3 | **Synchronizacja z Entra ID** | Członkowie grup przez Microsoft Graph → identyfikatory. Dodajesz człowieka do grupy „Księgowość" — dostaje agenta. Odejście odbiera dostęp automatycznie | 2–3 dni |
| 4 | **Brama modeli — LiteLLM** | Nie piszemy, stawiamy gotowe. Klucze wirtualne per agent, budżety egzekwowane na 4 poziomach, ograniczenie listy modeli | 2–3 dni |
| 5 | **Wspólny ślad audytowy** | Telemetria z wszystkich profili w jedno miejsce. Odpowiedź na pytanie audytora: kto, kiedy, przez którego agenta, po co | 2 dni |
| 6 | **Router w Teams** *(etap 2)* | Jeden bot firmowy rozpoznaje intencję i przekazuje do właściwego agenta, sprawdzając uprawnienia. Sens dopiero przy >3 agentach | 1–2 tyg. |

**Razem etap pierwszy: ok. dwóch tygodni.**

---

## Cztery ścieżki — ten sam miernik

| Ścieżka | Czas do pierwszego agenta w Teams | Robota własna | Główne ryzyko |
|---|---|---|---|
| Czysta Eve | 6–10 tyg. | Adapter Teams, uprawnienia, katalog | Piszemy kanał, który Hermes ma gotowy; beta |
| Piszemy sami | 3–6 mies. | Wszystko | Odtwarzamy za darmo dostępne piaskownice i pamięć; bus factor = 1 |
| Czysty Hermes | 1–2 tyg. | Brak | Działa, ale bez uprzęży — ręczna konfiguracja, budżetu nie widać |
| **Mieszana (rekomendacja)** | **3–4 tyg.** | Sześć elementów, ok. 2 tyg. | Utrzymujemy własny klej — mały, ale nasz |

Różnica między czystym Hermesem a wariantem mieszanym to dwa tygodnie — i dokładnie te dwa
tygodnie zamieniają „mamy agentów" w „zarządzamy agentami".

---

## Koszt: Claude Code jako narzędzie, nie jako silnik

- **Claude Code to narzędzie, którym piszemy uprząż** — koszt projektu (dwa tygodnie), nie eksploatacji.
- **Agenci produkcyjni chodzą na modelach wybieranych w bramie.** Czat — model tani i szybki.
  Nocna analiza rozliczeń — mocny. Decyzja w jednym pliku, zmienialna w każdej chwili.
- **Budżety są egzekwowane, nie obserwowane** — brama blokuje żądanie po przekroczeniu limitu
  na poziomie klucza, użytkownika, zespołu lub organizacji.
- **Infrastruktura to tania pozycja** — rachunek za modele przewyższa ją o rząd wielkości.

---

## Dwie rzeczy do potwierdzenia

1. **Czy Microsoft wrócił do gry?** W nocie 01 zapisałem „Microsoft odpada", teraz jest mowa
   o instalacji na Azure. Czytam to tak: **odpadał Copilot Studio jako platforma agentowa,
   a nie Azure jako miejsce hostingu.** Cały plan opiera się na tym założeniu.
2. **Ilu agentów naprawdę na start?** Przy trzech (marketing, sprzedaż, księgowość) rejestr
   i generator zwracają się od pierwszego dnia. Przy jednym — lepiej postawić sam Hermes
   z Teams w tydzień, sprawdzić na procesie rozliczeniowym, a uprząż dopisać, gdy będzie
   wiadomo, co naprawdę ma konfigurować.

---

## Źródła

1. [Hermes — konfiguracja](https://github.com/NousResearch/hermes-agent/blob/main/website/docs/user-guide/configuration.md)
   i [Hermes — Microsoft Teams](https://github.com/nousresearch/hermes-agent/blob/main/website/docs/user-guide/messaging/teams.md)
2. [Agent identity w Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/agents/concepts/agent-identity)
   i [Foundry Agent Service, Build 2026](https://devblogs.microsoft.com/foundry/agent-service-build2026/)
3. [Claude w Microsoft Foundry](https://learn.microsoft.com/en-us/azure/foundry/foundry-models/concepts/claude-models)
4. [Dokumentacja Eve](https://vercel.com/docs/eve) i [github.com/vercel/eve](https://github.com/vercel/eve)
5. [LiteLLM — kontrola dostępu](https://docs.litellm.ai/docs/proxy/access_control)
   i [budżety oraz limity](https://docs.litellm.ai/docs/proxy/users)
6. [Buzz od Block](https://techcrunch.com/2026/07/21/jack-dorsey-is-taking-on-slack-with-buzz-a-group-chat-platform-for-teams-and-their-ai-agents/)
7. [Grok Bot od xAI](https://www.unite.ai/xai-launches-grok-bot-always-on-ai-teammates-with-their-own-cloud-computers/)
