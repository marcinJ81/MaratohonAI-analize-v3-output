# Proces: Start zawodników — BC-06

## Główny flow: Start grupowy

**Wyzwalacz:** Sędzia trasy w ustalonym momencie startu grupy
**Rezultat:** Grupa wystartowana, czas startu zarejestrowany → BC-07 rozpoczyna pomiar

**Kroki:**
1. Administrator zamyka zapisy na dany dystans → Zamknięto zapisy na dany dystans
2. Obsługa punktu kontrolnego przeprowadza weryfikację przed startem (BC-05)
3. Sędzia trasy startuje grupę → Wystartowano grupę
4. System rejestruje czas startu grupy → Zarejestrowano czas startu grupy
5. Sędzia trasy potwierdza start każdego zawodnika w grupie → Wystartowano zawodnika
   - [zawodnik dyskwalifikowany] → Zdyskwalifikowano zawodnika (przy starcie)
   - [zawodnik rezygnuje] → Zakończono udział przedwcześnie

---

## Flow alternatywny: Start indywidualny

**Wyzwalacz:** Zawodnik zgłasza start poza grupą (np. spóźnienie)
**Rezultat:** Zawodnik wyciągnięty z grupy, zarejestrowany OSOBNY czas startu

**Kroki:**
1. Zawodnik zgłasza indywidualny start → Zgłoszono indywidualny start
2. Administrator wyciąga zawodnika z przypisanej grupy → Wyciągnięto zawodnika z grupy
3. Sędzia trasy rejestruje INDYWIDUALNY czas startu → Zarejestrowano czas startu (indywidualny)
4. Zawodnik startuje

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-017 | Czas startu grupy jest czasem bazowym dla rankingu (Q-005) dla zawodników startujących grupowo | 4 |
| RB-018 | Zawodnik z indywidualnym startem ma WŁASNY czas startu jako bazę do rankingu (Q-002) | 3 |
| RB-019 | Zamknięcie zapisów musi nastąpić przed startem grupy | 1 |
