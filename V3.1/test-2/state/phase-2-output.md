---
phase: process-level
session: maraton-rowerowy-24h
generated: 2026-06-06
status: completed
coverage: 88%
sources:
  - state/phase-1-output.md
  - user-input
---

# Process Level — System zarządzania i pomiaru czasu na maratonie rowerowym 24H

## Kontekst
Analiza Process Level objęła 10 bounded contexts zidentyfikowanych w Big Picture. Kluczowe odkrycia: wyjaśniono 3 blokery z Fazy 1 (niedopłata vs grupy, start indywidualny, podstawa rankingu). Obszar Wyniki/Ranking — nieobecny w ES — w pełni zamodelowany z rozmowy z użytkownikiem. Złożoność systemu potwierdzona jako średnia.

---

## Core Domain

**Nazwa:** Pomiar czasu i Wyniki/Ranking (BC-07 + BC-08)

**Uzasadnienie:** Unikalną wartością systemu jest precyzyjna rejestracja przejazdów przez sprzętowy system dyskietka+czytnik (BC-07) oraz klasyfikacja per dystans × płeć z niestandardową mechaniką kar pozycyjnych — nie czasowych (BC-08). Pozostałe konteksty są standardowe dla systemów obsługi wydarzeń sportowych.

---

## Bounded Contexts

| ID | Nazwa | Odpowiedzialność | Typ |
|---|---|---|---|
| BC-01 | Konfiguracja maratonu | Definiuje parametry zawodów: dystanse, regulamin, terminy, reguły kar | Supporting |
| BC-02 | Rejestracja | Zarządza cyklem życia uczestnika: zapis, weryfikacja, wybór i zmiana dystansu | Supporting |
| BC-03 | Opłata za udział | Obsługuje płatności, promocje, niedopłaty i zwroty nadpłat | Generic |
| BC-04 | Grupy startowe | Generuje i zarządza grupami startowymi per opłacony dystans | Supporting |
| BC-05 | Przygotowanie do startu | Weryfikuje zawodników, wydaje pakiety i dyskietki z numerami startowymi | Supporting |
| BC-06 | Start zawodników | Rejestruje moment startu grupowego lub indywidualnego, zamyka zapisy | Supporting |
| BC-07 | Pomiar czasu | Rejestruje przejazdy zawodników przez punkty kontrolne i metę (dyskietka+czytnik) | **Core** |
| BC-08 | Wyniki i ranking | Oblicza i publikuje ranking na żywo i końcowy (per dystans × płeć, z karami pozycyjnymi) | **Core** |
| BC-09 | Zarządzanie karami | Nakłada kary regulaminowe zmieniające pozycję w rankingu | Supporting |
| BC-10 | Zakończenie zawodów | Obsługuje nagrody, zwrot pakietów startowych i dyplomy | Supporting |

---

## Przepływy procesów

### Konfiguracja maratonu — BC-01

**Wyzwalacz:** Decyzja administratora o organizacji zawodów
**Rezultat:** Skonfigurowany maraton gotowy do przyjmowania rejestracji

**Kroki:**
1. Administrator rejestruje maraton (nazwa, data, miejsce)
2. Definiuje dostępne dystanse
3. Konfiguruje regulamin (reguły kar, reguły zmiany grupy, limity dystansów)
4. Ustawia terminy: rejestracji, generacji grup, zmiany grupy, dołączenia po terminie
5. Konfiguruje minimalne liczby uczestników per dystans

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-001 | Regulamin maratonu obowiązuje wszystkie inne BC jako polityka nadrzędna | 3 |
| RB-002 | Terminy muszą być ustawione przed otwarciem rejestracji | 4 |

---

### Rejestracja uczestnika — BC-02

**Wyzwalacz:** Uczestnik chce wziąć udział w maratonie
**Rezultat:** Uczestnik zarejestrowany z wybranym dystansem i wyliczoną opłatą

**Kroki:**
1. Uczestnik rejestruje się i wypełnia dane → Zarejestrowano uczestnika
2. Uczestnik wybiera dystans → Wybrano dystans
3. System weryfikuje dane kontaktowe → Potwierdzono dane kontaktowe
4. Obsługa weryfikuje tożsamość → Zweryfikowano zawodnika
5. System wylicza opłatę → Wyliczono opłatę → przekazano do BC-03

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-003 | Rejestracja tylko w terminie otwarcia (z BC-01) | 1 |
| RB-004 | Dystans musi być z listy dostępnych dystansów (z BC-01) | 2 |
| RB-005 | W trakcie sesji opłacania nie można zmienić dystansu | — |
| RB-006 | Zmiana dystansu bez opłaty → uczestnik pozostaje na opłaconym dystansie | — |
| RB-007 | Zmiana dystansu po wygenerowaniu grup → brak przypisania do ostatecznej grupy | — |

**Wyjątki i ścieżki alternatywne:**
| Warunek | Akcja | Skutek |
|---|---|---|
| Uczestnik składa wniosek o zmianę dystansu (nieopłacony) | System odrzuca zmianę dystansu | Uczestnik pozostaje na opłaconym dystansie |
| Uczestnik opłaca nowy dystans | System aktualizuje dystans | Zmieniono dystans, wyliczono różnicę opłaty |
| Zmiana dystansu po generacji grup | Manualna obsługa | Brak przypisania do ostatecznej grupy |

---

### Opłata za udział — BC-03

**Wyzwalacz:** Wyliczono opłatę (z BC-02)
**Rezultat:** Opłacono uczestnictwo → uczestnik aktywny finansowo

**Kroki:**
1. Uczestnik inicjuje płatność → Rozpoczęto opłacanie udziału
2. Uczestnik wykonuje przelew bankowy
3. Interfejs banku rejestruje przelew → Zarejestrowano przelew
4. Obsługa / system potwierdza → Zarejestrowano opłatę
5. System weryfikuje kwotę → Opłacono uczestnictwo / zaaplikowano promocję / wyliczono zwrot

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-008 | W trakcie sesji opłacania nie można zmienić dystansu | 1–5 |
| RB-009 | Promocja (-100%) aplikowana manualnie przez obsługę | 5 |
| RB-010 | Niedopłata za zmianę dystansu nie blokuje generacji grup — uczestnik pozostaje na opłaconym dystansie | — |

**Wyjątki:**
| Warunek | Akcja | Skutek |
|---|---|---|
| Nadpłata | Wylicz kwotę zwrotu → Zwróć nadpłatę | Zwrócono nadpłatę |
| Niedopłata za zmianę dystansu | Uczestnik pozostaje na dotychczasowym dystansie | Opłacono zmianę dystansu (częściowo) |

---

### Generacja grup startowych — BC-04

**Wyzwalacz:** Inicjacja przez administratora LUB harmonogram (termin z BC-01)
**Rezultat:** Wszyscy opłaceni zawodnicy z numerami przypisani do grup

**Kroki:**
1. System sprawdza: minimalna liczba uczestników z numerami startowymi + termin
2. System grupuje zawodników per opłacony dystans → Przypisano zawodnika do grupy
3. → Wygenerowano grupy startowe

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-011 | Minimalna liczba uczestników musi być spełniona | 1 |
| RB-012 | Tylko uczestnicy z przypisanym numerem startowym wchodzą do generacji | 1 |
| RB-013 | Uczestnik z nieopłaconą zmianą dystansu → trafia do grup na opłaconym dystansie | 2 |
| RB-014 | Generowanie grup w terminie z regulaminu | 1 |

**Wyjątki:**
| Warunek | Akcja | Skutek |
|---|---|---|
| Za mało uczestników z numerami | Blokada generacji | Nie udało się wygenerować grup |
| Uczestnik opłaca po generacji | Dołącz do grupy (jeśli termin nie minął) | Dołączono do grupy po terminie |
| Zawodnik składa wniosek o zmianę grupy | Walidacja 5 warunków | Zmieniono grupę / odmowa |

**Warunki zmiany grupy (wszystkie wymagane):**
1. Nie przekroczono terminu zmiany grup
2. Grupa docelowa ma wolne miejsca
3. Zawodnik nie zmieniał wcześniej grupy
4. Grupa docelowa jest z tego samego dystansu
5. Grupy startowe zostały już wygenerowane

---

### Przygotowanie do startu — BC-05

**Wyzwalacz:** Zawodnik przybywa do punktu kontrolnego
**Rezultat:** Wydano pakiet startowy (dyskietka + numer) → zawodnik gotowy

**Kroki:**
1. Obsługa weryfikuje tożsamość → Zweryfikowano tożsamość
2. Zawodnik podpisuje oświadczenie → Podpisano oświadczenie
3. Wydano pakiet startowy (dyskietka z numerem startowym)

**Wyjątki:**
| Warunek | Akcja | Skutek |
|---|---|---|
| Nie przeszedł weryfikacji tożsamości | Zatrzymaj proces | Zdyskwalifikowano zawodnika |
| Nie podpisał oświadczenia | Zatrzymaj proces | Zdyskwalifikowano zawodnika |
| Zgubienie dyskietki | Przejmij kaucję → dezaktywuj stary numer → wydaj nową dyskietkę | Wydano nową dyskietkę |

---

### Start zawodników — BC-06

**Wyzwalacz:** Sędzia trasy w momencie startu grupy
**Rezultat:** Czas startu zarejestrowany → BC-07 może identyfikować pomiary

**Kroki:**
1. Administrator zamyka zapisy na dystans → Zamknięto zapisy
2. Obsługa przeprowadza weryfikację przed startem (z BC-05)
3. Sędzia startuje grupę → Wystartowano grupę
4. System rejestruje czas startu grupy → Zarejestrowano czas startu grupy
5. Sędzia potwierdza start każdego zawodnika → Wystartowano zawodnika

**Wyjątki:**
| Warunek | Akcja | Skutek |
|---|---|---|
| Start indywidualny (spóźnienie) | Wyciągnij z grupy → zarejestruj OSOBNY czas startu | Zarejestrowano indywidualny czas startu |
| Dyskwalifikacja przy starcie | Sędzia wyklucza | Zdyskwalifikowano zawodnika |
| Rezygnacja | Sędzia odnotowuje | Zakończono udział przedwcześnie |

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-017 | Czas startu grupy = baza rankingu dla zawodników startujących grupowo | 4 |
| RB-018 | Start indywidualny → WŁASNY czas startu jako baza rankingu | wyjątek |

---

### Pomiar czasu — BC-07

**Wyzwalacz:** Zawodnik przykłada dyskietkę do czytnika
**Rezultat:** Czas zarejestrowany → BC-08 aktualizuje wyniki

**Kroki:**
1. Zawodnik przykłada dyskietkę → Zarejestrowano pomiar czasu
2. System identyfikuje zawodnika po numerze dyskietki
3. System zapisuje czas
4. Zawodnik przekracza metę → Zawodnik zakończył przejazd
5. Podliczono czasy przejazdu → BC-08: Zaktualizuj wyniki na żywo

**Wyjątki:**
| Warunek | Akcja | Skutek |
|---|---|---|
| Zawodnik zgłasza chęć przedłużenia dystansu | Obsługa potwierdza → przedłużono przejazd | Odnotowano dodatkowy pomiar czasu |
| Wszyscy dojechali LUB upłynął czas | System zamyka dystans | Zakończono przejazdy na danym dystansie |

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-020 | Limity czasu na dystans z regulaminu (BC-01) | 4 |
| RB-021 | Dyskietka = jedyny identyfikator zawodnika na trasie | 1–2 |
| RB-022 | Dezaktywowana dyskietka (po zgubieniu i wymianie) nie jest akceptowana | 1 |

---

### Wyniki i ranking — BC-08

**Wyzwalacz:** Zawodnik zakończył przejazd (z BC-07) — ciągłe, lub zakończenie zawodów
**Rezultat:** Ranking zaktualizowany i opublikowany

**Kroki (na żywo — po każdym przejeździe):**
1. Odebrano: zawodnik zakończył przejazd + czasy z BC-07
2. Oblicz czas: meta − czas startu grupy (lub indywidualny czas startu)
3. Sprawdź kary (BC-09): brak / koniec listy / wykluczenie
4. Sklasyfikuj zawodnika per dystans × płeć
5. Zaktualizowano wyniki na żywo → publikuj na tablicy i WWW

**Kroki (końcowe — po Zakończono przejazdy wszystkich dystansów):**
1. Zamroź ranking
2. Generuj finalną klasyfikację z karami
3. Opublikowano wyniki końcowe

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-025 | Ranking per dystans × płeć — brak innych kategorii | 4 |
| RB-026 | Baza czasu: czas startu grupy do mety (lub indywidualny czas startu) | 2 |
| RB-027 | Kara pozycyjna ma pierwszeństwo nad wynikiem czasowym | 3 |
| RB-028 | Wykluczenie = zawodnik niewidoczny w rankingu | 3 |

---

### Zarządzanie karami — BC-09

**Wyzwalacz:** Sędzia trasy stwierdza naruszenie
**Rezultat:** Kara zapisana → BC-08 aktualizuje pozycję

**Kroki:**
1. Sędzia identyfikuje zawodnika po numerze dyskietki
2. Sędzia zapisuje typ kary: złamanie regulaminu / pominięcie punktu kontrolnego
3. Nałożono karę → BC-08: Przenieś na koniec listy LUB Wyklucz z wyników

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-029 | Typy kar z regulaminu maratonu (BC-01) | 2 |
| RB-030 | Kara to zmiana pozycji (koniec listy / wykluczenie) — nie czas dodatkowy | 3 |

---

### Zakończenie zawodów — BC-10

**Wyzwalacz:** Zakończono przejazdy wszystkich dystansów (z BC-07)
**Rezultat:** Nagrody przyznane, pakiety zwrócone, dyplomy wydane

**Kroki:**
1. Główny organizator przyznaje nagrody → Przyznano nagrodę
2. Zawodnik (lub osoba upoważniona) zwraca pakiet startowy → Zwrócono pakiet startowy
3. Obsługa zwalnia kaucję → Kaucja została zwrócona
4. Zawodnik odbiera dyplom → Otrzymano dyplom

**Wyjątki:**
| Warunek | Akcja | Skutek |
|---|---|---|
| Timer zwrotu pakietu upłynął bez zwrotu | System wyzwala zdarzenie | Kaucja została zatrzymana |

---

## Zależności między kontekstami

| ID | Z (upstream) | Do (downstream) | Co jest przekazywane | Typ relacji | Kierunek |
|---|---|---|---|---|---|
| D-001 | BC-01 Konfiguracja | BC-02 Rejestracja | Terminy, dostępne dystanse | upstream-downstream | async |
| D-002 | BC-01 Konfiguracja | BC-04 Grupy startowe | Terminy generacji, zmiany, min. liczba | upstream-downstream | async |
| D-003 | BC-01 Konfiguracja | BC-07 Pomiar czasu | Limity czasu per dystans | upstream-downstream | async |
| D-004 | BC-01 Konfiguracja | BC-09 Kary | Typy kar i konsekwencje | upstream-downstream | async |
| D-005 | BC-02 Rejestracja | BC-03 Opłata | Kwota opłaty per dystans | upstream-downstream | sync |
| D-006 | BC-03 Opłata | BC-02 Rejestracja | Potwierdzenie opłacenia dystansu | upstream-downstream | async |
| D-007 | BC-03 Opłata | BC-04 Grupy startowe | Opłacony dystans uczestnika | upstream-downstream | async |
| D-008 | BC-02 Rejestracja | BC-04 Grupy startowe | Uczestnik z numerem startowym | upstream-downstream | async |
| D-009 | BC-04 Grupy startowe | BC-06 Start zawodników | Wygenerowane grupy per dystans | upstream-downstream | async |
| D-010 | BC-05 Przygot. do startu | BC-06 Start zawodników | Lista zweryfikowanych zawodników | upstream-downstream | sync |
| D-011 | BC-06 Start zawodników | BC-07 Pomiar czasu | Czas startu grupy / indywidualny | upstream-downstream | sync |
| D-012 | BC-07 Pomiar czasu | BC-08 Wyniki i ranking | Czasy przejazdów per zawodnik | upstream-downstream | async |
| D-013 | BC-09 Kary | BC-08 Wyniki i ranking | Kara per zawodnik (typ kary) | upstream-downstream | async |
| D-014 | BC-08 Wyniki i ranking | I-002 Strona WWW | Wyniki na żywo i końcowe | OHS | async push |
| D-015 | BC-08 Wyniki i ranking | I-003 Tablica wyników | Wyniki na żywo | OHS | async push |
| D-016 | BC-07 Pomiar czasu | BC-10 Zakończenie | Zakończono przejazdy wszystkich dystansów | upstream-downstream | async |
| D-017 | I-001 Interfejs banku | BC-03 Opłata | Rejestracja przelewu | ACL | async |

---

## Polityki (Policies)

| ID | Zdarzenie wyzwalające | Polityka / Reguła | Akcja / Komenda | BC |
|---|---|---|---|---|
| P-001 | Opłacono uczestnictwo | Uczestnik opłacony → przypisz numer startowy | Generuj numer startowy | BC-04 |
| P-002 | Zmieniono dystans + dystans nieopłacony | Uczestnik bez opłaty → zostaje na dotychczasowym dystansie | Odrzuć zmianę dystansu | BC-02/BC-03 |
| P-003 | Wygenerowano grupy + nowy opłacony uczestnik | Jeśli nie przekroczono terminu dołączenia | Dołącz do istniejącej grupy | BC-04 |
| P-004 | Zawodnik zakończył przejazd | Po każdym przejeździe → aktualizuj ranking | Zaktualizuj wyniki na żywo | BC-08 |
| P-005 | Nałożono karę | Kara regulaminowa → zmień pozycję w rankingu | Przenieś na koniec / wyklucz | BC-08/BC-09 |
| P-006 | Zakończono przejazdy wszystkich dystansów | Zawody zakończone → publikuj wyniki | Opublikuj wyniki końcowe | BC-08 |
| P-007 | Zakończono przejazdy wszystkich dystansów | Zawody zakończone → start timera pakietów | Uruchom timer zwrotu pakietów | BC-10 |
| P-008 | Upłynął czas zwrotu pakietu | Pakiet nie zwrócony → zatrzymaj kaucję | Zatrzymaj kaucję | BC-10 |
| P-009 | Zgłoszono indywidualny start | Start poza grupą → osobny czas | Zarejestruj indywidualny czas startu | BC-06 |
| P-010 | Zgubiono dyskietkę | Brak identyfikatora → przejmij kaucję, wymień | Wydaj nową dyskietkę | BC-05 |

---

## Wnioski

| ID | Wniosek | Pewność | Źródło |
|---|---|---|---|
| W-001 | Core Domain to BC-07 + BC-08 — pomiar czasu i ranking są nierozerwalnie powiązaną przewagą systemu | wysoka | derived |
| W-002 | Mechanika kar jest niestandardowa — nie czas dodatkowy lecz zmiana pozycji; wymaga specjalnej obsługi w modelu rankingu | wysoka | user-input |
| W-003 | BC-01 (Konfiguracja) jest de facto policy provider dla wszystkich innych BC — zmiany regulaminu mają kaskadowy wpływ | wysoka | derived |
| W-004 | Dyskietka z numerem startowym to centralny artefakt domeny — pojawia się w BC-05, BC-06, BC-07, BC-10; jej utrata/awaria dotyka wielu BC | wysoka | ES + derived |
| W-005 | Niedopłata za zmianę dystansu nie blokuje żadnego procesu — uczestnik zostaje na opłaconym dystansie, generacja grup przebiega normalnie | wysoka | user-input |
| W-006 | Start indywidualny ma własny czas startu jako bazę rankingu — to wyjątek od reguły "czas startu grupy" | wysoka | user-input |
| W-007 | BC-03 (Opłata) to najlepszy kandydat na Generic Subdomain — obsługa płatności jest niezróżnicowanym problemem, możliwy gotowy komponent | średnia | derived |
| W-008 | Obszar "Wyniki i ranking" był nieobecny w ES — ryzyko że inne obszary tej domeny mogą być niepełne w materiałach wejściowych | średnia | derived |

---

## Otwarte pytania

| ID | Pytanie | Priorytet | Dotyczy | Przeniesione do fazy |
|---|---|---|---|---|
| Q-006 | Czy sędzia może cofnąć nałożoną karę? Jak to wpływa na wyniki w czasie rzeczywistym? | blocker | BC-09, BC-08 | design-level / — |
| Q-007 | Jak obsługiwana jest awaria systemu pomiaru czasu — czy istnieje ręczny fallback? | nice-to-have | BC-07 | design-level / — |
| Q-008 | Czy integracja z bankiem jest automatyczna (webhook) czy ręczna (obsługa wpisuje)? | nice-to-have | BC-03, I-001 | design-level / — |
| Q-009 | Jak obsługiwany jest zawodnik bez przypisanej grupy w dniu startu (edge case po zmianie dystansu)? | blocker | BC-04, BC-06 | design-level / — |

---

## Korekty i uzupełnienia (retrospektywne)
<!-- Sekcja wypełniana przez orkiestratora gdy późniejsza faza zmienia wnioski tej fazy. -->
