# Proces: Pomiar czasu — BC-07

## Główny flow: Pomiar czasu dystansu standardowego

**Wyzwalacz:** Zawodnik przykłada dyskietkę do czytnika na punkcie kontrolnym / mecie
**Rezultat:** Zarejestrowano czas przejazdu → aktualizacja wyników w BC-08

**Kroki:**
1. Zawodnik przykłada dyskietkę do czytnika → Zarejestrowano pomiar czasu
2. System identyfikuje zawodnika po numerze na dyskietce
3. System zapisuje czas
4. Gdy zawodnik przekracza metę → Zawodnik zakończył przejazd (dystans standardowy)
5. System podlicza czasy przejazdu → Podliczono czasy przejazdu
6. System powiadamia BC-08 → Zaktualizuj wyniki na żywo

**Warunek zakończenia dystansu:**
- System wie że wszyscy dojechali LUB upłynął czas → Zakończono przejazdy na danym dystansie

**Warunek zakończenia zawodów:**
- Zakończono przejazdy na wszystkich dystansach → Zakończono przejazdy wszystkich dystansów

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-020 | Limity czasu na dany dystans zdefiniowane w regulaminie (BC-01) | 4 |
| RB-021 | Dyskietka z numerem startowym jest jedynym identyfikatorem zawodnika na trasie | 1–2 |
| RB-022 | Dezaktywowana dyskietka (skradziona/zgubiona i wymieniona) nie jest akceptowana przez czytnik | 1 |

---

## Flow alternatywny: Dystans wydłużony

**Wyzwalacz:** Zawodnik zgłasza chęć przejechania kolejnego dystansu przed zakończeniem swojego
**Rezultat:** Przejazd zawodnika przedłużony, dodatkowy pomiar zarejestrowany

**Kroki:**
1. Zawodnik zgłasza prośbę o przedłużenie → Zgłoszono prośbę o przedłużenie dystansu
2. Obsługa punktu kontrolnego potwierdza → Przedłużono przejazd zawodnika
3. Zawodnik kontynuuje jazdę
4. System rejestruje dodatkowy pomiar → Odnotowano dodatkowy pomiar czasu
5. Zawodnik kończy → Zawodnik zakończył przejazd (dystans wydłużony)

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-023 | Przedłużenie musi zostać zgłoszone przed zakończeniem dystansu standardowego | 1 |
| RB-024 | Reguły przedłużenia określone w regulaminie maratonu (BC-01) | 2 |
