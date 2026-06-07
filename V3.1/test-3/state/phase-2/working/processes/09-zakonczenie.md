# Proces: Zakończenie zawodów (BC-09)

## Wyzwalacz
E-094 Zakończono przejazdy wszystkich dystansów.

## Przepływ: generowanie wyników
1. System automatycznie generuje ranking → E-102 Wygenerowano wyniki / ranking
   - Ranking per dystans
   - Podstawa: czas ukończenia pełnego dystansu (linia mety) + czasy pośrednie z punktów pomiaru
   - Zawodnicy zdyskwalifikowani (BC-08) są wykluczeni z rankingu

## Przepływ: ceremonia nagród
1. Główny organizator zatwierdza wyniki
2. Główny organizator przyznaje nagrody → E-110 Przyznano nagrodę (per dystans / kategoria)

## Przepływ: zwrot pakietu startowego
1. Zawodnik (lub upoważniona osoba) zwraca pakiet startowy + dyskietkę → E-111 Zwrócono pakiet startowy
2. Obsługa rejestruje zwrot → E-112 Kaucja zwrócona (za dyskietkę, jeśli była pobrana)
3. Zawodnik otrzymuje dyplom → E-113 Otrzymano dyplom

## Przepływ: nieodebrana dyskietka / niezwrócony pakiet
1. Timer: upływa czas zwrotu z regulaminu → E-114 Upłynął czas zwrotu pakietu
2. System sprawdza: zwrócono pakiet?
   - [Nie] → E-115 Nie zwrócono pakietu startowego → E-116 Kaucja zatrzymana
   - [Tak] → kaucja zwrócona wcześniej (E-112)

## Reguły biznesowe
- RB-090: Ranking generowany automatycznie, per dystans
- RB-091: Zdyskwalifikowani są wykluczeni z rankingu
- RB-092: Dyplom wydawany po zwrocie pakietu startowego (lub niezależnie? — nieznane)
- RB-093: Czas zwrotu pakietu startowego z regulaminu (deadline)
- RB-094: Kaucja zatrzymana jeśli pakiet nie zwrócony w terminie

## Otwarte pytania
- Czy dyplom jest wydawany przed czy po zwrocie pakietu?
- Czy nagrody są per dystans i per kategoria wiekowa / płeć?
- Czy wyniki są publikowane publicznie (strona www) czy tylko wewnętrznie?
