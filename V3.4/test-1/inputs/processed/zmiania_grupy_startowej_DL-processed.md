---
source: zmiania_grupy_startowej_DL.png
converted: lossy
used-in: BP, PL
requires-verification: false
duplicate-of: zminia_grupystartowej-processed.md
user-confirmed: 2026-06-14
---
# Zmiana grupy startowej przez zawodnika — Design Level

## Opis wizualny
Diagram Design Level (zielone Read Models, pomarańczowa komenda Zmień grupę). Kompletna logika zmiany grupy: warunki wejściowe, stan systemu, warunki w komendzie, wyniki. Widoczne stany wejściowe (opłacono dystans, przypisano numer startowy).

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Zmieniono grupę | domenowe | wynik pozytywny |
| Nie udało się zmienić grupy | domenowe | wynik negatywny |
| Nie przypisano do ostatecznej grupy | domenowe | wynik negatywny alternatywny |

## Stany wejściowe (pomarańczowe — zdarzenia poprzednie)
| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Opłacono dystans | domenowe | warunek wejściowy |
| Przypisano numer startowy | domenowe | warunek wejściowy |

## Komendy (niebieskie/pomarańczowe)
- Zmień grupę (komenda użytkownika)

## Read Modele (zielone)
- Grupy startowe (wejście — sprawdzenie dostępności)
- Grupy startowe (stan po zmianie)
- Zmiany grup (historia)

## Polityki / Warunki (żółte)
- Nie przekroczono terminu zmiany grup
- Grupa docelowa ma wolne miejsca
- Zawodnik nie zmieniał wcześniej grupy
- Grupa docelowa jest z tego samego dystansu
- Grupy startowe zostały wygenerowane
- Nie złożono wcześniej wniosku (wynik negatywny — jeśli złożono)

## Zasady (różowe)
- Zmiana grupy jest możliwa tylko w pewnym oknie czasowym
- Reguły zmiany grupy: typ limit, dystans

## Aktorzy
- Zawodnik

## Granice kontekstów
- Zmiana grupy startowej przez zawodnika

## Hot Spoty / Problemy
- Reguła jednorazowej zmiany: "Zawodnik nie zmieniał wcześniej grupy" — ograniczenie biznesowe
- Okno czasowe: "zmiana możliwa tylko w pewnym oknie czasowym"
- Limit typu i dystansu — złożone reguły konfiguracyjne

## Cross-check z opisem systemu
- Generowanie grup: POKRYTE (zmiana grupy w obrębie wygenerowanych grup)
- Przydział numeru startowego: POKRYTE (jako warunek wejściowy)
- Weryfikacja opłaty: POKRYTE (opłacono dystans jako warunek)
