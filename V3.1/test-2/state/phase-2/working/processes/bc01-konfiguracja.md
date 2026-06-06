# Proces: Konfiguracja maratonu — BC-01

**Wyzwalacz:** Decyzja administratora o organizacji zawodów
**Rezultat:** Skonfigurowany maraton gotowy do przyjmowania rejestracji

**Kroki:**
1. Administrator rejestruje maraton w systemie (nazwa, data, miejsce)
2. Definiuje dostępne dystanse
3. Konfiguruje regulamin (reguły kar, reguły zmiany grupy, limity dystansów)
4. Ustawia terminy: rejestracji, generacji grup, zmiany grupy, dołączenia po terminie
5. Konfiguruje minimalne liczby uczestników per dystans
6. System potwierdza: Zarejestrowano maraton / Skonfigurowano regulamin

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-001 | Regulamin maratonu obowiązuje wszystkie inne BC jako polityka nadrzędna | 3 |
| RB-002 | Terminy muszą być ustawione przed otwarciem rejestracji | 4 |

**Wyjątki:**
| Warunek | Akcja | Skutek |
|---|---|---|
| Brak minimalnej liczby uczestników na dystansie | System blokuje generację grup dla tego dystansu | Nie udało się wygenerować grup |
