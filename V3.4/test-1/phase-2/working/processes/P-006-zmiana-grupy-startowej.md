# P-006 — Zmiana Grupy Startowej — BC-006

**Wyzwalacz:** A-001 Zawodnik (lub A-003 Administrator w imieniu zawodnika — Q-012)
           Po PE-001, przed PE-002 (okno czasowe)
**Rezultat:** E-043 Zmieniono grupę LUB E-044 Nie udało się zmienić grupy

---

## Kroki — Ścieżka sukcesu

1. [A-001 Zawodnik] --[Złóż wniosek o zmianę grupy]--> [BC-006] ==> [E-037 Złożono wniosek o zmianę grupy]
2. [A-009 System] --[Weryfikuj: nie złożono wcześniej wniosku]--> [BC-006] ==> [E-038 Nie złożono wcześniej wniosku]
3. [A-003 Administrator] --[Przyjmij wniosek do zmiany grupy]--> [BC-006] ==> [E-039 Przyjęto wniosek do zmiany grupy]
4. [A-009 System] --[Weryfikuj wszystkie warunki]--> [BC-006] ==> [warunki spełnione]
5. [A-009 System] --[Wykonaj zmianę grupy]--> [BC-006] ==> [E-041 Grupa została zmieniona]
6. [A-009 System] --[Potwierdź zmianę]--> [BC-006] ==> [E-043 Zmieniono grupę]

---

## Kroki — Ścieżka odrzucenia (warunek nie spełniony)

3b. [A-003 Administrator / A-009 System] --[Nie przyjmij wniosku]--> [BC-006] ==> [E-040 Nie przyjęto wniosku]
4b. [A-009 System] --[Odmów zmiany]--> [BC-006] ==> [E-042 Grupa nie została zmieniona]
5b. [A-009 System] --[Potwierdź brak zmiany]--> [BC-006] ==> [E-044 Nie udało się zmienić grupy]

**Ścieżka specjalna — brak opłaty:**
3c. [A-009 System] --[Wykryj brak opłaty zmiany grupy]--> [BC-006] ==> [E-045 Odmówiono zmiany grupy z powodu braku opłaty]

---

## Ścieżka B — Dołączenie do grupy po terminie generacji (ale przed PE-002)

B1. [PE-005 → polityka] --[Opłacono dystans (po terminie generacji)]--> [BC-006] ==> [trigger dołączenia]
B2. [A-009 System] --[Dołącz do grupy]--> [BC-006] ==> [E-049 Dołączono do grupy]
B3. [A-009 System] --[Nadobiegnij do grupy]--> [BC-006] ==> [E-050 Nadobiegnięto do grupy]

**Warunek blokujący:** Jeśli PE-002 minęło → brak możliwości dołączenia.

---

## Ścieżka C — Zmiana dystansu (po terminie — specjalna)

C1. [A-001 Zawodnik] --[Złóż wniosek o zmianę dystansu po terminie]--> [BC-006] ==> [E-048 Złożono wniosek o zmianę dystansu (po terminie)]
→ Prawdopodobnie skutkuje EX-014 (odmowa) — materiały niespójne → HS2-018

---

## Ścieżka D — Zmiana grupy z opłatą różnicy

D1. [A-002 Uczestnik] --[Przekaż opłatę za zmianę grupy]--> [BC-006 → BC-003] ==> [E-051 Przekazano opłatę za zmianę grupy]
D2. → Powrót do ścieżki sukcesu krok 4

**Reguła domenowa:** Brak zapłaty różnicy przy zmianie grupy = brak zmiany (RB-022). Czy "różnica" to różnica kosztów między grupami (godziny?), czy opłata administracyjna? → HS2-019.

---

## Read modele

| Krok | Aktor | Co musi widzieć |
|------|-------|----------------|
| 1 | Zawodnik | Lista dostępnych grup w tym samym dystansie, wolne miejsca, godziny startów |
| 2 | System | Historia wniosków zawodnika (czy zmieniał wcześniej) |
| 3 | Administrator | Wniosek zawodnika, stan grupy docelowej (miejsca) |
| 4 | System | Termin (z BC-001: czy okno otwarte), miejsca w grupie docelowej, status opłat (z BC-003) |

---

## Reguły biznesowe

| ID     | Reguła | Egzekwowana w kroku |
|--------|--------|---------------------|
| RB-022 | Brak zapłaty różnicy przy zmianie grupy = brak zmiany | 4 / ścieżka D |
| RB-023 | Zmiana grupy możliwa tylko w oknie czasowym (PE-001 do PE-002) | 4 |
| RB-024 | Zmiana grupy jednorazowa — zawodnik nie może zmienić grupy więcej niż raz | 2 |
| RB-025 | Zmiana grupy możliwa tylko w obrębie tego samego dystansu | 4 |
| RB-026 | Grupy startowe muszą istnieć (PE-001 musi być osiągnięte) | 4 |
| RB-027 | Grupa docelowa musi mieć wolne miejsca | 4 |

---

## Wyjątki i ścieżki alternatywne

| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-014 | PE-002 minęło (po terminie) | Odmowa automatyczna | E-044 / E-048 |
| EX-015 | Zawodnik zmieniał wcześniej grupę | Odmowa (RB-024) | E-040, E-044 |
| EX-016 | Brak wolnych miejsc w grupie docelowej | Odmowa | E-044 |
| EX-017 | Zmiana na inny dystans (naruszenie RB-025) | Odmowa | E-044 / E-040 |
| EX-018 | Brak opłaty zmiany grupy | E-045 | Odmowa do momentu opłacenia |

---

## Przepływ odwrotny

- Cofnięcie zmiany grupy: nie opisane. Jednorazowość (RB-024) sugeruje brak mechanizmu cofnięcia.
- Brak E-043 cofania się do E-037 stanu.

## Weryfikacja odwrotną narracją

Żeby E-043 (Zmieniono grupę) → E-041 (Grupa zmieniona). Żeby E-041 → E-039 (Przyjęto wniosek) + warunki spełnione. Żeby E-039 → E-037 (Wniosek złożony) + E-038 (nigdy nie zmieniał). Żeby E-037 → PE-001 (grupy istnieją) + okno otwarte (przed PE-002).

**Luki:**
1. Czy opłata za zmianę grupy (E-051) idzie przez BC-003 czy to wewnętrzna transakcja BC-006? → HS2-019
2. Rola A-003 w kroku 3 — czy administrator MUSI zaakceptować wniosek ręcznie czy to automatyczne? → HS2-020
3. Jak "dołączenie po terminie" (ścieżka B) różni się od normalnej zmiany grupy? → HS2-018
