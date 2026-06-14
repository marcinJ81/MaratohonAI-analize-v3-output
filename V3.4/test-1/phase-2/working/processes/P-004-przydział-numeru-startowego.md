# P-004 — Przydział Numeru Startowego — BC-004

**Wyzwalacz:** PE-005 (E-019/E-028 Opłacono uczestnictwo/dystans) — automatyczny
**Rezultat:** E-030 Przypisano numer startowy — uczestnik gotowy do generacji grup (BC-005)

---

## Kroki

1. [PE-005 → polityka automatyczna] --[Przypisz numer startowy]--> [BC-004] ==> [E-030 Przypisano numer startowy]

**UWAGA:** Czy przydział jest natychmiastowy po opłaceniu (synchronicznie z BC-003) czy opóźniony (asynchronicznie)?
→ Q-007 otwarte → HS2-014. Na potrzeby tego przepływu zakładamy automatyczny trigger z PE-005.

---

## Ścieżka B — Zastąpienie numeru (przy zgubieniu dyskietki — z BC-008)

B1. [A-003 Administrator] --[Odepnij numer startowy]--> [BC-004] ==> [E-067 Odpięto numer startowy]
B2. [A-005 Obsługa / A-003 Administrator] --[Przypisz nowy numer]--> [BC-004] ==> [E-068 Przypisano numer startowy (nowy)]

**Koordynacja z BC-008:** Zdarzenie E-065 (Zgubiono dyskietkę) inicjowane w BC-008 → trigger do B1 w BC-004. Przepływ odwrotny powiązany z BC-010 (kaucja).

---

## Read modele

| Krok | Aktor | Co musi widzieć |
|------|-------|----------------|
| 1 | System | Pula dostępnych numerów startowych (z konfiguracji BC-001), ID uczestnika, dystans |
| B1 | Administrator | Numer aktualnie przypisany do zawodnika |
| B2 | Obsługa/Administrator | Wolne numery w puli |

---

## Reguły biznesowe

| ID     | Reguła | Egzekwowana w kroku |
|--------|--------|---------------------|
| RB-013 | Numer startowy unikalny w obrębie jednej edycji maratonu | 1 |
| RB-014 | Numer startowy przypisywany tylko uczestnikowi z opłaconym dystansem | 1 (trigger: PE-005) |
| RB-015 | Zastąpienie numeru wymaga odczepienia starego przed przypisaniem nowego | B1 przed B2 |

---

## Wyjątki

| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-010 | Pula numerów wyczerpana | System blokuje E-030 | Administrator musi rozszerzyć pulę — brak opisanego mechanizmu → HS2-015 |

---

## Weryfikacja odwrotną narracją

Żeby BC-005 mogło działać → muszą być dostępne informacje o numerach startowych uczestników. Żeby E-030 → musiało być PE-005. Żeby PE-005 → E-019 lub E-028 (z BC-003). Ścieżka kompletna.

**Luka:** nie opisano jak BC-005 "wie" że numer jest przypisany — czy BC-004 emituje zdarzenie do BC-005, czy BC-005 odpytuje BC-004? → Q-007 (synchronicznie/asynchronicznie) → HS2-014.
