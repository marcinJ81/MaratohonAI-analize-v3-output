# Hot Spoty — Faza 2 (Process Level)

Format: HS2-NNN | opis | źródło (krok/proces) | status: open/resolved | rozstrzyga HS z Fazy 1 (H-ID)

---

## Hot spoty z Fazy 1 — rozstrzygnięte w Fazie 2

| ID | Opis | Źródło (krok) | Status | Rozstrzyga |
|----|------|--------------|--------|-----------|
| HS2-r01 | H-001 (granica systemu) | Krok 5 (BC) | **resolved**: system = 10 BC wewnętrznych + 2 integracje zewnętrzne (Bank, RFID). WWW wyników = osobny widok tej samej bazy (BC-009). | H-001 |
| HS2-r02 | H-003 (konflikt niedopłata vs generacja) | Krok 5, P-005 | **resolved**: uczestnik z nieopłaconą niedopłatą → trafia do grupy starego dystansu (RB-019, POL-006). Generacja nie jest blokowana. | H-003 |
| HS2-r03 | H-004 (real-time display) | Krok 5, P-009 | **resolved**: E-080 jest zdarzeniem w BC-009. Real-time display = personal read model z tej samej bazy. | H-004 |
| HS2-r04 | H-008 (dwie weryfikacje) | P-007 | **resolved**: dwa osobne procesy w BC-007: weryfikacja biurowa (E-055 + E-057 + E-059/E-060) i weryfikacja na starcie (E-061). | H-008 |
| HS2-r05 | H-010 (logika zamknięcia pomiaru) | P-009, POL-015 | **resolved**: trigger = 24h (E-088). Reguły klasyfikacji potwierdzone (fakt domenowy #7). | H-010 |
| HS2-r06 | H-016 (strona WWW wyników) | Krok 5, BC-009 | **resolved**: ta sama baza, osobny widok — nie osobny BC ani integracja. | H-016 |

---

## Hot spoty nowe — Faza 2

| ID | Opis | Źródło | Status | Rozstrzyga H z F1 |
|----|------|--------|--------|------------------|
| HS2-001 | Q-004: wygaśnięcie rejestracji — automatyczne czy manualne? E-015 (Przekroczono termin płatności) nie ma opisanego triggera czasowego. Co się dzieje gdy uczestnik nie opłaci w żadnym terminie? | P-002 (ścieżka B), EX-006 | **open** (bloker process-level) | H-012 |
| HS2-002 | H-002: nota "grupy mogą istnieć tylko po pewnych krokach jako reprezentacje list startowych" — nieczytelne. Czy "lista startowa" = stan grupy po PE-002 czy inny byt? | Krok 5, BC-006 | **open** (nice-to-have) | H-002 |
| HS2-003 | Q-008: dyskietka RFID w pakiecie czy osobno? Dwa zdarzenia (E-059, E-060) sugerują osobne akty, ale nie wiadomo czy mogą być jednoczesne i czy dyskietka ma osobny identyfikator. | P-007 (Część 1) | **open** (średni) | H-009 |
| HS2-004 | Nakładające się role przy decyzji o starcie (A-004/A-003/A-006) — brak jednoznacznego decydenta przy E-070 (Wystartowano grupę). | P-008 (krok 1) | **open** (nice-to-have) | — |
| HS2-005 | Q-011: co dzieje się z dyskietką RFID zawodnika który zakończył udział przedwcześnie (E-076)? Czy system przestaje rejestrować pomiary? Czy dyskietka jest zwracana? | P-008 (ścieżka D), P-009 (EX-028) | **open** (średni) | H-020 |
| HS2-006 | Q-005 (częściowy): kto kwalifikuje do dyplomu, w jakim formacie, jak odbierany (fizycznie w biurze vs elektronicznie)? E-093/E-094 bez opisu. | P-010 (dyplomy) | **open** (wysoki) | H-006 |
| HS2-007 | Brak mechanizmu notyfikacji uczestników o zmianach regulaminu po jego opublikowaniu (EX-002 w P-001). | P-001 (EX-002) | **open** (nice-to-have) | — |
| HS2-008 | Promocja -100%: kto aktywuje — A-004 ręcznie per uczestnik czy automatycznie przez system według reguły konfiguracyjnej? | P-003 (ścieżka 2) | **open** (średni) | — |
| HS2-009 | Zmiana dystansu po opłaceniu (ścieżka 4 P-003) — czy E-009 z BC-002 i E-009 w P-003 to to samo zdarzenie? Potencjalny duplikat pojęcia naruszający B1. | P-003 (ścieżka 4) | **open** (wysoki) | — |
| HS2-010 | Zwrot nadpłaty (E-027) wymaga przelewu wychodzącego — brak opisu mechanizmu (kto inicjuje, jak bank dostaje zlecenie). | P-003 (ścieżka 5) | **open** (średni) | — |
| HS2-011 | Q-013: "odpisanie uczestnictwa" (E-029) — rezygnacja uczestnika czy anulowanie przez organizatora? Różne konsekwencje finansowe (zwrot kaucji, zwrot opłaty). | P-003 (ścieżka 6) | **open** (nice-to-have) | H-007 |
| HS2-012 | Mechanizm dopasowania przelewu bankowego do uczestnika — brak opisu (tytuł przelewu? numer konta uczestnika? ID z systemu?). Ryzyko błędów przy ręcznych przelewach. | P-003 (D-013, ACL banku) | **open** (wysoki) | — |
| HS2-013 | Zwrot opłaty przy rezygnacji uczestnika po opłaceniu — brak mechanizmu opisanego w materiałach. | P-003 (przepływ odwrotny) | **open** (średni) | — |
| HS2-014 | Q-007: komunikacja BC-002→BC-003 i BC-003→BC-004 — synchronicznie czy asynchronicznie? Format wymiany (event? DTO? API call?)? Wpływa na architekturę. | Krok 8 (D-002, D-003) | **open** (wysoki) | H-014 |
| HS2-015 | Brak opisanego mechanizmu rozszerzenia puli numerów startowych (EX-010 w P-004). | P-004 (EX-010) | **open** (nice-to-have) | — |
| HS2-016 | Algorytm generacji grup — kolejność przydziału uczestników do grup (FIFO? losowo? per czas zapisu? per wyniki z poprzednich edycji?). Core Domain — kluczowy dla wyników. | P-005 (krok 5) | **open** (wysoki — dot. Core Domain) | — |
| HS2-017 | Uczestnik który opłaci niedopłatę MIĘDZY PE-001 (generacja) a PE-002 (blokada) — czy system automatycznie przenosi go do nowej grupy (nowy dystans)? | P-005 (EX-012), P-006 (ścieżka B) | **open** (wysoki) | H-003 (częściowy) |
| HS2-018 | Ścieżka B (dołączenie po terminie) i ścieżka C (wniosek po terminie) w P-006 — czy to ten sam flow czy dwa różne? Różnica między "dołączeniem" a "wnioskiem po terminie". | P-006 (ścieżki B i C) | **open** (średni) | — |
| HS2-019 | Opłata przy zmianie grupy (E-051, POL-010) — czy to idzie przez BC-003 (przelew bankowy?) czy to wewnętrzna transakcja BC-006? Różne implikacje dla accouting. | P-006 (ścieżka D), POL-010 | **open** (wysoki) | H-002 częściowy |
| HS2-020 | Czy akceptacja wniosku o zmianę grupy przez A-003 jest ręczna (ludzka polityka, POL-011) czy automatyczna (system waliduje i akceptuje)? Krucha polityka ludzka. | P-006 (krok 3), POL-011 | **open** (średni) | — |
| HS2-021 | Brak mechanizmu odwoławczego dla E-056 (zawodnik nie przeszedł weryfikacji biurowej). Co może zrobić zawodnik lub administrator? | P-007 (ścieżka B, EX-019) | **open** (średni) | — |
| HS2-022 | Q-010: zawodnik który nie stawił się na starcie — czy slot jest automatycznie zwalniany? Czy zawodnik jest oznaczany jako "nieobecny"? Co z dyskietką i pakietem? | P-007 (EX-022), P-008 (ścieżka E) | **open** (średni) | H-019 |
| HS2-023 | Start indywidualny — czas startu indywidualnego relative do grupy. Czy wyniki zawodnika są porównywalne z resztą grupy? Jak czas jest kalkulowany? | P-008 (ścieżka B) | **open** (średni) | — |
| HS2-024 | Przedłużenie dystansu (E-083) — czy wymaga dopłaty w BC-003? Jeśli tak → cross-context flow BC-009→BC-003. Jeśli nie → co z klasyfikacją (próba dłuższego = reguła z fakt #7)? | P-009 (ścieżka A) | **open** (wysoki) | — |
| HS2-025 | Fallback przy błędzie czytnika RFID (dyskietka nie reaguje). Czy jest manualna rejestracja? Brak opisanego mechanizmu. | P-009 (EX-029) | **open** (średni) | — |
| HS2-026 | Korekta wyników po publikacji (E-091). Brak mechanizmu opisanego. Protesty zawodników, błędy systemowe — jak jest obsługiwane? | P-009/P-010 (przepływ odwrotny) | **open** (średni) | H-005 |
| HS2-027 | Kryteria rankingu (E-089) — per dystans? globalny? Co jest kryterium dla dyplomów i nagród (top-3 per dystans? absolut winner? czas vs dystans)? | P-009 (krok 7), P-010 (krok 2) | **open** (wysoki) | H-005 |
| HS2-028 | Sposób odbioru dyplomu (E-094) — fizycznie w biurze czy elektronicznie (email, PDF)? Wpływa na przepływ P-010. | P-010 (krok 6) | **open** (nice-to-have) | H-006 |
| HS2-029 | H-015: mechanizm upoważnienia osoby do zwrotu pakietu w imieniu zawodnika. Brak systemu proxy/upoważnień. | P-010 (EX-033) | **open** (nice-to-have) | H-015 |
| HS2-030 | Kaucja zatrzymana (E-099, POL-018) — kto fizycznie egzekwuje zatrzymanie? Czy to automatyczna zmiana stanu czy wymaga akcji obsługi? | P-010 (ścieżka B, POL-018) | **open** (nice-to-have) | — |

---

## Zestawienie priorytetowe

### Blokery (open, wysoki wpływ na architekturę)
- HS2-001 (Q-004 wygaśnięcie rejestracji) — bloker BC-002/BC-003
- HS2-009 (duplikat E-009 — zmiana dystansu) — narusza B1 (jedno znaczenie w BC)
- HS2-012 (mechanizm dopasowania przelewu) — AC w D-013
- HS2-014 (Q-007 format wymiany BC-002→BC-003→BC-004) — wpływa na design relacji
- HS2-016 (algorytm generacji grup) — Core Domain, kluczowy
- HS2-017 (opłata niedopłaty między PE-001 a PE-002) — reguła domenowa niejasna
- HS2-019 (opłata przy zmianie grupy przez BC-003 czy nie?)
- HS2-024 (przedłużenie dystansu — dopłata czy nie?)
- HS2-027 (kryteria rankingu) — niezbędne do generacji dyplomów

### Średnie (open, wpływają na przepływ)
- HS2-003 (Q-008 dyskietka w pakiecie)
- HS2-005 (Q-011 dyskietka przy zakończeniu udziału)
- HS2-006 (Q-005 format dyplomu, kto kwalifikuje)
- HS2-008 (promocja -100% — automatyczna czy manualna)
- HS2-010 (zwrot nadpłaty — mechanizm)
- HS2-013 (zwrot opłaty przy rezygnacji)
- HS2-018 (dołączenie po terminie vs wniosek po terminie)
- HS2-020 (akceptacja wniosku o zmianę grupy — manual/auto)
- HS2-021 (brak mechanizmu odwoławczego przy E-056)
- HS2-022 (Q-010 nie stawił się na starcie)
- HS2-023 (start indywidualny — czas kalkulowany jak)
- HS2-025 (fallback RFID)
- HS2-026 (korekta wyników po publikacji)

### Nice-to-have
- HS2-002 (H-002 lista startowa)
- HS2-004 (nakładanie ról przy starcie)
- HS2-007 (notyfikacja o zmianach regulaminu)
- HS2-011 (Q-013 odpisanie uczestnictwa)
- HS2-015 (rozszerzenie puli numerów)
- HS2-028 (Q-005 sposób odbioru dyplomu)
- HS2-029 (H-015 proxy przy zwrocie)
- HS2-030 (egzekucja zatrzymania kaucji)
