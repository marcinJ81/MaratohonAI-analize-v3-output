---
source: zakończenie_zawodów.png
converted: lossy
used-in: BP, PL
requires-verification: true
---
# Zakończenie zawodów — widok ogólny (Frame 46)

## Opis wizualny
Widok ogólny zakończenia zawodów (Frame 46). Zawiera zdarzenie wejściowe "Zakończono przejazdy wszystkich dystansów" oraz ten sam scenariusz zakończenia co w zakończenie_2.png — prawdopodobnie ten sam frame w innym powiększeniu lub lekko różniąca się wersja.

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Zakończono przejazdy wszystkich dystansów | domenowe | zdarzenie wejściowe — trigger końca zawodów |
| Przyznano nagrodę | domenowe | inicjowane przez głównego organizatora |
| Zwrócono pakiet startowy | domenowe | zawodnik zwraca pakiet |
| Kaucja została zwrócona | domenowe | wynik pozytywny |
| Otrzymano dyplom | domenowe | wydanie dyplomu |
| Upłynął czas zwrotu pakietu | czasowe | wyzwalacz czasowy |
| Nie zwrócono pakietu startowego | domenowe | wynik negatywny |
| Kaucja zatrzymana (dolna część, częściowo nieczytelna) | domenowe | sankcja |

## Aktorzy
- Zawodnik
- Zawodnik lub osoba uprawniona przez zawodnika
- Główny organizator zawodów
- Obsługa punktu kontrolnego

## Polityki / Zasady (różowe)
- Reguły regulamin maratonu
- Zawodnik lub osoba uprawniona przez zawodnika

## Wyzwalacze czasowe
- Upłynął czas zwrotu pakietu (ikona zegara)

## Granice kontekstów
- Zakończenie zawodów (Frame 46)

## Hot Spoty / Problemy
- Dolna część diagramu częściowo uwidnia się poza kadrem — strata konwersji

## Uwaga o duplikacji
- Ten plik jest bardzo zbliżony do zakończenie_2.png — prawdopodobnie dwa widoki tego samego procesu (ogólny + szczegółowy). Należy zweryfikować czy zawierają różne scenariusze.

## Cross-check z opisem systemu
- Generowanie dyplomów: POKRYTE
- Zakończenie przejazdu i wyniki: POKRYTE (trigger z pomiar_czasu.png)
- Wyświetlanie w czasie rzeczywistym: NIEOBECNE — prawdopodobna strata konwersji
