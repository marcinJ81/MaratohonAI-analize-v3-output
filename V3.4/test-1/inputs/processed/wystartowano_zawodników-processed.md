---
source: wystartowano_zawodników.png
converted: lossy
used-in: BP, PL
requires-verification: true
---
# Zamknięcie zapisów + Start zawodników

## Opis wizualny
Dwie sekcje w jednym pliku (Frame 47): (1) Zamknięcie zapisów — zamknięcie listy startowej po starcie, (2) Start zawodników — rejestracja momentu startu. Widoczne zdarzenie wejściowe "Wystartowano zawodników" (pomarańczowe) na górze.

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Wystartowano zawodników | domenowe | zdarzenie wejściowe / wynik główny |
| Wystartowano zawodników na danym dystansie | domenowe | sekcja Zamknięcie zapisów |
| Zamknięto zapisy na dany dystans | domenowe | wynik zamknięcia |
| Wystartowano grupę | domenowe | sekcja Start zawodników |
| Zarejestrowano czas startu | domenowe | rejestracja momentu startu |
| Wystartowano zawodnika | domenowe | start indywidualny w ramach grupy |
| Zdyskwalifikowano zawodnika | domenowe | wynik negatywny startu |
| Zakończono udział przedwcześnie | domenowe | wynik negatywny startu |

## Aktorzy
- Administrator
- Sędzia trasy

## Granice kontekstów
- Zamknięcie zapisów
- Start zawodników

## Hot Spoty / Problemy
- Trzy możliwe wyniki startu (zdyskwalifikowano / zakończono przedwcześnie / start normalny)
- Powiązanie "zamknięcie zapisów" ze startem — logika czasowa

## Cross-check z opisem systemu
- Rejestracja czasu przejazdu: POKRYTE (rejestracja czasu startu)
- Generowanie grup: POKRYTE (pośrednio — wystartowanie grupy)
- Wyniki i dyplomy: NIEOBECNE — w osobnych plikach
- Wyświetlanie w czasie rzeczywistym: NIEOBECNE bezpośrednio — prawdopodobna strata konwersji
