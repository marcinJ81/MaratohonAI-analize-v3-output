---
source: przypisanie_zawodnika_do_listy.png
converted: lossy
used-in: BP, PL
requires-verification: true
---
# Przygotowanie zawodnika do startu

## Opis wizualny
Diagram Event Storming z dwoma subsections: (1) Kwalifikacja zawodnika do zawodnika startu oraz (2) Weryfikacja zawodnika. Opisuje proces check-in zawodnika przed startem.

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Zweryfikowano tożsamość zawodnika | domenowe | krok kwalifikacji |
| Zawodnik nie przeszedł weryfikacji | domenowe | ścieżka błędu |
| Podpisano oświadczenie zawodnika | domenowe | krok wymagany przed startem |
| Zawodnik nie podpisał oświadczenia | domenowe | ścieżka błędu |
| Wydano pakiet startowy | domenowe | wynik pozytywny kwalifikacji |
| Zdyskwalifikowano zawodnika | domenowe | wynik negatywny — brak podpisu lub weryfikacji |
| Zweryfikowano zawodnika przed startem | domenowe | sekcja Weryfikacja zawodnika |
| Zawodnik nie przeszedł weryfikacji (przed startem) | domenowe | ścieżka błędu w sekcji 2 |
| Zdyskwalifikowano zawodnika (przed startem) | domenowe | wynik negatywny sekcji 2 |

## Aktorzy
- Zawodnik
- Obsługa punktu kontrolnego

## Polityki / Zasady (różowe)
- Jeśli nie zweryfikowany w biurze
- Weryfikacja na podstawie listy startowej

## Granice kontekstów
- Kwalifikacja zawodnika do zawodnika startu
- Weryfikacja zawodnika (przed startem)

## Hot Spoty / Problemy
- Dwie odrębne weryfikacje: biurowa (kwalifikacja) i na starcie (weryfikacja) — różnica procesowa wymaga wyjaśnienia
- Wydanie pakietu startowego: czy zawiera chip RFID? Niejawne powiązanie z pomiarem czasu

## Cross-check z opisem systemu
- Rejestracja uczestnika: POKRYTE (pośrednio — kwalifikacja to krok po rejestracji)
- Weryfikacja opłaty: NIEOBECNE bezpośrednio — prawdopodobna strata konwersji (opłata sprawdzana wcześniej)
- Przydział numeru startowego: NIEOBECNE bezpośrednio — prawdopodobna strata konwersji, nie luka domenowa
- RFID: NIEOBECNE — powiązanie z pakietem startowym niejawne
