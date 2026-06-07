# Proces: Opłaty (BC-03)

## Wyzwalacz
BC-02 rejestruje uczestnika i oblicza wymaganą opłatę.

## Przepływ główny — opłata za uczestnictwo
1. Uczestnik inicjuje przelew → E-020 Rozpoczęto opłacenie udziału
2. Bank rejestruje przelew → E-021 Zarejestrowano przelew
   LUB obsługa rejestruje gotówkę → E-022 Zarejestrowano opłatę (ręcznie)
3. [Jeśli kwota = wymagana] → E-023 Opłacono uczestnictwo
4. [Jeśli zaaplikowana promocja 100%] → E-024 Zaaplikowano promocję → E-023 Opłacono uczestnictwo
5. [Jeśli kwota > wymagana] → E-025 Wyliczono kwotę zwrotu → E-026 Zwrócono nadpłatę → E-023 Opłacono uczestnictwo
6. [Jeśli kwota < wymagana] → uczestnik ma niedopłatę → E-027 Rozpoczęto opłacenie niedopłaty

## Przepływ — niedopłata
1. Uczestnik wnosi brakującą kwotę → E-028 Opłacono niedopłatę
2. → E-023 Opłacono uczestnictwo

## Przepływ — opłata za zmianę dystansu
1. BC-02 oblicza różnicę cen → E-030 Wyliczono opłatę do nowego dystansu
2. [Jeśli nowy dystans droższy] → uczestnik płaci różnicę → E-029 Opłacono zmianę dystansu
3. [Jeśli nowy dystans tańszy] → E-031 Wyliczono kwotę zwrotu → E-026 Zwrócono nadpłatę

## Reguły biznesowe
- RB-020: W trakcie opłacania nie można zmienić dystansu (hot spot → locked state)
- RB-021: Niedopłata blokuje zmianę dystansu, NIE blokuje uczestnictwa
- RB-022: Promocja 100% zwalnia z opłaty całkowicie
- RB-023: Zwroty realizuje pracownik obsługi / administrator (nie automatycznie)

## Wyjątki / Hot Spoty
- HS-01 (wyjaśniony): uczestnik z niedopłatą podczas generacji grup → zostaje na pierwotnym dystansie
- Pytanie otwarte: co jeśli przelew przyjdzie z opóźnieniem (po terminie generacji grup)?
