<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture + process-level -->
<!-- used-in: phase-1, phase-2 -->
<!-- converted: lossy — screenshoty, kolejność zdarzeń częściowo rekonstruowana -->
<!-- source-files: opłata_za_udział_1.png, opłata_za_udział_2.png -->

# Kontekst: Opłata za udział

## Zdarzenia domenowe
| Zdarzenie | Aktor | Obszar |
|---|---|---|
| Rozpoczęto opłacenie udziału | uczestnik | Opłata za udział |
| Zarejestrowano przelew | interfejs banku | Opłata za udział |
| Zarejestrowano opłatę | obsługa punktu kontrolnego | Opłata za udział |
| Opłacono uczestnictwo | system | Opłata za udział |
| zaaplikowano promocje (-100%) | obsługa punktu kontrolnego | Opłata za udział |
| Rozpoczęto opłacenie niedopłaty | uczestnik | Opłata za udział |
| Opłacono niedopłatę | system | Opłata za udział |
| Opłacono zmianę dystansu | system | Opłata za udział |
| wyliczono kwotę zwrotu | system | Opłata za udział |
| zwrócono nadpłatę | pracownik obsługi / administrator | Opłata za udział |
| złożono wniosek o zmianę dystansu | uczestnik | (po opłaceniu) |

## Aktorzy
| Aktor | Rola | Zewnętrzny? |
|---|---|---|
| interfejs banku | rejestruje przelewy | tak (system zewnętrzny) |
| główny organizator | zatwierdza operacje | nie |
| obsługa punktu kontrolnego | rejestruje opłatę ręcznie, stosuje promocje | nie |
| pracownik obsługi / administrator | wykonuje zwroty | nie |
| główny organizator zawodów | decyduje o zwrotach | nie |

## Granice i integracje
- **Interfejs banku** → Opłata za udział: rejestruje przelewy (integracja zewnętrzna)
- Opłata za udział → **Rejestracja**: po opłaceniu uczestnictwa dostępna zmiana dystansu
- Opłata za udział → **Grupy startowe**: opłacono zmianę dystansu → aktualizacja przydziału

## Reguły / Polityki
- W trakcie opłacania nie można zmienić dystansu
- Zmiana dystansu po opłaceniu → nowa kalkulacja opłaty (dopłata lub zwrot)

## Hot Spoty
| Opis | Priorytet |
|---|---|
| Co zrobić jak rozpocznie się generowanie list a czekamy na opłacenie niedopłaty? | blocker |

## Pytania otwarte
- Jak obsługiwana jest sytuacja gdy niedopłata nie zostanie uiszczona przed generowaniem grup?
- Kto inicjuje zwrot nadpłaty — system automatycznie czy ręcznie pracownik?
