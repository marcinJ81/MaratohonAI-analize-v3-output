# Granice systemu — working file

## Co jest w systemie
- Konfiguracja maratonu
- Rejestracja uczestników
- Obsługa opłat (kalkulacja, promocje, niedopłaty, zwroty)
- Generacja i zarządzanie grupami startowymi
- Przygotowanie zawodników do startu (weryfikacja, pakiety)
- Procedura startu (grupowy, indywidualny)
- Pomiar czasu (rejestracja przejazdów)
- Nakładanie kar
- Zakończenie zawodów (nagrody, pakiety, dyplomy)

## Co jest poza systemem (do potwierdzenia)
- Rzeczywisty przelew bankowy (obsługuje bank zewnętrzny)
- Fizyczne urządzenie pomiaru czasu (hardware)

## Wyniki / Ranking — uzupełnione
- Ranking na żywo (w trakcie zawodów) + wyniki końcowe (po zakończeniu)
- Podstawa: przejechany i zadeklarowany czas
- Kara = przeniesienie na koniec listy LUB wykluczenie z wyników (nie czas dodatkowy)
- Podział rankingu: per dystans × płeć (kobiety / mężczyźni) — brak innych kategorii
- Publikacja: tablica na miejscu zawodów + strona WWW (publiczny dostęp)

## Integracje zewnętrzne
| ID | System zewnętrzny | Kierunek | Typ |
|---|---|---|---|
| I-001 | Interfejs banku | wchodzi → system | async (przelew rejestrowany przez obsługę lub automatycznie) |
| I-002 | Strona WWW zawodów | system → wychodzi | publikacja wyników na żywo i końcowych |
| I-003 | Tablica wyników na miejscu | system → wychodzi | wyświetlanie wyników na żywo |

## Sprzęt wewnętrzny (boundary sprzętowa)
| ID | Urządzenie | Rola |
|---|---|---|
| HW-001 | Dyskietka z numerem startowym | Identyfikator zawodnika na trasie |
| HW-002 | Czytnik dyskietek | Wyzwalacz pomiaru czasu na punktach kontrolnych / mecie |
