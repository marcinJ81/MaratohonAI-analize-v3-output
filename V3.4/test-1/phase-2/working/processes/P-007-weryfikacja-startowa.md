# P-007 — Weryfikacja i Kwalifikacja Startowa — BC-007

**Wyzwalacz:** A-001 Zawodnik lub A-005 Obsługa — check-in w biurze zawodów (po PE-002, przed PE-003)
**Rezultat:** E-059 Wydano pakiet startowy + E-060 Wydano dyskietkę RFID → zawodnik dopuszczony do startu

---

## Część 1 — Weryfikacja biurowa (check-in)

1. [A-005 Obsługa punktu kontrolnego] --[Zweryfikuj tożsamość zawodnika]--> [BC-007] ==> [E-055 Zweryfikowano tożsamość zawodnika]
2. [A-001 Zawodnik] --[Podpisz oświadczenie]--> [BC-007] ==> [E-057 Podpisano oświadczenie zawodnika]
3. [A-005 Obsługa] --[Wydaj pakiet startowy]--> [BC-007] ==> [E-059 Wydano pakiet startowy]
4. [A-005 Obsługa] --[Wydaj dyskietkę RFID]--> [BC-007] ==> [E-060 Wydano dyskietkę RFID zawodnikowi]

**UWAGA (Q-008):** Czy dyskietka RFID jest częścią pakietu startowego (krok 3 i 4 razem) czy osobnym aktem wydania? → HS2-003. Materiały sugerują dwa odrębne zdarzenia (E-059 i E-060), ale mogą to być części jednego procesu fizycznego.

---

## Ścieżka B — Weryfikacja negatywna

1b. [A-005 Obsługa] --[Weryfikuj tożsamość]--> [BC-007] ==> [E-056 Zawodnik nie przeszedł weryfikacji]
→ Zawodnik nie dopuszczony do wydania pakietu. Co dalej? → HS2-021 (brak opisanego przepływu odwoławczego)

2b. [A-001 Zawodnik] --[odmawia podpisania oświadczenia]--> [BC-007] ==> [E-058 Zawodnik nie podpisał oświadczenia]
→ Zawodnik nie dopuszczony. Brak pakietu startowego.

---

## Część 2 — Weryfikacja na starcie (fizyczna, bezpośrednio przed startem)

1. [A-005 Obsługa / A-006 Sędzia] --[Zweryfikuj zawodnika przed startem]--> [BC-007] ==> [E-061 Zweryfikowano zawodnika przed startem]
2b. [A-005 / A-006] --[Weryfikacja negatywna]--> [BC-007] ==> [E-062 Zawodnik nie przeszedł weryfikacji (przed startem)]

**Reguła:** Weryfikacja na starcie na podstawie listy startowej (zamrożonej po PE-002). Zawodnik musi przejść weryfikację biurową (E-059/E-060) żeby być na liście startowej.

**H-008 z Fazy 1 ROZSTRZYGNIĘTE:** Dwie weryfikacje to dwa osobne akty w tym samym BC-007:
- Weryfikacja biurowa (E-055) = identyfikacja + dokumenty + oświadczenie + wydanie pakietu (może być dni wcześniej)
- Weryfikacja na starcie (E-061) = fizyczna kontrola przed wyścimniem z linii startowej

---

## Read modele

| Krok | Aktor | Co musi widzieć |
|------|-------|----------------|
| 1 | Obsługa | Lista startowa z BC-006 (po PE-002), dane uczestnika, dystans, numer startowy |
| 2 | Obsługa/Zawodnik | Formularz oświadczenia |
| 3-4 | Obsługa | Lista wydanych/niewydanych pakietów i dyskietek |
| Część 2, krok 1 | Obsługa/Sędzia | Lista startowa (finalna), kto odebrał pakiet (E-059), kto ma dyskietkę (E-060) |

---

## Reguły biznesowe

| ID     | Reguła | Egzekwowana w kroku |
|--------|--------|---------------------|
| RB-028 | Weryfikacja na podstawie zamrożonej listy startowej (po PE-002) | 1 |
| RB-029 | Zawodnik bez podpisanego oświadczenia nie może otrzymać pakietu | 2 |
| RB-030 | Weryfikacja na starcie dopuszcza tylko zawodników po weryfikacji biurowej (E-059) | Część 2, krok 1 |

---

## Wyjątki i ścieżki alternatywne

| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-019 | Zawodnik nie przeszedł weryfikacji biurowej (E-056) | Brak pakietu, brak startu | HS2-021: brak mechanizmu odwoławczego |
| EX-020 | Zawodnik nie podpisał oświadczenia (E-058) | Brak pakietu, brak startu | Bezwarunkowe — nie ma ścieżki alternatywnej |
| EX-021 | Zawodnik bez weryfikacji biurowej pojawia się na starcie | Odmowa przez E-062 | Sędzia/Obsługa niedopuszcza |
| EX-022 | Zawodnik nie stawił się na check-in | Q-010 open → HS2-022: czy slot zwolniony? |

---

## Przepływ odwrotny

- "Cofnięcie" wydania pakietu: fizycznie niemożliwe lub wyjątkowe (zgubienie, błąd)
- Brak opisanego mechanizmu w materiałach

## Weryfikacja odwrotną narracją

Żeby zawodnik mógł startować (BC-008) → E-061 (zweryfikowany na starcie). Żeby E-061 → E-059 + E-060 (pakiet i dyskietka wydane). Żeby E-059 → E-057 (oświadczenie) + E-055 (tożsamość). Żeby weryfikacja → zawodnik na liście startowej (z BC-006 po PE-002).

**Luki:**
1. Brak opisu co dzieje się z zawodnikiem który "nie stawił się na check-in" (Q-010) → HS2-022
2. H-009 (dyskietka w pakiecie czy osobno) → HS2-003
3. Brak opisu mechanizmu odwoławczego dla E-056 → HS2-021
