---
source: kontekst_rejestracja1.png
converted: lossy
used-in: BP, PL
requires-verification: true
---
# Rejestracja — kontekst (Frame 54 + Frame Rejestracja)

## Opis wizualny
Dwa framy: Frame 54 (Konfiguracja maratonu) oraz główny obszar Rejestracja. Zawiera zdarzenia rejestracji uczestnika i zmiany dystansu. Widoczne połączenia (strzałki łukowe) między zdarzeniami.

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Zarejestrowano maraton | domenowe | Frame 54 — konfiguracja |
| Zarejestrowano uczestnika | domenowe | główny flow rejestracji |
| Wybrano dystans | domenowe | krok rejestracji |
| Potwierdzono dane kontaktowe | domenowe | krok rejestracji |
| Zweryfikowano zawodnika | domenowe | weryfikacja tożsamości/danych |
| Wyliczono opłatę | domenowe | wyliczenie kwoty |
| Złożono wniosek o zmianę dystansu | domenowe | inicjowane przez uczestnika |
| Zmieniono dystans | domenowe | wynik pozytywny zmiany |
| Wyliczono opłatę (zmiana dystansu) | domenowe | przeliczenie opłaty po zmianie |
| Poinformowano użytkownika o zmianie opłaty | domenowe | komunikat do uczestnika |

## Aktorzy
- Uczestnik
- System
- Administrator

## Granice kontekstów
- Konfiguracja maratonu (Frame 54)
- Rejestracja (główny kontekst)

## Polityki / Zasady (różowe)
- Reguły konfiguracji maratonu (Frame 54)

## Hot Spoty / Problemy
- Połączenia łukowe między zdarzeniami sugerują zależności / wywołania między kontekstami
- Brak widocznego zdarzenia "potwierdzono rejestrację" — możliwa strata konwersji

## Cross-check z opisem systemu
- Rejestracja uczestnika na dystans: POKRYTE
- Weryfikacja opłaty: POKRYTE (wyliczono opłatę)
- Przydział numeru startowego: NIEOBECNE bezpośrednio — prawdopodobna strata konwersji, nie luka domenowa
