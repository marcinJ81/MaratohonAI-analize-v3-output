---
phase: process-level
session: System Zarządzania 24h Maratonem Rowerowym
generated: 2026-06-14
status: completed
coverage: 78%
input-quality: accepted
lossy-files: 15
sources:
  - state/inputs/processed/phase-2-seed.md
  - state/phase-1-output.md
  - potwierdzone fakty domenowe (#1–#9 z sesji orkiestratora)
---

# Process Level — System Zarządzania 24h Maratonem Rowerowym

## Kontekst

Analizowano system obsługi 24-godzinnego maratonu rowerowego na poziomie procesów. Zakres objął pełen cykl życia imprezy: konfiguracja → rejestracja → opłaty → przydział numerów → generacja grup → zarządzanie grupami → weryfikacja startowa → start → pomiar czasu → zakończenie/ceremonial. Wiedza pochodzi z phase-1-output.md (83% coverage), phase-2-seed.md (65% coverage, 15 plików lossy) oraz 9 potwierdzonych faktów domenowych z sesji z użytkownikiem. Reguły biznesowe wstępne uwzględnione (bleed-through zatwierdzone).

---

## Subdomeny

| ID     | Nazwa | Odpowiedzialność | Typ | Testy rozstrzygające (T1–T4) | Zdarzenia z Fazy 1 |
|--------|-------|-----------------|-----|-----------------------------|--------------------|
| SD-001 | Konfiguracja Maratonu | Definiowanie i publikacja parametrów imprezy (terminy, dystanse, limity, okna zmian, regulamin) | Supporting | T1: CMS wystarczy; T2: formularz min. wystarczy; T3: stabilizuje się po konfiguracji | E-001, E-002, E-003 |
| SD-002 | Rejestracja | Przyjmowanie uczestników, wybór dystansu, zmiana dystansu przed opłaceniem, potwierdzenie/anulowanie | Supporting | T1: brak zewnętrznej platformy (wykluczone), ale logika prosta; T2: formularz wystarczy; T3: stabilizuje się | E-004..E-015 |
| SD-003 | Opłata za Udział | Cykl płatniczy: pełna opłata, promocja, niedopłata, zmiana dystansu, zwroty, odpisanie | Supporting | T1: systemy płatnicze nie pasują (przelew bankowy + własne reguły); T2: złożona ale nie wyróżniająca; T4: błąd finansowy kosztowny ale operacyjny | E-016..E-029 |
| SD-004 | Przydział Numeru Startowego | Generowanie i przypisanie unikalnego numeru uczestnikowi, zarządzanie pulą | Generic | T1: auto-increment/pula wystarczy; T2: najtańsze wystarczy; T3: stan "gotowe" natychmiast | E-030, E-068 |
| SD-005 | Generacja Grup Startowych | Jednorazowe automatyczne tworzenie grup z listy opłaconych uczestników z numerami, z regułami (niedopłata, limity, terminy) | **Core** | T1: brak gotowego produktu; T2: najtańsze NIE wystarczy (błąd = katastrofa); T3: brak stanu końcowego; T4: najdroższy błąd (złe grupy = błędna klasyfikacja) | E-031..E-036 |
| SD-006 | Grupy Startowe (Zarządzanie) | Okno zmian grupy: wnioski od zawodników, walidacja (jednorazowość, miejsca, dystans, opłata), blokada po terminie | Supporting | T1: workflow z walidacją dostępny; T2: prosta walidacja wystarczy; T3: stabilizuje się | E-037..E-054 |
| SD-007 | Weryfikacja i Kwalifikacja Startowa | Check-in: weryfikacja tożsamości, oświadczenie, wydanie pakietu i dyskietki RFID, weryfikacja na starcie | Supporting | T1: CRM/event mgmt wystarczy; T2: min. rozwiązanie wystarczy; T3: stabilizuje się | E-055..E-062 |
| SD-008 | Start Zawodników | Start grupowy/indywidualny, rejestracja czasu startu, zamknięcie listy, zgubione dyskietki, zakończenie przedwcześnie | Supporting | T1: standard operacyjny; T2: prosta lista wystarczy; T3: stabilizuje się | E-063..E-077 |
| SD-009 | Pomiar Czasu i Wyniki | RFID pomiary, międzyczasy, real-time display, logika klasyfikacji 24h, generacja rankingu | Supporting (złożony) | T1: systemy chip-timing istnieją, ale reguły klasyfikacji specyficzne; T2: min. NIE wystarczy (reguły klasyfikacji złożone); T4: błąd klasyfikacji = protest | E-078..E-090 |
| SD-010 | Zakończenie Zawodów i Ceremonial | Wyniki końcowe, nagrody, dyplomy, zwrot pakietów, obsługa kaucji | Supporting | T1: gotowe wzorce; T2: min. wystarczy; T3: stabilizuje się | E-091..E-099 |

---

## Core Domain

**Nazwa:** Generacja Grup Startowych (SD-005)
**Uzasadnienie:** Brak gotowego produktu realizującego tę funkcję. Algorytm grupowania jest specyficzny dla domeny (10 dystansów, okna czasowe, reguła niedopłaty, limity grup). Błąd w generacji grup to katastrofa operacyjna: błędne przypisania = błędna klasyfikacja = protesty = utrata reputacji organizatora. Logika ewoluuje (nowe dystanse, nowe reguły). Biznes inwestuje tu najwięcej wiedzy eksperckiej i uwagi (3 triggery temporalne, okno zmian, twarda blokada PE-002). Testy T1 (brak gotowego), T2 (minimum nie wystarczy), T3 (brak stanu końcowego), T4 (najdroższy błąd) — wszystkie 4 wskazują na Core.

---

## Pivotal Events

| ID     | Zdarzenie (E-ID) | Dlaczego pivotal | Źródło | Status | Granica między (BC-ID → BC-ID) |
|--------|-----------------|-----------------|--------|--------|-------------------------------|
| PE-001 | E-036 — Wygenerowano ostateczne grupy startowe | Punkt bez powrotu dla standardowego przypisania; zmiana właściciela danych (Generacja → Zarządzanie); widoczne dla uczestników | Faza 1 | aktualny | BC-005 → BC-006 |
| PE-002 | E-052 — Minął czas na ewentualne zmiany grupy | Twarda blokada zmian grupy; lista startowa zamrożona; trigger czasowy (nieodwracalny) | Faza 1 | aktualny | BC-006 → BC-007 |
| PE-003 | E-073 — Wystartowano zawodników | Aktywacja pomiaru czasu; zamknięcie listy startowej (E-075); przejście faza administracyjna → operacyjna; widoczne fizycznie | Faza 1 | aktualny | BC-008 → BC-009 |
| PE-004 | E-090 — Zakończono przejazdy wszystkich dystansów | Trigger 24h (upłynął czas); zamknięcie pomiaru dla wszystkich dystansów; wyzwala ranking + dyplomy | Faza 1 | aktualny | BC-009 → BC-010 |
| PE-005 | E-019/E-028 — Opłacono uczestnictwo/dystans | Warunek konieczny do przydziału numeru; zmiana stanu uczestnika (oczekuje → opłacony); otwiera drogę do generacji grupy; nieodwracalne (przelew bankowy) | Faza 2 | aktualny | BC-003 → BC-004 |
| PE-006 | E-013 — Potwierdzono rejestrację | Tworzy uczestnika formalnie w systemie; otwiera ścieżkę opłaty; zmiana właściciela (Rejestracja → Opłata) | Faza 2 | aktualny (niepewny — zależny od Q-004) | BC-002 → BC-003 |

---

## Bounded Contexts

| ID     | Nazwa | Odpowiedzialność | Subdomena źródłowa (SD-ID) | Typ | Właściciel / Zespół | Uwagi |
|--------|-------|-----------------|--------------------------|-----|--------------------|----|
| BC-001 | Konfiguracja Maratonu | Parametry imprezy: terminy, dystanse, limity, okna zmian, regulamin | SD-001 | Supporting | Główny organizator (A-004) | Dostawca konfiguracji dla wszystkich BC. OHS. |
| BC-002 | Rejestracja | Rejestracja uczestnika, wybór/zmiana dystansu (przed opłatą), potwierdzenie, anulowanie | SD-002 | Supporting | Administratorzy biura zawodów | Zmiana dystansu PO opłacie → BC-003. Q-006 (Uczestnik/Zawodnik) otwarte. |
| BC-003 | Opłata za Udział | Cykl płatniczy: inicjacja, przelew bankowy (ACL), aplikacja opłat, niedopłaty, promocje, zwroty, odpisanie | SD-003 | Supporting | Administratorzy finansowi | Integracja z bankiem (I-001) przez ACL. |
| BC-004 | Przydział Numeru Startowego | Generowanie i przypisanie numeru, zarządzanie pulą, zastąpienie numeru przy zgubieniu | SD-004 | Generic | System (automatyczny) | Prosta logika. Koordynacja z BC-008 przy zgubieniu dyskietki. |
| BC-005 | Generacja Grup Startowych | Jednorazowa automatyczna generacja grup, reguła niedopłaty, walidacja warunków | SD-005 | **Core** | Algorytm (A-009) + Administrator | Core Domain. ACL od BC-003 i BC-004. |
| BC-006 | Grupy Startowe (Zarządzanie) | Okno zmian grupy: wnioski, walidacja reguł, blokada po PE-002, dołączenie po terminie | SD-006 | Supporting | Administratorzy biura zawodów | Aktywny tylko między PE-001 a PE-002. |
| BC-007 | Weryfikacja i Kwalifikacja Startowa | Check-in biurowy (tożsamość + oświadczenie + pakiet + dyskietka) + weryfikacja na starcie | SD-007 | Supporting | Obsługa punktu kontrolnego (A-005) | H-008 rozstrzygnięte: dwa procesy w jednym BC. |
| BC-008 | Start Zawodników | Start grupowy/indywidualny, rejestracja czasu, zamknięcie zapisów, dyskietki, nie-stawił-się | SD-008 | Supporting | Organizator/Administrator/Sędzia | Wyzwala PE-003 → aktywuje BC-009. |
| BC-009 | Pomiar Czasu i Wyniki | RFID pomiary (ACL), klasyfikacja 24h, real-time display, ranking końcowy | SD-009 | Supporting (złożony) | System (A-009) + RFID (A-008) | Real-time display = osobny widok (ta sama baza). ACL od RFID i BC-003. |
| BC-010 | Zakończenie Zawodów i Ceremonial | Wyniki końcowe, nagrody, dyplomy (auto), zwrot pakietów, kaucje | SD-010 | Supporting | Główny organizator (A-004) + Obsługa (A-005) | Q-005 (dyplomy) otwarte. |

---

## Przepływy procesów

### P-001 Konfiguracja Maratonu — BC-001

**Wyzwalacz:** A-004 Główny organizator — decyzja o przeprowadzeniu edycji
**Rezultat:** E-003 Opublikowano regulamin maratonu

**Kroki:**
1. [A-004] --[Zarejestruj maraton]--> [BC-001] ==> [E-001 Zarejestrowano maraton]
2. [A-004] --[Skonfiguruj parametry]--> [BC-001] ==> [E-002 Skonfigurowano parametry maratonu]
3. [A-004] --[Opublikuj regulamin]--> [BC-001] ==> [E-003 Opublikowano regulamin maratonu]

**Read modele:**
| Krok | Aktor | Co musi widzieć |
|------|-------|----------------|
| 2 | Organizator | Poprzednie edycje, wzorzec parametrów, lista dystansów |
| 3 | Organizator | Komplet parametrów do weryfikacji |

**Reguły biznesowe:**
| ID     | Reguła | Krok |
|--------|--------|------|
| RB-001 | Regulamin musi zawierać terminy, dystanse, limity, termin generacji, okno zmian grupy | 2 |
| RB-002 | Dystanse: 50,100,150,200,300,400,500,600,650,700 km | 2 |
| RB-003 | Konfiguracja niezmieniana po opublikowaniu (lub z notyfikacją uczestników) | 3 |

**Wyjątki:**
| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-001 | Parametry niekompletne | Odmowa E-003 | Powrót do kroku 2 |
| EX-002 | Zmiana po publikacji | Zmiana + notyfikacja uczestników | HS2-007 |

**Przepływ odwrotny:** Brak mechanizmu cofnięcia publikacji.
**Weryfikacja odwrotną narracją:** tak — brak luk w ścieżce głównej.

---

### P-002 Rejestracja Uczestnika — BC-002

**Wyzwalacz:** A-002 Uczestnik — decyzja o zgłoszeniu
**Rezultat:** E-013 Potwierdzono rejestrację (PE-006)

**Kroki:**
1. [A-002] --[Zarejestruj uczestnika]--> [BC-002] ==> [E-004 Zarejestrowano uczestnika]
2. [A-002] --[Wybierz dystans]--> [BC-002] ==> [E-005 Wybrano dystans]
3. [A-002] --[Potwierdź dane kontaktowe]--> [BC-002] ==> [E-006 Potwierdzono dane kontaktowe]
4. [A-009 System] --[Zweryfikuj dane]--> [BC-002] ==> [E-007 Zweryfikowano zawodnika]
5. [A-009 System] --[Wylicz opłatę]--> [BC-002] ==> [E-008 Wyliczono opłatę]
6. [A-009 System] --[Potwierdź rejestrację]--> [BC-002] ==> [E-013 Potwierdzono rejestrację] **→ PE-006**

**Ścieżka A — Zmiana dystansu podczas rejestracji:**
A1. [A-002/A-003] --[Złóż wniosek o zmianę dystansu]--> [BC-002] ==> [E-009]
A2. [A-003/A-009] --[Zmień dystans]--> [BC-002] ==> [E-010]
A3. [A-009] --[Wylicz opłatę za nowy dystans]--> [BC-002] ==> [E-011]
A4. [A-009] --[Poinformuj o zmianie]--> [BC-002] ==> [E-012] → powrót do kroku 6

**Ścieżka B — Przekroczenie terminu płatności:**
B1. [Trigger czasowy] --> [BC-002] ==> [E-015 Przekroczono termin płatności]
B2. [A-009/A-003] --[Anuluj rejestrację]--> [BC-002] ==> [E-014 Anulowano rejestrację]
**UWAGA:** Automatyczne vs manualne → Q-004 / HS2-001 (bloker)

**Read modele:**
| Krok | Aktor | Co musi widzieć |
|------|-------|----------------|
| 1-3 | Uczestnik | Formularz, lista dystansów, limity miejsc (z BC-001) |
| 5 | System | Cennik dystansów (BC-001), promocje |

**Reguły biznesowe:**
| ID     | Reguła | Krok |
|--------|--------|------|
| RB-004 | Rejestracja tylko w otwartym okresie rejestracji | 1 |
| RB-005 | Uczestnik rejestruje się na dokładnie jeden dystans | 2 |
| RB-006 | Zmiana dystansu możliwa tylko przed potwierdzeniem opłaty | A1 |
| RB-007 | W trakcie opłacania nie można zmienić dystansu (blokada BC-002/BC-003) | — |

**Wyjątki:**
| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-003 | Brak miejsc na dystansie | Odmowa E-005 | Inny dystans lub rezygnacja |
| EX-005 | Weryfikacja negatywna | Wstrzymanie rejestracji | Interwencja A-003 |
| EX-006 | Nieopłacone w terminie | E-015 + Q-004 open | HS2-001 |

**Weryfikacja odwrotną narracją:** tak — luka: brak triggera czasowego E-015 → HS2-001.

---

### P-003 Opłata za Udział — BC-003

**Wyzwalacz:** PE-006 (E-013) → uczestnik musi opłacić
**Rezultat:** PE-005 (E-019/E-028 Opłacono uczestnictwo)

**Kroki (ścieżka 1 — pełna opłata):**
1. [A-002/A-003] --[Rozpocznij opłacenie]--> [BC-003] ==> [E-016 Rozpoczęto opłacenie]
2. [A-007 Bank] --[przelew bankowy]--> [I-001 → ACL → BC-003] ==> [E-017 Zarejestrowano przelew]
3. [A-009] --[Zaaplikuj opłatę]--> [BC-003] ==> [E-018 Zaaplikowano opłatę]
4. [A-009] --[automatycznie]--> [BC-003] ==> [E-019 Opłacono uczestnictwo] **→ PE-005**

**Ścieżka 2 (promocja -100%):**
3a. [A-004/A-009] --[Zaaplikuj promocję]--> [BC-003] ==> [E-020] → E-019

**Ścieżka 3 (niedopłata):**
3b. [A-009] --[Wykryj niedopłatę]--> [BC-003] ==> [E-021]
3c. [A-002/A-003] --[Opłać niedopłatę]--> [BC-003] ==> [E-022] → E-017 → E-023 → powrót do 4

**Ścieżka 4 (zmiana dystansu po opłaceniu):**
[A-002] → E-009 (→ HS2-009 duplikat?) → E-024 → przelew → E-025 Opłacono zmianę **→ PE-005**

**Ścieżka 5 (nadpłata):**
[A-009] → E-026 Wyliczono zwrot → E-027 Zwrócono nadpłatę (HS2-010: mechanizm zwrotu)

**Ścieżka 6 (odpisanie):**
[A-004/A-003] → E-029 Odpisano uczestnictwo (HS2-011: Q-013)

**Read modele:**
| Krok | Aktor | Co musi widzieć |
|------|-------|----------------|
| 1 | Uczestnik | Kwota do zapłaty, nr konta, termin |
| 3 | System | Przelewy do dopasowania do uczestnika |

**Reguły biznesowe:**
| ID     | Reguła | Krok |
|--------|--------|------|
| RB-009 | W trakcie opłacania nie można zmienić dystansu (blokada) | 1 |
| RB-010 | Przelew musi być dopasowany do uczestnika | 2 (ACL) |
| RB-011 | Uczestnik z nieopłaconą niedopłatą w PE-001 → stary dystans | 3b (stan używany przez BC-005) |
| RB-012 | Promocja -100% eliminuje przelew bankowy | ścieżka 2 |

**Wyjątki:**
| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-007 | Przelew niedopasowany | Ręczne dopasowanie przez A-003 | HS2-012 |
| EX-008 | Brak opłaty w żadnym terminie | Q-004 open | HS2-001 |

**Weryfikacja odwrotną narracją:** tak — luki: HS2-012 (dopasowanie przelewu), HS2-010 (zwrot nadpłaty).

---

### P-004 Przydział Numeru Startowego — BC-004

**Wyzwalacz:** PE-005 (E-019/E-028) → automatyczny
**Rezultat:** E-030 Przypisano numer startowy

**Kroki:**
1. [PE-005 → polityka POL-005] --[Przypisz numer startowy]--> [BC-004] ==> [E-030]

**Ścieżka B (zastąpienie przy zgubieniu dyskietki z BC-008):**
B1. [A-003] --[Odepnij numer]--> [BC-004] ==> [E-067]
B2. [A-005/A-003] --[Przypisz nowy numer]--> [BC-004] ==> [E-068]

**Reguły biznesowe:**
| ID     | Reguła | Krok |
|--------|--------|------|
| RB-013 | Numer unikalny w edycji | 1 |
| RB-014 | Numer tylko dla opłaconego uczestnika | 1 |
| RB-015 | Zastąpienie: odczep stary przed przypisaniem nowego | B1 przed B2 |

**Weryfikacja odwrotną narracją:** tak — luka: Q-007 (synchroniczność BC-003→BC-004) → HS2-014.

---

### P-005 Generacja Grup Startowych — BC-005 (CORE DOMAIN)

**Wyzwalacz:** A-003 Administrator — inicjacja w terminie z regulaminu (BC-001)
**Rezultat:** PE-001 (E-036 Wygenerowano ostateczne grupy)

**Kroki:**
1. [A-003] --[Generuj grupy startowe]--> [BC-005] ==> [walidacja warunków]
2. [A-009] --[Sprawdź warunki: liczba ≥ X, termin, grupy nie istnieją]--> [BC-005] ==> [warunki OK]
3. [A-009] --[Pobierz snapshot uczestników z BC-004 + statusy opłat z BC-003 (przez ACL)]--> [BC-005] ==> [lista gotowa]
4. [A-009] --[Zastosuj regułę niedopłaty: uczestnik z nieopłaconą niedopłatą → stary dystans]--> [BC-005] ==> [lista z przypisanymi dystansami]
5. [A-009] --[Wykonaj algorytm generacji: grupy per dystans, per godzina startu, limity]--> [BC-005] ==> [E-033 Wygenerowano grupy]
6. [A-009] --[Przypisz uczestnika do grupy]--> [BC-005] ==> [E-031/E-032 per uczestnik]
7. [A-009] --[Finalizuj grupy]--> [BC-005] ==> [E-035 Grupy zostały wygenerowane]
8. [A-009] --[Opublikuj wynik]--> [BC-005] ==> [E-036 Wygenerowano ostateczne grupy] **→ PE-001**

**Ścieżka porażki:**
2b. [warunki niespełnione] → [E-034 Nie udało się wygenerować grup]

**Read modele:**
| Krok | Aktor | Co musi widzieć |
|------|-------|----------------|
| 1 | Administrator | Status generacji, data vs termin, liczba gotowych uczestników |
| 2 | System | Parametry z BC-001 (termin, limit X, harmonogram) |
| 3 | System | Uczestnicy z E-030 + PE-005 + statusy niedopłat (BC-003 przez ACL) |

**Reguły biznesowe:**
| ID     | Reguła | Krok |
|--------|--------|------|
| RB-016 | Generowanie idempotentne (tylko jeśli grupy nie istnieją) | 2 |
| RB-017 | Min. X uczestników z numerem (Q-009 — wartość X nieznana) | 2 |
| RB-018 | Generowanie w terminie z regulaminu (BC-001) | 2 |
| RB-019 | Uczestnik z nieopłaconą niedopłatą → stary dystans (fakt #6) | 4 |
| RB-020 | Uczestnik musi mieć numer startowy (E-030) | 3 |
| RB-021 | Grupy mają limity miejsc (z BC-001) | 5 |

**Wyjątki:**
| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-011 | Za mało uczestników (< X) | E-034 | Czekanie lub zmiana parametrów |
| EX-012 | Uczestnik z nieopłaconą niedopłatą | Stary dystans | E-036 zawiera go w starym dystansie |
| EX-013 | Uczestnik bez numeru startowego | Pominięty w generacji | E-046 Nie przypisano do grupy |

**Weryfikacja odwrotną narracją:** tak — luki: HS2-016 (algorytm kolejności), Q-009 (wartość X), HS2-017 (opłata niedopłaty między PE-001 a PE-002).

---

### P-006 Zmiana Grupy Startowej — BC-006

**Wyzwalacz:** A-001 Zawodnik (po PE-001, przed PE-002)
**Rezultat:** E-043 Zmieniono grupę LUB E-044 Nie udało się zmienić

**Kroki:**
1. [A-001] --[Złóż wniosek o zmianę grupy]--> [BC-006] ==> [E-037]
2. [A-009] --[Weryfikuj jednorazowość]--> [BC-006] ==> [E-038 Nie złożono wcześniej wniosku]
3. [A-003] --[Przyjmij/odrzuć wniosek]--> [BC-006] ==> [E-039/E-040] (POL-011 — ludzka!)
4. [A-009] --[Weryfikuj warunki: okno, miejsca, dystans, opłata]--> [BC-006] ==> [warunki OK]
5. [A-009] --[Wykonaj zmianę]--> [BC-006] ==> [E-041 Grupa zmieniona]
6. [A-009] --[Potwierdź]--> [BC-006] ==> [E-043 Zmieniono grupę]

**Odrzucenie:** E-045 (brak opłaty), E-044 (inne przyczyny), E-040 (wniosek odrzucony przez A-003)

**Ścieżka B (dołączenie po terminie generacji):**
B1. [PE-005 po PE-001] → [E-049 Dołączono do grupy] / [E-050 Nadobiegnięto do grupy]

**Reguły biznesowe:**
| ID     | Reguła | Krok |
|--------|--------|------|
| RB-022 | Brak zapłaty różnicy = brak zmiany | 4 / POL-010 |
| RB-023 | Zmiana tylko w oknie (PE-001 do PE-002) | 4 |
| RB-024 | Zmiana jednorazowa | 2 |
| RB-025 | Zmiana tylko w obrębie tego samego dystansu | 4 |
| RB-026 | Grupy muszą istnieć (PE-001) | 4 |
| RB-027 | Grupa docelowa musi mieć wolne miejsca | 4 |

**Wyjątki:**
| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-014 | Po PE-002 | Odmowa automatyczna | E-044/E-048 |
| EX-015 | Wcześniejsza zmiana | Odmowa (RB-024) | E-044 |
| EX-016 | Brak miejsc | Odmowa | E-044 |
| EX-018 | Brak opłaty | E-045 | Odmowa do opłacenia |

**Weryfikacja odwrotną narracją:** tak — luki: HS2-019 (opłata przez BC-003?), HS2-020 (manual vs auto), HS2-017 (opłata niedopłaty między PE-001 a PE-002).

---

### P-007 Weryfikacja i Kwalifikacja Startowa — BC-007

**Wyzwalacz:** A-001 Zawodnik lub A-005 Obsługa — check-in (po PE-002, przed PE-003)
**Rezultat:** E-059 + E-060 Wydano pakiet + dyskietkę → zawodnik dopuszczony do startu

**Część 1 — Weryfikacja biurowa:**
1. [A-005] --[Zweryfikuj tożsamość]--> [BC-007] ==> [E-055 Zweryfikowano tożsamość]
2. [A-001] --[Podpisz oświadczenie]--> [BC-007] ==> [E-057 Podpisano oświadczenie]
3. [A-005] --[Wydaj pakiet startowy]--> [BC-007] ==> [E-059 Wydano pakiet]
4. [A-005] --[Wydaj dyskietkę RFID]--> [BC-007] ==> [E-060 Wydano dyskietkę] (HS2-003)

**Część 2 — Weryfikacja na starcie:**
5. [A-005/A-006] --[Zweryfikuj zawodnika przed startem]--> [BC-007] ==> [E-061 Zweryfikowano przed startem]

**Ścieżki negatywne:** E-056 (tożsamość nie OK), E-058 (brak oświadczenia), E-062 (weryfikacja startowa negatywna)

**Reguły biznesowe:**
| ID     | Reguła | Krok |
|--------|--------|------|
| RB-028 | Weryfikacja na podstawie zamrożonej listy startowej (po PE-002) | 1 |
| RB-029 | Zawodnik bez oświadczenia nie otrzymuje pakietu | 2 |
| RB-030 | Weryfikacja na starcie tylko dla zawodników po weryfikacji biurowej | 5 |

**Wyjątki:**
| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-019 | E-056 | Brak pakietu | HS2-021 (brak odwołania) |
| EX-022 | Nie stawił się na check-in | ? | HS2-022 / Q-010 |

**Weryfikacja odwrotną narracją:** tak — luki: HS2-003 (dyskietka w pakiecie?), HS2-021 (brak odwołania), HS2-022 (Q-010).

---

### P-008 Start Zawodników — BC-008

**Wyzwalacz:** A-004/A-003/A-006 — sygnał startowy
**Rezultat:** PE-003 (E-073 Wystartowano zawodników) → aktywacja BC-009

**Kroki:**
1. [A-004/A-003/A-006] --[Wystartuj zawodników]--> [BC-008] ==> [E-070 Wystartowano grupę]
2. [A-009] --[Zarejestruj czas startu]--> [BC-008] ==> [E-071]
3. [A-009] --[Oznacz jako wystartowanych]--> [BC-008] ==> [E-072] (per zawodnik)
4. [A-009] --[Zamknij zapisy na dystans]--> [BC-008] ==> [E-075 Zamknięto zapisy]
5. [A-009] --[Potwierdź start wszystkich]--> [BC-008] ==> [E-073 Wystartowano zawodników] **→ PE-003**

**Ścieżka B (start indywidualny):** E-063 → E-064 (wyciągnięto z grupy) → inny czas odniesienia (HS2-023)
**Ścieżka C (zgubienie dyskietki):** E-065 → E-066 (kaucja) → E-067 (BC-004) → E-068 (BC-004) → E-069 (nowa dyskietka)
**Ścieżka D (zakończenie przedwcześnie):** E-076 (Q-011 → HS2-005)
**Ścieżka E (nie stawił się):** E-077 (Q-010 → HS2-022)

**Reguły biznesowe:**
| ID     | Reguła | Krok |
|--------|--------|------|
| RB-031 | Start w godzinie z harmonogramu grup (BC-001) | 1 |
| RB-032 | Lista startowa zamknięta po PE-002 | 4 |
| RB-033 | Kaucja pobierana przy zgubieniu dyskietki | C2 |
| RB-034 | Stary numer odpięty przed przypisaniem nowego | C3 przed C4 |

**Weryfikacja odwrotną narracją:** tak — luki: HS2-023 (start indywidualny czas), HS2-022 (Q-010), HS2-005 (Q-011).

---

### P-009 Pomiar Czasu i Wyniki — BC-009

**Wyzwalacz:** PE-003 (E-073) → aktywacja pomiaru
**Rezultat:** PE-004 (E-090 Zakończono przejazdy wszystkich dystansów)

**Cykl RFID (ciągły):**
1. [A-001] --[Przyłóż dyskietkę]--> [I-002 RFID] ==> [E-078]
2. [A-008 → ACL → BC-009] --[Zarejestruj pomiar]--> [BC-009] ==> [E-079 Zarejestrowano pomiar]
3. [A-009] --[Wyświetl real-time]--> [BC-009 read model] ==> [E-080]

**Logika klasyfikacji:**
4. [A-009] --[Sprawdź ukończenie dystansu]--> [BC-009] ==> [E-081 Zawodnik zakończył przejazd]

**Ścieżka przedłużenia dystansu:**
A1. [A-001] → E-082 → [A-004/A-003] → E-083 → E-084 (dodatkowy pomiar) → E-085 (HS2-024)

**Zamknięcie zawodów:**
5. [Trigger 24h] → [E-088 Upłynął czas maratonu]
6. [A-009] --[Podlicz czasy]--> [BC-009] ==> [E-086]
7. [A-009] --[Wygeneruj ranking]--> [BC-009] ==> [E-089 Wygenerowano ranking] (HS2-027)
8. [A-009] --[per dystans]--> [BC-009] ==> [E-087 Zakończono przejazd na dystansie]
9. [A-009] --[wszystkie dystanse]--> [BC-009] ==> [E-090 Zakończono przejazdy] **→ PE-004**

**Read modele:**
| Krok | Aktor | Co musi widzieć |
|------|-------|----------------|
| 1-3 | Wszyscy | Real-time board: pozycja, czasy, ranking live |
| 6 | System | Wszystkie pomiary + opłacony dystans (z BC-003 przez ACL, D-011) |

**Reguły biznesowe:**
| ID     | Reguła | Krok |
|--------|--------|------|
| RB-035 | Pomiar tylko dla aktywnych zawodników (po PE-003) | 2 |
| RB-036 | Klasyfikacja na opłaconym dystansie nawet jeśli próbował dłuższego (fakt #7) | 6-7 |
| RB-037 | Nieklasyfikowany jeśli nie ukończył opłaconego dystansu w 24h (fakt #7) | 6-7 |
| RB-038 | Trigger zamknięcia = 24h (fakt #7) | 5 |
| RB-039 | Checkpointy co 50 km (fakt #8) | 2 |
| RB-040 | 1 pętla = 100 km; dystanse: 50-700 km; niestandardowe: 50, 150, 650 km (fakt #8) | konfiguracja |

**Wyjątki:**
| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-027 | Nie ukończył opłaconego dystansu | Status "nieklasyfikowany" | E-087 z flagą |
| EX-028 | Zakończył udział przedwcześnie (E-076) | Pomiar zatrzymany | HS2-005 (Q-011) |
| EX-029 | Błąd RFID | ? | HS2-025 (brak fallback) |
| EX-030 | Jedzie po 24h "dla siebie" | E-079 rejestrowane ale poza klasyfikacją | Flaga w BC-009 |

**Weryfikacja odwrotną narracją:** tak — luki: HS2-027 (kryteria rankingu), HS2-024 (przedłużenie + dopłata), HS2-025 (fallback RFID), HS2-006 (ranking → dyplomy).

---

### P-010 Zakończenie Zawodów i Ceremonial — BC-010

**Wyzwalacz:** PE-004 (E-090) → automatyczny trigger
**Rezultat:** Dyplomy wydane, nagrody przyznane, pakiety zwrócone, kaucje rozliczone

**Kroki:**
1. [PE-004 → POL-017] --[Opublikuj wyniki]--> [BC-010] ==> [E-091 Opublikowano wyniki]
2. [A-004] --[Przyznaj nagrodę]--> [BC-010] ==> [E-092 Przyznano nagrodę]
3. [A-009] --[Wygeneruj dyplomy]--> [BC-010] ==> [E-093 Wygenerowano dyplom] per zawodnik (HS2-006)
4. [A-001/uprawniona osoba] --[Zwróć pakiet]--> [BC-010] ==> [E-095 Zwrócono pakiet]
5. [A-005] --[Zwróć kaucję]--> [BC-010] ==> [E-096 Kaucja zwrócona]
6. [A-001] --[Odbierz dyplom]--> [BC-010] ==> [E-094 Otrzymano dyplom] (HS2-028)

**Ścieżka B (brak zwrotu pakietu):**
B1. [Trigger czasowy] → [E-097 Upłynął czas zwrotu]
B2. [A-009] → [E-098 Nie zwrócono pakietu]
B3. [A-005/A-004] → [E-099 Kaucja zatrzymana]

**Reguły biznesowe:**
| ID     | Reguła | Krok |
|--------|--------|------|
| RB-041 | Wyniki publikowane automatycznie po PE-004 + E-089 | 1 |
| RB-042 | Dyplom generowany automatycznie przez system | 3 |
| RB-043 | Kaucja zwracana tylko przy zwrocie pakietu | 5 |
| RB-044 | Kaucja zatrzymana jeśli brak zwrotu w terminie | B3 |
| RB-045 | Zawodnik lub osoba uprawniona może zwrócić pakiet | 4 |

**Wyjątki:**
| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-031 | Zawodnik nieklasyfikowany | Dyplom uczestnictwa? | HS2-006 |
| EX-032 | Pakiet nie zwrócony | Kaucja zatrzymana | E-097→E-099 |
| EX-033 | Osoba uprawniona zwraca pakiet | Wymaga mechanizmu upoważnienia | HS2-029 |
| EX-034 | Korekta wyników | Brak mechanizmu | HS2-026 |

**Weryfikacja odwrotną narracją:** tak — luki: HS2-006 (kto kwalifikuje do dyplomu), HS2-027 (kryteria nagradzania), HS2-028 (sposób odbioru), HS2-026 (korekta wyników).

---

## Polityki (Policies)

| ID      | Zdarzenie wyzwalające (E-ID) | Polityka / Reguła | Komenda / Akcja | BC (ID) | Typ | Przekracza granicę BC? |
|---------|------------------------------|-------------------|-----------------|---------|-----|------------------------|
| POL-001 | E-006 Potwierdzono dane | Dane muszą być kompletne | Zweryfikuj zawodnika | BC-002 | automatyczna | nie |
| POL-002 | E-005/E-010 Wybrano/zmieniono dystans | System wylicza opłatę z cennika (BC-001) | Wylicz opłatę | BC-002 | automatyczna | tak (→ D-001) |
| POL-003 | E-013 Potwierdzono rejestrację (PE-006) | Uczestnik musi opłacić | BC-003 aktywne dla uczestnika | BC-003 | automatyczna | tak (D-002) |
| POL-004 | E-017+E-018 Przelew+aplikacja | Kwota wpłacona < należna → niedopłata | Wykryj niedopłatę → E-021 | BC-003 | automatyczna | nie |
| POL-005 | E-019/E-028 Opłacono (PE-005) | Każdy opłacony uczestnik dostaje numer | Przypisz numer → E-030 | BC-004 | automatyczna | tak (D-003) |
| POL-006 | E-033 Generacja grup (wewnątrz) | Uczestnik z nieopłaconą niedopłatą → stary dystans (fakt #6) | Przypisz do grupy starego dystansu | BC-005 | automatyczna | tak (czyta BC-003 → D-004) |
| POL-007 | E-052 Minął czas zmian (PE-002) | Po PE-002 żadna zmiana grupy | BC-006 zamknięty na zapis | BC-006 | automatyczna (trigger czasowy) | nie |
| POL-008 | E-037 Złożono wniosek | Jednorazowość zmiany grupy | Sprawdź historię → odmowa | BC-006 | automatyczna | nie |
| POL-009 | E-039 Przyjęto wniosek | Wolne miejsca w grupie docelowej | Sprawdź miejsca | BC-006 | automatyczna | nie |
| POL-010 | E-039 Przyjęto wniosek | Brak zapłaty różnicy = brak zmiany | Sprawdź status opłaty | BC-006 | automatyczna / ludzka | tak (czyta BC-003 → D-006) |
| POL-011 | E-037 Złożono wniosek | Administrator ręcznie przegląda wniosek | Przyjmij/odrzuć → E-039/E-040 | BC-006 | **ludzka** (krucha!) | nie |
| POL-012 | E-059+E-060 Wydano pakiet+dyskietkę | Zawodnik z pakietem dopuszczony do startu | Status → "gotowy" | BC-007 | automatyczna | tak (D-008) |
| POL-013 | E-073 Wystartowano (PE-003) | Lista startowa zamknięta | Zamknij zapisy → E-075 | BC-008 | automatyczna | nie |
| POL-014 | E-073 Wystartowano (PE-003) | Po starcie RFID rejestruje pomiary | BC-009 aktywny | BC-009 | automatyczna | tak (D-009) |
| POL-015 | E-088 Upłynął czas maratonu (24h) | Trigger zamknięcia (fakt #7); zawodnicy po czasie → "dla siebie" | Podlicz, generuj ranking, zamknij | BC-009 | automatyczna (trigger czasowy) | nie |
| POL-016 | E-086 Podliczono czasy | Klasyfikacja na opłaconym dystansie (fakt #7) | Klasyfikacja per zawodnik | BC-009 | automatyczna | nie |
| POL-017 | E-089 Wygenerowano ranking (po PE-004) | System automatycznie generuje dyplomy | Wygeneruj dyplom → E-093 | BC-010 | automatyczna | tak (D-010) |
| POL-018 | E-097 Upłynął czas zwrotu pakietu | Brak zwrotu → kaucja zatrzymana | E-099 | BC-010 | automatyczna/ludzka (HS2-030) | nie |
| POL-019 | E-029 Odpisano uczestnictwo | Decyzja administratora/organizatora | Usuń zobowiązanie finansowe | BC-003 | **ludzka** | nie |

---

## Relacje między kontekstami

| ID    | Upstream (BC-ID) | Downstream (BC-ID) | Wzorzec | Heurystyka | Komunikacja | Co jest przekazywane | Uwagi |
|-------|-----------------|-------------------|---------|-----------|------------|---------------------|-------|
| D-001 | BC-001 | BC-002, BC-003, BC-005 | OHS + Published Language | R4, R2 | synchroniczna (odczyt) | konfiguracja imprezy (terminy, cennik, limity, parametry grup) | BC-001 nie zmienia modelu pod konsumentów — Conformist dla downstream |
| D-002 | BC-002 | BC-003 | Customer–Supplier | R1, R2 | asynchroniczna (event PE-006) | dane uczestnika: ID, dystans, wyliczona opłata | BC-002 planuje interfejs pod BC-003 |
| D-003 | BC-003 | BC-004 | Customer–Supplier | R1, R2 | asynchroniczna (event PE-005) | potwierdzenie opłaty: {uczestnik-ID, opłacony-dystans} | BC-003 upstream, BC-004 downstream |
| D-004 | BC-003 | BC-005 | ACL (w BC-005) | R3, R2 | synchroniczna (snapshot przy generacji) | status niedopłat: {uczestnik-ID, flaga-niedopłaty, dystans-stary} | Core Domain (BC-005) chroni się przez ACL |
| D-005 | BC-004 | BC-005 | ACL (w BC-005) | R3 | synchroniczna (snapshot przy generacji) | lista: {uczestnik-ID, numer-startowy, dystans} | Core Domain (BC-005) chroniony przez ACL od Generic |
| D-006 | BC-005 | BC-006 | OHS + Published Language | R4, R1 | asynchroniczna (event PE-001) | lista grup: {grupa-ID, dystans, godzina-startu, lista-uczestników} | BC-005 Core — nie słucha BC-006 |
| D-007 | BC-006 | BC-007 | Customer–Supplier | R1, R2 | synchroniczna (odczyt po PE-002) | zamrożona lista startowa: {zawodnik-ID, numer, dystans, grupa, godzina} | |
| D-008 | BC-007 | BC-008 | Customer–Supplier | R1, R2 | synchroniczna (odczyt) | status dopuszczenia: {zawodnik-ID, czy-dopuszczony, ID-dyskietki-RFID} | |
| D-009 | BC-008 | BC-009 | Customer–Supplier | R1, R2 | asynchroniczna (event PE-003) | PE-003 + {zawodnik-ID, czas-startu, dystans, numer-dyskietki} | |
| D-010 | BC-009 | BC-010 | Customer–Supplier | R1, R2 | asynchroniczna (event PE-004) | PE-004 + ranking: {zawodnik-ID, dystans, czas, klasyfikacja} | |
| D-011 | BC-003 | BC-009 | ACL (w BC-009) | R3, R2 | synchroniczna (snapshot przy PE-003) | {zawodnik-ID, opłacony-dystans} — do klasyfikacji | BC-009 tłumaczy model opłat na "opłacony-dystans" |
| D-012 | BC-004 | BC-008 | Customer–Supplier | R1, R4 | synchroniczna | {stary-numer, zawodnik-ID} → nowy numer startowy | Przy zgubieniu dyskietki |
| D-013 | I-001 Bank (zewn.) | BC-003 | ACL (w BC-003) | R2, R3 | asynchroniczna (event push z banku) | przelew bankowy → E-017 → (ACL) → E-018 | Mechanizm push nieznany (webhook?) → HS2-012 |
| D-014 | I-002 RFID (zewn.) | BC-009 | ACL (w BC-009) | R2, R3 | synchroniczna (event push z hardware) | {karta-ID, czytnik-ID, timestamp} → (ACL) → E-079 | ACL tłumaczy karta-ID na zawodnik-ID, czytnik-ID na km checkpoint |

---

## Hot spoty

| ID      | Opis | Źródło (krok / proces) | Status | Rozstrzyga HS z Fazy 1 (H-ID) |
|---------|------|------------------------|--------|-------------------------------|
| HS2-001 | Q-004: wygaśnięcie rejestracji — automatyczne czy manualne? E-015 bez triggera. | P-002 (ścieżka B) | open (bloker) | H-012 |
| HS2-003 | Q-008: dyskietka RFID w pakiecie czy osobno? Osobny ID? | P-007 (Część 1) | open (średni) | H-009 |
| HS2-005 | Q-011: dyskietka przy zakończeniu udziału przedwcześnie (E-076) — co z pomiarem? | P-008/P-009 | open (średni) | H-020 |
| HS2-006 | Q-005 (częściowy): kto kwalifikuje do dyplomu, format, sposób odbioru (E-093/E-094) | P-010 (dyplomy) | open (wysoki) | H-006 |
| HS2-008 | Promocja -100%: ręczna (A-004 per uczestnik) czy automatyczna (reguła konfiguracyjna)? | P-003 (ścieżka 2) | open (średni) | — |
| HS2-009 | Zdarzenie E-009 (zmiana dystansu) istnieje w BC-002 i kontekście BC-003 — potencjalny duplikat pojęcia naruszający B1 | P-003 (ścieżka 4) | open (wysoki) | — |
| HS2-010 | Mechanizm zwrotu nadpłaty (E-027) — przelew wychodzący, brak opisu | P-003 (ścieżka 5) | open (średni) | — |
| HS2-011 | Q-013: odpisanie uczestnictwa (E-029) — rezygnacja uczestnika czy anulowanie przez organizatora? | P-003 (ścieżka 6) | open (nice-to-have) | H-007 |
| HS2-012 | Mechanizm dopasowania przelewu bankowego do uczestnika (tytuł? ID?) — ryzyko błędów | D-013 (ACL banku) | open (wysoki) | — |
| HS2-014 | Q-007: format wymiany BC-002→BC-003→BC-004 — sync czy async? Event czy API? | D-002, D-003 | open (wysoki) | H-014 |
| HS2-016 | Algorytm generacji grup — kolejność przydziału uczestników (FIFO? losowo? inne?) | P-005 (krok 5) | open (wysoki — Core Domain!) | — |
| HS2-017 | Uczestnik opłaca niedopłatę MIĘDZY PE-001 a PE-002 — czy system automatycznie przenosi do nowej grupy? | P-005/P-006 | open (wysoki) | H-003 (częściowy) |
| HS2-019 | Opłata przy zmianie grupy (E-051) — przez BC-003 (przelew bankowy) czy wewnętrzna transakcja BC-006? | P-006 (ścieżka D) | open (wysoki) | — |
| HS2-020 | Akceptacja wniosku o zmianę grupy — ręczna (A-003, POL-011) czy automatyczna? | P-006 (krok 3) | open (średni) | — |
| HS2-022 | Q-010: zawodnik który nie stawił się — slot automatycznie zwalniany? Status w systemie? | P-007/P-008 | open (średni) | H-019 |
| HS2-024 | Przedłużenie dystansu (E-083) — czy wymaga dopłaty przez BC-003? | P-009 (ścieżka A) | open (wysoki) | — |
| HS2-027 | Kryteria rankingu (E-089) — per dystans? globalny? Co decyduje o dyplomie i nagrodzie? | P-009 (krok 7) | open (wysoki) | H-005 |
| HS2-r01 | H-001 (granica systemu) | Krok 5 | **resolved**: 10 BC wewnętrznych + 2 integracje zewnętrzne (Bank, RFID). WWW = osobny widok BC-009. | H-001 |
| HS2-r02 | H-003 (konflikt niedopłata vs generacja) | P-005, POL-006 | **resolved**: uczestnik z nieopłaconą niedopłatą → stary dystans (RB-019). | H-003 |
| HS2-r03 | H-004 (real-time display) | P-009, BC-009 | **resolved**: E-080 w BC-009, osobny read model z tej samej bazy. | H-004 |
| HS2-r04 | H-008 (dwie weryfikacje) | P-007 | **resolved**: dwa procesy w BC-007 (biurowa E-055 + przed-startowa E-061). | H-008 |
| HS2-r05 | H-010 (logika zamknięcia) | P-009, POL-015 | **resolved**: trigger = 24h, reguły klasyfikacji potwierdzone (fakt #7). | H-010 |
| HS2-r06 | H-016 (strona WWW) | BC-009 | **resolved**: ta sama baza, osobny widok — brak osobnego BC ani integracji. | H-016 |

---

## Wnioski

| ID    | Wniosek | Pewność | Źródło |
|-------|---------|---------|--------|
| W-001 | System składa się z 10 Bounded Contexts z wyraźnymi granicami wyznaczonymi przez 6 Pivotal Events (PE-001..PE-006) | wysoka | derived z seed + fazy 1 + potwierdzone fakty |
| W-002 | Core Domain to Generacja Grup Startowych (BC-005, SD-005) — 4/4 testów T1-T4 wskazuje na Core; najdroższy błąd, brak gotowego produktu, logika złożona i ewoluująca | wysoka | derived (T1-T4) |
| W-003 | Pomiar Czasu i Wyniki (BC-009) to Supporting złożony — logika klasyfikacji jest specyficzna i może ewoluować, ale nie jest kluczowa strategicznie. Kandydat do bogatego modelu na Design Level | średnia | derived z faktów domenowych #7-#9, materiały lossy |
| W-004 | Context map ma 14 relacji (12 wewnętrznych + 2 zewnętrzne). Dominuje wzorzec Customer–Supplier (8 relacji). Core Domain (BC-005) jest chronione przez 2 ACL (od BC-003 i BC-004). Zewnętrzne integracje (Bank, RFID) chronione przez ACL | wysoka | derived z polityk i przepływów |
| W-005 | Trzy polityki są "ludzkie" (kruche): POL-011 (akceptacja wniosku o zmianę grupy przez A-003), POL-018 (egzekucja kaucji), POL-019 (odpisanie uczestnictwa). Ryzyko przeoczenia/opóźnienia. | średnia | derived z przepływów; materiały lossy dla POL-018 i POL-019 |
| W-006 | Przepływ generacji dyplomów (E-089→E-093→E-094) i ranking wyników (Q-005, HS2-006, HS2-027) to najsłabiej opisany obszar — brak kompletnych danych z materiałów. Wymaga sesji elicitation | średnia | seed (coverage 65%) + lossy + H-005, H-006 z Fazy 1 |
| W-007 | Zmiana dystansu (E-009) pojawia się w BC-002 (przed opłatą) i potencjalnie w BC-003 (po opłacie) — ten sam termin, inne zdarzenie — naruszenie B1. Wymaga rozróżnienia pojęciowego przed Design Level | średnia | P-002, P-003, HS2-009 — derived z logiki, brak potwierdzenia w materiałach |
| W-008 | Mechanizm dopasowania przelewu bankowego do uczestnika jest nieznany — kluczowy dla BC-003 i ACL (D-013). To bloker dla Design Level BC-003 | niska | brak w materiałach; wniosek oparty wyłącznie na logice procesu |
| W-009 | Uczestnik i Zawodnik (Q-006) to ta sama osoba w różnych stanach — brak drugiego agregatu. Granica stanu: E-059/E-060 (wydanie pakietu) → "Uczestnik" staje się "Zawodnikiem". | średnia | derived z przepływów P-007/P-008; potwierdzenie H-013 z Fazy 1 jako niejasności |
| W-010 | Obszary słabego pokrycia (pozostałe z 65% coverage seeda): algorytm generacji grup (HS2-016), kryteria rankingu (HS2-027), format dyplomów (HS2-006), mechanizm dopasowania przelewu (HS2-012) | niska | materiały lossy, brak bezpośredniego potwierdzenia |

---

## Otwarte pytania

| ID    | Pytanie | Priorytet | Dotyczy | Przeniesione do fazy |
|-------|---------|-----------|---------|---------------------|
| Q-004 | Co się dzieje gdy uczestnik zarejestruje się i nie opłaci w żadnym terminie? Automatyczne anulowanie (E-015 → E-014 auto) czy wymaga decyzji administratora? | **blocker** | BC-002/BC-003, POL, HS2-001 | design-level |
| Q-005 | Pełny przepływ dyplomów: kto kwalifikuje (sklasyfikowany? uczestnik?), format dyplomu, trigger (auto po E-089?), sposób odbioru (fizyczny? elektroniczny?)? | wysoki | BC-010, D-010, HS2-006 | design-level |
| Q-006 | Uczestnik vs Zawodnik — jeden byt (różne stany: przed E-059 = Uczestnik, po E-059 = Zawodnik) czy dwa agregaty? | wysoki | BC-002, BC-007, W-009 | design-level |
| Q-007 | Format wymiany danych BC-002→BC-003 i BC-003→BC-004: synchronicznie (API call) czy asynchronicznie (event)? Wpływa na design D-002 i D-003. | wysoki | D-002, D-003, HS2-014 | design-level |
| Q-008 | Dyskietka RFID w pakiecie startowym czy wydawana osobno? Czy ma osobny unikalny identyfikator (≠ numer startowy)? | średni | BC-007, D-014, HS2-003 | design-level |
| Q-009 | Minimalna liczba uczestników X wymagana do generacji grup (RB-017)? Wartość konfiguracyjna z BC-001? | średni | BC-005, RB-017 | design-level |
| Q-010 | Zawodnik który nie stawił się na starcie (E-077): slot automatycznie zwalniany? Jakie konsekwencje dla dyskietki, pakietu, kaucji? | średni | BC-008, HS2-022 | design-level |
| Q-011 | Dyskietka RFID zawodnika który zakończył udział przedwcześnie (E-076): system przestaje rejestrować pomiary? Dyskietka zwracana? Kaucja? | średni | BC-008/BC-009, HS2-005 | design-level |
| Q-012 | Zmiana dystansu (P-002 ścieżka A) — tylko uczestnik online czy też administrator w biurze może złożyć wniosek w imieniu uczestnika? | nice-to-have | BC-002, P-002 | design-level |
| Q-013 | Odpisanie uczestnictwa (E-029): rezygnacja inicjowana przez uczestnika czy anulowanie przez organizatora? Konsekwencje finansowe (zwrot opłaty)? | nice-to-have | BC-003, HS2-011 | design-level |
| Q-NEW-01 | Algorytm generacji grup (HS2-016): kolejność przydziału uczestników do grup — FIFO, losowo, według innych kryteriów? | wysoki (Core Domain!) | BC-005, P-005 | design-level |
| Q-NEW-02 | Uczestnik opłaca niedopłatę między PE-001 a PE-002 (HS2-017): system automatycznie przenosi do grupy nowego dystansu czy nie? | wysoki | BC-005/BC-006, P-005/P-006 | design-level |
| Q-NEW-03 | Opłata przy zmianie grupy (HS2-019): procesowana przez BC-003 (przelew bankowy) czy wewnętrzna operacja BC-006? | wysoki | BC-006, BC-003, D-006 | design-level |
| Q-NEW-04 | Mechanizm dopasowania przelewu bankowego do uczestnika (HS2-012): tytuł przelewu? ID uczestnika? Inne? | wysoki | BC-003, D-013, ACL | design-level |
| Q-NEW-05 | Kryteria rankingu końcowego (HS2-027): per dystans? globalny? Co jest kryterium dla nagród i dyplomów? | wysoki | BC-009, BC-010, E-089 | design-level |
| Q-NEW-06 | Przedłużenie dystansu (E-083, HS2-024): czy wymaga dopłaty przez BC-003? Jak klasyfikowane w kontekście fakt #7 (reguła opłaconego dystansu)? | wysoki | BC-009, BC-003 | design-level |

---

## Korekty i uzupełnienia (retrospektywne)

[2026-06-14]: W wyniku analizy Process Level — H-008 z Fazy 1 (dwie weryfikacje) rozstrzygnięte.
Poprzednio: H-008 open — "Dwie odrębne weryfikacje zawodnika: biurowa (kwalifikacja) i na starcie — różnica procesowa niejasna"
Aktualnie: Oba procesy w BC-007. Weryfikacja biurowa (E-055) = dane+dokument+oświadczenie+pakiet (może być dni przed startem). Weryfikacja na starcie (E-061) = fizyczna kontrola bezpośrednio przed linią startową. Jeden BC, dwa odrębne procesy, różni aktorzy (A-005 w obu).
Powód: Analiza przepływu P-007 na poziomie gramatyki aktor→komenda→zdarzenie.

[2026-06-14]: H-001 z Fazy 1 (granica systemu) rozstrzygnięte.
Poprzednio: H-001 open — "Brak explicite zdefiniowanej granicy systemu"
Aktualnie: System = 10 Bounded Contexts + 2 zewnętrzne integracje (I-001 Bank, I-002 RFID). Strona WWW wyników real-time = osobny widok BC-009 (ta sama baza). Nie ma osobnej aplikacji zewnętrznej.
Powód: Potwierdzone fakty domenowe #1 i #9 + weryfikacja w analizie BC.

[2026-06-14]: H-016 z Fazy 1 (strona WWW wyników) rozstrzygnięte.
Poprzednio: H-016 open (wysoki) — "Strona WWW do wyników real-time — ten sam system czy osobna aplikacja?"
Aktualnie: Ta sama baza danych, osobny widok w BC-009. Brak osobnej integracji.
Powód: Potwierdzone fakty domenowe #1 i #9.
