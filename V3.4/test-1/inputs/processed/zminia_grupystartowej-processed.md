---
source: zminia_grupystartowej.png
converted: lossy
used-in: BP, PL
requires-verification: true
---
# Zmiana grupy startowej przez zawodnika — widok ogólny

## Opis wizualny
Diagram Event Storming (Process Level / ogólny widok — mniejszy zoom niż DL). Przedstawia ten sam scenariusz co zmiania_grupy_startowej_DL.png ale w innej reprezentacji. Widoczne stany wejściowe, Read Modele (zielone), warunki (żółte), zdarzenia (pomarańczowe).

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Opłacono dystans | domenowe | warunek wejściowy |
| Przypisano numer startowy | domenowe | warunek wejściowy |
| Zmieniono grupę | domenowe | wynik pozytywny |
| Nie udało się zmienić grupy | domenowe | wynik negatywny |
| Nie przypisano do ostatecznej grupy | domenowe | wynik negatywny alternatywny |

## Komendy
- Zmień grupę

## Read Modele (zielone)
- Grupy startowe (wejście)
- Grupy startowe (wynik)
- Zmiany grup

## Polityki / Warunki (żółte)
- Nie przekroczono terminu zmiany grup
- Grupa docelowa ma wolne miejsca
- Zawodnik nie zmieniał wcześniej grupy
- Grupa docelowa jest z tego samego dystansu
- Grupy startowe zostały wygenerowane

## Zasady (różowe)
- Zmiana grupy jest możliwa tylko w pewnym oknie czasowym
- Reguły zmiany grupy: typ limit, dystans
- Nie złożono wcześniej wniosku

## Aktorzy
- Zawodnik (inicjuje)
- Grupy startowe (Read Model)

## Granice kontekstów
- Zmiana grupy startowej przez zawodnika

## Hot Spoty / Problemy
- Widok mniejszy — część karteczek częściowo nieczytelna (strata konwersji)

## Uwaga o duplikacji
- Bardzo zbliżony do zmiania_grupy_startowej_DL.png — prawdopodobnie ten sam proces w różnym powiększeniu (PL vs DL). Należy zweryfikować.

## Cross-check z opisem systemu
- Generowanie grup: POKRYTE
- Przydział numeru startowego: POKRYTE (warunek wejściowy)
- Weryfikacja opłaty: POKRYTE (opłacono dystans jako warunek)
