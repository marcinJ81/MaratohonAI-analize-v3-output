# Pivotal Events — System Zarządzania 24h Maratonem Rowerowym

Numeracja kontynuowana z Fazy 1. PE-001 do PE-004 z Fazy 1 zweryfikowane i uzupełnione.
PE-005+ to nowe pivotal events odkryte w Fazie 2 przy większej szczegółowości.

---

## PE-001 — Wygenerowano ostateczne grupy startowe

**Zdarzenie:** E-036
**Źródło:** Faza 1
**Status:** aktualny

**Dlaczego pivotal:**
- Punkt bez powrotu dla standardowego trybu przypisania do grup
- Po tym zdarzeniu zmiany grupy są możliwe tylko przez BC-06 (Grupy Startowe) z restrykcjami i w oknie czasowym
- Właściciel danych o przypisaniu przechodzi z SD-005 (Generacja) do SD-006 (Zarządzanie)
- Widoczny dla uczestników — dostają informację o przypisaniu do grupy

**Granica między:** BC-005 (Generacja Grup Startowych) → BC-006 (Grupy Startowe / Zarządzanie)

**Weryfikacja Faza 2:** Potwierdzone. PE-001 pozostaje pivotal. Reguła niedopłaty (fakt domenowy #6) potwierdza że w momencie PE-001 uczestnik z nieopłaconą niedopłatą trafia do grupy starego dystansu — logika decyzyjna działa dokładnie NA tym pivotal event.

---

## PE-002 — Minął czas na ewentualne zmiany grupy

**Zdarzenie:** E-052
**Źródło:** Faza 1
**Status:** aktualny

**Dlaczego pivotal:**
- Twarda blokada — żadna operacja modyfikacji grupy nie jest możliwa po tym zdarzeniu
- Przypisanie grupy staje się niezmienne
- Właściciel danych przechodzi do BC-007 (Weryfikacja Startowa) — lista startowa jest zamrożona
- Trigger czasowy (nie ludzki) — nieodwracalny

**Granica między:** BC-006 (Grupy Startowe / Zarządzanie) → BC-007 (Weryfikacja i Kwalifikacja Startowa)

**Weryfikacja Faza 2:** Potwierdzone. To "ściana" oddzielająca administrację od operacji startowej. Weryfikacja na starcie operuje na zamrożonej liście.

---

## PE-003 — Wystartowano zawodników

**Zdarzenie:** E-073
**Źródło:** Faza 1
**Status:** aktualny

**Dlaczego pivotal:**
- Aktywacja pomiaru czasu (zegar startowy)
- Zamknięcie zapisów na dany dystans (E-075) — lista startowa jest ostateczna
- Przejście z fazy administracyjnej (rejestracja, płatności, grupy) do operacyjnej (pomiar)
- Widoczne dla wszystkich obserwatorów (fizyczny start zawodów)

**Granica między:** BC-008 (Start Zawodników) → BC-009 (Pomiar Czasu i Wyniki)

**Weryfikacja Faza 2:** Potwierdzone. Moment startu zamraża listę zawodników aktywnych w pomiarze.

---

## PE-004 — Zakończono przejazdy wszystkich dystansów

**Zdarzenie:** E-090
**Źródło:** Faza 1
**Status:** aktualny

**Dlaczego pivotal:**
- Trigger końca fazy operacyjnej (24h upłynęło — fakt domenowy #7)
- Wyzwala podliczenie wyników i generację rankingu końcowego
- Zamknięcie pomiaru czasu dla wszystkich dystansów
- Przejście do fazy ceremonialnej (nagrody, dyplomy, kaucje)

**Granica między:** BC-009 (Pomiar Czasu i Wyniki) → BC-010 (Zakończenie Zawodów)

**Weryfikacja Faza 2:** Potwierdzone. Trigger = upłynięcie 24h (czasowe, nie ludzkie). Po tym zdarzeniu zawodnicy na trasie → "jadą dla siebie" (nieklasyfikowani) — reguła egzekwowana na tym pivotal event.

---

## PE-005 — Opłacono uczestnictwo (nowy — Faza 2)

**Zdarzenie:** E-019 / E-028
**Źródło:** Faza 2
**Status:** aktualny

**Dlaczego pivotal:**
- Warunek konieczny do przydziału numeru startowego (BC-004)
- Nieodwracalne w sensie płatniczym (przelew bankowy jest dokonany)
- Zmiana stanu uczestnika: "oczekuje na opłatę" → "opłacony" → otwiera drogę do generacji grupy
- Widoczne na zewnątrz (bank potwierdza przelew)

**Granica między:** BC-003 (Opłata za Udział) → BC-004 (Przydział Numeru Startowego)

**Uzasadnienie dodania:** W Fazie 1 skupiono się na PE organizacyjnych (grupy, start, koniec). Przy analizie przepływów w Fazie 2 widać że przejście "opłacono" → "numer" jest kluczowym punktem zmiany stanu i właściciela danych.

---

## PE-006 — Zarejestrowano uczestnika i potwierdzono rejestrację (nowy — Faza 2)

**Zdarzenie:** E-013 (Potwierdzono rejestrację)
**Źródło:** Faza 2
**Status:** aktualny (ale zależy od rozstrzygnięcia Q-004)

**Dlaczego pivotal:**
- Formalnie tworzy uczestnika w systemie — przed tym zdarzeniem "ktoś wypełnił formularz"
- Otwiera drogę do opłacenia (E-016)
- Zmiana właściciela: dane formularza → dane uczestnika w systemie

**Granica między:** BC-002 (Rejestracja) → BC-003 (Opłata za Udział)

**Uwaga:** H-011 z Fazy 1 jest nadal open — brak pewności czy E-013 istnieje jako odrębne zdarzenie czy jest implicite w E-004. Jeśli Q-004 (wygaśnięcie rejestracji) zostanie rozstrzygnięte → może zmienić status tego PE. **Pewność: średnia** (wniosek oparty na logice procesu, nie wprost z materiałów).

---

## Tabela przeglądowa

| ID     | Zdarzenie (E-ID) | Dlaczego pivotal | Źródło | Status | Granica między (BC-ID → BC-ID) |
|--------|-----------------|-----------------|--------|--------|-------------------------------|
| PE-001 | E-036 | Punkt bez powrotu generacji grup; zmiana właściciela danych; widoczne dla uczestników | Faza 1 | aktualny | BC-005 → BC-006 |
| PE-002 | E-052 | Twarda blokada zmian grupy; lista startowa zamrożona; trigger czasowy | Faza 1 | aktualny | BC-006 → BC-007 |
| PE-003 | E-073 | Aktywacja pomiaru czasu; zamknięcie zapisów; przejście adm.→oper. | Faza 1 | aktualny | BC-008 → BC-009 |
| PE-004 | E-090 | Trigger końca 24h; zamknięcie pomiaru; przejście oper.→ceremonial | Faza 1 | aktualny | BC-009 → BC-010 |
| PE-005 | E-019/E-028 | Warunek konieczny numeru; nieodwracalne; zmiana stanu uczestnika | Faza 2 | aktualny | BC-003 → BC-004 |
| PE-006 | E-013 | Tworzy uczestnika w systemie; otwiera ścieżkę opłaty | Faza 2 | aktualny (niepewny) | BC-002 → BC-003 |
