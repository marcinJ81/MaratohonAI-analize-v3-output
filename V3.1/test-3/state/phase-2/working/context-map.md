# Mapa kontekstów (Context Map)

## Diagram zależności

```
BC-01 Konfiguracja
    │ upstream (polityki / regulamin)
    ▼
BC-02 Rejestracja ──────────────────► BC-03 Opłaty
    │ (uczestnik z dystansem)              │ (status opłaty)
    │                                      │
    └──────────────────────────────────────┼──► BC-04 Grupy startowe
                                           │         │ (lista startowa)
                                           │         ▼
                                           │    BC-05 Przygotowanie do startu
                                           │         │ (zawodnik gotowy)
                                           │         ▼
                                           │    BC-06 Start zawodników
                                           │         │ (czas startu)
                                           │         ▼
                                           │    BC-07 Pomiar czasu ──► BC-08 Kary
                                           │         │                       │
                                           │         └──────────────────────►┤
                                           │                                  ▼
                                           └─────────────────────────── BC-09 Zakończenie zawodów
```

## Szczegóły relacji

| Z BC | Do BC | Typ relacji | Co jest przekazywane | Kierunek |
|---|---|---|---|---|
| BC-01 | BC-02 | OHS (Open Host Service) | Regulamin: dostępne dystanse, ceny, terminy | upstream → downstream |
| BC-01 | BC-03 | OHS | Regulamin: ceny dystansów, zasady promocji | upstream → downstream |
| BC-01 | BC-04 | OHS | Regulamin: termin generacji grup, termin zmiany grup, min. liczba uczestników | upstream → downstream |
| BC-01 | BC-05 | OHS | Regulamin: kaucja za dyskietkę, zasady weryfikacji | upstream → downstream |
| BC-01 | BC-06 | OHS | Regulamin: zasady startu indywidualnego | upstream → downstream |
| BC-01 | BC-07 | OHS | Regulamin: limity dystansów, punkty kontrolne, zasady wydłużenia | upstream → downstream |
| BC-01 | BC-08 | OHS | Regulamin: typy naruszeń → dyskwalifikacja | upstream → downstream |
| BC-01 | BC-09 | OHS | Regulamin: czas zwrotu pakietu, zasady nagród | upstream → downstream |
| BC-02 | BC-03 | Customer/Supplier | Uczestnik z wybranym dystansem + wymagana opłata | upstream → downstream |
| BC-02 | BC-04 | Customer/Supplier | Uczestnik z dystansem i numerem startowym | upstream → downstream |
| BC-03 | BC-04 | Customer/Supplier | Status opłaty (czy uczestnik zapłacił różnicę przy zmianie dystansu) | upstream → downstream |
| BC-04 | BC-05 | Customer/Supplier | Lista startowa (grupy + przypisani zawodnicy) | upstream → downstream |
| BC-05 | BC-06 | Customer/Supplier | Zawodnik zweryfikowany i gotowy do startu | upstream → downstream |
| BC-06 | BC-07 | Customer/Supplier | Czas startu grupy / zawodnika | upstream → downstream |
| BC-07 | BC-08 | Conformist | Dane o przejazdach przez punkty kontrolne (detekcja pominięcia) | upstream → downstream |
| BC-07 | BC-09 | Customer/Supplier | Podliczone czasy przejazdu per zawodnik | upstream → downstream |
| BC-08 | BC-09 | Customer/Supplier | Lista zdyskwalifikowanych zawodników | upstream → downstream |

## Uwagi
- BC-01 (Konfiguracja) to globalny upstream — dostarcza polityki wszystkim BC
- BC-03 (Opłaty) jest Generic — kandydat do wymiany na gotowe rozwiązanie (PayU, Stripe)
- Brak zidentyfikowanych Anti-Corruption Layer — relacje wydają się bezpośrednie
- BC-07 ↔ BC-08 wymaga szczególnej uwagi: kto i jak inicjuje detekcję pominięcia punktu?
