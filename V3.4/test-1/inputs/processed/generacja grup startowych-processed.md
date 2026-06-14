---
source: generacja grup startowych.png
converted: lossy
used-in: BP, PL
requires-verification: true
---
# Generacja grup startowych

## Opis wizualny
Diagram Event Storming przedstawiający proces generacji grup startowych. Zawiera trzy sekcje: główną (przydział do grupy), zmianę dystansu przed wygenerowaniem grup oraz zmianę dystansu po wygenerowaniu grup.

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Przypisano zawodnika do grupy | domenowe | wynik przypisania |
| Złożono wniosek o zmianę grupy | domenowe | inicjowane przez zawodnika |
| Nie złożono wcześniej wniosku | domenowe | ścieżka alternatywna |
| Nie przypisano do ostatecznej grupy | domenowe | ścieżka błędu |
| Przyjęto wniosek do zmiany grupy | domenowe | wynik pozytywny |
| Nie przyjęto wniosku o zmianę grupy | domenowe | wynik negatywny |
| Grupa została zmieniona | domenowe | sukces |
| Grupa nie została zmieniona | domenowe | porażka |
| Wygenerowano ostateczne grupy | domenowe | finalizacja |
| Złożono wniosek o zmianę dystansu | domenowe | inicjowane przez zawodnika — sekcja przed generowaniem |
| Złożono wniosek o zmianę dystansu (po terminie) | domenowe | inicjowane przez zawodnika — sekcja po generowaniu |
| Przypisano zawodnika do grupy (automatycznie) | domenowe | system automatycznej generacji |
| Minął czas ma ewentualne zmiany grupy — przypisanie jest na stałe | czasowe | trigger czasowy blokady zmian |

## Aktorzy
- Administrator
- Zawodnik
- System (automatyczna generacja grup)

## Granice kontekstów / swimlane'y
- Ręczna obsługa wniosków o zmianę grupy
- Automatyczna generacja grup startowych
- Zmiana dystansu przed wygenerowaniem grup startowych
- Zmiana dystansu po wygenerowaniu grup startowych

## Hot Spoty / Problemy
- Nota tekstowa po prawej stronie: "grupy mogą istnieć tylko po pewnych krokach jako reprezentacje list startowych..." (częściowo nieczytelna — prawdopodobna strata konwersji)
- Zależność: czy wniosek o zmianę grupy można złożyć dopiero po wygenerowaniu grup?

## Cross-check z opisem systemu
- Generowanie grup startowych: POKRYTE
- Przydział numeru startowego: NIEOBECNE w tym pliku — obsługiwane w osobnym pliku
- RFID/pomiar czasu: NIEOBECNE — obsługiwane w osobnym pliku
