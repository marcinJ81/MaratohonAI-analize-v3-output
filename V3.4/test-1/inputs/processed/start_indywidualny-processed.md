---
source: start_indywidualny.png
converted: lossy
used-in: BP, PL
requires-verification: true
---
# Start indywidualny + Zgubienie dyskietki z numerem startowym

## Opis wizualny
Dwa odrębne scenariusze w jednym pliku: (1) Start indywidualny — zawodnik startuje poza grupą, (2) Zgubienie dyskietki/numeru startowego — obsługa utraty karty RFID.

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Zgłoszono indywidualny start | domenowe | Zawodnik inicjuje start poza grupą |
| Wyciągnięto zawodnika z grupy | domenowe | usunięcie z listy grupy |
| Zgubiono dyskietkę z numerem startowym | domenowe | utrata karty RFID |
| Przejęto kaucję | domenowe | zabezpieczenie za zgubioną kartę |
| Odpięto numer startowy | domenowe | rozdzielenie numeru od karty |
| Przypisano numer startowy (nowy) | domenowe | ponowne przypisanie |
| Wydano nową dyskietkę | domenowe | fizyczne wydanie nowej karty RFID |

## Aktorzy
- Zawodnik
- Administrator
- Obsługa punktu kontrolnego

## Polityki / Zasady (różowe)
- Start indywidualny np. spóźnienie (hot spot — różowa karteczka)

## Granice kontekstów
- Start indywidualny
- Zgubienie dyskietki z numerem startowym (Frame 50)

## Hot Spoty / Problemy
- Hot spot: "start indywidualny np. spóźnienie" — przypadek spóźnienia jako główny trigger
- Kaucja za dyskietkę: mechanizm finansowy powiązany z kartą RFID

## Cross-check z opisem systemu
- RFID: POKRYTE (dyskietka = karta RFID; zgubienie karty obsługiwane)
- Przydział numeru startowego: POKRYTE (ponowne przypisanie w scenariuszu zgubienia)
- Generowanie grup: POKRYTE (pośrednio — wyciągnięcie z grupy = modyfikacja grupy)
- Wyniki i dyplomy: NIEOBECNE — obsługiwane w osobnych plikach
