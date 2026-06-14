---
source: ogólny_widok_kontekstu_rejestracja.png
converted: lossy
used-in: BP, PL
requires-verification: true
---
# Ogólny widok kontekstu rejestracja

## Opis wizualny
Panoramiczny widok czterech powiązanych framów (Frame 54, 52, 51) przedstawiający cały kontekst rejestracji i opłat. Widoczne połączenia między framami. Skala wyświetlania mała — część karteczek nieczytelna.

## Framy / Granice kontekstów
- Frame 54: Konfiguracja maratonu
- Frame 52: Rejestracja
- Frame 51: Opłata za udział
- Dodatkowy obszar po prawej: Przypisano numer startowy

## Zdarzenia domenowe (wyekstrahowane z ogólnego widoku)

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Zarejestrowano maraton | domenowe | Frame 54 |
| Zarejestrowano uczestnika | domenowe | Frame 52 |
| Wybrano dystans | domenowe | Frame 52 |
| Potwierdzono dane kontaktowe | domenowe | Frame 52 (nieczytelne częściowo) |
| Zweryfikowano zawodnika (nieczytelne) | domenowe | Frame 52 |
| Wyliczono opłatę | domenowe | Frame 52 |
| Złożono wniosek o zmianę dystansu | domenowe | Frame 52 |
| Zmieniono dystans | domenowe | Frame 52 |
| Rozpoczęto opłacenie udziału | domenowe | Frame 51 |
| Zarejestrowano przelew | domenowe | Frame 51 — zewnętrzne (bank) |
| Zaaplikowano opłatę | domenowe | Frame 51 |
| Opłacono uczestnictwo | domenowe | Frame 51 |
| Zaaplikowano promocje (-100%) | domenowe | Frame 51 — specjalny przypadek |
| Opłacono uczestnictwo (wariant) | domenowe | Frame 51 |
| Rozpoczęto opłacenie niedopłaty | domenowe | Frame 51 |
| Opłacono niedopłatę | domenowe | Frame 51 |
| Opłacono zmianę dystansu | domenowe | Frame 51 |
| Wyliczono kwotę zwrotu | domenowe | Frame 51 |
| Zwrócono nadpłatę | domenowe | Frame 51 |
| Złożono wniosek o zmianę dystansu | domenowe | Frame 51 dolny |
| Wyliczono opłatę za nowy dystans | domenowe | Frame 51 |
| Przypisano numer startowy | domenowe | skrajna prawa — nowy kontekst |
| Odpisano uczestnictwo (nieczytelne) | domenowe | Frame 51 dolny |

## Aktorzy
- Uczestnik
- Administrator
- Interfejs banku (zewnętrzny)
- Główny organizator
- Obsługa punktu kontrolnego
- Pracownik obsługi / administrator
- Główny organizator zawodów

## Hot Spoty / Problemy (różowe)
- Różowa karteczka przy Frame 51 środkowy obszar (treść nieczytelna z powodu skali)
  → flaga requires-verification: prawdopodobna strata konwersji, nie luka domenowa

## Cross-check z opisem systemu
- Rejestracja uczestnika: POKRYTE
- Weryfikacja opłaty: POKRYTE (opłata za udział — Frame 51)
- Przydział numeru startowego: POKRYTE (widoczne po prawej)
- Generowanie grup: NIEOBECNE w tym widoku — w osobnych plikach
- RFID/pomiar czasu: NIEOBECNE — w osobnych plikach
- Wyniki i dyplomy: NIEOBECNE — prawdopodobna strata konwersji, nie luka domenowa
