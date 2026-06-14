# Bounded Contexts — System Zarządzania 24h Maratonem Rowerowym

Punkt startu: 1 subdomena = 1 BC. Każde odstępstwo zapisane z uzasadnieniem.

---

## BC-001 — Konfiguracja Maratonu

**Odpowiedzialność:** Definiowanie i publikacja parametrów imprezy (terminy, dystanse, limity, okna zmian, regulamin).
**Subdomena źródłowa:** SD-001 (Konfiguracja Maratonu)
**Typ:** Supporting
**Właściciel/Zespół:** Administratorzy / Główny organizator
**Pivotal events na granicach:** brak wejściowych; wyjście = dane konfiguracyjne konsumowane przez BC-002, BC-005

**Terminologia wewnętrzna:**
- "maraton" = konkretna edycja imprezy z parametrami
- "regulamin" = zbiór parametrów konfiguracyjnych (odróżnić od dokumentu PDF dla uczestników)
- "okno czasowe" = przedział dat dla danej operacji (generacja, zmiana grupy, zwrot pakietu)

**Zdarzenia:** E-001, E-002, E-003

**Uwagi:** BC-001 jest dostawcą konfiguracji dla wszystkich pozostałych BC. Relacja OHS (Open Host Service) — wystawia stabilne API konfiguracyjne. Nie ma logiki biznesowej poza walidacją spójności parametrów.

---

## BC-002 — Rejestracja

**Odpowiedzialność:** Przyjmowanie uczestników, zbieranie danych, wybór dystansu, zmiana dystansu (przed opłaceniem), potwierdzenie i anulowanie rejestracji.
**Subdomena źródłowa:** SD-002 (Rejestracja)
**Typ:** Supporting
**Właściciel/Zespół:** Administratorzy biura zawodów
**Pivotal events na granicach:**
- Wejście: brak (inicjowany przez uczestnika)
- Wyjście: PE-006 (Potwierdzono rejestrację) → otwiera BC-003

**Terminologia wewnętrzna:**
- "uczestnik" = osoba w trakcie rejestracji (przed wydaniem pakietu)
- "wniosek o zmianę dystansu" = dokument inicjujący zmianę w obrębie BC-002
- "potwierdzenie rejestracji" = stan finalizujący dane w BC-002

**Zdarzenia:** E-004, E-005, E-006, E-007, E-008, E-009, E-010, E-011, E-012, E-013, E-014, E-015

**Uwagi:** Q-006 (Uczestnik vs Zawodnik) otwarte — decyzja o modelu stanowym wpłynie na ten BC w Design Level. Relacja Q-004 (wygaśnięcie) — niejasna logika anulowania wymaga rozstrzygnięcia (HS2-001).

**Odstępstwo od reguły 1:1:** BC-002 scalony z obsługą zmiany dystansu w trakcie rejestracji (E-009..E-012), bo zmiana dystansu PRZED opłaceniem to wewnętrzna korekta danych rejestracyjnych — nie przekracza granicy BC. Zmiana dystansu PO opłaceniu → BC-003 (inna subdomena, inny właściciel).

---

## BC-003 — Opłata za Udział

**Odpowiedzialność:** Cykl płatniczy: inicjacja opłaty, oczekiwanie na przelew bankowy, aplikowanie opłat, wykrywanie i obsługa niedopłat, promocje, zwroty nadpłat, odpisanie uczestnictwa.
**Subdomena źródłowa:** SD-003 (Opłata za Udział)
**Typ:** Supporting
**Właściciel/Zespół:** Administratorzy finansowi / biuro zawodów
**Pivotal events na granicach:**
- Wejście: PE-006 (Potwierdzono rejestrację)
- Wyjście: PE-005 (Opłacono uczestnictwo) → otwiera BC-004

**Terminologia wewnętrzna:**
- "opłata" = kwota przypisana do uczestnika przez system
- "przelew" = zewnętrzne zdarzenie bankowe (inny język niż "opłata")
- "niedopłata" = różnica między kwotą należną a zapłaconą
- "odpisanie uczestnictwa" = akt umorzenia zobowiązania (nie to samo co anulowanie rejestracji)

**Zdarzenia:** E-016, E-017 (zewnętrzne), E-018, E-019, E-020, E-021, E-022, E-023, E-024, E-025, E-026, E-027, E-028, E-029

**Uwagi:** Integracja z bankiem (I-001) — wymaga ACL po stronie BC-003 (przelew bankowy → opłata domenowa). Q-013 (odpisanie uczestnictwa) otwarte — czy to BC-003 czy BC-010.

---

## BC-004 — Przydział Numeru Startowego

**Odpowiedzialność:** Generowanie i przypisanie unikalnego numeru startowego do uczestnika który opłacił, zarządzanie pulą numerów.
**Subdomena źródłowa:** SD-004 (Przydział Numeru Startowego)
**Typ:** Generic
**Właściciel/Zespół:** System (automatyczny)
**Pivotal events na granicach:**
- Wejście: PE-005 (Opłacono uczestnictwo)
- Wyjście: dane numerów → BC-005 (warunek generacji grup)

**Terminologia wewnętrzna:**
- "numer startowy" = unikalny identyfikator uczestnika w imprezie (≠ ID systemu)
- "pula numerów" = zestaw dostępnych numerów dla danej imprezy

**Zdarzenia:** E-030, E-068

**Uwagi:** Prostota logiki uzasadnia Generic. Jednak odpowiedzialność za "zastąpienie numeru" przy zgubieniu dyskietki (E-067, E-068) należy tu — wymaga koordynacji ze Start (BC-008). Kandydat na Shared Kernel z BC-008 jeśli okaże się że logika zastępowania jest złożona.

---

## BC-005 — Generacja Grup Startowych

**Odpowiedzialność:** Jednorazowe, automatyczne tworzenie grup startowych z listy uczestników spełniających warunki, w terminie określonym w regulaminie, z zastosowaniem reguły niedopłaty.
**Subdomena źródłowa:** SD-005 (Generacja Grup Startowych)
**Typ:** **Core**
**Właściciel/Zespół:** Algorytm systemowy (A-009) inicjowany przez Administratora (A-003)
**Pivotal events na granicach:**
- Wejście: dane uczestników z numerami (z BC-004), parametry z BC-001
- Wyjście: PE-001 (Wygenerowano ostateczne grupy) → otwiera BC-006

**Terminologia wewnętrzna:**
- "uczestnik gotowy do grupy" = opłacony + numer przypisany + (dla zmiany dystansu: potwierdzenie płatności nowego dystansu)
- "grupy startowe" = lista grup z przypisanymi uczestnikami i godzinami startu
- "ostateczne grupy" = stan po wykonaniu generacji, przed otwarciem okna zmian

**Zdarzenia:** E-031, E-032, E-033, E-034, E-035, E-036

**Uwagi:** Core Domain. Brak alternatywnego podziału — subdomena jednoznacznie mapuje na BC. Reguła niedopłaty (fakt domenowy #6) egzekwowana tutaj: uczestnik z nieopłaconą niedopłatą → trafia do grupy starego dystansu.

---

## BC-006 — Grupy Startowe (Zarządzanie)

**Odpowiedzialność:** Obsługa okna czasowego zmian grupy: przyjmowanie i walidacja wniosków od zawodników, egzekwowanie reguł (jednorazowość, wolne miejsca, dystans, opłata), dołączanie po terminie.
**Subdomena źródłowa:** SD-006 (Grupy Startowe / Zarządzanie)
**Typ:** Supporting
**Właściciel/Zespół:** Administratorzy biura zawodów
**Pivotal events na granicach:**
- Wejście: PE-001 (grupy muszą istnieć)
- Wyjście: PE-002 (Minął czas na zmiany) → dane przekazywane do BC-007

**Terminologia wewnętrzna:**
- "wniosek o zmianę grupy" = podanie uczestnika z wybraną grupą docelową
- "zmiana grupy" = akt przeniesienia między grupami (jednorazowy)
- "dołączenie po terminie" = wyjątkowe przypisanie do grupy dla uczestników którzy opłacili po standardowym terminie

**Zdarzenia:** E-037..E-054

**Uwagi:** H2 z Fazy 1 (nota o grupach jako listach startowych) → HS2-002. Pytanie Q-012 (kto może złożyć wniosek) otwarte.

---

## BC-007 — Weryfikacja i Kwalifikacja Startowa

**Odpowiedzialność:** Fizyczny check-in w biurze zawodów: weryfikacja tożsamości, podpisanie oświadczenia, wydanie pakietu startowego i dyskietki RFID, odmowa dopuszczenia do startu.
**Subdomena źródłowa:** SD-007 (Weryfikacja i Kwalifikacja Startowa)
**Typ:** Supporting
**Właściciel/Zespół:** Obsługa punktu kontrolnego (A-005)
**Pivotal events na granicach:**
- Wejście: PE-002 (zamrożona lista startowa)
- Wyjście: "Zawodnik dopuszczony do startu" (stan, nie osobny PE) → BC-008

**Terminologia wewnętrzna:**
- "weryfikacja biurowa" = sprawdzenie danych uczestnika wobec listy + oświadczenie
- "weryfikacja na starcie" = sprawdzenie fizyczne bezpośrednio przed startem (inna operacja!)
- "pakiet startowy" = fizyczny zestaw (koszulka, materiały, numer na ramę?, kaucja)
- "dyskietka RFID" = karta do pomiaru czasu (odrębny element od pakietu — H-009 otwarte)

**Zdarzenia:** E-055, E-056, E-057, E-058, E-059, E-060, E-061, E-062

**Uwagi:** H-008 z Fazy 1 (dwie weryfikacje) wyjaśnione — dwa różne akty w tym samym BC: biurowa (E-055) i przed-startowa (E-061). Jeden BC, dwa procesy. Q-008 (dyskietka RFID w pakiecie czy osobno) otwarte → HS2-003.

---

## BC-008 — Start Zawodników

**Odpowiedzialność:** Organizacja fizycznego startu: start grupowy i indywidualny, rejestracja czasu startu, zamknięcie listy startowej, obsługa spóźnień i zgubień dyskietek.
**Subdomena źródłowa:** SD-008 (Start Zawodników)
**Typ:** Supporting
**Właściciel/Zespół:** Organizator / Administrator / Sędzia trasy (A-004/A-003/A-006)
**Pivotal events na granicach:**
- Wejście: zawodnicy dopuszczeni z BC-007
- Wyjście: PE-003 (Wystartowano zawodników) → otwiera BC-009

**Terminologia wewnętrzna:**
- "start grupowy" = jednoczesny start grupy w wyznaczonym czasie
- "start indywidualny" = start zawodnika poza grupą (spóźnienie lub inna przyczyna)
- "kaucja" = zabezpieczenie finansowe za dyskietkę RFID (powiązane z BC-010 przy zwrocie)

**Zdarzenia:** E-063..E-077

**Uwagi:** Nakładanie się ról (A-004/A-003/A-006) w decyzjach startowych → HS2-004. Q-010 (nie stawił się na starcie) otwarte. E-068 (nowy numer) wymaga koordynacji z BC-004.

---

## BC-009 — Pomiar Czasu i Wyniki

**Odpowiedzialność:** Rejestracja pomiarów RFID z checkpointów, obliczanie międzyczasów i wyników, real-time display, logika klasyfikacji (24h, opłacony dystans, "jadą dla siebie"), generacja rankingu końcowego.
**Subdomena źródłowa:** SD-009 (Pomiar Czasu i Wyniki)
**Typ:** Supporting (złożony)
**Właściciel/Zespół:** System (A-009), z wejściem zewnętrznym RFID (A-008)
**Pivotal events na granicach:**
- Wejście: PE-003 (Wystartowano zawodników)
- Wyjście: PE-004 (Zakończono przejazdy wszystkich dystansów) → otwiera BC-010

**Terminologia wewnętrzna:**
- "pomiar" = akt fizyczny RFID (zewnętrzne zdarzenie)
- "wynik" = kalkulacja domenowa (czas, dystans, klasyfikacja)
- "ranking" = agregacja wyników po zakończeniu
- "nieklasyfikowany" = zawodnik który nie ukończył opłaconego dystansu w czasie (≠ "jadą dla siebie")
- "jadą dla siebie" = zawodnicy na trasie po upłynięciu 24h

**Zdarzenia:** E-078..E-090

**Uwagi:** Integracja z RFID (I-002) — wymaga ACL po stronie BC-009. Real-time display = osobny widok tej samej bazy (potwierdzone przez orkiestratora — nie osobny BC). Q-011 (dyskietka po zakończeniu udziału) otwarte → HS2-005.

---

## BC-010 — Zakończenie Zawodów i Ceremonial

**Odpowiedzialność:** Faza po-zawodowa: publikacja wyników końcowych, przyznanie nagród, generacja dyplomów, obsługa zwrotu pakietów startowych, obsługa kaucji (zwrot lub zatrzymanie).
**Subdomena źródłowa:** SD-010 (Zakończenie Zawodów i Ceremonial)
**Typ:** Supporting
**Właściciel/Zespół:** Główny organizator (A-004) + Obsługa punktu kontrolnego (A-005)
**Pivotal events na granicach:**
- Wejście: PE-004 (Zakończono przejazdy wszystkich dystansów)
- Wyjście: brak (koniec procesu)

**Terminologia wewnętrzna:**
- "dyplom" = dokument potwierdzający ukończenie (generowany automatycznie przez system)
- "nagroda" = fizyczna nagroda przyznawana przez organizatora (inny proces niż dyplom)
- "pakiet startowy" = fizyczny zestaw do oddania (koszulka + dyskietka + inne)
- "kaucja" = zwracana przy oddaniu pakietu lub zatrzymana jeśli pakiet nie wrócił

**Zdarzenia:** E-091..E-099

**Uwagi:** Q-005 (przepływ dyplomów) otwarte → HS2-006. Q-013 (odpisanie uczestnictwa) może dotyczyć tego BC lub BC-003.

---

## Zestawienie BC

| ID     | Nazwa | Typ | SD-ID | Wejście (PE) | Wyjście (PE) |
|--------|-------|-----|-------|-------------|-------------|
| BC-001 | Konfiguracja Maratonu | Supporting | SD-001 | — | dane konfiguracyjne |
| BC-002 | Rejestracja | Supporting | SD-002 | — | PE-006 |
| BC-003 | Opłata za Udział | Supporting | SD-003 | PE-006 | PE-005 |
| BC-004 | Przydział Numeru Startowego | Generic | SD-004 | PE-005 | dane numerów |
| BC-005 | Generacja Grup Startowych | **Core** | SD-005 | dane z BC-004 | PE-001 |
| BC-006 | Grupy Startowe (Zarządzanie) | Supporting | SD-006 | PE-001 | PE-002 |
| BC-007 | Weryfikacja i Kwalifikacja Startowa | Supporting | SD-007 | PE-002 | dopuszczenie do startu |
| BC-008 | Start Zawodników | Supporting | SD-008 | dopuszczenie | PE-003 |
| BC-009 | Pomiar Czasu i Wyniki | Supporting | SD-009 | PE-003 | PE-004 |
| BC-010 | Zakończenie Zawodów i Ceremonial | Supporting | SD-010 | PE-004 | — |
