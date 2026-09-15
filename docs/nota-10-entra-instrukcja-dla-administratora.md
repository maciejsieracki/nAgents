# Instrukcja dla administratora NASTER — założenie dostępu przez konto firmowe

Ten dokument jest dla osoby, która **nie zna się na programowaniu**, ale ma
uprawnienia administratora w systemie Microsoft, w którym firma zarządza
kontami pracowników (ten sam system, co przy poczcie i logowaniu do Office).
System ten nazywa się **Microsoft Entra ID** — dalej w tym dokumencie
nazywany po prostu "panelem Microsoft".

**Po co to robimy.** Budujemy system, przez który pracownicy będą rozmawiać
z pomocnikami firmowymi (np. pomocnikiem do rozliczeń). Żeby system wiedział,
kim jest pracownik, musi zapytać o to panel Microsoft — tak samo, jak
pracownik loguje się dziś do poczty firmowej. Ten dokument prowadzi Cię przez
jednorazowe założenie takiego połączenia.

**Zastrzeżenie.** Poniższe nazwy są nazwami użytymi w aktualnej dokumentacji
Microsoft Learn. Jeśli nie widzisz dokładnie wskazanej nazwy, zatrzymaj się i
zapytaj zespół (patrz punkt 6); nie wybieraj zamiennika na wyczucie.

---

## 1. Kroki, jeden po drugim

**Krok 1.** Zaloguj się do panelu Microsoft swoim kontem administratora firmy.

**Krok 2.** W panelu Microsoft przejdź do **Entra ID → App registrations**.
To miejsce, w którym zgłasza się nowy program chcący korzystać z firmowego
logowania. [1]

**Krok 3.** Kliknij **New registration**.

**Krok 4.** Na ekranie, który się otworzy, wpisz nazwę rozpoznawalną dla
firmy, na przykład "nAgents — rozmowy z pomocnikami NASTER". Nazwa jest
wyłącznie etykietą do rozpoznania tej rejestracji na liście — nie wpływa na
działanie systemu.

**Krok 5.** Na tym samym ekranie, w polu **Supported account types**, wybierz
**Single tenant only** — w niektórych wersjach opisu jest to pokazane jako
**Accounts in this organizational directory only**. Ta opcja ogranicza dostęp
do kont w tej dzierżawie, a nie do kont prywatnych ani kont innych firm. [1]

**Krok 6.** Zatwierdź założenie rejestracji (przycisk "Register" albo
"Zarejestruj"). Panel pokaże ekran z podsumowaniem — na nim znajdziesz dwie
z trzech wartości opisanych w punkcie 2 poniżej. [1]

**Krok 7.** W utworzonej rejestracji przejdź do **Manage → Authentication**.
W sekcji konfiguracji adresu przekierowania wybierz **Add Redirect URI**,
następnie platformę **Web**, wpisz **adres powrotny** opisany w punkcie 4
poniżej i wybierz **Configure**. Nie dodawaj go na ekranie tworzenia
rejestracji ani jako platformy mobilnej lub klienckiej. [2]

**Krok 8.** Znajdź w tej samej rejestracji miejsce do tworzenia poświadczenia
aplikacji — **Certificates & secrets → Client secrets → New client secret**.
Utwórz client secret. Panel pokaże jego wartość **tylko raz** — to jest
trzecia, **tajna** wartość z punktu 2. Skopiuj ją od razu w bezpieczne miejsce.
Microsoft zaleca dla produkcji certyfikat albo poświadczenie federacyjne zamiast
client secret; zespół techniczny musi uwzględnić to przy wdrożeniu. [3]

**Krok 9.** W tej samej rejestracji wybierz **API permissions**. Dodaj do niej
dokładnie uprawnienia wymienione w punkcie 3 poniżej — żadnych innych.

**Krok 10.** Kliknij przycisk zatwierdzający te uprawnienia w imieniu całej
dzierżawy — w języku angielskim jest opisany jako **Grant admin consent for
<nazwa dzierżawy>**. Wartość `<nazwa dzierżawy>` oznacza nazwę organizacji
Microsoft, nie nazwę rejestracji aplikacji. Jeśli przycisk ma inną nazwę albo
nie jest dostępny, zatrzymaj się i zapytaj zespół — nie zgaduj. [4]

**Krok 11.** Przekaż zespołowi technicznemu trzy wartości z punktu 2,
zachowując zasadę rozdzielenia kanałów z punktu 6.

---

## 2. Trzy wartości do przekazania zespołowi

Po zakończeniu kroków 1–8 masz trzy wartości. Zespół potrzebuje wszystkich
trzech, żeby system w ogóle mógł zapytać panel Microsoft o tożsamość
pracownika.

| # | Wartość | Gdzie ją znaleźć | Czy jest tajna |
|---|---|---|---|
| 1 | Identyfikator tej rejestracji (Application ID / client ID) | ekran podsumowania po kroku 6 | NIE — można przekazać zwykłym mailem |
| 2 | Identyfikator Twojej firmy w chmurze Microsoft (Directory ID / tenant ID) | ten sam ekran podsumowania | NIE — można przekazać zwykłym mailem |
| 3 | Client secret — wartość poświadczenia aplikacji, utworzona w kroku 8 | pokazana tylko raz, zaraz po utworzeniu | **TAK — to jest sekret** |
Client secret jest poświadczeniem **tej aplikacji**. Nie jest hasłem do konta
całej firmy ani automatycznym dostępem do wszystkich danych. Jego ujawnienie
może pozwolić podszyć się pod aplikację; rzeczywisty zakres dostępu zależy od
skonfigurowanych uprawnień i udzielonej zgody w dzierżawie. Traktuj go jako
sekret wysokiej wrażliwości i przekaż zgodnie z punktem 6. [3][4]

---

## 3. Uprawnienia — pełna lista, z podziałem

System potrzebuje wyłącznie potwierdzenia, **kim jest** logujący się
pracownik. `openid` służy do logowania i daje aplikacji unikatowy identyfikator
użytkownika w postaci claimu `sub`. `profile` i `email` mogą wzbogacić dane
tożsamości; nie są gwarancją firmowego adresu poczty i nie zastępują `sub` jako
identyfikatora. Nie proś o dostęp do poczty, plików, kalendarza ani innych
danych firmowych. [7][8]

| Uprawnienie | Rola w logowaniu |
|---|---|
| `openid` — logowanie i identyfikator `sub` | aplikacja może uwierzytelnić użytkownika i otrzymać unikatowy identyfikator dla tej aplikacji |
| `profile` — dane profilu | może dostarczyć m.in. imię, nazwisko, preferowaną nazwę i object ID |
| `email` — claim adresu e-mail | adres jest zwracany tylko, gdy jest powiązany z kontem i dostępny; nie zakładaj, że zawsze istnieje |
Nie zakładaj, że każdy pracownik może samodzielnie udzielić zgody. Microsoft
podaje, że możliwość zgody użytkownika organizacyjnego zależy od ustawień
organizacji; gdy użytkownik nie może jej udzielić, zgodę musi udzielić
administrator. Dlatego wykonaj krok 10, jeśli panel go pokazuje i masz do tego
uprawnienie. Nie traktuj zgody użytkownika jako furtki ani nie zakładaj, że
krok 10 jest jedynym działaniem administracyjnym w każdej dzierżawie. [5][6]

Nie dodawaj żadnych innych uprawnień, nawet jeśli panel Microsoft je
zasugeruje jako "popularne" albo "zalecane" — nie są potrzebne w tym etapie
projektu.

---

## 4. Adres powrotny — musi zgadzać się co do znaku

Adres powrotny to miejsce, do którego panel Microsoft odsyła pracownika po
udanym zalogowaniu. Kończy się dokładnie ciągiem:

```
/auth/callback
```

Resztę adresu — nazwę strony internetowej, pod którą system będzie
dostępny — **poda Ci zespół techniczny**. To DO POTWIERDZENIA przez zespół,
nie coś, co możesz wymyślić sam.

**OSTRZEŻENIE.** Ten adres musi być wpisany w panelu Microsoft
**identycznie co do każdego znaku** — wielkość liter, myślniki, "https"
zamiast "http". Nawet jedna literówka sprawi, że logowanie przestanie
działać, a komunikat błędu, który zobaczy pracownik, nie wskaże wprost tej
przyczyny. Skopiuj adres, który dostaniesz od zespołu — nie przepisuj go
ręcznie.

---

## 5. Jak sprawdzamy działanie

**Decyzja C: administrator nie wykonuje ręcznego logowania do aplikacji.** Nie
otwieraj strony `/login` ani nie sprawdzaj ekranu po zalogowaniu. W tym projekcie testowe logowanie jest wykonywane
wyłącznie w automatycznych testach zespołu technicznego. Ich wynik, a nie ręczne
wejście do aplikacji, jest dowodem działania.

Jeśli test automatyczny zgłosi błąd, przekaż zespołowi dokładny komunikat i
wartość adresu przekierowania z punktu 4. Nie zmieniaj konfiguracji na wyczucie
i nie twórz ręcznej furtki logowania.

---

## 6. Czego nie robić

- **Nie wysyłaj tajnej wartości nr 3 (client secret) tym samym kanałem, co
  pozostałe dwie wartości albo co tę instrukcję.** Na przykład: dwie jawne
  wartości i instrukcję wyślij mailem, a client secret przekaż osobno —
  telefonicznie, przez inny komunikator albo w oddzielnej, zaszyfrowanej
  wiadomości. Jeśli ktoś przechwyci jedną wiadomość, nie dostanie kompletu.
- **Nie zapisuj client secret** w arkuszu, notatniku ani dokumencie, który
  nie jest przeznaczony do przechowywania haseł.
- **Nie dodawaj uprawnień spoza listy z punktu 3**, nawet jeśli panel
  Microsoft podpowiada inne jako "zalecane".
- **Nie klikaj przycisków usuwających, unieważniających albo odwołujących**
  przy innych, już istniejących rejestracjach w panelu — możesz przypadkiem
  wyłączyć coś, co firma już wykorzystuje.
- **Jeśli w trakcie wykonywania tych kroków panel wygląda inaczej, niż tu
  opisano** — zatrzymaj się i zapytaj zespół techniczny, zamiast zgadywać
  albo klikać na wyczucie.

## Sources

[1] https://learn.microsoft.com/en-us/entra/identity-platform/quickstart-register-app
[2] https://learn.microsoft.com/en-us/entra/identity-platform/how-to-add-redirect-uri
[3] https://learn.microsoft.com/en-us/entra/identity-platform/how-to-add-credentials
[4] https://learn.microsoft.com/en-us/entra/identity-platform/quickstart-configure-app-access-web-apis
[5] https://learn.microsoft.com/en-us/entra/identity-platform/delegated-access-primer
[6] https://learn.microsoft.com/en-us/entra/identity-platform/permissions-consent-overview
[7] https://learn.microsoft.com/en-us/entra/identity-platform/scopes-oidc
[8] https://learn.microsoft.com/en-us/entra/identity-platform/userinfo
