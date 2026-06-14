---
source: grupy_startowe_ds.png
converted: lossy
used-in: BP, PL
requires-verification: true
---
# GrupyStartowe — Design Level (zmiana grupy)

## Opis wizualny
Diagram Design Level (widoczne zielone Read Modele i niebieska komenda). Przedstawia logikę zmiany grupy startowej przez zawodnika. Tabela warunków po lewej → komenda Zmień → read modele (warunki) → zdarzenia domenowe.

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Zmieniono grupę | domenowe | wynik pozytywny |
| Nie udało się zmienić grupy | domenowe | wynik negatywny |

## Komendy (niebieskie)
- Zmień (grupę)

## Read Modele (zielone)
- Ostateczny termin zmiany grupy
- Grupa (docelowa)
- Zmiany grup (historia)
- Grupy startowe

## Polityki / Warunki (żółte)
- Nie przekroczono terminu zmiany grup
- Grupa docelowa ma wolne miejsca
- Zawodnik nie zmieniał wcześniej grupy
- Grupa docelowa jest z tego samego dystansu
- Grupy startowe zostały wygenerowane

## Aktorzy
- Zawodnik (implicite — inicjuje komendę Zmień)

## Granice kontekstów
- GrupyStartowe (bounded context jawnie nazwany)

## Hot Spoty / Problemy
- Reguła: "Zawodnik nie zmieniał wcześniej grupy" — czy zmiana jest jednorazowa? Wymaga weryfikacji z biznesem
- Warunek dystansu: zmiana możliwa tylko w obrębie tego samego dystansu

## Cross-check z opisem systemu
- Generowanie grup startowych: POKRYTE (kontekst zmiany grupy)
- Obsługa wielu dystansów: POKRYTE (warunek ten sam dystans)
