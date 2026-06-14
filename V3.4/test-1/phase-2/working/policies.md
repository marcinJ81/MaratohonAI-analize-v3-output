# Polityki — System Zarządzania 24h Maratonem Rowerowym

Format: `Gdy [E-ID zdarzenie] → [reguła] → [komenda/akcja]`
Typ: automatyczna (system) | ludzka (procedura, nawyk, czeklista)

---

## POL-001 — Weryfikacja danych przy rejestracji

**Gdy:** E-006 (Potwierdzono dane kontaktowe)
**Reguła:** Dane uczestnika muszą być kompletne i poprawne
**Akcja:** Komenda "Zweryfikuj zawodnika" → E-007 lub odmowa
**BC:** BC-002 (Rejestracja)
**Typ:** automatyczna (walidacja systemowa)
**Przekracza granicę BC?** nie

---

## POL-002 — Wyliczenie opłaty przy rejestracji

**Gdy:** E-005 (Wybrano dystans) lub E-010 (Zmieniono dystans)
**Reguła:** System wylicza opłatę na podstawie cennika z BC-001
**Akcja:** Komenda "Wylicz opłatę" → E-008 lub E-011
**BC:** BC-002 (Rejestracja)
**Typ:** automatyczna
**Przekracza granicę BC?** tak (czyta cennik z BC-001 → D-001)

---

## POL-003 — Inicjacja opłaty po potwierdzeniu rejestracji

**Gdy:** E-013 (Potwierdzono rejestrację) — PE-006
**Reguła:** Uczestnik musi opłacić w terminie
**Akcja:** BC-003 staje się aktywne dla tego uczestnika (otwierają się ścieżki opłaty)
**BC:** BC-003 (Opłata za Udział)
**Typ:** automatyczna (przejście stanu)
**Przekracza granicę BC?** tak (BC-002 → BC-003 → D-002)

---

## POL-004 — Wykrycie niedopłaty

**Gdy:** E-017 (Zarejestrowano przelew) + E-018 (Zaaplikowano opłatę)
**Reguła:** Kwota wpłacona < kwota należna → niedopłata
**Akcja:** Komenda "Wykryj niedopłatę" → E-021
**BC:** BC-003 (Opłata za Udział)
**Typ:** automatyczna
**Przekracza granicę BC?** nie

---

## POL-005 — Automatyczny przydział numeru startowego po opłaceniu

**Gdy:** E-019/E-028 (Opłacono uczestnictwo/dystans) — PE-005
**Reguła:** Każdy opłacony uczestnik dostaje numer startowy automatycznie
**Akcja:** Komenda "Przypisz numer startowy" → E-030
**BC:** BC-004 (Przydział Numeru)
**Typ:** automatyczna
**Przekracza granicę BC?** tak (BC-003 → BC-004 → D-003)

---

## POL-006 — Reguła niedopłaty przy generacji grup

**Gdy:** E-033 (Wygenerowano grupy startowe) — walidacja wejściowa
**Reguła:** Uczestnik z nieopłaconą niedopłatą na nowy dystans → trafia do grupy STAREGO dystansu
**Akcja:** Przypisanie do grupy ze starym dystansem (nie nowym)
**BC:** BC-005 (Generacja Grup Startowych)
**Typ:** automatyczna (fakt domenowy #6)
**Przekracza granicę BC?** tak (czyta status opłat z BC-003 → D-004)

---

## POL-007 — Blokada zmian grupy po PE-002

**Gdy:** E-052 (Minął czas na zmiany grupy) — PE-002
**Reguła:** Po PE-002 żadna zmiana grupy nie jest możliwa
**Akcja:** BC-006 zamknięty na zapis, wszelkie wnioski o zmianę odrzucane automatycznie
**BC:** BC-006 (Grupy Startowe / Zarządzanie)
**Typ:** automatyczna (trigger czasowy)
**Przekracza granicę BC?** nie (ale stan "zamrożony" propaguje do BC-007 → D-005)

---

## POL-008 — Jednorazowość zmiany grupy

**Gdy:** E-037 (Złożono wniosek o zmianę grupy)
**Reguła:** Zawodnik nie może zmienić grupy więcej niż raz
**Akcja:** Sprawdź historię wniosków → jeśli istnieje poprzednia zmiana → E-040 (odmowa)
**BC:** BC-006
**Typ:** automatyczna
**Przekracza granicę BC?** nie

---

## POL-009 — Warunek wolnych miejsc przy zmianie grupy

**Gdy:** E-039 (Przyjęto wniosek o zmianę grupy)
**Reguła:** Grupa docelowa musi mieć wolne miejsce
**Akcja:** Sprawdź miejsca → jeśli brak → E-042/E-044
**BC:** BC-006
**Typ:** automatyczna
**Przekracza granicę BC?** nie

---

## POL-010 — Warunek opłaty przy zmianie grupy

**Gdy:** E-039 (Przyjęto wniosek)
**Reguła:** Brak zapłaty różnicy = brak zmiany grupy
**Akcja:** Sprawdź status opłaty zmiany → jeśli nieopłacona → E-045 (odmowa z powodu braku opłaty)
**BC:** BC-006
**Typ:** automatyczna / ludzka (kto sprawdza status opłaty? — HS2-019)
**Przekracza granicę BC?** tak (czyta status opłat z BC-003 → D-006)

---

## POL-011 — Wniosek o zmianę grupy przez administratora (ludzka!)

**Gdy:** E-037 (Złożono wniosek) — krok przyjęcia przez A-003
**Reguła:** Administrator ręcznie przegląda i akceptuje lub odrzuca wniosek
**Akcja:** E-039 (Przyjęto) lub E-040 (Odrzucono)
**BC:** BC-006
**Typ:** **ludzka** — "Grażyna/Administrator pamięta że trzeba przejrzeć wnioski" → krucha!
**Przekracza granicę BC?** nie
**Hot spot:** HS2-020 — czy akceptacja jest automatyczna czy ręczna? Ryzyko przeoczenia wniosku.

---

## POL-012 — Dopuszczenie do startu po weryfikacji biurowej

**Gdy:** E-059 (Wydano pakiet startowy) + E-060 (Wydano dyskietkę RFID)
**Reguła:** Zawodnik z pakietem i dyskietką jest dopuszczony do weryfikacji na starcie
**Akcja:** Status zawodnika → "gotowy do startu"
**BC:** BC-007 (Weryfikacja Startowa)
**Typ:** automatyczna (zmiana stanu)
**Przekracza granicę BC?** tak (przekazuje dane do BC-008 → D-008)

---

## POL-013 — Zamknięcie listy startowej po PE-003

**Gdy:** E-073 (Wystartowano zawodników) — PE-003
**Reguła:** Lista startowa jest zamrożona — żaden uczestnik nie może być dodany
**Akcja:** Komenda "Zamknij zapisy na dystans" → E-075
**BC:** BC-008 (Start Zawodników)
**Typ:** automatyczna (trigger: PE-003)
**Przekracza granicę BC?** nie

---

## POL-014 — Aktywacja pomiaru czasu po PE-003

**Gdy:** E-073 (Wystartowano zawodników) — PE-003
**Reguła:** Po starcie każde przyłożenie dyskietki do czytnika jest rejestrowane jako pomiar
**Akcja:** BC-009 aktywny — E-079 zaczynają napływać
**BC:** BC-009 (Pomiar Czasu i Wyniki)
**Typ:** automatyczna
**Przekracza granicę BC?** tak (BC-008 → BC-009 → D-009)

---

## POL-015 — Trigger zamknięcia pomiaru po 24h (fakt domenowy #7)

**Gdy:** E-088 (Upłynął czas maratonu — 24h)
**Reguła:** Po 24h trigger zamknięcia. Zawodnicy na trasie → "jadą dla siebie" (nieklasyfikowani)
**Akcja:** System podlicza czasy (E-086), generuje ranking (E-089), zamyka pomiar per dystans (E-087)
**BC:** BC-009
**Typ:** automatyczna (trigger czasowy)
**Przekracza granicę BC?** nie (wynik propaguje do BC-010 przez PE-004 → D-010)

---

## POL-016 — Reguła klasyfikacji przy przekroczeniu dystansu (fakt domenowy #7)

**Gdy:** E-086 (Podliczono czasy przejazdu) — klasyfikacja per zawodnik
**Reguła:** Zawodnik który ukończył opłacony dystans i próbował dłuższego lecz nie zmieścił się → klasyfikowany na opłaconym dystansie (nie dłuższym)
**Akcja:** Klasyfikacja na dystansie opłaconym → E-087
**BC:** BC-009
**Typ:** automatyczna
**Przekracza granicę BC?** nie (ale wymaga danych o opłaconym dystansie z BC-003 → D-004/D-011)

---

## POL-017 — Automatyczna generacja dyplomów po PE-004

**Gdy:** E-089 (Wygenerowano ranking wyników) po PE-004
**Reguła:** System automatycznie generuje dyplomy dla zakwalifikowanych zawodników
**Akcja:** Komenda "Wygeneruj dyplom" → E-093 (per zawodnik)
**BC:** BC-010 (Zakończenie Zawodów)
**Typ:** automatyczna (potwierdzone przez fakty domenowe: dyplomy generowane wewnętrznie)
**Przekracza granicę BC?** tak (BC-009 → BC-010 przez PE-004 → D-010)

---

## POL-018 — Kaucja zatrzymana przy braku zwrotu pakietu

**Gdy:** E-097 (Upłynął czas zwrotu pakietu)
**Reguła:** Brak zwrotu pakietu w terminie → kaucja zatrzymana
**Akcja:** E-099 Kaucja zatrzymana
**BC:** BC-010
**Typ:** automatyczna (trigger czasowy) / ludzka (kto "egzekwuje"? — HS2-030)
**Przekracza granicę BC?** nie

---

## POL-019 — Odpisanie uczestnictwa (ludzka!)

**Gdy:** E-029 (Odpisano uczestnictwo) — inicjacja przez A-004/A-003
**Reguła:** Uczestnictwo może być odpisane przez organizatora lub administratora — brak automatyzmu
**Akcja:** Usunięcie zobowiązania finansowego lub anulowanie uczestnictwa
**BC:** BC-003 (Opłata za Udział)
**Typ:** **ludzka** — decyzja administratora/organizatora
**Przekracza granicę BC?** nie (ale skutki mogą dotyczyć BC-002)
**Hot spot:** HS2-011 — Q-013 nierozstrzygnięte

---

## Zestawienie polityk ludzkich (kruche!)

| ID | Polityka | Ryzyko |
|----|---------|--------|
| POL-011 | Wniosek o zmianę grupy ręcznie akceptowany przez A-003 | Przeoczenie, opóźnienie — HS2-020 |
| POL-018 (częściowo) | Kaucja zatrzymana — kto egzekwuje? | HS2-030 |
| POL-019 | Odpisanie uczestnictwa — decyzja człowieka | HS2-011 (Q-013) |

---

## Polityki przekraczające granicę BC

| POL-ID | BC-FROM | BC-TO | Co przekazywane | Relacja (D-ID) |
|--------|---------|-------|----------------|---------------|
| POL-002 | BC-002 | BC-001 | Cennik dystansów (czyt) | D-001 |
| POL-003 | BC-002 | BC-003 | Potwierdzenie rejestracji (PE-006) | D-002 |
| POL-005 | BC-003 | BC-004 | Potwierdzenie opłaty (PE-005) | D-003 |
| POL-006 | BC-005 | BC-003 | Status opłat/niedopłat (czyt) | D-004 |
| POL-007 | BC-006 | BC-007 | Lista startowa zamrożona (PE-002) | D-005 |
| POL-010 | BC-006 | BC-003 | Status opłaty zmiany grupy (czyt) | D-006 |
| POL-012 | BC-007 | BC-008 | Zawodnik dopuszczony do startu (stan) | D-008 |
| POL-014 | BC-008 | BC-009 | PE-003 → aktywacja pomiaru | D-009 |
| POL-016 | BC-009 | BC-003 | Opłacony dystans (czyt) | D-011 |
| POL-017 | BC-009 | BC-010 | PE-004 → ranking → dyplomy | D-010 |
