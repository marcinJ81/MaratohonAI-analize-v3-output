<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture -->
<!-- used-in: big-picture, process-level -->
<!-- converted: lossless -->

# Opłata za udział

## Źródła
- opłata_za_udział_1.png (Frame 51)
- opłata_za_udział_2.png (Frame 51 — ciąg dalszy)
- ogólny_widok_kontekstu_rejestracja.png (Fragment Frame 51)

---

## Zdarzenia domenowe

| Zdarzenie | Aktor | Obszar |
|---|---|---|
| Rozpoczęto opłacenie udziału | uczestnik / system | Opłata za udział |
| Zarejestrowano przelew | interfejs banku | Opłata za udział |
| Zarejestrowano opłatę | system | Opłata za udział |
| Opłacono uczestnictwo | system | Opłata za udział |
| Zaaplikowano promocję (-100%) | obsługa punktu kontrolnego | Opłata za udział |
| Rozpoczęto opłacenie niedopłaty | system | Opłata za udział |
| Opłacono niedopłatę | system | Opłata za udział |
| Opłacono zmianę dystansu | system | Opłata za udział |
| Wyliczono kwotę zwrotu | pracownik obsługi / administrator | Opłata za udział |
| Zwrócono nadpłatę | główny organizator zawodów | Opłata za udział |
| Opisano uczestnictwo | system | Opłata za udział |
| Złożono wniosek o zmianę dystansu | uczestnik | Opłata za udział |

## Aktorzy

| Nazwa | Rola | Zewnętrzny? |
|---|---|---|
| interfejs banku | Integracja z systemem płatności | Tak (zewnętrzny) |
| główny organizator | Inicjuje procesy płatności | Nie |
| obsługa punktu kontrolnego | Aplikuje promocje | Nie |
| pracownik obsługi / administrator | Wylicza zwroty | Nie |
| główny organizator zawodów | Zatwierdza zwrot nadpłaty | Nie |

## Reguły biznesowe

- W trakcie opłacania nie można zmienić dystansu (różowy sticky)
- Możliwość promocji 100% (bezpłatne uczestnictwo)
- Niedopłata powstaje przy zmianie dystansu na droższy
- Nadpłata powstaje przy zmianie dystansu na tańszy

## Granice kontekstu

- **Opłata za udział** (Frame 51): obsługuje wszystkie scenariusze płatności
- Integracja z zewnętrznym bankiem (interfejs banku)
- Powiązany z kontekstem Rejestracja (zmiana dystansu wpływa na opłatę)

## Hot Spoty / Otwarte pytania

- **HOT SPOT (różowy)**: Co zrobić jak rozpocznie się generowanie list a czekamy na opłacenie niedopłaty?
- Jak długo system czeka na opłatę przed anulowaniem?
- Kto inicjuje opłacenie — uczestnik samodzielnie czy system wysyła link?
