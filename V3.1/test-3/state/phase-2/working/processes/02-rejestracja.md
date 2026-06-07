# Proces: Rejestracja (BC-02)

## Wyzwalacz
Uczestnik chce wziąć udział w maratonie.

## Przepływ główny
1. Uczestnik rejestruje się w systemie → E-010 Zarejestrowano uczestnika
2. Wybiera dystans → E-011 Wybrano dystans
3. System weryfikuje dane → E-012 Potwierdzono dane kontaktowe, E-013 Zweryfikowano zawodnika
4. System oblicza opłatę → E-014 Wyliczono opłatę
5. Uczestnik przechodzi do BC-03 (Opłaty) → opłacenie udziału

## Przepływ: zmiana dystansu (przed generacją grup)
1. Uczestnik składa wniosek → E-015 Złożono wniosek o zmianę dystansu
2. System zmienia dystans → E-016 Zmieniono dystans
3. System przelicza opłatę → E-014 Wyliczono opłatę (nowa kwota)
4. System informuje o zmianie → E-017 Poinformowano uczestnika o zmianie opłaty
5. Uczestnik musi wyrównać różnicę → BC-03

## Przepływ: zmiana dystansu (po generacji grup)
1. Uczestnik składa wniosek → E-032 Złożono wniosek (po opłaceniu)
2. System sprawdza niedopłatę → [jeśli niedopłata: uczestnik musi zapłacić różnicę; jeśli nie zapłaci → pozostaje na pierwotnym dystansie]
3. Jeśli opłacono → E-016 Zmieniono dystans + zmiana grupy (→ BC-04)

## Reguły biznesowe
- RB-010: Zmiana dystansu jest możliwa do ostatecznego terminu z regulaminu
- RB-011: Zmiana dystansu po generacji grup wymaga zapłaty różnicy ceny PRZED zmianą
- RB-012: Brak zapłaty różnicy = pozostanie na pierwotnym dystansie (nie blokuje startu)

## Wyjątki
- E-013 Weryfikacja negatywna → uczestnik nie może się zarejestrować (szczegóły nieznane)

## Otwarte pytania
- Q-007: Czy anulowanie rejestracji jest możliwe i na jakich warunkach?
- Czy weryfikacja zawodnika to weryfikacja danych osobowych czy czegoś innego?
