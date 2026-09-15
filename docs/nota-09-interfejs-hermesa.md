# Nota 09 — styk z Hermesem: co faktycznie oferuje ta instalacja

TEMAT: NAG-INFRA-001-interfejs-hermesa
DATA: 2026-09-10; KOREKTA R1 po kontroli Evaluatora
METODA: `hermes --help`, pomoc podpolecen, `--version`, pomocniczo odczyt
zainstalowanego zrodla (`~/.hermes/hermes-agent/hermes_cli/oneshot.py`) — bez
uruchamiania serwera, gatewaya, demona ani zadnego rzeczywistego zapytania do
modelu. Dla protokolow integracyjnych uzupelniono opis na podstawie oficjalnej
dokumentacji Hermesa; nie traktuje sie go jako wyniku uruchomienia tej wersji.

Zainstalowana wersja: `Hermes Agent v0.21.1 (2026.9.7)`, install method `git`,
180 commitow za upstreamem (`hermes --version`).

Ta notatka pokazuje stan rozpoznania w chwili zapisu. R1 koryguje twierdzenia
o pelnosci katalogu, rozdziela kontrakty `-z`, `chat -q`, `--oneshot`,
`--cli` i `-Q`, poprawia warunek `--provider`, uzupelnia profile OAuth oraz
dodaje ACP, TUI Gateway i API Server jako odrebne protokoly integracyjne.

---

## 1. Jakie polecenia udostepnia `hermes`

`hermes --help` jest zrodlem aktualnej, pelnej listy podpolecen dla tej
instalacji. Ta nota nie jest katalogiem pelnym. Wynik pomocy wymienia m.in.:

- rozmowe i modele: `chat`, `model`, `moa`, `fallback`, `sessions`, `prompt-size`;
- prace i integracje: `worktree`, `browser`, `lsp`, `send`, `acp`, `mcp`;
- bramy i serwery: `gateway`, `serve`, `proxy`, `dashboard`, `desktop`, `gui`;
- zarzadzanie i bezpieczenstwo: `profile`, `config`, `auth`, `secrets`,
  `egress`, `security`, `approvals`, `backup`, `checkpoints`;
- automatyzacje i obserwowalnosc: `cron`, `webhook`, `kanban`, `monitoring`,
  `logs`, `status`.

Wynik zawiera takze `migrate`, `sync`, `peer`, `portal`, `project`, `hooks`,
`doctor`, `verify`, `dump`, `debug`, `import`, `skills`, `plugins`, `memory`,
`tools`, `computer-use`, `insights`, `claw`, `update`, `uninstall`,
`completion` i inne. Sformulowanie „m.in.” jest celowe: pelny stan ustala sie
kazdorazowo przez `hermes --help`, a nie przez te notatke.

To jest samodzielny produkt CLI z zarzadzaniem profilami, sesjami, narzedziami,
bramami komunikatorow i kilkoma powierzchniami integracyjnymi — nie tylko
cienki klient jednego zapytania. Ma to konsekwencje dla D-010; patrz sekcja 7.

## 2. Wywolanie nieinteraktywne z jednym zapytaniem

### `hermes -z PROMPT` / `hermes --oneshot PROMPT`

**PRAWDA, z dowodem lokalnym.** Pomoc opisuje one-shot jako wyslanie jednego
promptu i wypisanie **wylacznie koncowej odpowiedzi** na stdout: bez bannera,
spinnera, podgladu narzedzi i linii `session_id`. Jest to kontrakt dla skryptow
i potokow.

### `hermes chat -q QUERY --oneshot`

To osobny interfejs podpolecenia `chat`. `--oneshot` oznacza odpowiedz na
zapytanie i wyjscie (legacy single-query behavior). Nie nalezy utozsamiac go
z kontraktem wyjscia `-z`: pomoc `chat` nie obiecuje „tylko tekstu odpowiedzi”
w taki sam sposob jak pomoc glowna.

### `hermes chat -q QUERY -Q` / `--quiet`

`-Q` jest trybem programowym: ukrywa banner, spinner i podglady narzedzi, ale
pomoc jawnie mowi, ze wyjscie obejmuje koncowa odpowiedz **i informacje o sesji**.
To rozni `-Q` od `-z`, ktore nie wypisuje `session_id`.

Na nieinteraktywnym stdin `chat -q` odpowiada i konczy sie zgodnie z opisem
pomocy; na prawdziwym TTY samo `-q` zasiewa pierwsza ture sesji. `--oneshot`
lub `-Q` wymusza zachowanie jednego zapytania.

### `hermes --cli`

`--cli` nie jest trybem one-shot. Wymusza klasyczny REPL oparty na
prompt_toolkit i nadpisuje ustawienie `display.interface: tui`. Jest to wybor
interfejsu interaktywnego, nie kontrakt skryptowego wyjscia.

### Kody wyjscia

Nie wykonano rzeczywistego zapytania do modelu, wiec nie sa to wyniki testu
runtime. Odczyt `hermes_cli/oneshot.py`, funkcja `run_oneshot()`, pokazuje dla
sciezki `-z`:

| Kod | Warunek wynikajacy ze zrodla |
|---|---|
| `0` | niepusta odpowiedz zostala wypisana na stdout |
| `1` | agent rzucil wyjatek albo nie powstala odpowiedz bez oznaczenia wyniku jako `failed`/`partial` |
| `2` | pusta odpowiedz ma wynik `failed` albo `partial` |
| `2` | `--provider` podano bez modelu i bez `HERMES_INFERENCE_MODEL` |

Warunek `--provider` jest precyzyjny: kod sprawdza, czy nie ma ani jawnego
`--model`, ani niepustej zmiennej `HERMES_INFERENCE_MODEL`. Sam provider jest
odrzucony tylko w tym przypadku; przy modelu ze zmiennej srodowiskowej moze
byc pomiety jawny `--model`. To zachowanie nie zostalo uruchomione z prawdziwym
zapytaniem.

`--usage-file PATH` jest dostepne tylko dla `-z`/`--oneshot` i zapisuje JSON
z szacowanym kosztem, tokenami, modelem i liczba wywolan API; zrodlo deklaruje,
ze raport jest zapisywany takze przy bledzie.

## 3. Model, provider i poziom reasoning

Na poziomie `hermes` pomoc rozroznia:

- `-m MODEL, --model MODEL` — jednorazowy override; dla `-z`/`--oneshot` i
  `--tui`; mozliwy takze przez `HERMES_INFERENCE_MODEL`;
- `--provider PROVIDER` — jednorazowy override; dla `-z`/`--oneshot` i
  `--tui`;
- `--reasoning LEVEL` — poziom dla tego wywolania: `none`, `minimal`, `low`,
  `medium`, `high`, `xhigh`, `max` albo `ultra`.

Na poziomie `hermes chat` opisy sa inne i nie wolno ich traktowac jako
identycznych: `--model` wybiera model sesji, `--provider` ma domysl `auto`, a
`--reasoning` opisuje poziom dla sesji. `-Q/--quiet` dotyczy wyjscia i
programowego uzycia, nie poziomu reasoning.

## 4. Rozdzielenie sesji i profili agentow

**PRAWDA, z dowodem z pomocy i oficjalnej dokumentacji.** `hermes profile
--help` opisuje profile jako wiele izolowanych instancji Hermesa. Profil ma
wlasny katalog `HERMES_HOME`, w szczegolnosci `config.yaml`, `.env`, `SOUL.md`,
pamieci, sesje, skills, cron, baze stanu i stan gatewaya.

`profile create --clone` kopiuje `config.yaml`, `.env`, `SOUL.md` i skills.
`--clone-all` kopiuje szerszy stan, ale wyklucza historie per-profile, m.in.
sesje i `state.db`. Profile sa wiec osobna granica konfiguracji i danych
Hermesa. `--resume`/`--continue` dotycza sesji, a `--worktree` katalogu
roboczego; nie sa zamiennikami profilu.

**Korekta OAuth:** osobny `.env` i osobne dane profilu nie oznaczaja, ze kazdy
profil ma osobny login OAuth. Oficjalna dokumentacja stwierdza, ze loginy OAuth
Anthropic, OpenAI Codex i xAI korzystaja z jednorazowych tokenow odswiezania;
`--clone-all` usuwa te wiersze z klonu, a profile czytaja login z root
`~/.hermes/auth.json`. Odswiezenie wykonane w dowolnym profilu zapisuje sie do
root, wiec login jest wspoldzielony. Statyczne klucze API sa kopiowane jak
zwykle. Osobny login OAuth dla profilu wymaga `hermes -p <name> auth add
<provider>`.

To rozdziela trzy rzeczy: dane profilu, statyczne klucze w `.env` oraz wspolny
rootowy stan OAuth. Nie ustalano lokalnym uruchomieniem, jakie loginy sa obecnie
skonfigurowane.

## 5. Interfejsy lokalne i protokoly integracyjne

Nie ma „trzech mechanizmow” jako pelnego stanu. Sa rozne powierzchnie o roznych
transportach i odbiorcach; zadna nie zostala uruchomiona w tym rozpoznaniu.

### Powierzchnie potwierdzone w lokalnej instalacji

- `serve --help`: headless backend server, JSON-RPC/WebSocket gateway dla
  desktopu i zdalnych klientow; domyslnie `[REDACTED]:9119`; `--insecure` jest
  oznaczone jako deprecated/no-op, a publiczny bind wymaga password albo OAuth.
- `proxy --help`: lokalny serwer HTTP przekazujacy zadania w formacie
  OpenAI-compatible do dostawcy uwierzytelnionego przez OAuth. To proxy do
  providerow, nie to samo co API sterujace agentem Hermesa.
- `gateway --help`: brama komunikatorow, m.in. Telegram, Discord, WhatsApp i
  Weixin; to kanal komunikacyjny, nie ogolny protokol programistyczny.
- `acp --help`: komenda uruchamiajaca Hermesa jako serwer ACP dla integracji
  z VS Code, Zed i JetBrains. Lokalny katalog instalacji zawiera tez
  `acp_adapter/`, `tui_gateway/server.py`, `tui_gateway/ws.py` i
  `gateway/platforms/api_server.py`; serwerow nie uruchamiano.

### Oficjalnie opisane protokoly programistyczne

Oficjalna dokumentacja programistycznej integracji opisuje trzy protokoly,
ktore prowadza do tego samego rdzenia `AIAgent`:

| Protokol | Transport | Zastosowanie |
|---|---|---|
| ACP | JSON-RPC over stdio | IDE i klienci mowiacymi Agent Client Protocol |
| TUI Gateway | JSON-RPC over stdio albo WebSocket | wlasny host z sesjami, slash commands, approval i streamingiem |
| API Server | HTTP + Server-Sent Events | frontend OpenAI-compatible i klienci web niezalezni od jezyka |

Dokumentacja podaje dla API Server m.in. `POST /v1/chat/completions`,
`POST /v1/responses`, przebieg `/v1/runs` i endpointy zdarzen SSE. Te endpointy
sa opisem protokolu oficjalnego; nie sa wynikiem uruchomienia serwera w tej
notatce.

NIE USTALONO runtime: czy wszystkie szczegoly tych protokolow sa identycznie
aktywne w zainstalowanej wersji `v0.21.1`. Potwierdzono lokalnie obecnosc
komendy `acp`, opcji `--tui`, pomocy `serve` oraz plikow zrodlowych; uruchomienie
serwera lub zapytania bylo poza zakresem.

## 6. Konsekwencje dla ryzyka z `docs/spec/01-mvp1.md` pkt 12

Ryzyko brzmialo: interfejs Hermesa do proxy okaze sie inny niz zakladano.
Stan faktyczny jest szerszy niz pierwotnie zapisano:

- `-z`/`--oneshot` i `chat -q`/`--oneshot` sa podobne funkcjonalnie, ale maja
  rozne kontrakty wyjscia; `-Q` dodaje informacje o sesji, a `--cli` jest
  interaktywnym klasycznym REPL;
- `serve` udostepnia backend JSON-RPC/WebSocket dla desktopu i zdalnych klientow;
- ACP jest protokolem IDE;
- TUI Gateway jest protokolem JSON-RPC dla wlasnych hostow;
- API Server jest HTTP/SSE dla klientow OpenAI-compatible;
- `proxy` przekazuje zadania do uwierzytelnionego providera OAuth;
- `gateway` obsluguje kanaly komunikatorow.

Wybor glownego punktu styku dla architektury nAgents pozostaje decyzja
projektowa, nie faktem technicznym. Ta nota nie wybiera za wlasciciela.

## 7. Zrodla i odtwarzalnosc

Zrodla lokalne, uruchomione lub odczytane:

- `hermes --version`
- `hermes --help`
- `hermes chat --help`
- `hermes --cli --help`
- `hermes chat --oneshot --help`
- `hermes acp --help`, `hermes serve --help`, `hermes gateway --help`,
  `hermes proxy --help`
- `~/.hermes/hermes-agent/hermes_cli/oneshot.py`, funkcja `run_oneshot()`;
  warunek providera: linie 183–191, kody wyniku: 235–263.

Zrodla oficjalne rzedu 1:

- https://hermes-agent.nousresearch.com/docs/developer-guide/programmatic-integration
- https://hermes-agent.nousresearch.com/docs/user-guide/profiles?full=1

Nie uruchamiano serwera, gatewaya, demona, OAuth ani rzeczywistego zapytania do
modelu. Nie testowano kodow wyjscia empirycznie.
