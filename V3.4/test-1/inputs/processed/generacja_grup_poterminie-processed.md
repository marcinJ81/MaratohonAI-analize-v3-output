---
source: generacja_grup_poterminie.png
converted: lossy
used-in: BP, PL
requires-verification: true
---
# Generacja grup startowych po terminie

## Opis wizualny
Diagram Event Storming (uproszczony / mniej rozbudowany) przedstawiający scenariusz dołączenia do grupy po upłynięciu terminu. Przepływ od lewej: zdarzenia wejściowe → System → Dołącz do grupy → wyniki.

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Opłacono dystans | domenowe | warunek wejściowy |
| Przyznano numer startowy | domenowe | warunek wejściowy |
| Przekazano opłatę za zmianę grupy (karteczka częściowo nieczytelna) | domenowe | prawdopodobna strata konwersji |
| Grupy startowe zostały wygenerowane | domenowe | stan systemu |
| Dołączono do grupy | domenowe | wynik pozytywny |
| Nie przekroczono terminu dołączenia do dystansu | domenowe | warunek |
| Upłynął czas na zmianę grupy | czasowe | trigger czasowy — blokada |
| Upłynął termin zmiany grupy dystansowo | czasowe | alternatywny trigger |
| Nadobiegnięto do grupy (karteczka nieczytelna, dolny prawy róg) | domenowe | prawdopodobna strata konwersji |

## Aktorzy
- System

## Granice kontekstów / swimlane'y
- Dołączanie do grupy po terminie
- Obsługa terminów zmiany grupy

## Hot Spoty / Problemy
- Kilka karteczek jest nieczytelnych (małe i rozmyte) — prawdopodobna strata konwersji
- Niejasna sekwencja: co się dzieje gdy upłynął czas a zawodnik jeszcze nie dołączył?

## Cross-check z opisem systemu
- Generowanie grup startowych: POKRYTE (scenariusz po terminie)
- Weryfikacja opłaty: POKRYTE (opłacono dystans jako warunek wejściowy)
- Przydział numeru startowego: POKRYTE (warunek wejściowy)
