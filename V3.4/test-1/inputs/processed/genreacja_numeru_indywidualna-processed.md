---
source: genreacja_numeru_indywidualna.png
converted: lossy
used-in: BP, PL
requires-verification: true
---
# Generacja grup startowych dla zawodników z przypisanym numerem startowym

## Opis wizualny
Diagram Event Storming (Design Level — widoczne zielone karteczki Read Models i niebieskie komendy). Przedstawia logikę generowania grup startowych z uwzględnieniem warunków: minimalna liczba uczestników, ostateczny termin, uczestnicy z numerami startowymi.

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Wygenerowano grupy startowe | domenowe | wynik pozytywny głównego flow |
| Nie udało się wygenerować grup startowych | domenowe | wynik negatywny |

## Komendy (niebieskie)
- Generuj grupy startowe

## Read Modele (zielone)
- Uczestnicy z przypisanym numerem (wejście)
- Wygenerowano grupy (potwierdzenie)

## Polityki / Warunki (żółte)
- Jest przynajmniej X uczestników z numerami startowymi
- Minimalna liczba uczestników
- Uczestnicy z przypisanym numerem
- Nie przekroczono ostatecznego terminu generowania grup zapisanego w regulaminie
- Ostateczny termin generowania grup
- Ostateczny termin zmiany grupy
- Minimalna liczba uczestników
- Generowanie grup odbywa się w określonym w regulaminie terminie

## Aktorzy
- Administrator (implicite — przez komendę Generuj)

## Granice kontekstów / swimlane'y
- GeneracjaGrupStartowych (bounded context jawnie nazwany)

## Hot Spoty / Problemy
- Reguła: minimalna liczba X uczestników — konkretna wartość konfiguracyjna nieznana
- Zielona nota: "Generowanie grup odbywa się w określonym w regulaminie terminie" — wymaga konfiguracji

## Cross-check z opisem systemu
- Generowanie grup startowych: POKRYTE
- Przydział numeru startowego jako warunek wejściowy: POKRYTE (pośrednio)
- Weryfikacja opłaty: NIEOBECNE bezpośrednio — prawdopodobna strata konwersji, nie luka domenowa
