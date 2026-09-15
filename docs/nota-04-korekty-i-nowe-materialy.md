# Nota 04 — Korekty i nowe materiały

**NASTER · projekt nAgents · 22 sierpnia 2026**

Sprostowanie błędu z noty 02, odpowiedź na pytanie o Buzz, wnioski z transkrypcji
Rauch/Vercel oraz ocena Openbota.

---

## 1. Sprostowanie: Eve MA wbudowany kanał do Teams

W nocie 02 napisałem, że Eve ma tylko przykład dla Slacka i że adapter do Teams trzeba
dopisać samemu. **To był błąd rzeczowy** — oparłem się na opisie repozytorium zamiast na
dokumentacji kanałów, która wymienia **Teams wśród kanałów wbudowanych**, obok Slacka,
Discorda, Telegrama, Twilio, GitHuba i Lineara. Rauch potwierdza to w transkrypcji.

**Co to zmienia:** Eve traci największą wadę względem Hermesa. Przewaga Hermesa zawęża się
z dwóch punktów do jednego — udokumentowanej listy dostępu per pracownik
(`TEAMS_ALLOWED_USERS` z identyfikatorami Entra ID). U Eve odpowiednika nie znalazłem;
są tokeny per użytkownik przez Connect, ale nie katalog „kto widzi którego agenta".

**Co teraz przemawia za Eve:** evals w standardzie, trwałe wykonanie z checkpointami,
Vercel Connect (100+ systemów, subskrypcja zdarzeń — „every time Stripe has a failed
payment, let the agent know").

Nota 02 została poprawiona w trzech miejscach.

---

## 2. Buzz — odpowiedź na pytanie

**Buzz już jest open source** (Apache 2.0, samohostowalny, na protokole Nostr).
Nie trzeba szukać odpowiednika ani niczego budować.

**Nie budujmy własnego komunikatora.** To najdroższa i najmniej potrzebna rzecz w całym
projekcie — Teams istnieje, Buzz istnieje, a budowa komunikatora to projekt na kwartały.

### Trzy role, jakie taka warstwa mogłaby pełnić

| Rola | Czy to luka? |
|---|---|
| Kanał dla pracowników | **Nie** — Teams to pokrywa, oba silniki mają adapter |
| Konsola administracyjna dla właściciela i admina | **Tak** — i nota 03 wypełnia ją agentem zarządzającym w formie czatu |
| Warstwa zapasowa, gdyby Teams odpadł | Marginalna |

### Gotowe rozwiązania open source w roli konsoli

- **LibreChat** — najbliżej naszych potrzeb: RBAC, panel administracyjny, SSO przez
  OpenID Connect i SAML, **synchronizacja grup z Entra ID przez Microsoft Graph**
  (z zapisem Entra Object ID), przypisywanie różnych modeli różnym agentom.
  Ta synchronizacja pokrywa element 3 z naszej wyceny.
- **Open WebUI** — prostsze RBAC (admin/user), mocne w narzędziach i lokalnych modelach
- **Dify, AnythingLLM, LobeChat, OpenHands, Flowise** — pozostałe w kategorii

---

## 3. Transkrypcja Rauch/Vercel — cztery wnioski

### 3.1 Rauch podważa naszą topologię 27 agentów

Pytanie „god agent czy zespół agentów" pada wprost, w **dokładnie naszym kontekście**
(„the marketing team has access to these things and the finance team — I don't even want
the marketing team to know about certain finance documents"). Odpowiedź Raucha:
**„it's more on the god model"**. V jest routerem — pytanie o dokumentację idzie do jednej
zdolności, o wsparcie do agenta wsparcia z dostępem do zgłoszeń.

**To nie jest sprzeczne z naszymi 6 domenami** — jego sub-agenty to nasze agenty projektowe.
Spór dotyczy 20 agentów stanowiskowych i liczby punktów wejścia.

**Kluczowa obserwacja: wybór jest częściowo wymuszony przez silnik.**
Hermes ma otwarty błąd wspólnej pamięci w jednej instancji (#11430), więc trzeba rozdzielać
profile. Eve/V trzyma pamięć per użytkownik w jednym agencie, więc router działa.
Czyli topologia 27 profili to po części *obejście ograniczenia Hermesa*, a nie czysty wybór
architektoniczny.

**Wniosek:** router wraca do planu — ale jako **interfejs**, nie hydraulika. Rauch ma rację,
że nikt nie zapamięta 27 botów.

### 3.2 Rauch rozwiązuje nasze ryzyko nr 3 (wiedza w górę)

W nocie 03 zapisałem „comiesięczny przegląd, co zasługuje na awans do skilla domeny" —
czyli nawyk, którego nikt nie dotrzyma. Rauch ma mechanizm:

> kciuk w dół pod odpowiedzią → **nocne zadanie agreguje negatywny feedback** →
> agent sam proponuje poprawkę własnego skilla → człowiek zatwierdza

Plus evals jako testy jednostkowe agenta — także evals na osobowość („nasz agent był zbyt
rozwlekły"). **Adoptuję to zamiast przeglądu.**

### 3.3 Co potwierdza

- **Model szybki interaktywnie, dokładny asynchronicznie** — „if I'm talking to an agent
  interactively, I want fast. If the agent is doing an asynchronous job, I want accuracy."
  Dokładnie nasz podział warstw. Rauch dorzuca pomysł konsorcjum modeli do nocnej analizy.
- **Uzależnienie od dostawcy** — „so many people are resistant to [Claude Tag] because they
  don't want to get locked into only Claude's models". Twoje wymaganie potwierdzone rynkowo.
- **„Nowa praca IT"** — kontrola dostępu, narzędzia, guardraile, ścieżki audytowe. To jest
  nasza warstwa 4, nazwana jego słowami.
- **Zacząć od jednej nudnej czynności, która ma system** — nasza księgowość.
- **soul.md / instructions.md** — dusza agenta. Mamy w planie.

### 3.4 Model zdarzeniowy

Vercel Connect: 100+ systemów, subskrypcja zdarzeń. „Everything is an event" — wiadomość
w czacie to też zdarzenie. Warto to przenieść do naszego planu proaktywności: webhook
z systemu rozliczeniowego jako wyzwalacz agenta, nie tylko harmonogram nocny.

---

## 4. Openbot — sprawdzony, ciekawy, za wcześnie

**Czym jest:** projekt CopilotKit (twórcy protokołu AG-UI), licencja MIT, ogłoszony
**19 sierpnia 2026** — trzy dni temu. Samohostowalne „AI coworkers", każdy z własnym
komputerem: przeglądarka, pliki, powłoka, narzędzia MCP.

**Co robi dobrze i co warto skopiować niezależnie od decyzji o narzędziu:**

- **Bramka akcji z domyślną odmową.** Każda operacja przeglądarki, pliku, powłoki i MCP
  przechodzi przez sprawdzenie polityki *przed* wykonaniem. Brak reguły zezwalającej =
  akcja zablokowana. To jest odwrotność „damy agentowi dostęp i mamy nadzieję".
- **Ścieżka audytowa rejestrująca akcje dozwolone, odrzucone i nieudane** wraz z wynikiem
  polityki. Nasz element 7, ale z zapisem także tego, czego agent *próbował*.
- **Przejęcie sterów przez człowieka** — agent utyka na kodzie dwuskładnikowym, prosi
  o pomoc, człowiek klika, agent kontynuuje od tego samego miejsca.
- **Harness-agnostyczny** — dowolny agent zgodny z AG-UI (protokół przyjęty m.in. przez
  LangChain, Mastrę, Pydantic AI, Google, Microsoft i AWS).

**Dlaczego nie teraz:**

- **Wersja 0.0.1, oznaczona jako alfa, trzy dni na rynku.** Autorzy sami odradzają wpinanie
  do wrażliwych systemów. Przy danych PPE i rozliczeniach to dyskwalifikuje.
- **Własny kanał rozmów, nie Teams** — ta sama kategoria problemu co Buzz.
- Materiał, z którego pochodzi, to film promocyjny prowadzący do płatnego szkolenia —
  fakty się potwierdziły, ale ton nie jest bezstronny.

**Werdykt:** obserwować. Wzorzec „bramka z domyślną odmową + audyt prób" wchodzi do naszego
projektu od razu, bo jest wart skopiowania niezależnie od narzędzia.

---

## 5. Decyzja do podjęcia

Po korekcie o Teams różnica między Eve a Hermesem zawęziła się do jednego punktu.
**Czy otwieramy ponownie wybór silnika?**

| | Hermes | Eve |
|---|---|---|
| Dostępność per pracownik | **Udokumentowana** | Brak katalogu — do zbudowania |
| Teams | Tak | Tak *(korekta)* |
| Wybór modelu | 200+ backendów | Bramka modeli, każdy dostawca |
| Konfiguracja czy kod | YAML | TypeScript |
| Evals | Brak w standardzie | **W standardzie** |
| Trwałe wykonanie | Brak | **Checkpointy** |
| Pamięć wielu użytkowników | **Otwarty błąd #11430** | Brak przeszkody |
| Zdarzenia z systemów | Do zbudowania | **Connect, 100+ systemów** |

Moja rekomendacja pozostaje przy Hermesie na etap pierwszy — bo kontrola dostępu jest
wymaganiem numer jeden, a konfiguracja zamiast kodu przy jednej osobie technicznej ma
znaczenie. Ale **przewaga jest teraz cienka** i uczciwie: gdyby doszedł deweloper
TypeScriptu, wybrałbym Eve.

---

## 6. Aneks — praktyki z transkrypcji o oprogramowaniu osobistym

Materiał innego gatunku (budowanie osobistego oprogramowania przez nietechnicznych),
więc architektury nie zmienia. Cztery rzeczy przenoszą się wprost.

### 6.1 Dyscyplina czterech plików — adoptuję do repozytorium

| Plik | Zawartość | Po co u nas |
|---|---|---|
| `project.md` | dla kogo, co się dzieje dziś, co ma się dziać zamiast, gdzie ma działać, co ma zostać prywatne | zastępuje rozproszone ustalenia z rozmów |
| `decisions.md` | **każdy wybór dotyczący kosztu, danych, dostępu, wdrożenia lub odwracalności: opcje, rekomendacja, uzasadnienie** | przy jednej osobie technicznej to jedyna obrona przed bus factor = 1 |
| `scenarios.md` | realne sytuacje, które system ma obsłużyć | to są nasze evals, zapisane zanim powstanie kod |
| `CLAUDE.md` | jak ma się zachowywać narzędzie budujące | krótkie, zmienne — trwała prawda siedzi w trzech powyższych |

Zasada towarzysząca: **model ma ujawniać wybory zwykłym językiem, zanim je podejmie** —
dwie lub trzy realne opcje, rekomendacja, co staje się łatwiejsze, a co trudniejsze
do zmiany później.

### 6.2 „Ukrycie przycisku to nie jest kontrola dostępu"

Autoryzacja musi być egzekwowana na poziomie danych, nie interfejsu.
**To potwierdza decyzję z noty 03**, żeby agenci stanowiskowi nie mieli własnych kluczy:
`TEAMS_ALLOWED_USERS` jest listą na poziomie kanału, a nie na poziomie danych.
Prawdziwą granicą jest to, jakie klucze profil w ogóle posiada.

### 6.3 Luka, której nie pokryliśmy: kopie zapasowe

W żadnej nocie nie ma słowa o backupie, a mamy zaplanowane 27 profili z narastającą
pamięcią. **Do planu wchodzi:** cykliczny eksport profili (`hermes profile export` daje
archiwum bez kluczy) plus **udowodnione odtworzenie** — nie „zrobiliśmy kopię", tylko
„odtworzyliśmy ją i sprawdziliśmy, że działa". Element do etapu pierwszego, ok. 1 dzień.

### 6.4 Rewizja: pilot nie powinien startować na danych osobowych

Rada z materiału: nie zaczynaj od wrażliwych danych, nie wchodź od razu na trudny poziom.
Zestawione z naszym **nierozwiązanym ryzykiem nr 2** (gdzie fizycznie mieszka wspólna
pamięć) daje konkretny wniosek.

Nota 03 wskazywała księgowość jako pilota — bo tam są gotowe skille i wynik sprawdzalny
co do złotówki. To nadal dobry wybór **procesu**, ale niesie numery PPE i dane z umów.

**Rozwiązanie:** rozdzielić dwie rzeczy, które niepotrzebnie związaliśmy.
Pilot rusza na procesie księgowym, ale **ze wspólną pamięcią wyłączoną albo trzymaną
wyłącznie lokalnie**, dopóki kwestia rezydencji danych nie zostanie rozstrzygnięta.
Dowodzimy, że proces działa; włączenie pamięci domenowej to osobna decyzja, po sprawdzeniu
dostawcy i podpisaniu umowy powierzenia.

### 6.5 Testowanie

„Nie można poprosić agenta kodującego, żeby przetestował wszystko za ciebie, i przyjąć,
że skoro mówi »gotowe«, to jest gotowe." Dotyczy to także mnie przy budowie uprzęży —
kryterium odbioru etapu pierwszego zostaje takie, jak w nocie 03: trzy zamknięte miesiące
przeliczone przez agenta zgadzają się co do złotówki z liczeniem ręcznym.

*Uwaga o źródle: materiał pochodzi od twórcy promującego własne produkty (OpenBrain,
OpenSkills, OpenEngine, Ringer). Praktyki powyżej są niezależne od tych narzędzi;
samych narzędzi nie oceniam, bo nie są nam potrzebne.*
