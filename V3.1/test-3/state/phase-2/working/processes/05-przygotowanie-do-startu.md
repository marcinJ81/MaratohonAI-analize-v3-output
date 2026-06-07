# Proces: Przygotowanie do startu (BC-05)

## Wyzwalacz
Zawodnik przybywa do biura zawodów przed startem.

## Przepływ główny — kwalifikacja zawodnika
1. Obsługa punktu kontrolnego weryfikuje tożsamość → E-060 Zweryfikowano tożsamość zawodnika
   - [Nie zweryfikowany w biurze] → E-064 Zdyskwalifikowano zawodnika
2. Zawodnik podpisuje oświadczenie → E-061 Podpisano oświadczenie zawodnika
   - [Nie podpisał] → E-065 Zdyskwalifikowano zawodnika
3. Obsługa wydaje pakiet startowy → E-062 Wydano pakiet startowy
4. Obsługa przypisuje numer startowy (dyskietkę) → E-066 Przypisano numer startowy

## Przepływ: weryfikacja przed startem (na linii startu)
1. Obsługa sprawdza zawodnika na liście startowej → E-063 Zweryfikowano zawodnika przed startem
   - [Nie na liście] → E-064 Zdyskwalifikowano zawodnika

## Przepływ: zgubienie dyskietki
1. Zawodnik zgłasza zgubienie → E-067 Zgubiono dyskietkę z numerem startowym
2. Obsługa pobiera kaucję → E-068 Przejęto kaucję
3. Obsługa odpina numer startowy → E-069 Odpięto numer startowy
4. Obsługa wydaje nową dyskietkę → E-070 Wydano nową dyskietkę

## Reguły biznesowe
- RB-050: Oświadczenie jest obowiązkowe — brak = dyskwalifikacja
- RB-051: Tożsamość musi być zweryfikowana przed wydaniem pakietu
- RB-052: Kaucja zabezpiecza zwrot dyskietki (kwota z regulaminu)
- RB-053: Numer startowy jest powiązany z chipem pomiarowym (dyskietka = transponder)

## Wyjątki
- Dyskwalifikacja jest natychmiastowa i ostateczna na tym etapie
