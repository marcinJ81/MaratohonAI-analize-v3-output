# Proces: Przygotowanie do startu — BC-05

## Główny flow: Kwalifikacja zawodnika

**Wyzwalacz:** Zawodnik przybywa do biura / punktu kontrolnego przed zawodami
**Rezultat:** Wydano pakiet startowy (dyskietka + numer) → zawodnik gotowy do startu

**Kroki:**
1. Obsługa punktu kontrolnego weryfikuje tożsamość → Zweryfikowano tożsamość zawodnika
   - [nie przeszedł] → Zawodnik nie przeszedł weryfikacji → Zdyskwalifikowano zawodnika
2. Zawodnik podpisuje oświadczenie → Podpisano oświadczenie zawodnika
   - [nie podpisał] → Zawodnik nie podpisał oświadczenia → Zdyskwalifikowano zawodnika
3. Obsługa wydaje pakiet startowy → Wydano pakiet startowy (zawiera dyskietkę z numerem startowym)

---

## Flow alternatywny: Weryfikacja przed startem (tuż przed biegiem)

**Wyzwalacz:** Zawodnik podchodzi do strefy startowej
**Rezultat:** Zawodnik dopuszczony do startu LUB zdyskwalifikowany

**Kroki:**
1. Obsługa sprawdza zawodnika na liście startowej → weryfikacja na podstawie listy startowej
2. [OK] → Zweryfikowano zawodnika przed startem
3. [nie ma na liście / brak kwalifikacji] → Zawodnik nie przeszedł weryfikacji → Zdyskwalifikowano zawodnika

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-015 | Lista startowa pochodzi z BC-04 (wygenerowane grupy) | 1 |
| RB-016 | Brak podpisu oświadczenia lub brak weryfikacji tożsamości → bezwzględna dyskwalifikacja | 1–2 |

---

## Flow alternatywny: Zgubienie dyskietki z numerem startowym

**Wyzwalacz:** Zawodnik zgłasza zgubienie dyskietki
**Rezultat:** Zawodnik otrzymuje nową dyskietkę i może startować

**Kroki:**
1. Zawodnik zgłasza zgubienie → Zgubiono dyskietkę z numerem startowym
2. Obsługa przejmuje kaucję → Przejęto kaucję za dyskietkę
3. Odpięto stary numer startowy (deaktywacja w systemie) → Odpięto numer startowy
4. Przypisano nowy numer startowy → Przypisano nowy numer startowy
5. Wydano nową dyskietkę → Wydano nową dyskietkę
