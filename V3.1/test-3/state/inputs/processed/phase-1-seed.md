# Seed: Phase 1 — Big Picture
<!-- generated-by: orchestrator, session: maraton-rowerowy-24h -->
<!-- coverage: 72% -->
<!-- sources: state/inputs/processed/es-processed.md -->

## Kontekst sesji

System: zarządzanie 24-godzinnym maratonem rowerowym
Cel analizy: uzupełnienie procesu
Tryb: Tryb 1 — Agentowy
Złożoność: średnia

## Skompilowane dane wejściowe z Event Stormingu

### Aktorzy (8 zidentyfikowanych)

| ID | Nazwa | Zewnętrzny? |
|---|---|---|
| A-01 | Uczestnik / Zawodnik | Nie |
| A-02 | Administrator | Nie |
| A-03 | Główny organizator zawodów | Nie |
| A-04 | Sędzia trasy | Nie |
| A-05 | Obsługa punktu kontrolnego | Nie |
| A-06 | Pracownik obsługi | Nie |
| A-07 | System pomiaru czasu (chip timing) | Tak |
| A-08 | Interfejs banku | Tak |

### Obszary / Bounded Contexts (9 zidentyfikowanych)

1. **Konfiguracja maratonu** — admin konfiguruje zasady, regulamin
2. **Rejestracja** — uczestnik rejestruje się, wybiera dystans, zmienia dystans
3. **Opłata za udział** — płatności, bank, zwroty, niedopłaty, promocje
4. **Grupy startowe** — generacja grup, zmiana grupy, przypisanie zawodników
5. **Przygotowanie do startu** — weryfikacja tożsamości, pakiet startowy, dyskietki
6. **Start zawodników** — start grupowy/indywidualny, zamknięcie zapisów
7. **Pomiar czasu** — chip timing, zakończenie przejazdu, wydłużenie dystansu
8. **Kary** — kary za złamanie regulaminu, pominięte punkty kontrolne *(słabo opisane)*
9. **Zakończenie zawodów** — nagrody, zwrot pakietów, dyplomy, kaucje

### Zdarzenia domenowe (~50 zdarzeń — patrz es-processed.md)

Kluczowe zdarzenia per obszar skrócone:

**Rejestracja:** Zarejestrowano uczestnika → Wybrano dystans → Wyliczono opłatę → Zmieniono dystans
**Opłaty:** Opłacono uczestnictwo | Opłacono niedopłatę | Zwrócono nadpłatę | Opłacono zmianę dystansu
**Grupy:** Wygenerowano grupy startowe | Zmieniono grupę | Przypisano zawodnika do grupy
**Start:** Wystartowano grupę | Zarejestrowano czas startu | Zgłoszono indywidualny start
**Pomiar czasu:** Zawodnik zakończył przejazd | Zakończono przejazd zawodników na danym dystansie | Zakończono przejazdy wszystkich dystansów
**Kary:** Nałożono karę | Naliczono karę za pominięte punkty kontrolne
**Zakończenie:** Przyznano nagrodę | Zwrócono pakiet startowy | Otrzymano dyplom

### Integracje zewnętrzne

1. **Bank / interfejs banku** — przelewy przychodzące (opłaty) i wychodzące (zwroty)
2. **System pomiaru czasu (chip timing)** — dyskietki/czytniki RFID, wyzwala zdarzenia pomiaru

### Hot Spoty z materiałów

- HS-01: Co z zawodnikami z niedopłatą gdy startuje generacja list?
- HS-02: Kontekst Kar bardzo słabo opisany
- HS-03: Brak przepływu generowania rankingów/wyników
- HS-04: Zmiana dystansu po wygenerowaniu grup — niejasna koordynacja

### Otwarte pytania do zbadania w Big Picture

- Q-001: Jak generowane są wyniki/rankingi?
- Q-002: Typy kar i ich wpływ na wynik
- Q-007: Jeden czy wiele dystansów per zawodnik?
- Q-008: Logika punktów kontrolnych
