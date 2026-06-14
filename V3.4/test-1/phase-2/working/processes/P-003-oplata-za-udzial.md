# P-003 — Opłata za Udział — BC-003

**Wyzwalacz:** PE-006 (Potwierdzono rejestrację) → uczestnik musi opłacić
**Rezultat:** PE-005 (E-019/E-028 Opłacono uczestnictwo/dystans) → otwiera BC-004

---

## Ścieżka 1 — Pełna opłata (standard)

1. [A-002 Uczestnik / A-003 Administrator] --[Rozpocznij opłacenie]--> [BC-003] ==> [E-016 Rozpoczęto opłacenie udziału]
2. [A-007 Bank (zewnętrzny)] --[przelew bankowy]--> [I-001 → ACL → BC-003] ==> [E-017 Zarejestrowano przelew]
3. [A-009 System] --[Zaaplikuj opłatę]--> [BC-003] ==> [E-018 Zaaplikowano opłatę]
4. [A-009 System] --[automatycznie]--> [BC-003] ==> [E-019 Opłacono uczestnictwo] → **PE-005**

---

## Ścieżka 2 — Promocja -100%

1. [A-002 Uczestnik / A-003 Administrator] --[Rozpocznij opłacenie]--> [BC-003] ==> [E-016 Rozpoczęto opłacenie udziału]
2. [A-004 Główny organizator / A-009 System] --[Zaaplikuj promocję]--> [BC-003] ==> [E-020 Zaaplikowano promocje (-100%)]
3. [A-009 System] --[automatycznie]--> [BC-003] ==> [E-019 Opłacono uczestnictwo] → **PE-005**

**Reguła:** Promocja -100% eliminuje potrzebę przelewu bankowego. Kto aktywuje promocję? A-004 (organizator) lub A-009 (system, jeśli reguła promocyjna jest w konfiguracji)?
→ HS2-008: niejasne czy promocja jest przypisana do uczestnika ręcznie czy automatycznie.

---

## Ścieżka 3 — Opłata z niedopłatą

3a. [A-009 System] --[Wykryj niedopłatę po przelwie]--> [BC-003] ==> [E-021 Wykryto niedopłatę]
3b. [A-002 Uczestnik / A-003 Administrator] --[Rozpocznij opłacenie niedopłaty]--> [BC-003] ==> [E-022 Rozpoczęto opłacenie niedopłaty]
3c. [A-007 Bank] --[przelew bankowy]--> [ACL → BC-003] ==> [E-017 Zarejestrowano przelew (niedopłata)]
3d. [A-009 System] --[automatycznie]--> [BC-003] ==> [E-023 Opłacono niedopłatę] → powrót do ścieżki 1 krok 4

**Reguła domenowa (fakt #6):** Uczestnik z nieopłaconą niedopłatą w momencie PE-001 → pozostaje na starym dystansie. Zmiana grupy po opłaceniu niedopłaty → BC-006 (jeśli okno nadal otwarte).

---

## Ścieżka 4 — Zmiana dystansu po opłaceniu (dopłata)

4a. [A-002 Uczestnik] --[Złóż wniosek o zmianę dystansu (po opłaceniu)]--> [BC-003] ==> [E-009 (przekierowane z BC-002)]

**UWAGA:** E-009 w Fazie 1 jest przypisane do BC-002 (Rejestracja). Jednak zmiana dystansu PO opłaceniu wymaga kalkulacji nowej kwoty i przelewu → logika należy do BC-003.
Czy to ten sam E-009 czy nowe zdarzenie? → HS2-009 (potencjalny duplikat pojęcia).

4b. [A-009 System] --[Wylicz opłatę za nowy dystans]--> [BC-003] ==> [E-024 Wyliczono opłatę za nowy dystans]
4c. [A-007 Bank] --[przelew]--> [ACL → BC-003] ==> [E-017]
4d. [A-009 System] --[automatycznie]--> [BC-003] ==> [E-025 Opłacono zmianę dystansu] → **PE-005** (dla nowego dystansu)

---

## Ścieżka 5 — Nadpłata i zwrot

5a. [A-009 System] --[Wykryj nadpłatę]--> [BC-003] ==> [E-026 Wyliczono kwotę zwrotu]
5b. [A-003 Administrator / A-009 System] --[Zwróć nadpłatę]--> [BC-003] ==> [E-027 Zwrócono nadpłatę]

**UWAGA:** Zwrot nadpłaty wymaga przelewu wychodzącego (I-001 w odwrotnym kierunku). Brak opisu mechanizmu w materiałach → HS2-010.

---

## Ścieżka 6 — Odpisanie uczestnictwa

6a. [A-004 Organizator / A-003 Administrator] --[Odpisz uczestnictwo]--> [BC-003] ==> [E-029 Odpisano uczestnictwo]

**UWAGA (Q-013):** "Odpisanie uczestnictwa" jest częściowo nieczytelne w materiałach (H-007 z Fazy 1). Czy to rezygnacja (inicjowana przez uczestnika) czy anulowanie przez organizatora (kara, niespełnienie warunków)? → HS2-011.

---

## Read modele

| Krok | Aktor | Co musi widzieć |
|------|-------|----------------|
| 1 | Uczestnik/Administrator | Kwota do zapłaty (z E-008), numer konta, termin płatności |
| 2 | Bank | Dane przelewu (uczestnik inicjuje ręcznie) |
| 3 | System | Zarejestrowane przelewy do dopasowania do uczestnika |
| 3a | System | Kwota należna vs kwota wpłacona |
| 5a | System | Kwota wpłacona vs kwota należna |

---

## Reguły biznesowe

| ID     | Reguła | Egzekwowana w kroku |
|--------|--------|---------------------|
| RB-009 | W trakcie opłacania nie można zmienić dystansu (blokada krzyżowa BC-002/BC-003) | 1 (blokuje zmiany w BC-002) |
| RB-010 | Przelew musi być powiązany z uczestnikiem (identyfikacja po tytule lub numerze) | 2 (I-001 ACL) |
| RB-011 | Uczestnik z nieopłaconą niedopłatą w momencie PE-001 → stary dystans | 3a (stan sprawdzany w BC-005) |
| RB-012 | Promocja -100% wyklucza przelew bankowy | ścieżka 2 |

---

## Wyjątki i ścieżki alternatywne

| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-007 | Przelew niedopasowany (błędny tytuł) | BC-003 czeka lub Administrator dopasowuje ręcznie | E-017 opóźnione — HS2-012 |
| EX-008 | Uczestnik nie opłaca w żadnym terminie | ? → Q-004 nierozstrzygnięte | HS2-001 |
| EX-009 | Zwrot nadpłaty niemożliwy (brak danych bankowych) | Administrator kontaktuje się z uczestnikiem | HS2-010 |

---

## Przepływ odwrotny

- Zwrot uczestnictwa (rezygnacja po opłaceniu): nie opisane w materiałach → HS2-013

## Weryfikacja odwrotną narracją

Żeby PE-005 (E-028 Opłacono dystans) → musiało być E-018 (Zaaplikowano opłatę). Żeby E-018 → E-017 (przelew bankowy) lub E-020 (promocja). Żeby E-017 → E-016 (inicjacja opłaty) + fizyczny przelew przez uczestnika. Żeby E-016 → PE-006 (potwierdzona rejestracja).

**Luki znalezione:**
1. Mechanizm dopasowania przelewu do uczestnika (tytuł przelewu?) — brak opisu → HS2-012
2. Przelew wychodzący (zwrot nadpłaty) — brak opisu mechanizmu → HS2-010
3. Brak zdarzenia "Termin płatności minął" jako triggera → E-015 w BC-002 ale konsekwencja w BC-003 (brak opłaty) → koordynacja BC-002/BC-003 niejasna
