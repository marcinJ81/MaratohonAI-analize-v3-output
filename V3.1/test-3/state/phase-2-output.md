---
phase: process-level
session: maraton-rowerowy-24h
generated: 2026-06-07
status: partial
coverage: 68%
sources:
  - state/phase-1-output.md
  - state/inputs/processed/phase-2-seed.md
  - state/inputs/processed/es-processed.md
---

# Process Level — System zarządzania maratonem rowerowym 24h

## Kontekst

Analiza przepływów procesów i granic odpowiedzialności systemu zarządzania 24h maratonem rowerowym. Zidentyfikowano 9 Bounded Contexts, Core Domain (BC-04 + BC-07), 16 polityk i mapę zależności między kontekstami. Pokrycie 68% — obszary Kar i DNS wymagają uzupełnienia.

---

## Bounded Contexts

| ID | Nazwa | Odpowiedzialność | Typ |
|---|---|---|---|
| BC-01 | Konfiguracja maratonu | Definiuje zasady, regulamin i parametry zawodów (terminy, limity, ceny) | Supporting |
| BC-02 | Rejestracja | Zarządza cyklem życia uczestnika: od zgłoszenia do gotowości do startu | Supporting |
| BC-03 | Opłaty | Obsługuje płatności, zwroty, niedopłaty i status finansowy uczestnictwa | Generic |
| BC-04 | Grupy startowe | Generuje i zarządza grupami startowymi; reguły przydziału i zmian | **Core** |
| BC-05 | Przygotowanie do startu | Weryfikuje gotowość zawodnika i wydaje wyposażenie (pakiet, dyskietka) | Supporting |
| BC-06 | Start zawodników | Zarządza momentem startu grup i indywidualnym; zamyka zapisy | Supporting |
| BC-07 | Pomiar czasu | Rejestruje przejazdy przez punkty i linię mety; generuje dane do rankingu | Generic |
| BC-08 | Kary | Rejestruje naruszenia regulaminu i dyskwalifikuje zawodników | Supporting |
| BC-09 | Zakończenie zawodów | Generuje wyniki, przyznaje nagrody, obsługuje zwrot wyposażenia | Supporting |

---

## Core Domain

**Nazwa:** Grupy startowe (BC-04)

**Uzasadnienie:**
BC-04 (Grupy startowe) — unikalna logika przydziału specyficzna dla 24h maratonu rowerowego: okno czasowe zmiany, limit jednej zmiany per zawodnik, warunek tego samego dystansu, automatyczna generacja z progiem minimalnej liczby uczestników, obsługa dołączeń po terminie. Logika ta jest niestandardowa i nie istnieje jako gotowe rozwiązanie — to prawdziwa przewaga systemu.

**BC-07 (Pomiar czasu) — Generic** (korekta po weryfikacji): łatwo zastępowalny gotowym systemem chipowym (MyLaps, Championchip, itp.). Integracja przez standardowy protokół.
**BC-03 (Opłaty) — Generic:** kandydat do outsourcingu (PayU, Stripe, Przelewy24).
**BC-01 (Konfiguracja) — Supporting:** standardowy wzorzec konfiguracji / admin panel.

---

## Przepływy procesów

### BC-01: Konfiguracja maratonu
**Wyzwalacz:** Administrator tworzy nową edycję maratonu.

| Krok | Akcja | Reguły |
|---|---|---|
| 1 | Administrator rejestruje maraton (E-001) | — |
| 2 | Ustawia dostępne dystanse i ceny | RB-002: terminy spójne |
| 3 | Ustawia terminy: generacji grup, zmiany grup, zmiany dystansu | RB-002 |
| 4 | Ustawia min. liczbę uczestników w grupie | RB-003: > 0 |
| 5 | Definiuje punkty kontrolne i lokalizacje | — |
| 6 | Definiuje czas zwrotu pakietu i zasady nagród | — |

**Reguły:** RB-001 (regulamin = read-only dla innych BC), RB-002 (spójność terminów), RB-003 (min. uczestników > 0)

---

### BC-02: Rejestracja
**Wyzwalacz:** Uczestnik chce wziąć udział.

**Przepływ główny:**
1. Uczestnik rejestruje się → E-010
2. Wybiera dystans → E-011
3. Weryfikacja danych → E-012, E-013
4. Obliczenie opłaty → E-014 → przejście do BC-03

**Przepływ — zmiana dystansu przed generacją grup:**
1. Wniosek → E-015 → E-016 Zmieniono dystans → E-014 Przeliczono opłatę → E-017 Powiadomienie → BC-03 (nowa kwota)

**Przepływ — zmiana dystansu po generacji grup:**
1. Wniosek → E-032 → BC-03 sprawdza czy niedopłata opłacona
2. [Opłacono różnicę] → E-016 + wniosek zmiany grupy → BC-04
3. [Nie opłacono] → uczestnik pozostaje na pierwotnym dystansie

**Reguły:** RB-010 (termin zmiany dystansu), RB-011 (po generacji grup: wymagana wpłata różnicy), RB-012 (brak zapłaty ≠ blokada startu)

---

### BC-03: Opłaty
**Wyzwalacz:** BC-02 oblicza wymaganą opłatę.

**Przepływ — opłata pełna:** E-020 → E-021 (bank) lub E-022 (ręcznie) → [kwota OK] → E-023
**Przepływ — nadpłata:** E-021 → E-025 → E-026 → E-023
**Przepływ — niedopłata:** E-021 → [za mało] → E-027 → E-028 → E-023
**Przepływ — promocja 100%:** E-024 → E-023
**Przepływ — zmiana dystansu:** E-030 → [droższy] E-029 / [tańszy] E-031 → E-026

**Reguły:** RB-020 (zmiana dystansu zablokowana w trakcie opłacania), RB-021 (niedopłata ≠ blokada startu), RB-022 (promocja 100%), RB-023 (zwroty ręczne)

---

### BC-04: Grupy startowe (Core)
**Wyzwalacz A:** Timer — termin generacji grup.
**Wyzwalacz B:** Zawodnik składa wniosek zmiany grupy.
**Wyzwalacz C:** Administrator ręcznie przypisuje zawodnika.

**Przepływ — automatyczna generacja:**
1. Warunki: termin OK + min. X uczestników z numerem
2. [OK] → E-046 Wygenerowano grupy → E-048 Przypisano nieprzydzielonych
3. [NG] → E-047 Nie udało się wygenerować

**Przepływ — zmiana grupy przez zawodnika:**
1. E-041 Złożono wniosek
2. Sprawdź łącznie (wszystkie wymagane):
   - RB-040: nie przekroczono terminu zmiany
   - RB-041: grupy zostały wygenerowane
   - RB-042: zawodnik nie zmieniał wcześniej (1 zmiana)
   - RB-043: wolne miejsca w grupie docelowej
   - RB-044: dystans grupy docelowej = dystans zawodnika
3. [OK] → E-044 Zmieniono grupę
4. [NG] → E-045 / E-043 / E-053

**Przepływ — dołączenie po terminie:** E-049 (jeśli termin dołączenia nie minął)

**Reguły:** RB-040..046 (patrz powyżej)

---

### BC-05: Przygotowanie do startu
**Wyzwalacz:** Zawodnik przybywa do biura zawodów.

| Krok | Akcja | Wyjątek |
|---|---|---|
| 1 | Weryfikacja tożsamości (E-060) | Brak weryfikacji → E-064 Dyskwalifikacja |
| 2 | Podpisanie oświadczenia (E-061) | Brak podpisu → E-065 Dyskwalifikacja |
| 3 | Wydanie pakietu startowego (E-062) | — |
| 4 | Przypisanie dyskietki / numeru (E-066) | Zgubienie → kaucja + nowa dyskietka (E-067..E-070) |
| 5 | Weryfikacja na liście startowej (E-063) | Brak na liście → E-064 Dyskwalifikacja |

**Reguły:** RB-050 (oświadczenie obowiązkowe), RB-051 (kolejność: weryfikacja → pakiet), RB-052 (kaucja z regulaminu), RB-053 (dyskietka = transponder chipa)

---

### BC-06: Start zawodników
**Wyzwalacz:** Sędzia trasy ogłasza start.

**Przepływ — zamknięcie zapisów:** Administrator → E-080 (po wystartowaniu wszystkich grupy dystansu)

**Przepływ — start grupowy:**
1. Sędzia → E-081 Wystartowano grupę → E-082 Zarejestrowano czas startu
2. Przy starcie: [dyskwalifikacja] E-086 / [rezygnacja] E-087

**Przepływ — start indywidualny:**
1. Zawodnik → E-084 → Administrator → E-085 Wyciągnięto z grupy → Sędzia → E-083 → E-082

**Reguły:** RB-060 (start indywidualny = wyciągnięcie z grupy), RB-061 (czas startu → podstawa BC-07), RB-062 (zamknięcie po wystartowaniu)

---

### BC-07: Pomiar czasu (Core)
**Wyzwalacz:** Zawodnik przykłada dyskietkę do czytnika (chip timing).

**Przepływ — standardowy:**
1. E-090 Odczyt dyskietki → system identyfikuje zawodnik + punkt
2. [Punkt pośredni] → zapisz czas pośredni
3. [Linia mety + dystans ukończony] → E-091 Zawodnik zakończył przejazd
4. [Wszyscy / upłynął czas 24h] → E-092 → E-093 Dystans zakończony
5. [Wszystkie dystanse] → E-094 → wyzwala BC-09

**Przepływ — wydłużenie dystansu:**
1. E-095 Zgłoszono prośbę → obsługa weryfikuje z regulaminem → E-096 Przedłużono
2. E-097 Dodatkowe odczyty → E-098 Ukończono wydłużony dystans

**Reguły:** RB-070 (limity dystansu), RB-071 (24h twardy limit), RB-072 (2 stałe punkty kontrolne na trasie — nie licząc startu/mety; dłuższe dystanse = więcej przejazdów przez te same punkty), RB-073 (dane z punktów → detekcja pominięć → BC-08; próg: ≥2 pominięcia = dyskwalifikacja), RB-074 (ranking: czas mety + czasy pośrednie), RB-075 (rywalizacja per dystans), **RB-076: wydłużenie dystansu możliwe tylko do sąsiedniego/przylegającego dystansu wyższego (nie dowolny skok); skrócenie dystansu możliwe ale zawodnik nie jest klasyfikowany na krótszym**

---

### BC-08: Kary
**Wyzwalacz:** Sędzia trasy stwierdza naruszenie.

| Typ naruszenia | Zdarzenie | Skutek |
|---|---|---|
| Złamanie regulaminu | E-100 | E-103 Dyskwalifikacja |
| Pominięty punkt kontrolny | E-101 (+ detekcja z BC-07) | E-103 Dyskwalifikacja |

**Reguły:** RB-080 (jedyna kara = dyskwalifikacja), RB-081 (typy naruszeń: złamanie regulaminu, pominięty punkt kontrolny), RB-082 (zatwierdzenie przez sędziego), RB-083 (zdyskwalifikowani poza rankingiem), **RB-084: próg dyskwalifikacji za punkty kontrolne = 2 lub więcej pominięć (1 pominięcie jest dopuszczalne)**

---

### BC-09: Zakończenie zawodów
**Wyzwalacz:** E-094 Zakończono przejazdy wszystkich dystansów.

| Krok | Akcja |
|---|---|
| 1 | Generuj ranking automatycznie → E-102 (per dystans, bez zdyskwalifikowanych) |
| 2 | Organizator zatwierdza → E-110 Przyznano nagrody |
| 3 | Zawodnik zwraca pakiet + dyskietkę → E-111 → E-112 Kaucja zwrócona |
| 4 | Zawodnik otrzymuje dyplom → E-113 |
| 5 | Timer: upłynął czas zwrotu → E-114 → [brak zwrotu] E-115 → E-116 Kaucja zatrzymana |

**Reguły:** RB-090..094

---

## Polityki (Event → Policy → Command)

| ID | Gdy | Warunek | Akcja |
|---|---|---|---|
| POL-01 | E-021 Przelew | kwota = wymagana | E-023 Opłacono |
| POL-02 | E-021 Przelew | kwota > wymagana | E-025+E-026+E-023 Zwrot nadpłaty |
| POL-03 | E-021 Przelew | kwota < wymagana | Rejestruj niedopłatę, czekaj |
| POL-04 | E-024 Promocja 100% | zawsze | E-023 Opłacono |
| POL-05 | Timer: termin gen. grup | min. X uczestników + termin OK | E-046 Generuj grupy |
| POL-06 | E-046 Wygenerowano grupy | nieprzypisani istnieją | E-048 Przypisz auto |
| POL-07 | E-016 Zmiana dystansu (po gen.) | zawsze | Złóż wniosek → BC-04 |
| POL-08 | Timer: termin zmiany grup | zawsze | Lock przydziałów |
| POL-09 | E-090 Odczyt dyskietki | dystans ukończony | E-091 Zakończył przejazd |
| POL-10 | E-090 Odczyt dyskietki | punkt pominięty wcześniej | E-101+E-103 Dyskwalifikacja |
| POL-11 | E-100 / E-101 Kara | zawsze | E-103 Dyskwalifikacja |
| POL-12 | E-093 Dystans zakończony | wszystkie dystanse zakończone | E-094 Wszystkie zakończone |
| POL-13 | E-094 Wszystkie zakończone | zawsze | E-102 Generuj ranking |
| POL-14 | Timer: czas zwrotu pakietu | pakiet nie zwrócony | E-116 Zatrzymaj kaucję |
| POL-15 | Weryfikacja negatywna | tożsamość / lista | E-064 Dyskwalifikacja |
| POL-16 | Brak podpisu oświadczenia | zawsze | E-065 Dyskwalifikacja |

---

## Zależności między kontekstami

| Z BC | Do BC | Typ | Co jest przekazywane |
|---|---|---|---|
| BC-01 | Wszystkie | OHS (Regulamin) | Polityki, terminy, limity |
| BC-02 | BC-03 | Customer/Supplier | Uczestnik + wymagana opłata |
| BC-02 | BC-04 | Customer/Supplier | Uczestnik z dystansem + numer startowy |
| BC-03 | BC-04 | Customer/Supplier | Status: różnica dystansu opłacona? |
| BC-04 | BC-05 | Customer/Supplier | Lista startowa (grupy + zawodnicy) |
| BC-05 | BC-06 | Customer/Supplier | Zawodnik zweryfikowany i gotowy |
| BC-06 | BC-07 | Customer/Supplier | Czas startu grupy / zawodnika |
| BC-07 | BC-08 | Conformist | Dane przejazdów przez punkty kontrolne |
| BC-07 | BC-09 | Customer/Supplier | Podliczone czasy per zawodnik |
| BC-08 | BC-09 | Customer/Supplier | Lista zdyskwalifikowanych |

---

## Wnioski

| ID | Wniosek | Pewność | Źródło |
|---|---|---|---|
| W-201 | BC-04 (Grupy startowe) i BC-07 (Pomiar czasu) to Core Domain — unikalna logika niemożliwa do outsourcingu | wysoka | analiza agenta |
| W-202 | BC-03 (Opłaty) jest Generic — kandydat do outsourcingu lub gotowej biblioteki | wysoka | analiza agenta |
| W-203 | BC-01 (Konfiguracja) to globalny upstream dostarczający polityki wszystkim BC — zmiana regulaminu propaguje się do 8 kontekstów | wysoka | es-processed.md |
| W-204 | Jedyna kara to dyskwalifikacja — upraszcza model danych (brak kar czasowych, sum punktów) | wysoka | odpowiedź użytkownika |
| W-205 | Ranking generowany automatycznie per dystans na podstawie czasu mety — zdyskwalifikowani są wykluczeni | wysoka | odpowiedź użytkownika |
| W-206 | Niedopłata blokuje wyłącznie zmianę dystansu, nie uczestnictwo — uczestnik z niedopłatą może startować | wysoka | odpowiedź użytkownika |
| W-207 | BC-07 i BC-08 są ściśle sprzężone — detekcja pominięcia punktu kontrolnego wymaga danych z pomiaru czasu | średnia | analiza agenta |
| W-208 | Kaucja za dyskietkę to mechanizm bezpieczeństwa fizycznego wyposażenia, nie mechanizm finansowy | średnia | es-processed.md |

---

## Otwarte pytania

| ID | Pytanie | Priorytet | Dotyczy |
|---|---|---|---|
| Q-101 | Ile punktów kontrolnych na trasie? Jeden pominięty = dyskwalifikacja czy próg? | blocker | BC-07, BC-08 |
| Q-102 | DNS (Did Not Start) — czy istnieje ten status? Jak jest obsługiwany? | wysoki | BC-06 |
| Q-103 | Czy zawodnik może startować na wielu dystansach jednocześnie? | wysoki | BC-02, BC-04 |
| Q-104 | Czy dyplom jest wydawany przed czy po zwrocie pakietu? | średni | BC-09 |
| Q-105 | Nagrody per dystans tylko, czy też per kategoria (wiek / płeć)? | średni | BC-09 |
| Q-106 | Czy anulowanie rejestracji jest możliwe? Zasady zwrotu? | średni | BC-02, BC-03 |
| Q-107 | Czy konfigurację maratonu można modyfikować po otwarciu rejestracji? | średni | BC-01 |
| Q-108 | Czy istnieje mechanizm powiadomień (email/SMS) dla uczestników? | niski | BC-02, BC-04 |

---

## Korekty i uzupełnienia (retrospektywne)

[weryfikacja-użytkownika]: Wniosek W-201 wymaga korekty.
Poprzednio: Core Domain = BC-04 Grupy startowe + BC-07 Pomiar czasu
Aktualnie: **Core Domain = wyłącznie BC-04 Grupy startowe**; BC-07 Pomiar czasu sklasyfikowany jako **Generic** (łatwo zastępowalny gotowym rozwiązaniem: MyLaps, Championchip, itp.)
Powód: decyzja użytkownika — pomiar czasu to standardowa, outsourcowalna usługa

[weryfikacja-użytkownika]: Wniosek Q-101 uzupełniony — reguła punktów kontrolnych.
Poprzednio: nieznana liczba punktów i próg dyskwalifikacji
Aktualnie: **2 stałe punkty kontrolne na trasie** (nie licząc startu i mety). Zawodnicy na dłuższych dystansach przejeżdżają przez te same punkty wielokrotnie. **1 pominięcie = dopuszczalne; 2 lub więcej pominięć = dyskwalifikacja.**
Powód: odpowiedź użytkownika

[weryfikacja-użytkownika]: Wniosek Q-103 uzupełniony — zasady zmiany dystansu w trakcie zawodów.
Poprzednio: zawodnik nie może startować na wielu dystansach
Aktualnie: Zawodnik zarejestrowany jest na **jednym dystansie**. W trakcie zawodów może:
- **Skrócić dystans** (pojechać mniej) → nie jest klasyfikowany na krótszym dystansie
- **Wydłużyć dystans** (upgrade) → dozwolone tylko do **sąsiedniego/przylegającego dystansu** (np. 600→650 lub 600→700), NIE do dowolnie wyższego (300 NIE może przejść na 600)
Reguła wydłużenia: zależy od aktualnie jedzonego dystansu — możliwy jest tylko upgrade do najbliższego wyższego.
Powód: odpowiedź użytkownika
