---
source: zakończenie_2.png
converted: lossy
used-in: BP, PL
requires-verification: false
duplicate-of: zakończenie_zawodów-processed.md
user-confirmed: 2026-06-14
---
# Zakończenie zawodów — wersja 2

## Opis wizualny
Diagram Event Storming przedstawiający zakończenie zawodów. Dwa główne scenariusze: przyznanie nagród (przez głównego organizatora) oraz obsługa zwrotu pakietu startowego (przez zawodnika). Wyzwalacz czasowy (ikona zegara) dla zwrotu pakietu.

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Przyznano nagrodę | domenowe | inicjowane przez głównego organizatora zawodów |
| Zwrócono pakiet startowy | domenowe | zawodnik zwraca pakiet |
| Kaucja została zwrócona | domenowe | wynik pozytywny zwrotu |
| Otrzymano dyplom | domenowe | wydanie dyplomu uczestnikowi |
| Upłynął czas zwrotu pakietu | czasowe | wyzwalacz czasowy (ikona zegara) |
| Nie zwrócono pakietu startowego | domenowe | wynik po upłynięciu czasu |
| Kaucja została zatrzymana | domenowe | sankcja za niezwrócenie |

## Aktorzy
- Zawodnik
- Zawodnik lub osoba uprawniona przez zawodnika (hot spot)
- Główny organizator zawodów
- Obsługa punktu kontrolnego

## Polityki / Zasady (różowe)
- Reguły regulamin maratonu
- Zawodnik lub osoba uprawniona przez zawodnika (różowa — szczególny aktor)
- Czas do zwrotu ustalony w regulaminie (różowa)

## Wyzwalacze czasowe
- Upłynął czas zwrotu pakietu (ikona zegara — trigger czasowy)

## Granice kontekstów
- Zakończenie zawodów

## Hot Spoty / Problemy
- Możliwość zwrotu przez osobę uprawnioną (proxy) — wymaga modelowania w systemie
- Zatrzymanie kaucji — powiązanie z modułem finansowym

## Cross-check z opisem systemu
- Generowanie dyplomów: POKRYTE (otrzymano dyplom)
- Generowanie wyników: NIEOBECNE bezpośrednio — prawdopodobna strata konwersji, nie luka domenowa
- RFID: NIEOBECNE — wymóg oddania karty/dyskietki może być powiązany
