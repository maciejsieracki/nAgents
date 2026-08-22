# Scenariusze

Realne sytuacje, które system ma obsłużyć. **Powstają przed kodem** i są źródłem
przypadków testowych. Każdy scenariusz ma etap, w którym musi zacząć działać.

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
| C2 | Awaria instancji Hermesa — rozmowy przenoszone, sesje odtwarzane | MVP4 |
| C3 | Żądanie usunięcia danych osoby zrealizowane, audyt zanonimizowany | MVP4 |
| C4 | Eksport danych osoby w formacie tekstowym | MVP4 |
| C5 | Instalacja dla drugiego klienta w jeden dzień, wyłącznie konfiguracja | MVP4 |
| C6 | W ruchu do dostawcy modelu wyłącznie tokeny zamiast numerów PPE | MVP4 |
