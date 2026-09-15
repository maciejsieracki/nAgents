# Nota decyzyjna 05 — Kupić czy zbudować

**NASTER · projekt nAgents · 22 sierpnia 2026**

Materiał o platformie appto (film Szymona Negacza) opisuje architekturę, którą projektujemy
od czterech not. Różnica jest taka, że tam ona już istnieje i jest na sprzedaż.
Ta nota stawia pytanie, którego dotąd nie postawiłem.

---

## 1. Pytanie, które trzeba zadać przed etapem pierwszym

Przez cztery noty projektowaliśmy warstwę pośredniczącą: centralny kontekst, uprawnienia
i role, rutyny z wyzwalaczami, kreator asystentów, panel kosztów, audyt.
**Polski dostawca sprzedaje dokładnie to.**

Z opisu produktu: jeden panel do zarządzania użytkownikami, asystentami i kontekstem firmy —
kto ma dostęp, na jakich asystentach pracuje, z jakiej wiedzy korzysta i jakie ma uprawnienia.
Do tego wyzwalacze uruchamiające rutyny, powiadomienia o wynikach, integracje
(Gmail, HubSpot, Kalendarz Google) oraz **dzienniki audytowe z historią kto, co i kiedy —
opisane jako gotowe pod wymagania zgodności**. Rozliczenie: płatność za użycie
albo pakiet kredytów.

Twoje własne słowa z początku rozmowy brzmiały: *„najlepiej użyć już dostępnych narzędzi"*.
Uczciwość wymaga, żebym zastosował to również do planu, który sam napisałem.

## 2. Bilans

| | Kupić (appto lub podobne) | Zbudować (Hermes + uprząż) |
|---|---|---|
| **Czas do działania** | Dni | ~20 dni roboczych + wdrożenie |
| **Kto utrzymuje** | Dostawca | Ty |
| **Wybór modelu** | Tak, w panelu | Tak, 200+ backendów |
| **Uprawnienia per pracownik** | Tak, w panelu | Do zbudowania |
| **Audyt** | Gotowy | Do zbudowania |
| **Gdzie mieszkają dane** | U dostawcy — **wymaga umowy powierzenia** | Tam, gdzie postawimy |
| **Kanał Teams** | Do sprawdzenia — widziałem Slacka i Gmaila | Natywny |
| **Koszt** | Za użycie lub kredyty, rosnie z liczbą osób | Tokeny + infrastruktura + Twój czas |
| **Bus factor** | Dostawcy | **Jeden — Ty** |
| **Uzależnienie** | Od dostawcy | Od Hermesa, ale kod otwarty i przenośny |

## 3. Rekomendacja

**Zanim ruszy etap pierwszy, poświęć jeden dzień na demo appto i porównanie z listą wymagań.**
To jest tani test hipotezy, na której stoi cały plan.

Kryterium decyzyjne stawiam ostro: **jeśli gotowa platforma pokrywa co najmniej 80%
wymagań i obsługuje Teams, budowanie własnej uprzęży przestaje być uzasadnione**
przy firmie z jedną osobą techniczną. Dwadzieścia dni pracy plus utrzymanie na zawsze
to wysoka cena za pozostałe 20%.

Trzy rzeczy, które przesądzają na korzyść budowania mimo wszystko:

1. **Dane nie mogą opuścić firmy.** Jeśli rezydencja danych jest twardym wymogiem,
   platforma zewnętrzna odpada niezależnie od funkcji.
2. **Teams jest warunkiem koniecznym, a platforma go nie ma.**
3. **Koszt przy 27 agentach i 20 osobach przewyższa koszt budowy** — do policzenia
   na konkretnym cenniku, nie na przeczuciach.

Sprawdź to w tej kolejności. Pierwsze pytanie zamyka sprawę najszybciej.

---

## 4. Dwie techniki do przyjęcia niezależnie od decyzji

### 4.1 Warstwa anonimizacji przed wysłaniem do API

Materiał wymienia anonimizację danych wrażliwych przed wysłaniem promptu do zewnętrznego
API. To adresuje nasze ryzyko nr 2 — ale **wymaga sprostowania, bo łatwo się tu przecenić**:

- **Pseudonimizacja** (zamiana numeru PPE na token, z tabelą mapowania trzymaną osobno)
  redukuje ryzyko, ale **nie zwalnia z RODO** — dane pseudonimizowane pozostają danymi
  osobowymi w rozumieniu przepisów.
- **Anonimizacja** zwalnia z RODO, ale wymaga trwałego i nieodwracalnego usunięcia każdej
  drogi do identyfikacji. Przy rozliczeniach, gdzie wynik musi wrócić do konkretnej umowy,
  jest z definicji niemożliwa.

**Wniosek praktyczny:** tokenizacja PPE i numerów umów przed wysłaniem to dobra praktyka
i realne ograniczenie szkody przy wycieku — ale **nie zastępuje umowy powierzenia
z dostawcą modelu**. Wzorzec do przyjęcia brzmi: tokenizuj po swojej stronie, umowę
podpisz mimo to.

### 4.2 Skrócone indeksy zamiast pełnej bazy wiedzy

Zamiast przesyłać całą bazę przy każdym zapytaniu, model dostaje skrócone indeksy,
a pełne dokumenty pobiera dynamicznie tylko wtedy, gdy są potrzebne.

Przy 27 profilach to jest **realna dźwignia kosztowa**, nie mikrooptymalizacja — kontekst
jest przesyłany przy każdej turze rozmowy, więc oszczędność mnoży się przez liczbę agentów
i liczbę tur. Wchodzi do projektu skilli domenowych.

---

## 5. Trzeci niezależny głos przeciwko 27 profilom

Materiał opisuje **wielopoziomowy kontekst** zamiast wielu osobnych agentów:

1. kontekst ogólnofirmowy — misja, oferta, wspólne zasady
2. kontekst zespołowy — procedury, wytyczne, case studies
3. prywatna pamięć użytkownika — indywidualne preferencje i styl pracy

To dokładnie nasze trzy warstwy, ale wyrażone jako **poziomy kontekstu w jednym systemie**,
a nie jako 27 osobnych profili. Prywatna pamięć użytkownika daje to samo, co agent
stanowiskowy, bez mnożenia instancji.

Licząc: Rauch („it's more on the god model"), Negacz (wielopoziomowy kontekst) i sama
konstrukcja Eve — **trzy niezależne źródła układają to tak samo.** Nasza topologia 27
profili jest po części obejściem otwartego błędu wspólnej pamięci w Hermesie (#11430),
a nie wyborem architektonicznym.

**To wzmacnia rekomendację z punktu 3:** decyzję o silniku i topologii warto podjąć
po demie gotowej platformy, a nie przed.

---

## 6. Czego materiał nie rozstrzyga

- **Czy platforma obsługuje Teams** — widziałem wymienione Slacka, Gmaila, HubSpot
  i Kalendarz Google. Teams nie pojawił się. Pytanie numer jeden na demo.
- **Gdzie fizycznie przetwarzane są dane** i na jakich warunkach powierzenia.
- **Ile to kosztuje przy 20 użytkownikach** — cennik za użycie trzeba przeliczyć na nasz
  wolumen, bo to jest oś, na której budowanie własnego wygrywa albo przegrywa.

---

## 7. Wniosek strategiczny z materiału, który podtrzymuję

> Większość funkcji AI staje się towarem powszechnym. Prawdziwa wartość leży w ułożeniu
> procesów, integracji z narzędziami firmy i nauczeniu zespołów korzystania
> z ustandaryzowanego systemu.

To jest ten sam wniosek co u Raucha: własnością intelektualną nie jest platforma,
tylko skille i kontekst. **A skoro tak, to platforma jest wymienna — i tym bardziej warto
sprawdzić, czy trzeba ją budować samodzielnie.** Skille i kontekst, które napiszemy,
przeniosą się i tak.

---

## 8. Uzupełnienie po drugim podsumowaniu

Szczegółowszy opis wzmacnia rekomendację z punktu 3 o trzy konkretne rzeczy.

### 8.1 Produkt powstał dla firm dokładnie waszej wielkości

Deklarowany powód budowy: **duzi dostawcy nie oferują elastycznych wdrożeń dla zespołów
40–50 osób**. To nie jest platforma dla korporacji przycięta w dół ani narzędzie dla
jednoosobowej działalności rozdmuchane w górę — celowała w segment, w którym jesteście.
Przy ocenie „czy pokrywa 80% wymagań" to zwiększa szanse na tak.

### 8.2 Wsparcie wdrożeniowe odpowiada na ryzyko, które zgłaszam od noty 01

W każdej dotychczasowej nocie wracał ten sam problem: **bus factor równy jeden**.
Materiał wymienia bezpośrednie wsparcie wdrożeniowe jako kluczowy wymóg rynku polskiego —
i to jest dokładnie ta pozycja, której własna uprząż nie ma i mieć nie będzie.
Kupując, kupujecie także kogoś, kto odbierze telefon, gdy system stanie, a Ciebie nie ma.

### 8.3 Integracje z lokalnymi systemami

Wymieniony jest Comarch ERP i Optima. **Pytanie na demo: czy używacie Comarcha?**
Jeśli tak, gotowa integracja z polskim ERP to pozycja, której w planie własnym nawet nie
wyceniałem — a przy rozliczeniach i prowizjach byłaby to praca na tygodnie, nie dni.

### 8.4 Element projektowy, którego nam brakuje

> „Pamięć prywatna a pamięć firmowa: możliwość **odcięcia prywatnych nawyków użytkownika
> od krytycznych asystentów procesowych**."

To jest przełącznik, którego nasza architektura nie ma, a powinna. W nocie 03 pisałem,
że przy przejmowaniu stanowiska następca dziedziczy pamięć poprzednika i trzeba ustalić,
co do niej trafia. Tu jest to rozwiązane inaczej i lepiej: **agent procesowy w ogóle nie
czyta prywatnych nawyków użytkownika.** Rozliczenie ma przebiegać tak samo niezależnie od
tego, kto o nie pyta — prywatne preferencje są w tym miejscu zakłóceniem, nie pomocą.

Do przyjęcia niezależnie od decyzji kupić-czy-zbudować: **rozdzielić pamięć na warstwę
procesową (wspólna, obowiązkowa, wersjonowana) i warstwę nawyków (prywatna, ignorowana
przez agentów krytycznych).**

### 8.5 Wzorzec rutyny, wart skopiowania w całości

Zademonstrowany przepływ jest dokładnie tym, co chcemy osiągnąć przy rozliczeniach:

> formularz na stronie → wyzwolenie rutyny → asystent pobiera zasady z bazy wiedzy →
> kwalifikuje → dodaje notatkę i tag w CRM → zmienia etap → powiadomienie na komunikatorze

Plus najważniejsza część: **dodanie jednej reguły wykluczenia w bazie wiedzy natychmiast
zmienia zachowanie systemu**, bez dotykania kodu i bez aktualizowania promptów u ludzi.
To jest operacyjna definicja tego, co nazywaliśmy „centralnym kontekstem" — i dobre
kryterium odbioru dla naszego etapu pierwszego, niezależnie od wybranej drogi.
