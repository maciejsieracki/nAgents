# Dispatch — NAG-INFO-001-appto-research

```text
TEMAT:   NAG-INFO-001-appto-research
DOMENA:  INFORMACYJNY
DATA:    2026-08-25
RUNDA:   1 z 3

WYZWALACZ
Polecenie właściciela, 2026-08-25: „https://www.appto.ai/pl/ to jest to, co
dokładnie chcemy osiągnąć, plus nasze dodatkowe specyfikacje. Zrób dokładny
research na temat tej konkurencji, funkcjonalności, zarządzania i na czym
prawdopodobnie to stoi."

Kontekst: appto pojawiło się już w decyzji D-001 jako platforma odrzucona przy
wyborze budować-czy-kupić, na podstawie jednego streszczenia nagrania. Została
tam nazwana „wzorcem projektowym". Dziś właściciel prosi o zbadanie tego wzorca
u źródła, a nie ze streszczenia.

GOAL
Wiemy, co appto faktycznie robi, jak zarządza dostępem i kosztem, na czym
prawdopodobnie stoi technicznie, oraz które z jego funkcji są w naszej
specyfikacji, których brakuje, a które świadomie odrzucamy.

ROZDZIAŁ FAKTU OD DOMYSŁU — wymaganie nadrzędne tego tematu
Każde twierdzenie w raporcie musi być oznaczone jednym z trzech znaczników:

  [F] FAKT — potwierdzony u źródła. Obowiązkowo adres strony i cytat albo
      opis, gdzie dokładnie to widać. Bez adresu nie ma znacznika [F].
  [W] WNIOSEK — wyprowadzony z faktów rozumowaniem. Obowiązkowo przesłanka
      i słowo „ponieważ".
  [D] DOMYSŁ — prawdopodobne, ale niepotwierdzone. Obowiązkowo poziom
      pewności i zdanie, co by go potwierdziło albo obaliło.

Materiał marketingowy jest źródłem faktu o tym, CO FIRMA TWIERDZI, nie o tym,
jak jest. Zdanie ze strony głównej wolno zapisać jako [F] wyłącznie w formie
„appto twierdzi, że…". Twierdzenie o działaniu produktu wymaga dowodu poza
materiałem sprzedażowym: dokumentacji, zrzutu interfejsu, cennika, regulaminu,
polityki prywatności, ogłoszenia o pracę, wpisu w rejestrze.

Ten projekt ma udokumentowaną historię błędu tego rodzaju — patrz §12.2 skilla.
Twierdzenie o konkurencie wpisane bez znacznika jest naruszeniem procesu.

KRYTERIA KOŃCA
- Każde twierdzenie ma znacznik [F], [W] albo [D]. Zero twierdzeń bez znacznika.
- Każdy [F] ma adres strony. Adres bez cytatu albo opisu miejsca nie wystarcza.
- Zbadane cztery obszary z polecenia: funkcjonalność, zarządzanie, model
  rozliczenia, warstwa techniczna.
- Jest zestawienie funkcji appto wobec naszych czterech etapów: co mamy w
  specyfikacji, czego nie mamy, co świadomie odrzucamy i dlaczego.
- Jest osobna sekcja: czego nie udało się ustalić i co by to rozstrzygnęło.
  Sekcja pusta jest podejrzana i wraca do poprawy.
- Rozstrzygnięte, czy ustalenia zmieniają decyzję D-001. Jeśli tak — wniosek
  o zmianę decyzji, nie ciche jej podważenie.

ZAKRES
W zakresie:      docs/nota-06-appto-research.md — nowy plik
                 docs/process/tematy.md — wpis o temacie
Poza zakresem:   zmiana docs/spec/decisions.md — dziennik decyzji zmienia
                 wyłącznie właściciel. Zmiana specyfikacji etapów.
                 Kontakt z appto, zakładanie konta próbnego, wypełnianie
                 formularzy na ich stronie.

ALLOWLISTA
- docs/nota-06-appto-research.md
- docs/process/tematy.md
- docs/process/dispatch/NAG-INFO-001-appto-research.md
Zakazane bezwzględnie: .env*, docs/spec/**, .claude/**, .git/**

IZOLACJA
Temat informacyjny, bez kodu. Praca na gałęzi `claude/git-connection-9sz6dg`.

PLAN TESTÓW
1. Test znaczników: każde twierdzenie ma [F], [W] albo [D]. Losowa próba
   dziesięciu [F] — sprawdzić adresy u źródła, nie w raporcie.
2. Test marketingu: czy któryś [F] powtarza obietnicę sprzedażową jako
   stwierdzenie o działaniu produktu.
3. Test kompletności: czy cztery obszary z polecenia są pokryte.
4. Test przydatności: czy zestawienie z naszą specyfikacją prowadzi do
   konkretnego wniosku, czy kończy się na wyliczance.

ZALEŻNOŚCI
Zależy od:    brak
Blokuje:      ewentualną rewizję D-001
Decyzje:      D-001 — może wymagać ponownego rozpatrzenia przez właściciela

BARIERY DOTKNIĘTE PRZEZ TEN TEMAT
Żadnego zakładania kont, żadnego wysyłania danych firmowych na stronę
konkurenta, żadnego kontaktu handlowego. Badamy to, co publiczne.
Bariera 7 — push wyłącznie na `claude/git-connection-9sz6dg`.
```
