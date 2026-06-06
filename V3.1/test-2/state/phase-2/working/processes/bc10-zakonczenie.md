# Proces: Zakończenie zawodów — BC-10

## Główny flow: Zakończenie i nagrody

**Wyzwalacz:** Zakończono przejazdy wszystkich dystansów (z BC-07)
**Rezultat:** Nagrody przyznane, pakiety zwrócone, dyplomy wydane

**Kroki:**
1. Główny organizator przyznaje nagrody per kategoria rankingowa → Przyznano nagrodę
2. Zawodnik zwraca pakiet startowy (dyskietka, numer) → Zwrócono pakiet startowy
3. Obsługa punktu kontrolnego przyjmuje pakiet → Kaucja została zwrócona
4. Zawodnik otrzymuje dyplom → Otrzymano dyplom

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-032 | Zwrot pakietu musi nastąpić w czasie określonym w regulaminie | 2 |
| RB-033 | Brak zwrotu pakietu w terminie → kaucja zatrzymana | — |
| RB-034 | Pakiet może zwrócić zawodnik LUB osoba upoważniona przez zawodnika | 2 |

---

## Flow alternatywny: Brak zwrotu pakietu startowego

**Wyzwalacz:** Upłynął czas zwrotu pakietu (timer z harmonogramu)
**Rezultat:** Kaucja zatrzymana

**Kroki:**
1. Timer wyzwala → Upłynął czas zwrotu pakietu
2. Obsługa sprawdza: pakiet nie zwrócony → Nie zwrócono pakietu startowego
3. Kaucja zostaje zatrzymana → Kaucja została zatrzymana
