# P-002 — Rejestracja Uczestnika — BC-002

**Wyzwalacz:** Uczestnik (A-002) — decyzja o zgłoszeniu do maratonu
**Rezultat:** E-013 Potwierdzono rejestrację — uczestnik zarejestrowany, otwarta droga do opłaty (BC-003)

---

## Kroki — Ścieżka główna

1. [A-002 Uczestnik] --[Zarejestruj uczestnika]--> [BC-002] ==> [E-004 Zarejestrowano uczestnika]
2. [A-002 Uczestnik] --[Wybierz dystans]--> [BC-002] ==> [E-005 Wybrano dystans]
3. [A-002 Uczestnik] --[Potwierdź dane kontaktowe]--> [BC-002] ==> [E-006 Potwierdzono dane kontaktowe]
4. [A-009 System] --[Zweryfikuj dane uczestnika]--> [BC-002] ==> [E-007 Zweryfikowano zawodnika]
5. [A-009 System] --[Wylicz opłatę]--> [BC-002] ==> [E-008 Wyliczono opłatę]
6. [A-009 System] --[Potwierdź rejestrację]--> [BC-002] ==> [E-013 Potwierdzono rejestrację] → **PE-006**

---

## Ścieżka A — Zmiana dystansu podczas rejestracji (przed potwierdzeniem)

A1. [A-002 Uczestnik] --[Złóż wniosek o zmianę dystansu]--> [BC-002] ==> [E-009 Złożono wniosek o zmianę dystansu]
A2. [A-003 Administrator / A-009 System] --[Zmień dystans]--> [BC-002] ==> [E-010 Zmieniono dystans]
A3. [A-009 System] --[Wylicz opłatę za nowy dystans]--> [BC-002] ==> [E-011 Wyliczono opłatę (zmiana dystansu)]
A4. [A-009 System] --[Poinformuj o zmianie opłaty]--> [BC-002] ==> [E-012 Poinformowano użytkownika o zmianie opłaty]
→ Powrót do kroku 6 (potwierdzenie)

**Pytanie Q-012:** Kto może złożyć wniosek — tylko uczestnik online czy też administrator w biurze?
Odpowiedź z materiałów: niejasna — seed pokazuje A-002, ale fizycznie A-003 może działać w biurze.
→ Zapisano jako Q-012 (nice-to-have) — bez wpływu na przepływ główny.

---

## Ścieżka B — Anulowanie rejestracji

B1. [A-009 System (trigger czasowy)] --[Wykryj przekroczenie terminu płatności]--> [BC-002] ==> [E-015 Przekroczono termin płatności]
B2. [A-009 System / A-003 Administrator] --[Anuluj rejestrację]--> [BC-002] ==> [E-014 Anulowano rejestrację]

**UWAGA (Q-004):** Czy przekroczenie terminu płatności powoduje automatyczne anulowanie (B1→B2 auto) czy wymaga decyzji administratora (B1 → notyfikacja → B2 ręcznie)?
→ NIEROZSTRZYGNIĘTE. Otwarte jako pytanie w handoffie. HS2-001.

---

## Read modele

| Krok | Aktor | Co musi widzieć |
|------|-------|----------------|
| 1-3 | Uczestnik | Formularz rejestracyjny, lista dostępnych dystansów, limity miejsc (z BC-001) |
| 4 | System | Dane uczestnika, lista wymaganych pól |
| 5 | System | Cennik dystansów (z BC-001), promocje dostępne (z BC-001 lub BC-003?) |
| A1 | Uczestnik/Administrator | Aktualnie wybrany dystans, limity na inne dystanse |

---

## Reguły biznesowe

| ID     | Reguła | Egzekwowana w kroku |
|--------|--------|---------------------|
| RB-004 | Rejestracja możliwa tylko dla otwartego okresu rejestracji (z BC-001) | 1 |
| RB-005 | Uczestnik może zarejestrować się na dokładnie jeden dystans | 2 |
| RB-006 | Zmiana dystansu możliwa tylko przed potwierdzeniem opłaty (w BC-002) | A1 |
| RB-007 | W trakcie opłacania nie można zmienić dystansu (reguła z seeda) | — blokuje wejście do ścieżki A gdy jest aktywna opłata w BC-003 |
| RB-008 | Termin płatności jest datą z konfiguracji BC-001 | B1 |

---

## Wyjątki i ścieżki alternatywne

| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-003 | Brak miejsc na wybranym dystansie | BC-002 odmawia E-005 | Uczestnik wybiera inny dystans lub rezygnuje |
| EX-004 | Dane kontaktowe nieprawidłowe | BC-002 odrzuca E-006 | Uczestnik poprawia dane |
| EX-005 | Weryfikacja negatywna (E-007 fail) | System wstrzymuje rejestrację | Brak E-013 — wymaga interwencji A-003 (HS2-001) |
| EX-006 | Przekroczono termin płatności | E-015 + ? (Q-004 open) | Anulowanie auto lub manualne — nierozstrzygnięte |

---

## Przepływ odwrotny

- Anulowanie rejestracji (E-014): usuwa uczestnika z systemu lub zmienia status. Czy E-004 jest cofalne? Prawdopodobnie tak przez A-003.
- Brak mechanizmu przywrócenia po anulowaniu — pełna nowa rejestracja.

## Weryfikacja odwrotną narracją

Żeby E-013 → musiały być E-007 i E-008. Żeby E-008 → E-005 (wybrany dystans + cennik z BC-001). Żeby E-005 → E-004 (zarejestrowany uczestnik). Żeby E-004 → decyzja uczestnika + otwarta rejestracja.

**Luki znalezione wstecz:**
1. Brak zdarzenia "Otwarto rejestrację" (które tworzy kontekst dla E-004) → może być w BC-001 jako parametr konfiguracyjny, nie osobne zdarzenie. Akceptowalne.
2. Brak wyraźnego zdarzenia "Wyliczono opłatę pierwszą" (E-008) jako triggera do BC-003 — relacja BC-002→BC-003 przez PE-006, a E-008 to informacja wewnętrzna BC-002. OK.
3. Mechanizm wygaśnięcia (E-015) nie ma opisanego triggera czasowego. → HS2-001.
