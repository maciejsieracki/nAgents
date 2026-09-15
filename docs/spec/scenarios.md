# Scenariusze

Realne sytuacje, które system ma obsłużyć. **Powstają przed kodem** i są źródłem
przypadków testowych. Każdy scenariusz ma etap, w którym musi zacząć działać.

> **D-014 / platform override:** bieżący runtime scenariuszy to OpenClaw Gateway,
> agenci, sesje, kanały, automatyzacje, tasks i Task Flow. Dawne odniesienia do
> Hermesa są historycznym mapowaniem; aktualny zakres i luki są w
> `docs/OPENCLAW-STRATEGY.md`.

---

## Dostęp i tożsamość

| # | Scenariusz | Etap |
|---|---|---|
| A1 | Pracownik księgowości loguje się kontem firmowym i rozmawia ze swoim agentem | MVP1 |
| A2 | Pracownik marketingu nie widzi agenta księgowości ani nie dobija się do niego z pominięciem interfejsu | MVP1 |
| A3 | Konto wyłączone w katalogu — logowanie nieudane, trwające sesje unieważnione | MVP1 |
| A4 | Dodanie osoby do grupy w katalogu daje jej dostęp w ciągu minuty | MVP2 |
| A5 | Usunięcie z grupy odbiera dostęp do agentów tej grupy, pozostałe bez zmian | MVP2 |
| A6 | Zwolnienie dyscyplinarne — natychmiastowe odcięcie bez czekania na cykl synchronizacji | MVP2 |
| A7 | Przejęcie stanowiska: następca widzi dorobek, poprzednik traci dostęp | MVP2 |

## Rozliczenia — proces pilotażowy

| # | Scenariusz | Etap |
|---|---|---|
| R1 | Podkładka pod fakturę: suma prowizji zgadza się co do grosza z kwotą docelową | MVP1 |
| R2 | Usunięte pozycje trafiają do osobnego arkusza z sumami kontrolnymi | MVP1 |
| R3 | Trzy zamknięte miesiące przeliczone przez agenta zgadzają się z liczeniem ręcznym | MVP1 |
| R4 | Pytanie o umowę spoza pliku — agent przyznaje się do braku danych, nie podaje liczby | MVP1 |
| R5 | Weryfikacja korekt: dopasowanie po numerze umowy, PPE i dacie wejścia w życie | MVP2 |
| R6 | Zapis wyniku do systemu wstrzymany do zatwierdzenia przez człowieka | MVP2 |
| R7 | Nowa korekta w systemie wyzwala weryfikację automatycznie | MVP3 |

## Wiedza

| # | Scenariusz | Etap |
|---|---|---|
| W1 | Dodanie reguły w wiedzy zespołu zmienia zachowanie wszystkich jego agentów | MVP3 |
| W2 | Zmiana psująca zachowanie nie wchodzi — testy zatrzymują ją przed wdrożeniem | MVP3 |
| W3 | Cofnięcie złej zmiany przywraca poprzedni stan | MVP3 |
| W4 | To samo pytanie od dwóch osób do agenta procesowego daje identyczną odpowiedź | MVP3 |
| W5 | W audycie widać, po które dokumenty agent sięgnął przy danej odpowiedzi | MVP3 |
| W6 | Trzy oceny negatywne na ten sam temat rodzą rano propozycję poprawki | MVP3 |

## Koszty

| # | Scenariusz | Etap |
|---|---|---|
| K1 | Każda rozmowa ma przypisany koszt do agenta i do człowieka | MVP1 |
| K2 | Wyczerpanie budżetu agenta zatrzymuje go z czytelnym komunikatem | MVP1 |
| K3 | Podniesienie limitu wymaga roli i zapisuje się w audycie | MVP2 |
| K4 | Podgląd zużycia per agent, człowiek i model, dzień i miesiąc | MVP2 |
| K5 | Zmiana modelu na tańszy nie wymaga zmian w agentach | MVP1 |

## Proaktywność

| # | Scenariusz | Etap |
|---|---|---|
| P1 | Poniedziałek rano — raport metryk bez udziału człowieka | MVP3 |
| P2 | Webhook z systemu firmowego uruchamia rutynę | MVP3 |
| P3 | Webhook z błędnym podpisem odrzucony i zapisany w audycie | MVP3 |
| P4 | Nocna analiza na modelu mocnym, rozmowa w ciągu dnia na tanim | MVP3 |

## Ciągłość i zgodność

| # | Scenariusz | Etap |
|---|---|---|
| C1 | Odtworzenie bazy z kopii — przećwiczone, nie zadeklarowane | MVP1 |
| C2 | Awaria OpenClaw Gatewaya — rozmowy przenoszone, sesje odtwarzane zgodnie z potwierdzonym modelem runtime | MVP4 |
| C3 | Żądanie usunięcia danych osoby zrealizowane, audyt zanonimizowany | MVP4 |
| C4 | Eksport danych osoby w formacie tekstowym | MVP4 |
| C5 | Instalacja dla drugiego klienta w jeden dzień, wyłącznie konfiguracja | MVP4 |
| C6 | W ruchu do dostawcy modelu wyłącznie tokeny zamiast numerów PPE | MVP4 |

## Interfejs pracownika i administracja

| # | Scenariusz | Etap |
|---|---|---|
| U1 | Pracownik loguje się do OpenClaw Control UI, kanału albo webowej warstwy 8gent i widzi gotowego agenta oraz wyłącznie przydzielone sesje | MVP1 |
| U2 | Pracownik otwiera czat bez znajomości gatewaya, adresu serwera, modelu, poświadczeń i routingu | MVP1 |
| U3 | Zamknięcie przeglądarki odłącza widok, ale serwerowy backend i rozpoczęta zdrowa praca pozostają aktywne; ponowne wejście pozwala odczytać ten sam stan | MVP1 |
| U4 | Pracownik nie ma dostępu do profili technicznych, budżetów, poświadczeń, routingu ani zaawansowanych ustawień | MVP1 |
| U5 | Administrator zarządza profilami, agentami, uprawnieniami i ustawieniami przez web administracyjny albo terminal; Desktop nie jest wymagany | MVP2 |
| U6 | Nakładka Desktopu korzysta z działającego obiegu webowo-serwerowego i nie zatrzymuje pracy po zamknięciu klienta | MVP2 |

## Serwerowy pomocnik procesu

| # | Scenariusz | Etap |
|---|---|---|
| U7 | OpenClaw automation uruchomiona na serwerze dostarcza pełną dyspozycję do Task Flow bez otwartego klienta | MVP1 |
| U8 | Task Flow po terminalnym stanie uruchamia dokładnie następny zatwierdzony etap grafu Operator → Evaluator → Obrona warunkowo → Final Control | MVP1 |
| U9 | Plugin/proces nie tworzy nowego zakresu ani pustej Obrony; używa wyłącznie istniejącej lub ściśle przewidzianej kontynuacji | MVP1 |
| U10 | Niejasny wynik, brak dowodu, obcy agent/projekt albo konflikt dostarczenia zatrzymuje strumień i tworzy eskalację bez niejawnej mutacji | MVP1 |
| U11 | Restart Gatewaya, pluginu albo replay tej samej automatyzacji nie tworzy drugiego tasku, flow ani dostarczenia | MVP1 |
