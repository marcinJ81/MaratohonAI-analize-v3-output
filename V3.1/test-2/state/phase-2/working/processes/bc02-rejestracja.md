# Proces: Rejestracja — BC-02

## Główny flow: Rejestracja uczestnika

**Wyzwalacz:** Uczestnik chce wziąć udział w maratonie
**Rezultat:** Uczestnik zarejestrowany z wybranym dystansem i wyliczoną opłatą

**Kroki:**
1. Uczestnik wypełnia dane i rejestruje się w systemie → Zarejestrowano uczestnika
2. Uczestnik wybiera dystans → Wybrano dystans
3. System weryfikuje dane kontaktowe → Potwierdzono dane kontaktowe
4. Obsługa punktu kontrolnego weryfikuje tożsamość (lub system automatycznie) → Zweryfikowano zawodnika
5. System wylicza opłatę → Wyliczono opłatę
6. System przekazuje informację do BC-03 (Opłata za udział)

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-003 | Rejestracja możliwa tylko w terminie otwarcia rejestracji (z BC-01) | 1 |
| RB-004 | Wybór dystansu musi być z listy dostępnych dystansów (z BC-01) | 2 |

---

## Flow alternatywny: Zmiana dystansu

**Wyzwalacz:** Uczestnik składa wniosek o zmianę dystansu
**Rezultat:** Dystans zmieniony lub odrzucony (jeśli nieopłacony)

**Kroki:**
1. Uczestnik składa wniosek → Złożono wniosek o zmianę dystansu
2. System sprawdza czy uczestnik opłacił nowy dystans (integracja z BC-03)
3a. [opłacono nowy dystans] → Zmieniono dystans, System wylicza różnicę opłaty → Poinformowano uczestnika
3b. [NIE opłacono nowego dystansu] → Uczestnik POZOSTAJE na dotychczasowym opłaconym dystansie (Q-001)

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-005 | W trakcie opłacania (sesja płatności otwarta) nie można zmienić dystansu | 1 |
| RB-006 | Zmiana dystansu bez opłaty → uczestnik pozostaje na opłaconym dystansie, nie blokuje innych procesów | 3b |
| RB-007 | Zmiana dystansu po wygenerowaniu grup → uczestnik nie jest przypisany do ostatecznej grupy; wymaga obsługi manualnej | 3a |
