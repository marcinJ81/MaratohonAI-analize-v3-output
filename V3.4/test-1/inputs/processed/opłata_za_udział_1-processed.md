---
source: opłata_za_udział_1.png
converted: lossy
used-in: BP, PL
requires-verification: true
---
# Opłata za udział — część 1

## Opis wizualny
Diagram Event Storming (Frame 51). Dwa główne flow: (1) pełna opłata uczestnictwa, (2) opłata niedopłaty. Widoczny hot spot (różowa karteczka z pytaniem).

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Rozpoczęto opłacenie udziału | domenowe | trigger głównego flow |
| Zarejestrowano przelew | zewnętrzne | zdarzenie z systemu bankowego (interfejs banku) |
| Zaaplikowano opłatę | domenowe | |
| Opłacono uczestnictwo | domenowe | wynik pozytywny pełnej opłaty |
| Zaaplikowano promocje (-100%) | domenowe | specjalny przypadek promocji pełnej |
| Rozpoczęto opłacenie niedopłaty | domenowe | trigger flow niedopłaty |
| Zarejestrowano przelew (niedopłata) | zewnętrzne | zdarzenie z systemu bankowego |
| Opłacono niedopłatę | domenowe | wynik opłaty brakującej kwoty |
| Opłacono zmianę dystansu | domenowe | wynik dodatkowej opłaty za zmianę |

## Aktorzy
- Interfejs banku (aktor zewnętrzny)
- Główny organizator
- Obsługa punktu kontrolnego

## Polityki / Zasady (różowe)
- W trakcie opłacania nie można zmienić dystansu

## Hot Spoty / Problemy
- Pytanie: "co zrobić jak rozpocznie się generowanie list a czekamy na opłacenie niedopłaty?"
  → otwarte pytanie biznesowe — wymaga decyzji

## Cross-check z opisem systemu
- Weryfikacja opłaty: POKRYTE
- Rejestracja uczestnika: POKRYTE (pośrednio — opłata jest krokiem po rejestracji)
- Obsługa promocji: NIEOBECNE w opisie systemu — prawdopodobna strata konwersji lub dodatkowy zakres
