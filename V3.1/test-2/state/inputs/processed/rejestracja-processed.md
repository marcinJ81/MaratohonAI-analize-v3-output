<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture + process-level -->
<!-- used-in: phase-1, phase-2 -->
<!-- converted: lossy — screenshoty, struktura karteczek mogła być niekompletna -->
<!-- source-files: kontekst_rejestracja1.png, ogólny_widok_kontekstu_rejestracja.png -->

# Kontekst: Rejestracja

## Zdarzenia domenowe
| Zdarzenie | Aktor | Obszar |
|---|---|---|
| Zarejestrowano maraton | administrator | Konfiguracja maratonu |
| Zarejestrowano uczestnika | uczestnik | Rejestracja |
| Wybrano dystans | uczestnik | Rejestracja |
| Potwierdzone dane kontaktowe | uczestnik / system | Rejestracja |
| Zweryfikowano zawodnika | obsługa punktu kontrolnego | Rejestracja |
| złożono wniosek o zmianę dystansu | uczestnik | Rejestracja |
| Zmieniono dystans | system | Rejestracja |
| wyliczono opłatę | system | Rejestracja |
| Poinformowano użytkownika o zmianie opłaty | system | Rejestracja |

## Aktorzy
| Aktor | Rola | Zewnętrzny? |
|---|---|---|
| administrator | konfiguruje maraton | nie |
| uczestnik | rejestruje się, wybiera dystans | nie |
| obsługa punktu kontrolnego | weryfikuje dane zawodnika | nie |
| system | oblicza opłatę, zmienia dystans | nie |

## Granice i integracje
- Rejestracja → **Opłata za udział**: po wyliczeniu opłaty inicjuje proces płatności
- Rejestracja → **Grupy startowe**: zmiana dystansu wpływa na przynależność do grupy
- Konfiguracja maratonu → Rejestracja: reguły konfiguracji maratonu są policyą dla rejestracji

## Reguły / Polityki
- Reguły konfiguracji maratonu obowiązują w procesie rejestracji

## Hot Spoty
_(brak zidentyfikowanych hot spotów w tym kontekście)_

## Pytania otwarte
_(brak)_
