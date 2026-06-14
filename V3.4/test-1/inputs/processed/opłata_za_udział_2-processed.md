---
source: opłata_za_udział_2.png
converted: lossy
used-in: BP, PL
requires-verification: true
---
# Opłata za udział — część 2

## Opis wizualny
Kontynuacja Frame 51. Widoczny górny hot spot (to samo pytanie co w części 1). Dodatkowy flow: opłacenie niedopłaty z wyliczeniem zwrotu nadpłaty. Trzecia sekcja: złożenie wniosku o zmianę dystansu po opłaceniu uczestnictwa.

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Rozpoczęto opłacenie niedopłaty | domenowe | trigger — kontynuacja z części 1 |
| Zarejestrowano przelew (niedopłata) | zewnętrzne | zdarzenie z systemu bankowego |
| Opłacono niedopłatę | domenowe | wynik opłaty |
| Opłacono zmianę dystansu | domenowe | |
| Wyliczono kwotę zwrotu | domenowe | kalkulacja różnicy kwot |
| Zwrócono nadpłatę | domenowe | wynik pozytywny zwrotu |
| Opłacono uczestnictwo | domenowe | stan wyjściowy dla wniosku |
| Złożono wniosek o zmianę dystansu | domenowe | akcja uczestnika po opłaceniu |

## Aktorzy
- Interfejs banku (zewnętrzny)
- Główny organizator
- Pracownik obsługi / administrator
- Główny organizator zawodów

## Hot Spoty / Problemy
- Pytanie (górna część): "co zrobić jak rozpocznie się generowanie list a czekamy na opłacenie niedopłaty?" (powtórzone z części 1)
  → otwarte pytanie biznesowe

## Cross-check z opisem systemu
- Weryfikacja opłaty: POKRYTE (w tym scenariusz zwrotu nadpłaty)
- Zmiana dystansu po opłaceniu: POKRYTE (pośrednio — opis mówi o kilku dystansach)
- Generowanie dyplomów: NIEOBECNE — obsługiwane w osobnych plikach
