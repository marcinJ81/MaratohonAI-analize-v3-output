# P-005 — Generacja Grup Startowych — BC-005 (CORE DOMAIN)

**Wyzwalacz:** A-003 Administrator — inicjacja w terminie określonym w regulaminie (BC-001)
**Rezultat:** PE-001 (E-036 Wygenerowano ostateczne grupy startowe) → otwiera BC-006

---

## Kroki — Ścieżka sukcesu

1. [A-003 Administrator] --[Generuj grupy startowe]--> [BC-005] ==> [walidacja warunków wstępnych]
2. [A-009 System] --[Sprawdź warunki: liczba uczestników ≥ X, termin generacji, grupy nie istnieją]--> [BC-005] ==> [warunki spełnione]
3. [A-009 System] --[Pobierz listę uczestników z numerami z BC-004 (snapshot)]--> [BC-005] ==> [lista gotowa]
4. [A-009 System] --[Zastosuj reguły niedopłaty: uczestnik z nieopłaconą niedopłatą → stary dystans]--> [BC-005] ==> [lista uczestników z przypisanymi dystansami]
5. [A-009 System] --[Wykonaj algorytm generacji: przypisz uczestników do grup per dystans, per godzina startu]--> [BC-005] ==> [E-033 Wygenerowano grupy startowe]
6. [A-009 System] --[Przypisz zawodnika do grupy]--> [BC-005] ==> [E-031/E-032 Przypisano zawodnika do grupy (automatycznie)]
7. [A-009 System] --[Finalizuj: oznacz grupy jako ostateczne]--> [BC-005] ==> [E-035 Grupy startowe zostały wygenerowane]
8. [A-009 System] --[Opublikuj wynik]--> [BC-005] ==> [E-036 Wygenerowano ostateczne grupy] → **PE-001**

---

## Kroki — Ścieżka porażki

2b. [A-009 System] --[warunki niespełnione]--> [BC-005] ==> [E-034 Nie udało się wygenerować grup startowych]

**Przyczyny porażki:**
- Liczba uczestników < X (Q-009 open — wartość X nieznana)
- Termin generacji przekroczony lub przed terminem
- Grupy już wygenerowane (idempotentność)

---

## Algorytm generacji (detail — Core Domain)

**Dane wejściowe:**
- Lista uczestników: ID, dystans opłacony, numer startowy
- Reguła niedopłaty: dla uczestnika z nieopłaconą niedopłatą → użyj starego dystansu (nie nowego)
- Konfiguracja grup z BC-001: liczba grup per dystans, limity miejsc per grupa, harmonogram godzin startów

**Logika:**
- Grupowanie per dystans
- Podział na grupy z zachowaniem limitu miejsc
- Przydział do godziny startu (kolejność nie jest w materiałach opisana — HS2-016: czy FIFO, losowo, inaczej?)
- Zawodnicy z nieopłaconą niedopłatą na nowy dystans → zostają na starym dystansie (RB-011)

**Wynik:** lista grup z przypisanymi zawodnikami i godzinami startu

---

## Read modele

| Krok | Aktor | Co musi widzieć |
|------|-------|----------------|
| 1 | Administrator | Status generacji (czy już uruchomiono), aktualna data vs termin z regulaminu, liczba gotowych uczestników |
| 2 | System | Parametry konfiguracyjne (BC-001): termin, limit X, harmonogram |
| 3 | System | Wszyscy uczestnicy z E-030 (numer) i PE-005 (opłaceni) |
| 4 | System | Status opłat uczestników (czy jest nieopłacona niedopłata) — z BC-003 |
| Po 8 | Zawodnik/Administrator | Lista grup z przypisaniami (read model dla BC-006) |

---

## Reguły biznesowe

| ID     | Reguła | Egzekwowana w kroku |
|--------|--------|---------------------|
| RB-016 | Generowanie możliwe tylko gdy grupy jeszcze nie zostały wygenerowane (idempotentność) | 2 |
| RB-017 | Generowanie wymaga min. X uczestników z przypisanym numerem startowym (X = wartość konfiguracyjna z BC-001 — Q-009) | 2 |
| RB-018 | Generowanie odbywa się w terminie określonym w regulaminie (BC-001) | 2 |
| RB-019 | Uczestnik z nieopłaconą niedopłatą na nowy dystans → stary dystans w grupie (fakt domenowy #6) | 4 |
| RB-020 | Uczestnik musi mieć przypisany numer startowy, żeby trafić do grupy | 3 |
| RB-021 | Grupy mają limity miejsc (z BC-001) | 5 |

---

## Wyjątki i ścieżki alternatywne

| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-011 | Za mało uczestników (< X) | E-034 Nie udało się wygenerować | Administrator czeka, może obniżyć X (?) lub zmienić termin |
| EX-012 | Uczestnik z nieopłaconą niedopłatą | Wchodzi do grupy starego dystansu | E-036 zawiera go w starym dystansie, nowy dystans aktywny dopiero po E-023 |
| EX-013 | Nie wszyscy uczestnicy z numerem (np. błąd BC-004) | Ci bez numeru pominięci | E-046 Nie przypisano do ostatecznej grupy |

---

## Przepływ odwrotny

- Brak mechanizmu cofnięcia generacji grup opisanego w materiałach
- Po PE-001 jedyna modyfikacja → BC-006 (Zarządzanie Grupami) w oknie czasowym
- Błąd po generacji → interwencja manualna administratora → HS2-016

## Weryfikacja odwrotną narracją

Żeby PE-001 (E-036) → musiało być E-033 (Wygenerowano grupy). Żeby E-033 → musiały być E-031/E-032 dla każdego uczestnika. Żeby E-031 → E-030 (numer) + PE-005 (opłacony). Żeby E-030 → E-019 (opłata). Żeby E-019 → E-017 (przelew) + E-018 (aplikacja).

**Luki znalezione:**
1. Algorytm podziału na grupy (kolejność, kryteria) → HS2-016
2. Wartość X (min. uczestników) → Q-009 open
3. Jak BC-005 pobiera dane o niedopłatach z BC-003? Snapshot czy zapytanie? → Q-007 / HS2-014
4. Co z uczestnikiem który opłaci niedopłatę MIĘDZY generacją a PE-002? → otwarte → HS2-017
