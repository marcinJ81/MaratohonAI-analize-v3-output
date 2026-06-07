# Core Domain

## Propozycja agenta: Zarządzanie wyścigiem

### Kandydaci Core
- **BC-04 Grupy startowe** — unikalna, złożona logika przydziału z wieloma regułami (okno czasowe, limit miejsc, ten sam dystans, historia zmian, automatyczna generacja). Nie istnieje jako gotowe rozwiązanie.
- **BC-07 Pomiar czasu i wyniki** — serce rywalizacji; rejestracja przejazdów przez punkty kontrolne, linia mety, wydłużenia, generowanie rankingu per dystans.

### Uzasadnienie Core vs Supporting
BC-04 i BC-07 razem stanowią to, czego nie można zastąpić gotowym softem — logika specyficzna dla 24h maratonu rowerowego:
- grupy startowe uwzględniają okno 24h, zmiany dystansu w locie, ręczne i automatyczne przypisania
- pomiar czasu obsługuje wydłużenia, punkty pośrednie i finalny czas ukończenia

BC-03 (Opłaty) jest Generic — możliwy do outsourcingu (Stripe, PayU).
BC-01 (Konfiguracja) jest Supporting — wzorzec admin panel.

### Do potwierdzenia przez użytkownika
Core Domain: **Grupy startowe + Pomiar czasu i wyniki**
