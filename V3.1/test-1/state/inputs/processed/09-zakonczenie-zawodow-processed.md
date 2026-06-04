<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture -->
<!-- used-in: big-picture, process-level -->
<!-- converted: lossless -->

# Zakończenie zawodów

## Źródła
- zakończenie_2.png (Frame 46)
- zakończenie_zawodów.png (Frame 46)

---

## Zdarzenia domenowe

| Zdarzenie | Aktor | Obszar |
|---|---|---|
| Przyznano nagrodę | główny organizator zawodów | Zakończenie |
| Zwrócono pakiet startowy | zawodnik / osoba upoważniona | Zakończenie |
| Kaucja została zwrócona | obsługa punktu kontrolnego | Zakończenie |
| Kaucja została zatrzymana | system / obsługa | Zakończenie |
| Otrzymano dyplom | zawodnik | Zakończenie |
| Nie zwrócono pakietu startowego | system | Zakończenie |

## Trigger

- Zdarzenie wejściowe: **zakończono przejazdy wszystkich dystansów** (z kontekstu Pomiar czasu)

## Sub-procesy

### Nagrody
- Główny organizator zawodów + reguły regulamin maratonu → przyznano nagrodę

### Zwrot pakietu startowego
- Zawodnik lub osoba upoważniona zwraca pakiet
- Jeśli upłynął czas zwrotu → nie zwrócono pakietu → kaucja zatrzymana
- Jeśli zwrócono w terminie → kaucja zwrócona

### Dyplom
- Zawodnik otrzymuje dyplom

## Aktorzy

| Nazwa | Rola | Zewnętrzny? |
|---|---|---|
| główny organizator zawodów | Przyznaje nagrody | Nie |
| zawodnik | Zwraca pakiet, odbiera dyplom i nagrodę | Nie |
| osoba upoważniona przez zawodnika | Może zwrócić pakiet w imieniu zawodnika | Nie |
| obsługa punktu kontrolnego | Przyjmuje zwrot pakietu | Nie |

## Reguły biznesowe

- Czas do zwrotu pakietu ustalony w regulaminie (różowy sticky)
- Po upływie czasu → automatyczne zatrzymanie kaucji
- Nagrody wg regulaminu maratonu

## Granice kontekstu

- **Zakończenie zawodów**: finalny kontekst, wyzwalany przez koniec pomiaru czasu
- Powiązany z: Pomiar czasu, Przygotowanie do startu (kaucja z dyskietki)

## Hot Spoty / Otwarte pytania

- Jak są wyznaczani zwycięzcy (ranking)?
- Czy dyplomy są generowane automatycznie czy ręcznie?
- Co wchodzi w skład "pakietu startowego"?
