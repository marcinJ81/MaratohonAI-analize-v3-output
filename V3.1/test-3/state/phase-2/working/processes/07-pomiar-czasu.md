# Proces: Pomiar czasu (BC-07) — Core Domain

## Wyzwalacz
Zawodnik przykłada dyskietkę (transponder) do czytnika na punkcie pomiarowym.

## Przepływ: pomiar czasu zadeklarowanego dystansu
1. Zawodnik przyłącza dyskietkę → E-090 Zarejestrowano pomiar czasu
2. System pomiaru czasu identyfikuje zawodnika i punkt pomiarowy
3. System sprawdza: czy zawodnik ukończył wymagany dystans (wszystkie okrążenia / linia mety)?
   - [Nie] → czeka na kolejne odczyty
   - [Tak] → E-091 Zawodnik zakończył przejazd (deklarowany dystans)
4. Gdy wszyscy zawodnicy danego dystansu ukończyli LUB upłynął czas (24h):
   → E-092 Podliczono czasy przejazdu
   → E-093 Zakończono przejazd zawodników na danym dystansie
5. Gdy wszystkie dystanse zakończyły:
   → E-094 Zakończono przejazdy wszystkich dystansów → wyzwala BC-09

## Przepływ: pomiar czasu na wydłużonym dystansie
1. Zawodnik zgłasza prośbę o przedłużenie dystansu → E-095 Zgłoszono prośbę o przedłużenie
2. Obsługa punktu kontrolnego (weryfikuje z regulaminem):
   - RB-070: warunki przedłużenia z regulaminu muszą być spełnione
3. E-096 Przedłużono przejazd zawodnika
4. Zawodnik jedzie dalej → E-097 Odnotowano dodatkowy pomiar czasu
5. Zawodnik kończy wydłużony dystans → E-098 Zawodnik zakończył przejazd (wydłużony dystans)

## Reguły biznesowe
- RB-070: Limity dystansu z regulaminu (liczba okrążeń lub km)
- RB-071: Czas 24h jest twardy limit — system wie gdy "wszyscy dojechali albo że minął czas"
- RB-072: Punkty kontrolne na trasie — przejazd przez nie jest rejestrowany
- RB-073: Dane z punktów kontrolnych są podstawą do wykrycia pominięcia punktu (→ BC-08)
- RB-074: Wyniki generowane na podstawie: czas ukończenia dystansu (linia mety) + czasy pośrednie
- RB-075: Rywalizacja odbywa się w obrębie zadeklarowanego dystansu

## Otwarte pytania
- Q-005: Ile punktów kontrolnych jest na trasie? Czy przejazd przez każdy jest obowiązkowy?
- Czy pomiar rejestruje każde okrążenie (maraton to zawody okrążeniowe)?
- Jak system wie które okrążenie jest "ostatnim" dla danego dystansu?
