---
source: utowrzenie_grup_ds.png
converted: lossy
used-in: BP, PL
requires-verification: true
---
# GeneracjaGrupStartowych — Design Level (tworzenie grup)

## Opis wizualny
Diagram Design Level (zielone karteczki Read Models, niebieska komenda Generuj). Logika tworzenia grup startowych — warunki i wyniki. Analogiczny do genreacja_numeru_indywidualna.png ale bardziej szczegółowy.

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Wygenerowano grupy startowe | domenowe | wynik pozytywny — główne zdarzenie |
| Nie udało się wygenerować grup startowych | domenowe | wynik negatywny |
| Wygenerowano grupy (Read Model) | domenowe | zielona karteczka — stan dostępny dla innych |

## Komendy (niebieskie)
- Generuj (grupy startowe)

## Read Modele (zielone)
- Wygenerowano grupy (wynik — Read Model)
- Grupy startowe (wynik — duży element)

## Polityki / Warunki (żółte)
- Grupy startowe nie zostały jeszcze wygenerowane
- Jest przynajmniej X uczestników z numerami startowymi
- Minimalna liczba uczestników
- Uczestnicy z przypisanym numerem
- Nie przekroczono ostatecznego terminu generowania grup zapisanego w regulaminie
- Ostateczny termin generowania grup
- Generowanie grup odbywa się w określonym w regulaminie terminie (różowa)

## Aktorzy
- Administrator (implicite — komenda Generuj)

## Granice kontekstów
- GeneracjaGrupStartowych (bounded context jawnie nazwany)

## Hot Spoty / Problemy
- Różowa karteczka: "Generowanie grup odbywa się w określonym w regulaminie terminie" — reguła konfiguracyjna

## Cross-check z opisem systemu
- Generowanie grup startowych: POKRYTE (kompletny Design Level)
- Przydział numeru startowego jako warunek wejściowy: POKRYTE
- Weryfikacja opłaty: NIEOBECNE bezpośrednio — warunek pośredni (numer startowy przypisywany po opłacie)
