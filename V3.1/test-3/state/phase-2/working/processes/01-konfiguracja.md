# Proces: Konfiguracja maratonu (BC-01)

## Wyzwalacz
Administrator tworzy nową edycję maratonu.

## Kroki
1. Administrator rejestruje maraton w systemie (E-001)
2. Ustawia parametry globalne (regulamin):
   - dostępne dystanse i ich ceny
   - ostateczny termin generowania grup
   - ostateczny termin zmiany grupy
   - ostateczny termin zmiany dystansu (przed generacją grup)
   - minimalną liczbę uczestników w grupie
   - czas zwrotu pakietu startowego
   - kary regulaminowe (typy naruszeń → dyskwalifikacja)
   - lokalizację punktów kontrolnych

## Reguły biznesowe
- RB-001: Regulamin jest read-only dla wszystkich BC poza BC-01 (propaguje polityki)
- RB-002: Terminy muszą być spójne: termin zmiany grupy ≤ termin generacji grup ≤ data zawodów
- RB-003: Minimalna liczba uczestników w grupie > 0

## Wyjątki
- Brak (konfiguracja jest prerogatywą administratora)

## Otwarte pytania
- Czy konfigurację można modyfikować po rozpoczęciu rejestracji?
- Czy istnieje wersjonowanie regulaminu?
