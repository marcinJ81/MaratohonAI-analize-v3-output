# Seed: Phase 1 — Big Picture
<!-- generated-by: orchestrator, session: maraton-rowerowy-24h -->
<!-- coverage: 78% -->
<!-- sources: 01-konfiguracja-rejestracja-processed.md, 02-oplata-za-udzial-processed.md, 03-generacja-grup-startowych-processed.md, 04-zmiana-grupy-startowej-processed.md, 05-przygotowanie-start-processed.md, 06-start-zawodow-processed.md, 07-pomiar-czasu-processed.md, 08-kary-processed.md, 09-zakonczenie-zawodow-processed.md -->

## Kontekst sesji

**System**: 24-godzinny maraton rowerowy  
**Cel analizy**: Zrozumienie domeny  
**Tryb**: Agentowy (Tryb 1)  
**Złożoność**: średnia  

---

## Wstępnie zidentyfikowane konteksty (z Event Storming)

1. Konfiguracja maratonu
2. Rejestracja
3. Opłata za udział
4. Generacja grup startowych
5. Zmiana grupy startowej
6. Przygotowanie zawodnika do startu
7. Start zawodów
8. Pomiar czasu
9. Nakładanie kar
10. Zakończenie zawodów

---

## Wstępnie zidentyfikowani aktorzy

- administrator
- uczestnik / zawodnik
- obsługa punktu kontrolnego
- sędzia trasy
- główny organizator zawodów
- pracownik obsługi
- interfejs banku (zewnętrzny)
- system (automatyczny)

---

## Kluczowe zdarzenia domenowe (wybrane, ~70 łącznie)

- Zarejestrowano maraton
- Zarejestrowano uczestnika / Wybrano dystans / Wyliczono opłatę
- Opłacono uczestnictwo / Opłacono niedopłatę / Zwrócono nadpłatę
- Wygenerowano grupy startowe / Zmieniono grupę
- Wydano pakiet startowy / Zweryfikowano zawodnika
- Wystartowano grupę / Zarejestrowano czas startu
- Zarejestrowano pomiar czasu / Zakończono przejazd
- Nałożono karę / Naliczono karę
- Przyznano nagrodę / Otrzymano dyplom / Zwrócono pakiet startowy

---

## Hot Spoty z materiałów

1. **Reguły konfiguracji maratonu** — niezdefiniowane szczegółowo
2. **Co zrobić gdy generowanie list startuje a niedopłata nierozliczona?**
3. **Kary** — słabo opisany kontekst (tylko 2 zdarzenia)
4. **Czas do zwrotu pakietu** — zdefiniowany w regulaminie (wartość nieznana)

---

## Integracje zewnętrzne

- **Interfejs banku** — obsługa przelewów
- **Sprzęt: czytniki dyskietek** — pomiar czasu

---

## Instrukcja dla fazy Big Picture

Korzystając z powyższych danych przeprowadź analizę Big Picture:
1. Zweryfikuj i uzupełnij listę aktorów
2. Zbuduj kompletną mapę zdarzeń domenowych
3. Wyznacz granice systemu (co w środku, co na zewnątrz)
4. Zidentyfikuj i nazwij integracje zewnętrzne
5. Wylistuj i skategoryzuj hot spoty
6. Zapisz wyniki do `state/phase-1-output.md`
