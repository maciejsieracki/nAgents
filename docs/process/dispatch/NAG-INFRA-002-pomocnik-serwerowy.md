# NAG-INFRA-002-pomocnik-serwerowy — dispatch

STATUS: DISPATCHED
DOMAIN: INFRA
ROLE: Orkiestrator / decyzja właściciela
TEMAT: NAG-INFRA-002-pomocnik-serwerowy

## Wyzwalacz

Właściciel wybrał wariant A: serwerowy pomocnik z trwałą instrukcją procesu,
który może prowadzić kwalifikowane przejścia Kanbana podczas nieobecności
właściciela. Wybór został przekazany jednoznacznie jako `A` po porównaniu
wariantów A/B/C.

## GOAL

NAgents ma mieć niezależnego od Desktopu serwerowego pomocnika procesu, który
odbiera dyspozycje Crona, pilnuje zatwierdzonego grafu Operator → Evaluator →
Obrona warunkowo → Final Control, uruchamia wyłącznie kwalifikowane następne
etapy i eskaluje każdy stan niejednoznaczny.

## Zakres

- Cron pozostaje read-only generatorem dyspozycji.
- Pomocnik działa na serwerze jako proces nadzorowany przez systemd/gateway,
  bez zależności od bieżącego czatu Desktopu.
- Reguły procesu pochodzą z kanonicznych dokumentów nAgents oraz kontraktu
  AutoBot; routing przejść jest fail-closed.
- Pomocnik nie tworzy nowego zakresu pracy ani nie podejmuje decyzji
  właścicielskich. Może utworzyć/reużyć wyłącznie przewidzianą techniczną kartę
  kontynuacji, z niezmiennym ID i idempotency key.
- Każde przejście wymaga readbacku karty, runu, eventu, artefaktu i receiptu.

## Kryteria końca

1. Dyspozycja Crona dociera do serwerowego pomocnika bez otwartego Desktopu.
2. Pomocnik zna i stosuje bieżącą wersję reguł nAgents/AutoBot.
3. Zakończenie Operatora uruchamia Evaluatora; zarzuty uruchamiają obronę;
   brak zarzutów prowadzi do Final Control.
4. `FAIL`, `BLOCK`, `TIMEOUT`, `INFRA`, brak dowodu albo niejasna zależność
   zatrzymują tylko właściwy strumień i tworzą eskalację.
5. Replay tej samej dyspozycji nie tworzy drugiego runu, successorа ani
   dostarczenia.
6. Zamknięcie Desktopu nie przerywa pomocnika ani kwalifikowanego workera.
7. Żadne przejście nie zmienia GOAL, allowlisty, decyzji właściciela ani nie
   wykonuje push/merge/deploy bez osobnej zgody.

## Allowlista

- `CLAUDE.md`
- `docs/spec/00-architektura.md`
- `docs/spec/README.md`
- `docs/spec/decisions.md`
- `docs/spec/scenarios.md`
- `docs/process/tematy.md`
- `docs/process/echo.md`
- `docs/process/dispatch/NAG-INFRA-002-pomocnik-serwerowy.md`
- dokumentacja i testy pomocnika w osobnym zatwierdzonym temacie

## Zakazy

- brak sekretów, tokenów, danych osobowych i surowych logów w repozytorium;
- brak ręcznego kopiowania `state.db` lub transcriptów;
- brak uruchamiania kart `TRIAGE`, `TODO`, `BLOCKED` i process-only bez spełnionej
  zależności;
- brak automatycznego rozstrzygania kosztu, danych, dostępu, prawa,
  odwracalności i zakresu;
- brak merge, push, deploy, restartu gatewaya lub live install w tym dispatchu;
- brak uznania samego raportu, etykiety UI albo statusu `queued` za dowód.

## Następna bramka

Po zapisaniu decyzji i scenariuszy: utworzyć izolowany temat implementacyjny
pomocnika, przeprowadzić test canary z zamkniętym Desktopem, a dopiero potem
rozważyć wznowienie kwalifikowanych strumieni AutoBot.
