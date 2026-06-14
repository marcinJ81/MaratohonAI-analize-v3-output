# Context Map — System Zarządzania 24h Maratonem Rowerowym

Heurystyki: R1 (układ sił), R2 (jakość modelu), R3 (ochrona core), R4 (liczba konsumentów), R5 (koszt integracji)

---

## Relacje wewnętrzne (między BC)

### D-001 — BC-001 → BC-002, BC-003, BC-005 (konfiguracja)

**Upstream:** BC-001 (Konfiguracja Maratonu)
**Downstream:** BC-002 (Rejestracja), BC-003 (Opłata), BC-005 (Generacja Grup)
**Wzorzec:** OHS + Published Language
**Heurystyka:** R4 — wielu konsumentów (3+ BC czyta konfigurację), R2 — model konfiguracyjny jest stabilny i pasuje do języka konsumentów.

**Uzasadnienie:**
- BC-001 wystawia konfigurację imprezy (terminy, cennik, limity, parametry grup) jako API odczytowe
- Wszystkie BC downstream są konformistami wobec modelu BC-001 — nie ma potrzeby ACL bo model jest prosty i własny
- BC-001 nie słucha wymagań BC-002/BC-005 — Conformist od strony downstream

**Co jest przekazywane:**
- BC-002: terminy rejestracji, lista dystansów, limity miejsc, cennik
- BC-003: cennik, promocje
- BC-005: termin generacji, min. X uczestników, parametry grup, harmonogram startów

**Komunikacja:** synchroniczna (odczyt), Published Language (model konfiguracyjny)

---

### D-002 — BC-002 → BC-003 (rejestracja → opłata)

**Upstream:** BC-002 (Rejestracja)
**Downstream:** BC-003 (Opłata za Udział)
**Wzorzec:** Customer–Supplier
**Heurystyka:** R1 — BC-003 "zamawia" uczestnika z BC-002 (downstream może negocjować); R2 — model uczestnika z BC-002 pasuje do potrzeb BC-003

**Uzasadnienie:**
- BC-003 potrzebuje danych uczestnika (ID, wybrany dystans, kwota) od BC-002
- BC-002 jest upstream i może planować interfejs pod BC-003
- Zdarzenie integracyjne: PE-006 (Potwierdzono rejestrację)

**Co jest przekazywane:** ID uczestnika, wybrany dystans, wyliczona opłata → jako zdarzenie integracyjne (Event: PotwierdzononRejestrację)
**Komunikacja:** asynchroniczna (zdarzenie domenowe, publikowane po E-013)

---

### D-003 — BC-003 → BC-004 (opłata → numer)

**Upstream:** BC-003 (Opłata za Udział)
**Downstream:** BC-004 (Przydział Numeru Startowego)
**Wzorzec:** Customer–Supplier
**Heurystyka:** R1 — BC-004 jest prosto zależny od BC-003; R2 — model opłaty jest prosty i czytelny

**Uzasadnienie:**
- BC-004 musi wiedzieć że opłata jest dokonana (PE-005) żeby przydzielić numer
- BC-003 upstream, BC-004 downstream — BC-003 może planować interfejs
- Zdarzenie integracyjne: PE-005 (Opłacono uczestnictwo)

**Co jest przekazywane:** ID uczestnika, opłacony dystans → jako zdarzenie integracyjne
**Komunikacja:** asynchroniczna (zdarzenie po E-019/E-028)

---

### D-004 — BC-003 → BC-005 (opłata → generacja grup, dane o niedopłatach)

**Upstream:** BC-003 (Opłata za Udział)
**Downstream:** BC-005 (Generacja Grup Startowych) — **Core Domain**
**Wzorzec:** ACL (Anticorruption Layer) po stronie BC-005
**Heurystyka:** R3 — Core Domain (BC-005) nigdy nie jest Conformistą; R2 — model opłat z BC-003 (terminy, kwoty, niedopłaty) wymaga tłumaczenia na język generacji grup

**Uzasadnienie:**
- BC-005 musi wiedzieć o statusie niedopłat (RB-019: uczestnik z nieopłaconą niedopłatą → stary dystans)
- BC-005 jest Core Domain → chronimy jego model przez ACL
- BC-003 nie zmienia swojego modelu pod BC-005 — BC-005 tłumaczy sobie

**Co jest przekazywane:** lista uczestników z flagami statusu opłaty (opłacony/niedopłata) → jako snapshot przy inicjacji generacji
**Komunikacja:** synchroniczna (snapshot w momencie generacji) — BC-005 odpytuje BC-003 przez ACL przy starcie generacji
**ACL:** tłumaczy: "uczestnik z nieopłaconą niedopłatą na dystans X" → "uczestnik klasyfikowany na dystansie Y (starym)" w modelu generacji

---

### D-005 — BC-004 → BC-005 (numer → generacja grup)

**Upstream:** BC-004 (Przydział Numeru)
**Downstream:** BC-005 (Generacja Grup Startowych) — **Core Domain**
**Wzorzec:** ACL po stronie BC-005
**Heurystyka:** R3 — Core Domain chroniony przez ACL; R2 — model numerów jest trywialny ale chronimy Core

**Uzasadnienie:**
- BC-005 potrzebuje listy uczestników z przypisanymi numerami startowymi (RB-020)
- BC-004 to Generic — jego model (prosta tabela numerów) może się zmienić
- ACL w BC-005 tłumaczy "numer startowy" na "uczestnik gotowy do grupy"

**Co jest przekazywane:** lista {uczestnik-ID, numer-startowy, dystans} → snapshot przy generacji
**Komunikacja:** synchroniczna (snapshot w momencie generacji)

---

### D-006 — BC-005 → BC-006 (generacja → zarządzanie grupami)

**Upstream:** BC-005 (Generacja Grup Startowych) — **Core Domain**
**Downstream:** BC-006 (Grupy Startowe / Zarządzanie)
**Wzorzec:** OHS + Published Language
**Heurystyka:** R4 — BC-006 jest jedynym bezpośrednim konsumentem; R1 — BC-005 jest Core, nie słucha wymagań BC-006

**Uzasadnienie:**
- PE-001 (Wygenerowano ostateczne grupy) jest granicą — BC-006 zaczyna operować na danych z BC-005
- BC-005 wystawia wynik generacji jako Published Language (model grup startowych)
- BC-006 jest konformistą — przyjmuje model grup bez tłumaczenia (model własny, spójny)

**Co jest przekazywane:** lista grup z przypisaniami {grupa-ID, dystans, godzina-startu, lista uczestników} → zdarzenie PE-001
**Komunikacja:** asynchroniczna (zdarzenie po PE-001)

---

### D-007 — BC-006 → BC-007 (zarządzanie grupami → weryfikacja startowa)

**Upstream:** BC-006 (Grupy Startowe / Zarządzanie)
**Downstream:** BC-007 (Weryfikacja i Kwalifikacja Startowa)
**Wzorzec:** Customer–Supplier
**Heurystyka:** R1 — BC-007 potrzebuje zamrożonej listy startowej; R2 — model listy grupy pasuje

**Uzasadnienie:**
- PE-002 (Minął czas zmian grupy) → lista startowa zamrożona → BC-007 może ją odczytać
- BC-006 upstream, BC-007 downstream
- Model listy startowej jest spójny i nie wymaga tłumaczenia

**Co jest przekazywane:** lista startowa {zawodnik-ID, numer-startowy, dystans, grupa, godzina-startu} → stan po PE-002
**Komunikacja:** synchroniczna (BC-007 odpytuje zamrożoną listę)

---

### D-008 — BC-007 → BC-008 (weryfikacja → start)

**Upstream:** BC-007 (Weryfikacja Startowa)
**Downstream:** BC-008 (Start Zawodników)
**Wzorzec:** Customer–Supplier
**Heurystyka:** R1 — BC-008 potrzebuje statusu dopuszczenia; R2 — model statusu prosty

**Uzasadnienie:**
- BC-008 potrzebuje wiedzieć kto odebrał pakiet i jest dopuszczony do startu (E-059/E-060)
- BC-007 upstream, BC-008 downstream
- Prosta flaga "dopuszczony" nie wymaga ACL

**Co jest przekazywane:** status zawodnika {zawodnik-ID, czy-dopuszczony, numer-dyskietki-RFID}
**Komunikacja:** synchroniczna (BC-008 odpytuje BC-007 przy starcie grupy)

---

### D-009 — BC-008 → BC-009 (start → pomiar)

**Upstream:** BC-008 (Start Zawodników)
**Downstream:** BC-009 (Pomiar Czasu i Wyniki)
**Wzorzec:** Customer–Supplier
**Heurystyka:** R1 — BC-009 jest zależny od BC-008 (czas startu jest potrzebny do kalkulacji wyników); R2 — model czasu startu jest prosty

**Uzasadnienie:**
- PE-003 (Wystartowano zawodników) aktywuje BC-009
- BC-009 potrzebuje czasów startu per zawodnik/grupa do obliczenia wyników
- BC-008 upstream, BC-009 downstream

**Co jest przekazywane:** PE-003 + {zawodnik-ID, czas-startu, dystans, numer-dyskietki-RFID} → zdarzenie integracyjne
**Komunikacja:** asynchroniczna (zdarzenie PE-003)

---

### D-010 — BC-009 → BC-010 (pomiar → zakończenie)

**Upstream:** BC-009 (Pomiar Czasu i Wyniki)
**Downstream:** BC-010 (Zakończenie Zawodów)
**Wzorzec:** Customer–Supplier
**Heurystyka:** R1 — BC-010 jest zależny od wyników z BC-009; R2 — model rankingu pasuje

**Uzasadnienie:**
- PE-004 (Zakończono przejazdy) + E-089 (Ranking) → trigger dla BC-010
- BC-009 upstream, BC-010 downstream
- Model rankingu jest przekazywany jako gotowy wynik

**Co jest przekazywane:** PE-004 + ranking wyników {zawodnik-ID, dystans-opłacony, dystans-ukończony, czas, klasyfikacja} → zdarzenie integracyjne
**Komunikacja:** asynchroniczna (zdarzenie PE-004 + payload rankingu)

---

### D-011 — BC-003 → BC-009 (opłaty → pomiar, dane klasyfikacyjne)

**Upstream:** BC-003 (Opłata za Udział)
**Downstream:** BC-009 (Pomiar Czasu i Wyniki)
**Wzorzec:** ACL po stronie BC-009 (analogicznie do D-004)
**Heurystyka:** R3 — BC-009 jest złożony (Supporting złożony) i chroni swój model; R2 — model opłat wymaga tłumaczenia na "opłacony-dystans" w kontekście klasyfikacji

**Uzasadnienie:**
- BC-009 musi znać opłacony dystans każdego zawodnika (RB-036/RB-037: reguła klasyfikacji)
- Model opłat z BC-003 ("kwota", "niedopłata") tłumaczony na "opłacony dystans" w języku pomiaru
- Dane pobierane jako snapshot przy PE-003 (starcie)

**Co jest przekazywane:** {zawodnik-ID, opłacony-dystans} → snapshot przy starcie
**Komunikacja:** synchroniczna (snapshot przy PE-003)

---

### D-012 — BC-008 ↔ BC-004 (start ↔ numery przy zgubieniu dyskietki)

**Upstream:** BC-004 (Przydział Numeru)
**Downstream:** BC-008 (Start Zawodników)
**Wzorzec:** Customer–Supplier
**Heurystyka:** R1 — BC-008 inicjuje zmianę numeru, BC-004 przetwarza; R4 — jedna relacja punktowa

**Uzasadnienie:**
- Przy zgubieniu dyskietki (E-065) BC-008 inicjuje żądanie zmiany numeru do BC-004
- BC-004 jest upstream (ma pulę numerów), BC-008 downstream (potrzebuje nowego numeru)
- Koordynacja: E-067 (Odpięto) + E-068 (Przypisano) w BC-004

**Co jest przekazywane:** żądanie zmiany numeru {stary-numer, zawodnik-ID} → nowy numer
**Komunikacja:** synchroniczna (w trakcie startu, pilne)

---

## Integracje zewnętrzne

### D-013 — I-001 Bank → BC-003 (przelew → opłata)

**Zewnętrzny system:** Bank (A-007)
**BC w systemie:** BC-003 (Opłata za Udział)
**Wzorzec:** ACL po stronie BC-003
**Heurystyka:** R2 — model bankowy (przelew, tytuł, kwota, nadawca) jest obcy wobec języka domeny (opłata, uczestnik, dystans); R3 — BC-003 chroni się przed brudnym modelem bankowym

**Uzasadnienie:**
- Bank pushuje zdarzenie przelewu (E-017) w swoim modelu (bankowym)
- ACL tłumaczy: "przelew od nadawcy X z tytułem Y na kwotę Z" → "opłata uczestnika N za dystans D"
- Brak informacji o mechanizmie push (webhook? polling?) → HS2-012

**Co jest przekazywane:** E-017 (Zarejestrowano przelew) z danymi bankowymi → ACL → E-018 (Zaaplikowano opłatę)
**Komunikacja:** asynchroniczna (event push z banku)
**ACL:** dopasowanie przelewu do uczestnika (po tytule lub ID uczestnika)

---

### D-014 — I-002 RFID → BC-009 (pomiar fizyczny → rejestracja)

**Zewnętrzny system:** Czytnik RFID (A-008)
**BC w systemie:** BC-009 (Pomiar Czasu i Wyniki)
**Wzorzec:** ACL po stronie BC-009
**Heurystyka:** R2 — model RFID (ID karty, ID czytnika, timestamp) jest techniczny, wymaga tłumaczenia na zdarzenie domenowe; R3 — BC-009 chroni swój model domenowy

**Uzasadnienie:**
- RFID wysyła surowe dane: {karta-ID, czytnik-ID, timestamp}
- ACL tłumaczy: "karta-ID" → "zawodnik-ID", "czytnik-ID" → "checkpoint" (km), "timestamp" → "czas pomiaru"
- ACL też filtruje: pomiary po 24h (E-088) zawodników "dla siebie" oznaczane specjalną flagą

**Co jest przekazywane:** E-078 (Przyłożenie dyskietki) → ACL → E-079 (Zarejestrowano pomiar czasu)
**Komunikacja:** synchroniczna (event push z hardware)

---

## Mapa wizualna (tekstowa)

```
[I-001 Bank] --ACL--> [BC-003 Opłata] 
[I-002 RFID] --ACL--> [BC-009 Pomiar]

[BC-001 Konfiguracja] --OHS/PL--> [BC-002 Rejestracja]
                                  [BC-003 Opłata]     
                                  [BC-005 Generacja]  

[BC-002 Rejestracja] --CS/event--> [BC-003 Opłata]
[BC-003 Opłata] --CS/event--> [BC-004 Numer]
[BC-003 Opłata] --ACL<BC-005-- [BC-005 Generacja] (Core czyta z BC-003)
[BC-003 Opłata] --ACL<BC-009-- [BC-009 Pomiar] (BC-009 czyta z BC-003)
[BC-004 Numer] --ACL<BC-005-- [BC-005 Generacja] (Core czyta z BC-004)
[BC-004 Numer] --CS--> [BC-008 Start] (przy zgubieniu dyskietki)
[BC-005 Generacja] --OHS/PE-001--> [BC-006 GrupyStartowe]
[BC-006 GrupyStartowe] --CS/PE-002--> [BC-007 Weryfikacja]
[BC-007 Weryfikacja] --CS--> [BC-008 Start]
[BC-008 Start] --CS/PE-003--> [BC-009 Pomiar]
[BC-009 Pomiar] --CS/PE-004--> [BC-010 Zakończenie]
```

---

## Zestawienie relacji

| ID    | Upstream (BC-ID) | Downstream (BC-ID) | Wzorzec | Heurystyka | Komunikacja | Co jest przekazywane |
|-------|-----------------|-------------------|---------|-----------|------------|---------------------|
| D-001 | BC-001 | BC-002, BC-003, BC-005 | OHS + Published Language | R4, R2 | synchroniczna | konfiguracja imprezy |
| D-002 | BC-002 | BC-003 | Customer–Supplier | R1, R2 | asynchroniczna (event) | PE-006: dane uczestnika |
| D-003 | BC-003 | BC-004 | Customer–Supplier | R1, R2 | asynchroniczna (event) | PE-005: potwierdzenie opłaty |
| D-004 | BC-003 | BC-005 | ACL (w BC-005) | R3, R2 | synchroniczna (snapshot) | status niedopłat uczestników |
| D-005 | BC-004 | BC-005 | ACL (w BC-005) | R3 | synchroniczna (snapshot) | lista uczestnik+numer |
| D-006 | BC-005 | BC-006 | OHS + Published Language | R4, R1 | asynchroniczna (event PE-001) | lista grup z przypisaniami |
| D-007 | BC-006 | BC-007 | Customer–Supplier | R1, R2 | synchroniczna (odczyt) | zamrożona lista startowa |
| D-008 | BC-007 | BC-008 | Customer–Supplier | R1, R2 | synchroniczna (odczyt) | status dopuszczenia zawodnika |
| D-009 | BC-008 | BC-009 | Customer–Supplier | R1, R2 | asynchroniczna (event PE-003) | czas startu per zawodnik |
| D-010 | BC-009 | BC-010 | Customer–Supplier | R1, R2 | asynchroniczna (event PE-004) | ranking wyników |
| D-011 | BC-003 | BC-009 | ACL (w BC-009) | R3, R2 | synchroniczna (snapshot) | opłacony dystans per zawodnik |
| D-012 | BC-004 | BC-008 | Customer–Supplier | R1, R4 | synchroniczna | nowy numer startowy |
| D-013 | I-001 Bank | BC-003 | ACL (w BC-003) | R2, R3 | asynchroniczna (event push) | przelew bankowy |
| D-014 | I-002 RFID | BC-009 | ACL (w BC-009) | R2, R3 | synchroniczna (event push) | odczyt RFID |
