<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture -->
<!-- used-in: big-picture, process-level -->
<!-- converted: lossless -->

# Konfiguracja maratonu i Rejestracja

## Źródła
- kontekst_rejestracja1.png (Frame 54)
- ogólny_widok_kontekstu_rejestracja.png (Frame 52)

---

## Zdarzenia domenowe

| Zdarzenie | Aktor | Obszar |
|---|---|---|
| Zarejestrowano maraton | administrator | Konfiguracja maratonu |
| Zarejestrowano uczestnika | uczestnik | Rejestracja |
| Wybrano dystans | uczestnik | Rejestracja |
| Potwierdzono dane kontaktowe | system | Rejestracja |
| Zweryfikowano zawodnika | system | Rejestracja |
| Wyliczono opłatę | system | Rejestracja |
| Złożono wniosek o zmianę dystansu | uczestnik | Rejestracja |
| Zmieniono dystans | system | Rejestracja |
| Poinformowano użytkownika o zmianie opłaty | system | Rejestracja |

## Aktorzy

| Nazwa | Rola | Zewnętrzny? |
|---|---|---|
| administrator | Konfiguruje maraton, ustawia reguły | Nie |
| uczestnik | Rejestruje się, wybiera dystans | Nie |
| obsługa punktu kontrolnego | Weryfikuje i obsługuje zawodnika | Nie |
| system | Oblicza opłaty, weryfikuje dane | Nie (automatyczny) |

## Reguły biznesowe

- reguły konfiguracji maratonu (różowy sticky — hot spot)
- Zmiana dystansu możliwa w trakcie rejestracji (ale zmienia opłatę)

## Granice kontekstu

- **Konfiguracja maratonu** (Frame 54): administrator rejestruje maraton, ustawia reguły
- **Rejestracja** (Frame 52/54): uczestnik rejestruje się, wybiera dystans, system weryfikuje i liczy opłatę

## Hot Spoty / Otwarte pytania

- Brak szczegółów dotyczących reguł konfiguracji maratonu (różowy sticky)
- Jak system weryfikuje zawodnika na etapie rejestracji online?
