# P-009 — Pomiar Czasu i Wyniki — BC-009

**Wyzwalacz:** PE-003 (E-073 Wystartowano zawodników) → aktywacja pomiaru
**Rezultat:** PE-004 (E-090 Zakończono przejazdy wszystkich dystansów) → wyzwala BC-010

---

## Część 1 — Rejestracja pomiarów RFID (cykl ciągły)

1. [A-001 Zawodnik] --[Przyłóż dyskietkę do czytnika RFID]--> [I-002 System RFID] ==> [E-078 Przyłożenie dyskietki do czytnika]
2. [A-008 System RFID → ACL → BC-009] --[Zarejestruj pomiar]--> [BC-009] ==> [E-079 Zarejestrowano pomiar czasu]
3. [A-009 System] --[Wyświetl wyniki real-time]--> [BC-009 → Read Model] ==> [E-080 Wyświetlono wyniki w czasie rzeczywistym]

**Cykl:** Kroki 1-3 powtarzają się na każdym checkpoincie (co 50 km — jeden mniej niż pętli).
**Real-time display:** ta sama baza, osobny widok (potwierdzone przez orkiestratora — nie osobny BC).

---

## Część 2 — Logika klasyfikacji zawodnika

4. [A-009 System] --[Sprawdź czy zawodnik ukończył swój dystans]--> [BC-009] ==> [E-081 Zawodnik zakończył przejazd]

**Reguły klasyfikacji (fakt domenowy #7):**
- Trigger = upłynięcie 24h (E-088 Upłynął czas maratonu)
- Zawodnik który ukończył opłacony dystans w czasie 24h → klasyfikowany na tym dystansie
- Zawodnik który ukończył opłacony dystans i próbował dłuższego lecz nie zmieścił się → **klasyfikowany na opłaconym dystansie**
- Zawodnik który nie ukończył opłaconego dystansu w czasie 24h → **nieklasyfikowany**
- Zawodnicy na trasie po 24h → "jadą dla siebie" (nieoficjalnie, nie wliczani do klasyfikacji)

---

## Część 3 — Ścieżka przedłużenia dystansu

A1. [A-001 Zawodnik] --[Zgłoś prośbę o przedłużenie dystansu]--> [BC-009] ==> [E-082 Zgłoszono prośbę o przedłużenie dystansu]
A2. [A-004/A-003] --[Przedłuż przejazd zawodnika]--> [BC-009] ==> [E-083 Przedłużono przejazd zawodnika]
A3. [A-001 Zawodnik → A-008 RFID] --[Pomiar na kolejnym checkpoincie]--> [BC-009] ==> [E-084 Odnotowano dodatkowy pomiar czasu]
A4. [A-009 System] --[sprawdź ukończenie wydłużonego dystansu]--> [BC-009] ==> [E-085 Zawodnik zakończył przejazd (wydłużony)]

**UWAGA:** "Przedłużenie" czy to zmiana opłaconego dystansu (wymaga dopłaty w BC-003) czy inny mechanizm? → HS2-024. Jeśli wymaga dopłaty → cross-context: BC-009 → BC-003.

---

## Część 4 — Zamknięcie zawodów i wyniki

5. [Trigger czasowy E-088] --[Upłynął czas maratonu (24h)]--> [BC-009] ==> [E-088 Upłynął czas maratonu]
6. [A-009 System] --[Podlicz czasy przejazdu]--> [BC-009] ==> [E-086 Podliczono czasy przejazdu]
7. [A-009 System] --[Wygeneruj ranking wyników]--> [BC-009] ==> [E-089 Wygenerowano ranking wyników]
8. [A-009 System] --[per dystans: zakończ]--> [BC-009] ==> [E-087 Zakończono przejazd zawodników na danym dystansie]
9. [A-009 System] --[wszystkie dystanse: zakończ]--> [BC-009] ==> [E-090 Zakończono przejazdy wszystkich dystansów] → **PE-004**

---

## Read modele

| Krok | Aktor | Co musi widzieć |
|------|-------|----------------|
| 1-3 | Zawodnicy/Obserwatorzy | Real-time board: aktualna pozycja zawodnika, czasy na checkpointach, ranking live |
| 4 | System | Lista pomiarów zawodnika, opłacony dystans (z BC-003), czasy checkpointów |
| 6 | System | Wszystkie pomiary, lista klasyfikacyjna, reguły klasyfikacji (opłacony dystans) |
| 7 | System | Podliczone czasy → ranking per dystans |

---

## Reguły biznesowe

| ID     | Reguła | Egzekwowana w kroku |
|--------|--------|---------------------|
| RB-035 | Pomiar rejestrowany tylko dla zawodników z aktywną dyskietką (po PE-003) | 2 |
| RB-036 | Zawodnik klasyfikowany na opłaconym dystansie nawet jeśli próbował dłuższego (fakt #7) | 6-7 |
| RB-037 | Zawodnik nieklasyfikowany jeśli nie ukończył opłaconego dystansu w 24h (fakt #7) | 6-7 |
| RB-038 | Trigger zamknięcia = 24h (fakt #7) — nie "wszyscy dojechali" | 5 |
| RB-039 | Checkpointy co 50 km (o jeden mniej niż liczba pętli) | 2 (konfiguracja tras) |
| RB-040 | 1 pętla = 100 km, dystanse: 50-700 km (niestandardowe: 50, 150, 650 km) | 2 |

---

## Wyjątki i ścieżki alternatywne

| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-027 | Zawodnik nie ukończył opłaconego dystansu | Klasyfikacja jako "nieklasyfikowany" (E-087 z flagą) | Może kontynuować "dla siebie" |
| EX-028 | Zawodnik zakończył udział przedwcześnie (E-076 z BC-008) | Pomiar zatrzymany | Q-011: co z dyskietką? → HS2-005 |
| EX-029 | Dyskietka RFID nie reaguje na czytniku | Brak E-079 → ручна interwencja? | HS2-025: brak opisanego fallback |
| EX-030 | Zawodnik jeździ po 24h ("dla siebie") | E-079 rejestrowane ale nieuwzględniane w klasyfikacji | Pomiar może być ignorowany lub rejestrowany z flagą |

---

## Przepływ odwrotny

- PE-004 jest nieodwracalne
- Korekta wyników po PE-004 → BC-010 (publikacja wyników)
- Brak opisanego mechanizmu korekty wyników po publikacji → HS2-026

## Weryfikacja odwrotną narracją

Żeby PE-004 (E-090) → E-087 (per dystans) + E-089 (ranking). Żeby E-089 → E-086 (podliczone czasy). Żeby E-086 → E-088 (24h trigger) + wszystkie E-079 (pomiary). Żeby E-079 → E-078 (przyłożenie RFID) + PE-003 (zawodnik aktywny). Żeby PE-003 → BC-008.

**Luki:**
1. Q-005 (ranking → dyplomy): przepływ BC-009→BC-010 przez E-089? Kto inicjuje dyplomy? → HS2-006
2. Q-011 (dyskietka przy zakończeniu udziału): BC-009 powinno "wiedzieć" że zawodnik wycofał się → HS2-005
3. Brak fallback dla błędu RFID (nieodczytana dyskietka) → HS2-025
4. Przedłużenie dystansu (ścieżka A) — czy wymaga dopłaty? → HS2-024
5. Ranking: per dystans czy globalny? Kryteria? → HS2-027 (otwarte)
