# Proces: Grupy startowe (BC-04) — Core Domain

## Wyzwalacz A — Automatyczna generacja grup
Timer systemowy osiąga ostateczny termin generowania grup z regulaminu.

## Przepływ: Automatyczna generacja grup
1. System sprawdza warunek: nie przekroczono terminu generowania grup (z regulaminu)
2. System sprawdza: jest przynajmniej X uczestników z numerami startowymi (minimalna liczba z konfiguracji)
3. [Warunki spełnione] → E-046 Wygenerowano grupy startowe
4. System przypisuje nieprzydzielonych zawodników → E-048 Przypisano nieprzydzielonych (auto)
5. [Warunki niespełnione] → E-047 Nie udało się wygenerować grup startowych

## Przepływ: Zmiana grupy przez zawodnika (przed terminem)
Wyzwalacz: zawodnik składa wniosek o zmianę grupy

1. E-041 Złożono wniosek o zmianę grupy
2. System sprawdza reguły łącznie (wszystkie muszą być spełnione):
   - RB-040: Nie przekroczono terminu zmiany grup (z regulaminu)
   - RB-041: Grupy startowe zostały już wygenerowane
   - RB-042: Zawodnik nie zmieniał wcześniej grupy (tylko jedna zmiana)
   - RB-043: Grupa docelowa ma wolne miejsca
   - RB-044: Grupa docelowa jest z tego samego dystansu
3. [Wszystkie OK] → E-044 Zmieniono grupę / E-042 Przyjęto wniosek
4. [Któryś warunek niespełniony] → E-045 Nie zmieniono grupy / E-043 Nie przyjęto wniosku
   - Przypadek: E-053 Nie przypisano do ostatecznej grupy (gdy nie złożono wcześniej wniosku)

## Przepływ: Zmiana grupy przez administratora
Wyzwalacz: administrator ręcznie przypisuje zawodnika

1. E-040 Przypisano zawodnika do grupy (bez ograniczeń okna czasowego?)
2. Brak dodatkowych warunków — administrator ma nadpisanie

## Przepływ: Dołączenie do grupy po terminie generacji
Wyzwalacz: uczestnik opłacił dystans po wygenerowaniu grup

1. System sprawdza: Nie przekroczono terminu dołączenia do dystansu
2. [OK] → E-049 Dołączono do grupy
3. [Po terminie] → brak możliwości dołączenia

## Przepływ: Zmiana dystansu → konsekwencje dla grupy
Wyzwalacz: BC-02 zatwierdza zmianę dystansu

1. Zawodnik zmienił dystans → obecna grupa jest nieaktualna
2. [Przed generacją grup] → zmiana dystansu wystarczy, nowe grupy uwzględnią nowy dystans
3. [Po generacji grup] → zawodnik musi złożyć wniosek o zmianę grupy (BC-04 przepływ zmiany)

## Reguły biznesowe
- RB-040: Termin zmiany grup z regulaminu (okno czasowe)
- RB-041: Zmiana grupy możliwa tylko po wygenerowaniu grup (nie przed)
- RB-042: Jedna zmiana grupy per zawodnik
- RB-043: Wolne miejsca w grupie docelowej
- RB-044: Dystans grupy docelowej = dystans zawodnika
- RB-045: Po upłynięciu terminu zmiany → przydział jest ostateczny (na stałe)
- RB-046: Automatyczna generacja odbywa się w określonym terminie z regulaminu

## Wyjątki
- Brak wystarczającej liczby uczestników → grupy nie mogą być wygenerowane
- Wszystkie miejsca w grupach docelowych zajęte → brak możliwości zmiany
