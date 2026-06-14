# Subdomeny — System Zarządzania 24h Maratonem Rowerowym

Odkrywanie heurystykami H1–H4 na podstawie zdarzeń z Fazy 1 i sesji analizy.

---

## SD-001 — Konfiguracja Maratonu

**Odpowiedzialność:** Definiowanie parametrów imprezy: terminy, dystanse, limity uczestników, okna czasowe zmian grupy, regulamin, promocje.

**Heurystyki:**
- H2: Decydent = Główny organizator (A-004) — jedyna osoba mająca prawo zmiany parametrów imprezy
- H3: Zmienia się rzadko (raz przed imprezą), z innego powodu niż reszta (strategia, regulamin federacji)
- H4: Można by oddzielić jako CMS/formularz konfiguracyjny — naturalna granica

**Zdarzenia z Fazy 1:**
- E-001 Zarejestrowano maraton
- E-002 Skonfigurowano parametry maratonu
- E-003 Opublikowano regulamin maratonu

---

## SD-002 — Rejestracja

**Odpowiedzialność:** Przyjmowanie uczestników na imprezę: rejestracja danych osobowych, wybór dystansu, zmiana dystansu przed opłaceniem, weryfikacja danych.

**Heurystyki:**
- H1: "Uczestnik" (A-002) vs "Zawodnik" (A-001) — ta sama osoba, inne słownictwo w tym vs innym obszarze (H-013 z Fazy 1). Granica językowa = granica subdomeny.
- H2: Decydent o ważności danych = Administrator (A-003) / System; decydent o dyskwalifikacji = inny kontekst
- H3: Rejestracja zmienia się gdy zmieniają się wymagania formalne (formularz, pola); zmienia się niezależnie od opłat
- H4: Zewnętrzna platforma rejestracyjna WYKLUCZONA — ale granica jest naturalna

**Zdarzenia z Fazy 1:**
- E-004 Zarejestrowano uczestnika
- E-005 Wybrano dystans
- E-006 Potwierdzono dane kontaktowe
- E-007 Zweryfikowano zawodnika
- E-008 Wyliczono opłatę
- E-009 Złożono wniosek o zmianę dystansu
- E-010 Zmieniono dystans
- E-011 Wyliczono opłatę (zmiana dystansu)
- E-012 Poinformowano użytkownika o zmianie opłaty
- E-013 Potwierdzono rejestrację
- E-014 Anulowano rejestrację
- E-015 Przekroczono termin płatności

---

## SD-003 — Opłata za Udział

**Odpowiedzialność:** Obsługa cyklu płatniczego uczestnika: pełna opłata, promocja -100%, wykrycie i obsługa niedopłaty, dopłata za zmianę dystansu, zwrot nadpłaty, odpisanie uczestnictwa.

**Heurystyki:**
- H1: "Przelew" dla banku vs "opłata" dla systemu — dwa języki, jeden interfejs (ACL potrzebny od strony banku)
- H2: Logika przeliczeń kwot = właściciel Opłata; decyzja o promocji = Organizator; rozliczenie banku = zewnętrzne
- H3: Zmienia się gdy zmieniają się reguły cenowe, promocje, polityka zwrotów — niezależnie od rejestracji
- H4: Można zastąpić zewnętrznym systemem płatniczym (z ACL), ale tu reguły domenowe są złożone

**Zdarzenia z Fazy 1:**
- E-016 Rozpoczęto opłacenie udziału
- E-017 Zarejestrowano przelew (zewnętrzne)
- E-018 Zaaplikowano opłatę
- E-019 Opłacono uczestnictwo
- E-020 Zaaplikowano promocje (-100%)
- E-021 Wykryto niedopłatę
- E-022 Rozpoczęto opłacenie niedopłaty
- E-023 Opłacono niedopłatę
- E-024 Wyliczono opłatę za nowy dystans
- E-025 Opłacono zmianę dystansu
- E-026 Wyliczono kwotę zwrotu
- E-027 Zwrócono nadpłatę
- E-028 Opłacono dystans
- E-029 Odpisano uczestnictwo

---

## SD-004 — Przydział Numeru Startowego

**Odpowiedzialność:** Generowanie i przypisanie unikalnego numeru startowego uczestnikowi, który spełnił warunki (opłacił, zarejestrował się), zarządzanie pulą numerów.

**Heurystyki:**
- H1: "Numer startowy" to inny byt niż "numer RFID" — kandydat na dwa pojęcia (H-009)
- H2: Decydent nad pulą numerów = Administrator/System; reguła przydziału = własna logika
- H3: Zmienia się gdy zmienia się pula numerów lub logika przydziału (rzadko)
- H4: Może działać jako prosty mikroserwis CRUD — ale zależy od warunków domenowych

**Zdarzenia z Fazy 1:**
- E-030 Przypisano numer startowy
- E-068 Przypisano numer startowy (nowy) — przy zgubieniu dyskietki

---

## SD-005 — Generacja Grup Startowych

**Odpowiedzialność:** Automatyczne tworzenie grup startowych na podstawie listy opłaconych uczestników z numerami startowymi, w terminie określonym w regulaminie, z zachowaniem limitów grup.

**Heurystyki:**
- H1: "Generacja grupy" (jednorazowy akt systemowy) vs "zarządzanie grupą" (wnioski, zmiany) — różny język
- H2: Decydent inicjujący = Administrator; algorytm = System; wynik jest niezmienny po upłynięciu okna
- H3: Zmienia się gdy zmienia się algorytm grupowania lub warunki — może ewoluować (core?)
- H4: Trudno zastąpić zewnętrznym produktem, bo reguły są specyficzne domenowo

**Zdarzenia z Fazy 1:**
- E-031 Przypisano zawodnika do grupy
- E-032 Przypisano zawodnika do grupy (automatycznie)
- E-033 Wygenerowano grupy startowe
- E-034 Nie udało się wygenerować grup startowych
- E-035 Grupy startowe zostały wygenerowane
- E-036 Wygenerowano ostateczne grupy [PE-001]

---

## SD-006 — Grupy Startowe (Zarządzanie)

**Odpowiedzialność:** Obsługa wniosków o zmianę grupy przez zawodników, egzekwowanie reguł (jednorazowość, wolne miejsca, ten sam dystans, okno czasowe), blokada po terminie.

**Heurystyki:**
- H1: "Wniosek o zmianę grupy" ≠ "generacja grupy" — różny język (podanie vs akt systemowy)
- H2: Decydent nad przyjęciem wniosku = Administrator; reguły walidacji = System
- H3: Zmienia się gdy zmieniają się reguły zmiany grupy — niezależnie od generacji
- H4: To okno czasowe tymczasowe — po PE-002 ta subdomena "zamiera"

**Zdarzenia z Fazy 1:**
- E-037 Złożono wniosek o zmianę grupy
- E-038 Nie złożono wcześniej wniosku
- E-039 Przyjęto wniosek do zmiany grupy
- E-040 Nie przyjęto wniosku o zmianę grupy
- E-041 Grupa została zmieniona
- E-042 Grupa nie została zmieniona
- E-043 Zmieniono grupę
- E-044 Nie udało się zmienić grupy
- E-045 Odmówiono zmiany grupy z powodu braku opłaty
- E-046 Nie przypisano do ostatecznej grupy
- E-047 Złożono wniosek o zmianę dystansu (przed generowaniem)
- E-048 Złożono wniosek o zmianę dystansu (po terminie)
- E-049 Dołączono do grupy
- E-050 Nadobiegnięto do grupy
- E-051 Przekazano opłatę za zmianę grupy
- E-052 Minął czas na ewentualne zmiany grupy [PE-002]
- E-053 Upłynął czas na zmianę grupy
- E-054 Upłynął termin zmiany grupy dystansowo

---

## SD-007 — Weryfikacja i Kwalifikacja Startowa

**Odpowiedzialność:** Check-in zawodnika przed startem: weryfikacja tożsamości, podpisanie oświadczenia, wydanie pakietu startowego i dyskietki RFID, niedopuszczenie do startu w przypadku niespełnienia warunków.

**Heurystyki:**
- H1: "Weryfikacja biurowa" (dane) vs "weryfikacja na starcie" (fizyczna) — dwa słowa, dwa znaczenia, ale w tej samej subdomenie? H-008 z Fazy 1 otwarte
- H2: Decydent = Obsługa punktu kontrolnego (A-005) — różny od Administratora (A-003)
- H3: Zmienia się gdy zmienia się proces check-in (np. nowy formularz oświadczeń) — rzadko
- H4: Fizyczny check-in nie jest łatwy do outsource'owania, ale logika systemowa jest prosta

**Zdarzenia z Fazy 1:**
- E-055 Zweryfikowano tożsamość zawodnika
- E-056 Zawodnik nie przeszedł weryfikacji
- E-057 Podpisano oświadczenie zawodnika
- E-058 Zawodnik nie podpisał oświadczenia
- E-059 Wydano pakiet startowy
- E-060 Wydano dyskietkę RFID zawodnikowi
- E-061 Zweryfikowano zawodnika przed startem
- E-062 Zawodnik nie przeszedł weryfikacji (przed startem)

---

## SD-008 — Start Zawodników

**Odpowiedzialność:** Zarządzanie fizycznym startem: start grupowy/indywidualny, rejestracja czasu startu, zamknięcie listy startowej, obsługa spóźnień, obsługa zgubionych dyskietek, zamknięcie zapisów.

**Heurystyki:**
- H1: "Start grupowy" vs "start indywidualny" — różny tryb, ale ta sama subdomena operacyjna
- H2: Decydent = Główny organizator/Administrator/Sędzia — nakładanie się ról (hot spot)
- H3: Zmienia się gdy zmienia się procedura startowa — rzadko, ale jest złożona
- H4: Silnie związany z fizycznym zdarzeniem — trudno outsource'ować

**Zdarzenia z Fazy 1:**
- E-063 Zgłoszono indywidualny start
- E-064 Wyciągnięto zawodnika z grupy
- E-065 Zgubiono dyskietkę z numerem startowym
- E-066 Przejęto kaucję
- E-067 Odpięto numer startowy
- E-069 Wydano nową dyskietkę
- E-070 Wystartowano grupę
- E-071 Zarejestrowano czas startu
- E-072 Wystartowano zawodnika
- E-073 Wystartowano zawodników [PE-003]
- E-074 Wystartowano zawodników na danym dystansie
- E-075 Zamknięto zapisy na dany dystans
- E-076 Zakończono udział przedwcześnie
- E-077 Nie stawił się na starcie

---

## SD-009 — Pomiar Czasu i Wyniki

**Odpowiedzialność:** Rejestracja pomiarów RFID na checkpointach, obliczanie wyników i międzyczasów, wyświetlanie real-time, logika klasyfikacji zawodników, generacja rankingu końcowego.

**Heurystyki:**
- H1: "Pomiar" (akt fizyczny RFID) vs "wynik" (kalkulacja domenowa) vs "ranking" (agregacja) — trzy różne języki, kandydat na podział, ale ten sam właściciel
- H2: Logika klasyfikacji = właściciel tej subdomeny; decyzja o dyskwalifikacji = Sędzia (poza zakresem)
- H3: Zmienia się gdy zmieniają się reguły klasyfikacji — może ewoluować (core?)
- H4: RFID hardware jest zewnętrzny (I-002), logika przetwarzania jest wewnętrzna

**Zdarzenia z Fazy 1:**
- E-078 Przyłożenie dyskietki do czytnika (zewnętrzne)
- E-079 Zarejestrowano pomiar czasu
- E-080 Wyświetlono wyniki w czasie rzeczywistym
- E-081 Zawodnik zakończył przejazd
- E-082 Zgłoszono prośbę o przedłużenie dystansu
- E-083 Przedłużono przejazd zawodnika
- E-084 Odnotowano dodatkowy pomiar czasu
- E-085 Zawodnik zakończył przejazd (wydłużony)
- E-086 Podliczono czasy przejazdu
- E-087 Zakończono przejazd zawodników na danym dystansie
- E-088 Upłynął czas maratonu [czasowe]
- E-089 Wygenerowano ranking wyników
- E-090 Zakończono przejazdy wszystkich dystansów [PE-004]

---

## SD-010 — Zakończenie Zawodów i Ceremonial

**Odpowiedzialność:** Obsługa fazy po-zawodowej: przyznanie nagród, generacja dyplomów, zwrot pakietów startowych, obsługa kaucji (zwrot lub zatrzymanie), publikacja wyników końcowych.

**Heurystyki:**
- H1: "Dyplom" to osobny akt od "nagrody" i od "zwrotu kaucji" — różne znaczenia, jeden obszar ceremonialny
- H2: Decydent nagrody = Główny organizator; decydent dyplomu = System (automatycznie); kaucja = Obsługa (A-005)
- H3: Zmienia się gdy zmienia się format dyplomu, polityka kaucji — rzadko
- H4: Można outsource'ować druk dyplomów, ale logika jest wewnętrzna (potwierdzone)

**Zdarzenia z Fazy 1:**
- E-091 Opublikowano wyniki końcowe
- E-092 Przyznano nagrodę
- E-093 Wygenerowano dyplom
- E-094 Otrzymano dyplom
- E-095 Zwrócono pakiet startowy
- E-096 Kaucja została zwrócona
- E-097 Upłynął czas zwrotu pakietu
- E-098 Nie zwrócono pakietu startowego
- E-099 Kaucja zatrzymana

---

## Uwaga techniczna: kandydaci na podział / scalenie

- SD-005 i SD-006 to dwa różne etapy procesu grupowego, ale język różny → utrzymujemy jako dwa BC
- SD-009 (Pomiar+Wyniki) jest kandydatem na rozbicie — patrz Krok 3 (T3)
- SD-007 i SD-008 mają nakładające się role aktorów (A-005, A-003) — sprawdzić w BC
