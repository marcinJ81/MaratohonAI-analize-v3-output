# Typologia Subdomen — System Zarządzania 24h Maratonem Rowerowym

Klasyfikacja core / supporting / generic per organizacja (mała firma/stowarzyszenie prowadzące maraton rowerowy).

---

## SD-001 — Konfiguracja Maratonu → **Supporting**

**T1 (kup-czy-buduj):** Istnieją gotowe CMS-y/formularze konfiguracyjne, ale wymagają adaptacji do specyficznych pól maratonu (dystanse, okna czasowe). Możliwe zastąpienie prostym CRUD-em.
**T2 (najtańsze rozwiązanie):** Prosty formularz administracyjny wystarczy. Biznes nie wymaga lepszego niż minimum.
**T3 (zmienności):** Stan "gotowe" jest osiągalny — raz przed imprezą, potem zamrożone.
**T4 (koszt błędu):** Błąd w konfiguracji (np. zły termin) jest poważny, ale to błąd operacyjny, nie domenowy. Nie wymaga bogatego modelu.

**Uzasadnienie:** Prosta, konfiguracyjna, zmienia się rzadko. CRUD jest wystarczający. Nie wyróżnia organizatora na rynku — każdy maraton ma konfigurację.

---

## SD-002 — Rejestracja → **Supporting**

**T1:** Istnieją gotowe platformy rejestracyjne (Eventbrite, dedykowane systemy). Jednak zewnętrzna platforma jest WYKLUCZONA przez domenę (potwierdzone). Budujemy sami, ale logika jest prosta.
**T2:** Najtańsze działające rozwiązanie (formularz rejestracyjny + walidacja) wystarczy biznesowi.
**T3:** Stan "gotowe" osiągalny — formularz się stabilizuje.
**T4:** Błąd rejestracji (zły dystans, złe dane) jest kosztowny ale rzadki i łatwy do naprawy przez Administratora.

**Uzasadnienie:** Konieczne, ale nie wyróżniające. Logika umiarkowanie złożona (ścieżki zmian dystansu), ale nie jest to przewaga konkurencyjna. Supporting.

**Uwaga: Pytanie Q-006 (Uczestnik vs Zawodnik) może zmienić ocenę jeśli okaże się że to dwa agregaty z bogatą logiką stanową → przeniesienie do Core.**

---

## SD-003 — Opłata za Udział → **Supporting**

**T1:** Systemy płatnicze (Stripe, PayU) istnieją — ale tu mamy bank z przelewami (nie bramkę płatniczą). Logika przeliczeń i ścieżek (5+ wariantów) jest własna.
**T2:** Najtańsze rozwiązanie to prosta ewidencja przelewów. Jednak złożoność (niedopłata, promocja -100%, zmiana dystansu, zwroty) wymaga własnej logiki.
**T3:** Może ewoluować (nowe promocje, nowe reguły płatności) — ale stabilizuje się.
**T4:** Błąd finansowy (zła kwota, przeoczenie niedopłaty) jest kosztowny. Jednak to koszt operacyjny, nie strategiczny.

**Uzasadnienie:** Umiarkowanie złożona, ale nie jest przewagą konkurencyjną. Konieczne. Supporting. Granica z bankiem wymaga ACL (różne języki: przelew vs opłata).

---

## SD-004 — Przydział Numeru Startowego → **Generic**

**T1:** Numery startowe to sekwencja lub pula — gotowe biblioteki/wzorce istnieją. Logika trywialna.
**T2:** Najtańsze rozwiązanie (auto-increment lub pula przedalokowanych numerów) wystarczy.
**T3:** Stan "gotowe" natychmiast — brak ewolucji logiki.
**T4:** Błąd (zduplikowany numer) jest kłopotliwy ale łatwy do naprawy. Niski koszt domenowy.

**Uzasadnienie:** Trivial CRUD. Nie budujemy przewagi konkurencyjnej na logice numeracji. Generic (można by użyć tabeli sekwencyjnej z dowolnej bazy danych).

---

## SD-005 — Generacja Grup Startowych → **CORE DOMAIN**

**T1:** Brak gotowego produktu który robi to samo. Algorytm grupowania jest specyficzny dla tej domeny (dystanse 50–700 km, okna czasowe, limity, reguły kto wchodzi do której grupy, reguła niedopłaty).
**T2:** Najtańsze rozwiązanie NIE wystarczy — błąd w generacji grup to katastrofa operacyjna (zawodnicy w złych grupach = błędne wyniki = brak zaufania do organizatora).
**T3:** BRAK stanu "gotowe" — reguły mogą ewoluować z edycji na edycję (nowe dystanse, nowe reguły wejścia do grupy).
**T4:** Najdroższy błąd domenowy: złe grupy = zła klasyfikacja = roszczenia zawodników = utrata reputacji. Biznes inwestuje tu najwięcej uwagi eksperckiej.

**Uzasadnienie:** To jest serce systemu. Żaden gotowy produkt tego nie robi. Algorytm jest złożony i specyficzny. Błąd jest najdroższy. Ewoluuje. → **CORE DOMAIN**

**Dodatkowe uzasadnienie:** H3 (zmienności) — generacja grup łączy w sobie reguły z wielu innych subdomen (płatności, numery, konfiguracja) i musi być spójna w jednym momencie decyzyjnym. To naturalne centrum grawitacji domeny.

---

## SD-006 — Grupy Startowe (Zarządzanie) → **Supporting**

**T1:** Zarządzanie listami z wnioskami to pattern znany (workflow z walidacją). Gotowe rozwiązania istnieją.
**T2:** Prosta walidacja wielowarunkowa (5 warunków zmiany grupy) jest możliwa tanio.
**T3:** Reguły zmiany grupy stabilizują się szybko. Stan "gotowe" osiągalny.
**T4:** Błąd (zezwolenie na zmianę po terminie) jest kosztowny ale rzadki.

**Uzasadnienie:** Konieczne, ale po wygenerowaniu grup to "okno obsługi" z prostą logiką. Supporting. Wyraźnie rozdzielone od SD-005 przez PE-001.

---

## SD-007 — Weryfikacja i Kwalifikacja Startowa → **Supporting**

**T1:** Check-in fizyczny z listą to standard — dowolny system CRM/zarządzania eventami mógłby to obsłużyć.
**T2:** Najtańsze rozwiązanie (lista na tablecie z checkbox) wystarczy.
**T3:** Stabilizuje się szybko.
**T4:** Błąd (wypuszczenie niezweryfikowanego zawodnika) jest poważny bezpieczeństwo, ale to procedura fizyczna, nie złożona logika domenowa.

**Uzasadnienie:** Konieczne, ale rutynowe. Supporting.

---

## SD-008 — Start Zawodników → **Supporting**

**T1:** Zarządzanie startem to standard operacyjny wielu imprez sportowych.
**T2:** Prosta lista + rejestracja czasu. Wystarczy minimum.
**T3:** Stabilizuje się.
**T4:** Błąd (zły czas startu) jest kosztowny dla wyników — ale czas startu to dane, nie logika domenowa.

**Uzasadnienie:** Operacyjne. Supporting. Ale ściśle sprzężone z PE-003 jako trigger dla Pomiaru Czasu.

---

## SD-009 — Pomiar Czasu i Wyniki → **CORE DOMAIN (współdzielony)**

**T1:** Systemy pomiaru czasu istnieją (chip timing, Race Result, itd.) — ale zewnętrzna integracja nie jest tu standardem, a logika klasyfikacji (reguły 24h, dystans opłacony vs próba dłuższego) jest specyficzna.
**T2:** Najtańsze rozwiązanie nie wystarczy — reguły klasyfikacji są złożone i specyficzne dla tego maratonu. Błąd = protest zawodnika.
**T3:** Reguły klasyfikacji mogą ewoluować (nowe dystanse, nowe reguły). Brak stanu "gotowe".
**T4:** Błąd klasyfikacji = najdroższy błąd post-zawodowy (roszczenia, odwołania).

**Uzasadnienie:** Logika klasyfikacji (kto jest nieklasyfikowany, kto na jakim dystansie, reguła opłaconego dystansu) jest złożona i specyficzna. Kandydat na Core.

**UWAGA:** Istnieje napięcie między SD-005 (Generacja Grup) a SD-009 (Pomiar/Wyniki) o tytuł Core Domain. Decyzja: **SD-005 jest Core Domain** (kompleksowość algorytmu, punkt bez powrotu PE-001), SD-009 jest **Core Supporting** (złożona logika, ale mniej kluczowa strategicznie — wyniki są determinowane przez fizyczne przejazdy, nie przez algorytm).

**Finalna klasyfikacja SD-009: Supporting (złożony)** — z adnotacją że logika klasyfikacji wymaga bogatego modelu.

---

## SD-010 — Zakończenie Zawodów i Ceremonial → **Supporting**

**T1:** Systemy ceremonii, dyplomów, zarządzania kaucjami — gotowe wzorce.
**T2:** Minimum (druk dyplomu PDF, zwrot kaucji przy oddaniu pakietu) wystarczy.
**T3:** Stabilizuje się.
**T4:** Błąd (zły dyplom, nieoddana kaucja) jest kłopotliwy ale nie katastrofalny.

**Uzasadnienie:** Post-procesowe, rutynowe. Supporting.

---

## Podsumowanie typologii

| ID     | Nazwa | Typ |
|--------|-------|-----|
| SD-001 | Konfiguracja Maratonu | Supporting |
| SD-002 | Rejestracja | Supporting |
| SD-003 | Opłata za Udział | Supporting |
| SD-004 | Przydział Numeru Startowego | Generic |
| SD-005 | Generacja Grup Startowych | **Core** |
| SD-006 | Grupy Startowe (Zarządzanie) | Supporting |
| SD-007 | Weryfikacja i Kwalifikacja Startowa | Supporting |
| SD-008 | Start Zawodników | Supporting |
| SD-009 | Pomiar Czasu i Wyniki | Supporting (złożony) |
| SD-010 | Zakończenie Zawodów i Ceremonial | Supporting |

**Core Domain: SD-005 — Generacja Grup Startowych**

Łączna liczba: 1 Core, 8 Supporting, 1 Generic.
