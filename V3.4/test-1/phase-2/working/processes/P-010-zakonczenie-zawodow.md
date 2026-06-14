# P-010 — Zakończenie Zawodów i Ceremonial — BC-010

**Wyzwalacz:** PE-004 (E-090 Zakończono przejazdy wszystkich dystansów) → automatyczny trigger
**Rezultat:** Dyplomy wydane, nagrody przyznane, pakiety zwrócone, kaucje rozliczone

---

## Kroki — Ścieżka ceremonialna (główna)

1. [PE-004 → polityka] --[Opublikuj wyniki końcowe]--> [BC-010] ==> [E-091 Opublikowano wyniki końcowe]
2. [A-004 Główny organizator] --[Przyznaj nagrodę]--> [BC-010] ==> [E-092 Przyznano nagrodę]
3. [A-009 System] --[Wygeneruj dyplomy]--> [BC-010] ==> [E-093 Wygenerowano dyplom] (per zawodnik)
4. [A-001 Zawodnik / osoba uprawniona] --[Zwróć pakiet startowy]--> [BC-010] ==> [E-095 Zwrócono pakiet startowy]
5. [A-005 Obsługa] --[Zwróć kaucję]--> [BC-010] ==> [E-096 Kaucja została zwrócona]
6. [A-001 Zawodnik] --[Odbierz dyplom]--> [BC-010] ==> [E-094 Otrzymano dyplom]

---

## Ścieżka B — Brak zwrotu pakietu

B1. [Trigger czasowy] --[upłynął czas zwrotu]--> [BC-010] ==> [E-097 Upłynął czas zwrotu pakietu]
B2. [A-009 System] --[odnotuj brak zwrotu]--> [BC-010] ==> [E-098 Nie zwrócono pakietu startowego]
B3. [A-005 Obsługa / A-004 Organizator] --[Zatrzymaj kaucję]--> [BC-010] ==> [E-099 Kaucja zatrzymana]

---

## Przepływ dyplomów — Q-005 ROZSZERZONY

**Trigger generacji dyplomu:** PE-004 → E-089 (ranking) → automatyczny system

**Kto kwalifikuje do dyplomu:**
- Zawodnik który ukończył opłacony dystans → dyplom z wynikiem
- Zawodnik nieklasyfikowany → dyplom uczestnictwa (?) lub brak dyplomu — **HS2-006: niejasne**
- Zawodnik "dla siebie" (po 24h) → dyplom uczestnictwa (?) — **HS2-006**

**Format dyplomu:** generowany wewnętrznie przez system (potwierdzone). Treść/format nieznany → HS2-006.

**Sposób odbioru:** E-094 "Otrzymano dyplom" — fizycznie w biurze czy elektronicznie? → HS2-028.

---

## Read modele

| Krok | Aktor | Co musi widzieć |
|------|-------|----------------|
| 1 | Wszyscy | Wyniki końcowe (ranking z E-089) — publiczny |
| 2 | Organizator | Ranking top-N per dystans, kryteria nagradzania |
| 3 | System | Ranking z E-089, lista zawodników z wynikami |
| 4 | Obsługa | Lista wydanych pakietów (z BC-007: E-059/E-060), kto zwrócił |
| 5 | Obsługa | Lista kaucji do zwrotu (powiązane z E-059/E-060) |
| 6 | Zawodnik | Wygenerowany dyplom (E-093) |

---

## Reguły biznesowe

| ID     | Reguła | Egzekwowana w kroku |
|--------|--------|---------------------|
| RB-041 | Wyniki publikowane automatycznie po PE-004 + E-089 | 1 |
| RB-042 | Dyplom generowany automatycznie przez system (nie przez człowieka) | 3 |
| RB-043 | Kaucja zwracana tylko przy zwrocie pakietu startowego | 5 (warunek: E-095 przed E-096) |
| RB-044 | Kaucja zatrzymana jeśli pakiet nie zwrócony w terminie (E-097) | B3 |
| RB-045 | Zawodnik lub osoba uprawniona przez zawodnika może zwrócić pakiet | 4 |

---

## Wyjątki i ścieżki alternatywne

| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-031 | Zawodnik nieklasyfikowany | Dyplom uczestnictwa (?) lub brak — HS2-006 | Niejasne |
| EX-032 | Pakiet nie zwrócony w terminie | E-097 → E-099 (kaucja zatrzymana) | Finalne |
| EX-033 | Osoba uprawniona zwraca pakiet | Wymaga mechanizmu upoważnienia — H-015 z Fazy 1 | HS2-029: brak opisu |
| EX-034 | Korekta wyników po publikacji | Brak mechanizmu opisanego → HS2-026 | Wymaga interwencji administratora |

---

## Przepływ odwrotny

- E-099 (kaucja zatrzymana) jest ostateczna — brak cofnięcia
- E-091 (wyniki) opublikowane — korekta przez HS2-026 (nieopisana)

## Weryfikacja odwrotną narracją

Żeby E-094 (Otrzymano dyplom) → E-093 (Wygenerowano dyplom). Żeby E-093 → E-089 (Ranking) + PE-004. Żeby E-089 → E-086 + E-088 (z BC-009). Żeby PE-004 → BC-009 zakończyło pracę.

Żeby E-096 (Kaucja zwrócona) → E-095 (Zwrócono pakiet). Żeby E-095 → zawodnik ma pakiet (wydany w BC-007: E-059/E-060).

**Luki:**
1. Q-005 (dyplom) — kto kwalifikuje, format, sposób odbioru → HS2-006, HS2-028
2. H-015 (proxy przy zwrocie) → HS2-029
3. Korekta wyników → HS2-026
4. Kryteria nagradzania (per dystans? globalny?) → HS2-027
