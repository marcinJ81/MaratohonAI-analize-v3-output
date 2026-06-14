---
phase: big-picture
session: System Zarządzania 24h Maratonem Rowerowym
generated: 2026-06-14
status: completed
coverage: 83%
input-quality: accepted
lossy-files: 15
sources:
  - state/inputs/processed/phase-1-seed.md
  - state/inputs/processed/generacja grup startowych-processed.md
  - state/inputs/processed/generacja_grup_poterminie-processed.md
  - state/inputs/processed/genreacja_numeru_indywidualna-processed.md
  - state/inputs/processed/grupy_startowe_ds-processed.md
  - state/inputs/processed/kontekst_rejestracja1-processed.md
  - state/inputs/processed/ogólny_widok_kontekstu_rejestracja-processed.md
  - state/inputs/processed/opłata_za_udział_1-processed.md
  - state/inputs/processed/opłata_za_udział_2-processed.md
  - state/inputs/processed/pomiar_czasu-processed.md
  - state/inputs/processed/przypisanie_zawodnika_do_listy-processed.md
  - state/inputs/processed/start_indywidualny-processed.md
  - state/inputs/processed/utowrzenie_grup_ds-processed.md
  - state/inputs/processed/wystartowano_zawodników-processed.md
  - state/inputs/processed/zakończenie_zawodów-processed.md
  - state/inputs/processed/zminia_grupystartowej-processed.md
---

# Big Picture — System Zarządzania 24h Maratonem Rowerowym

## Kontekst

Analizowano system obsługi 24-godzinnego maratonu rowerowego: od konfiguracji imprezy, przez rejestrację uczestników i obsługę płatności bankowych, generację grup startowych, pomiar czasu przez RFID na checkpointach, po zakończenie zawodów z dyplomami i zwrotem kaucji. Cel analizy: weryfikacja kompletności procesu (tryb audit), identyfikacja brakujących zdarzeń i niespójności. System oceniony jako wysoce złożony: 10 kontekstów domenowych, ~80 zdarzeń domenowych (po uzupełnieniach z weryfikacji od tyłu), 2 integracje zewnętrzne.

---

## Aktorzy

| ID    | Nazwa | Rola | Zewnętrzny? | Uwagi |
|-------|-------|------|-------------|-------|
| A-001 | Zawodnik | Uczestnik po wydaniu pakietu startowego — startuje, jedzie, zwraca pakiet | nie | Ten sam człowiek co A-002 w innym stanie procesu. Inicjuje fizyczny trigger RFID. |
| A-002 | Uczestnik | Uczestnik przed wydaniem pakietu — rejestruje się, opłaca, składa wnioski | nie | Stan przed check-inem. Po weryfikacji i wydaniu pakietu staje się A-001 (Zawodnikiem). Hot spot H-013. |
| A-003 | Administrator | Zarządzanie uczestnikami, generacja grup, przypisywanie numerów, biuro | nie | Rola biurowa. Może działać w imieniu uczestnika. |
| A-004 | Główny organizator zawodów | Decyzje strategiczne: promocje, nagrody, przedłużenia dystansu | nie | Najwyższy autorytet domenowy. |
| A-005 | Obsługa punktu kontrolnego | Check-in, wydanie i odbiór pakietów, obsługa dyskietek, kaucje | nie | Fizyczna obsługa na checkpointach i w biurze zawodów. |
| A-006 | Sędzia trasy | Zakończenie udziału przedwcześnie | nie | Bardzo ograniczona rola w systemie; kary/dyskwalifikacja POZA ZAKRESEM. |
| A-007 | Interfejs banku | Push zdarzeń przelewów do systemu | tak | Zewnętrzny system płatniczy — async event push po przelewie uczestnika. |
| A-008 | System RFID (czytnik dyskietek) | Fizyczny trigger pomiaru czasu na checkpointach | tak | Hardware zewnętrzny; zawodnik przyłoża kartę, czytnik generuje zdarzenie. |
| A-009 | System (automatyczna generacja) | Logika automatyczna: grupy, numery, wyliczenia, rankingi, dyplomy | nie | Wewnętrzny aktor systemowy — silnik reguł. |

---

## Zdarzenia domenowe

Zdarzenia w kolejności chronologicznej. Zdarzenia z weryfikacji od tyłu oznaczone [K4] lub [K5].

| ID    | Zdarzenie | Aktor (ID) | Typ | Obszar | Hot Spot? |
|-------|-----------|------------|-----|--------|-----------|
| E-001 | Zarejestrowano maraton | A-004 | domenowe | KonfiguracjaMaratonu | nie |
| E-002 | Skonfigurowano parametry maratonu | A-004 | domenowe | KonfiguracjaMaratonu | nie — [K4] brakujące |
| E-003 | Opublikowano regulamin maratonu | A-004 | domenowe | KonfiguracjaMaratonu | nie — [K5] brakujące |
| E-004 | Zarejestrowano uczestnika | A-002 | domenowe | Rejestracja | tak (H-011) |
| E-005 | Wybrano dystans | A-002 | domenowe | Rejestracja | nie |
| E-006 | Potwierdzono dane kontaktowe | A-002 | domenowe | Rejestracja | nie |
| E-007 | Zweryfikowano zawodnika | A-003/A-009 | domenowe | Rejestracja | nie |
| E-008 | Wyliczono opłatę | A-009 | domenowe | Rejestracja | nie |
| E-009 | Złożono wniosek o zmianę dystansu | A-002 | domenowe | Rejestracja | nie |
| E-010 | Zmieniono dystans | A-003 | domenowe | Rejestracja | nie |
| E-011 | Wyliczono opłatę (zmiana dystansu) | A-009 | domenowe | Rejestracja | nie |
| E-012 | Poinformowano użytkownika o zmianie opłaty | A-009 | domenowe | Rejestracja | nie |
| E-013 | Potwierdzono rejestrację | A-009 | domenowe | Rejestracja | tak (H-011) — [K5] brakujące |
| E-014 | Anulowano rejestrację | A-009/A-003 | domenowe | Rejestracja | tak (H-012) — [K5] edge case |
| E-015 | Przekroczono termin płatności | — | czasowe | Rejestracja/Opłata | tak (H-012) — [K5] edge case |
| E-016 | Rozpoczęto opłacenie udziału | A-002/A-003 | domenowe | OpłataZaUdział | nie |
| E-017 | Zarejestrowano przelew | A-007 | zewnętrzne | OpłataZaUdział | nie |
| E-018 | Zaaplikowano opłatę | A-009 | domenowe | OpłataZaUdział | nie |
| E-019 | Opłacono uczestnictwo | A-009 | domenowe | OpłataZaUdział | nie |
| E-020 | Zaaplikowano promocje (-100%) | A-004/A-009 | domenowe | OpłataZaUdział | nie |
| E-021 | Wykryto niedopłatę | A-009 | domenowe | OpłataZaUdział | tak (H-003) — [K5] brakujące |
| E-022 | Rozpoczęto opłacenie niedopłaty | A-002 | domenowe | OpłataZaUdział | tak (H-003) |
| E-023 | Opłacono niedopłatę | A-009 | domenowe | OpłataZaUdział | tak (H-003) |
| E-024 | Wyliczono opłatę za nowy dystans | A-009 | domenowe | OpłataZaUdział | nie |
| E-025 | Opłacono zmianę dystansu | A-009 | domenowe | OpłataZaUdział | nie |
| E-026 | Wyliczono kwotę zwrotu | A-009 | domenowe | OpłataZaUdział | nie |
| E-027 | Zwrócono nadpłatę | A-009 | domenowe | OpłataZaUdział | nie |
| E-028 | Opłacono dystans | A-009 | domenowe | OpłataZaUdział | nie |
| E-029 | Odpisano uczestnictwo | A-004/A-003 | domenowe | OpłataZaUdział | tak (H-007) — częściowo nieczytelne |
| E-030 | Przypisano numer startowy | A-009/A-003 | domenowe | PrzydziałNumeruStartowego | nie |
| E-031 | Przypisano zawodnika do grupy | A-009 | domenowe | GeneracjaGrupStartowych | nie |
| E-032 | Przypisano zawodnika do grupy (automatycznie) | A-009 | domenowe | GeneracjaGrupStartowych | nie |
| E-033 | Wygenerowano grupy startowe | A-003/A-009 | domenowe | GeneracjaGrupStartowych | nie |
| E-034 | Nie udało się wygenerować grup startowych | A-009 | domenowe | GeneracjaGrupStartowych | tak (H-018) |
| E-035 | Grupy startowe zostały wygenerowane | A-009 | domenowe | GeneracjaGrupStartowych | nie |
| E-036 | Wygenerowano ostateczne grupy | A-009 | domenowe | GeneracjaGrupStartowych | nie — PE-001 |
| E-037 | Złożono wniosek o zmianę grupy | A-001 | domenowe | GrupyStartowe | nie |
| E-038 | Nie złożono wcześniej wniosku | A-009 | domenowe | GrupyStartowe | nie |
| E-039 | Przyjęto wniosek do zmiany grupy | A-003 | domenowe | GrupyStartowe | nie |
| E-040 | Nie przyjęto wniosku o zmianę grupy | A-003/A-009 | domenowe | GrupyStartowe | nie |
| E-041 | Grupa została zmieniona | A-009 | domenowe | GrupyStartowe | nie |
| E-042 | Grupa nie została zmieniona | A-009 | domenowe | GrupyStartowe | nie |
| E-043 | Zmieniono grupę | A-009 | domenowe | GrupyStartowe | nie |
| E-044 | Nie udało się zmienić grupy | A-009 | domenowe | GrupyStartowe | nie |
| E-045 | Odmówiono zmiany grupy z powodu braku opłaty | A-009 | domenowe | GrupyStartowe | nie — [K5] brakujące |
| E-046 | Nie przypisano do ostatecznej grupy | A-009 | domenowe | GrupyStartowe | nie |
| E-047 | Złożono wniosek o zmianę dystansu (przed generowaniem) | A-001 | domenowe | GrupyStartowe | nie |
| E-048 | Złożono wniosek o zmianę dystansu (po terminie) | A-001 | domenowe | GrupyStartowe | nie |
| E-049 | Dołączono do grupy | A-009 | domenowe | GrupyStartowe | nie |
| E-050 | Nadobiegnięto do grupy | A-009 | domenowe | GrupyStartowe | tak (H-002) — częściowo nieczytelne |
| E-051 | Przekazano opłatę za zmianę grupy | A-002 | domenowe | GrupyStartowe | tak (H-002) — częściowo nieczytelne |
| E-052 | Minął czas na ewentualne zmiany grupy | — | czasowe | GrupyStartowe | nie — PE-002 |
| E-053 | Upłynął czas na zmianę grupy | — | czasowe | GrupyStartowe | nie |
| E-054 | Upłynął termin zmiany grupy dystansowo | — | czasowe | GrupyStartowe | nie |
| E-055 | Zweryfikowano tożsamość zawodnika | A-005 | domenowe | WeryfikacjaStartu | tak (H-008) |
| E-056 | Zawodnik nie przeszedł weryfikacji | A-005 | domenowe | WeryfikacjaStartu | tak (H-008) |
| E-057 | Podpisano oświadczenie zawodnika | A-001/A-005 | domenowe | WeryfikacjaStartu | nie |
| E-058 | Zawodnik nie podpisał oświadczenia | A-001 | domenowe | WeryfikacjaStartu | nie |
| E-059 | Wydano pakiet startowy | A-005 | domenowe | WeryfikacjaStartu | tak (H-009) |
| E-060 | Wydano dyskietkę RFID zawodnikowi | A-005 | domenowe | WeryfikacjaStartu | tak (H-009) — [K5] niejawne |
| E-061 | Zweryfikowano zawodnika przed startem | A-005 | domenowe | WeryfikacjaStartu | tak (H-008) |
| E-062 | Zawodnik nie przeszedł weryfikacji (przed startem) | A-005 | domenowe | WeryfikacjaStartu | tak (H-008) |
| E-063 | Zgłoszono indywidualny start | A-001 | domenowe | StartZawodników | nie |
| E-064 | Wyciągnięto zawodnika z grupy | A-003 | domenowe | StartZawodników | nie |
| E-065 | Zgubiono dyskietkę z numerem startowym | A-001 | domenowe | StartZawodników | nie |
| E-066 | Przejęto kaucję | A-005 | domenowe | StartZawodników | nie |
| E-067 | Odpięto numer startowy | A-005 | domenowe | StartZawodników | nie |
| E-068 | Przypisano numer startowy (nowy) | A-005/A-003 | domenowe | StartZawodników | nie |
| E-069 | Wydano nową dyskietkę | A-005 | domenowe | StartZawodników | nie |
| E-070 | Wystartowano grupę | A-004/A-003/A-006 | domenowe | StartZawodników | nie |
| E-071 | Zarejestrowano czas startu | A-009/A-005 | domenowe | StartZawodników | nie |
| E-072 | Wystartowano zawodnika | A-009 | domenowe | StartZawodników | nie |
| E-073 | Wystartowano zawodników | A-009 | domenowe | StartZawodników | nie — PE-003 |
| E-074 | Wystartowano zawodników na danym dystansie | A-009 | domenowe | StartZawodników | nie |
| E-075 | Zamknięto zapisy na dany dystans | A-009 | domenowe | StartZawodników | nie |
| E-076 | Zakończono udział przedwcześnie | A-001/A-006 | domenowe | StartZawodników/PomiarCzasu | tak (H-020) |
| E-077 | Nie stawił się na starcie | A-001 | domenowe | StartZawodników | tak (H-019) — [K5] edge case |
| E-078 | Przyłożenie dyskietki do czytnika | A-001 | zewnętrzne | PomiarCzasu | nie |
| E-079 | Zarejestrowano pomiar czasu | A-008/A-009 | domenowe | PomiarCzasu | nie |
| E-080 | Wyświetlono wyniki w czasie rzeczywistym | A-009 | domenowe | PomiarCzasu | nie — [K4] potwierdzone |
| E-081 | Zawodnik zakończył przejazd | A-009 | domenowe | PomiarCzasu | nie |
| E-082 | Zgłoszono prośbę o przedłużenie dystansu | A-001 | domenowe | PomiarCzasu | nie |
| E-083 | Przedłużono przejazd zawodnika | A-004/A-003 | domenowe | PomiarCzasu | nie |
| E-084 | Odnotowano dodatkowy pomiar czasu | A-009 | domenowe | PomiarCzasu | nie |
| E-085 | Zawodnik zakończył przejazd (wydłużony) | A-009 | domenowe | PomiarCzasu | nie |
| E-086 | Podliczono czasy przejazdu | A-009 | domenowe | PomiarCzasu | tak (H-005) |
| E-087 | Zakończono przejazd zawodników na danym dystansie | A-009 | domenowe | PomiarCzasu | tak (H-010) |
| E-088 | Upłynął czas maratonu | — | czasowe | PomiarCzasu | tak (H-010) — [K5] brakujące |
| E-089 | Wygenerowano ranking wyników | A-009 | domenowe | PomiarCzasu | tak (H-005) — [K5] brakujące |
| E-090 | Zakończono przejazdy wszystkich dystansów | A-009 | domenowe | PomiarCzasu | nie — PE-004 |
| E-091 | Opublikowano wyniki końcowe | A-009/A-004 | domenowe | ZakończenieZawodów | tak (H-005) — [K5] brakujące |
| E-092 | Przyznano nagrodę | A-004 | domenowe | ZakończenieZawodów | nie |
| E-093 | Wygenerowano dyplom | A-009 | domenowe | ZakończenieZawodów | tak (H-006) — [K5] brakujące |
| E-094 | Otrzymano dyplom | A-001 | domenowe | ZakończenieZawodów | nie |
| E-095 | Zwrócono pakiet startowy | A-001/proxy | domenowe | ZakończenieZawodów | tak (H-015) |
| E-096 | Kaucja została zwrócona | A-005 | domenowe | ZakończenieZawodów | nie |
| E-097 | Upłynął czas zwrotu pakietu | — | czasowe | ZakończenieZawodów | nie |
| E-098 | Nie zwrócono pakietu startowego | A-009 | domenowe | ZakończenieZawodów | nie |
| E-099 | Kaucja zatrzymana | A-005/A-004 | domenowe | ZakończenieZawodów | tak (H-007) — częściowo nieczytelne |
| E-100 | Zdyskwalifikowano zawodnika (kwalifikacja) | A-006 | domenowe | WeryfikacjaStartu | nie — POZA ZAKRESEM decyzja sędziego |

---

## Pivotal Events

| ID     | Zdarzenie (E-ID) | Dlaczego pivotal |
|--------|------------------|-----------------|
| PE-001 | E-036 — Wygenerowano ostateczne grupy startowe | Punkt bez powrotu dla standardowego przypisania do grup. Po tym zdarzeniu zmiany tylko przez BC-06 (GrupyStartowe) z restrykcjami. Właściciel danych przechodzi z GeneracjaGrupStartowych do GrupyStartowe. |
| PE-002 | E-052 — Minął czas na ewentualne zmiany grupy — przypisanie jest na stałe | Twarda blokada zmian grup. Żadna operacja modyfikacji grupy nie jest możliwa po tym zdarzeniu. Właściciel danych przechodzi do WeryfikacjaStartu. |
| PE-003 | E-073 — Wystartowano zawodników | Aktywacja pomiaru czasu. Zamknięcie zapisów na dany dystans. Przejście z fazy administracyjnej do operacyjnej. Po tym zdarzeniu lista startowa jest zamrożona. |
| PE-004 | E-090 — Zakończono przejazdy wszystkich dystansów | Trigger końca zawodów operacyjnych. Przejście do ceremonii. Zamknięcie pomiaru czasu dla wszystkich dystansów. Wyzwala podliczenie i generację rankingu. |

---

## Granice systemu

### Opis granic

System jest samodzielną aplikacją obsługującą pełen cykl życia 24h maratonu rowerowego. Zewnętrzne punkty integracji to bank (push zdarzeń przelewów) i fizyczny system RFID (push zdarzeń przyłożenia karty). Dyplomy, wyniki i rankingi są generowane wewnętrznie. Nie ma zewnętrznej platformy rejestracyjnej ani wysyłkowej.

Granica systemu nie jest wyraźnie określona w materiałach pod kątem UI: nie wiadomo czy strona WWW z wynikami real-time jest w tym samym systemie czy osobną aplikacją (H-001, H-016).

### Integracje zewnętrzne

| ID    | System zewnętrzny | Kierunek | Typ integracji | Uwagi |
|-------|-------------------|----------|----------------|-------|
| I-001 | Bank (przelew bankowy) | wchodzi | async / event-push | Bank pushuje zdarzenie po przelewie uczestnika. Zdarzenie: Zarejestrowano przelew (E-017). Brak informacji o modelu API (webhook? polling?). |
| I-002 | System RFID (czytnik dyskietek) | wchodzi | sync / event-push fizyczny | Czytnik RFID przesyła zdarzenie przyłożenia karty. Zdarzenie: Przyłożenie dyskietki do czytnika (E-078). Zawodnik jest fizycznym inicjatorem. |

---

## Hot Spoty

| ID    | Opis | Powód | Powiązane zdarzenia (ID) | Status | Priorytet |
|-------|------|-------|--------------------------|--------|-----------|
| H-001 | Brak explicite zdefiniowanej granicy systemu jako całości (co jest w systemie, co poza) | niejasność | — | open | wysoki |
| H-002 | Nota: "grupy mogą istnieć tylko po pewnych krokach jako reprezentacje list startowych..." — częściowo nieczytelna | brak wiedzy / strata konwersji | E-033, E-031 | open | średni |
| H-003 | Co zrobić gdy rozpocznie się generowanie list a czekamy na opłacenie niedopłaty? | konflikt — opłata vs termin generacji | E-033, E-021, E-023 | **resolved: uczestnik pozostaje na starym dystansie do chwili potwierdzenia płatności; generowanie nie blokuje — uczestnik wchodzi do grupy zgodnej z opłaconym dystansem** | ~~blocker~~ |
| H-004 | Wyświetlanie w czasie rzeczywistym nieobecne w materiałach źródłowych | strata konwersji | E-079 | resolved: potwierdzone przez orkiestratora — checkpointy na checkpointach 50 km co pętlę, biuro + WWW | niski |
| H-005 | Brak opisanego przepływu generowania wyników/rankingów po "Podliczono czasy przejazdu" | brak wiedzy | E-086, E-089, E-091 | open | wysoki |
| H-006 | Brak opisanego przepływu generowania dyplomów (trigger, kto, kiedy, format) | brak wiedzy | E-093, E-094 | open | wysoki |
| H-007 | Różowa karteczka przy Frame 51 — treść nieczytelna | strata konwersji | E-029, E-099 | open | średni |
| H-008 | Dwie odrębne weryfikacje zawodnika: biurowa (kwalifikacja) i na starcie — różnica procesowa niejasna | niejasność | E-055, E-061 | open | średni |
| H-009 | Pakiet startowy a dyskietka RFID — czy dyskietka jest częścią pakietu czy osobnym wydaniem? | niejasność | E-059, E-060 | open | średni |
| H-010 | Logika zamknięcia pomiaru — dwa triggery: "wszyscy dojechali" OR "minął czas". Który obowiązuje, co gdy obydwa? | konflikt | E-087, E-088, E-090 | **resolved: trigger = upłynięcie czasu zawodów (24h). Zawodnicy na trasie po czasie → "jadą dla siebie" (nieklasyfikowani). Szczególna reguła: zawodnik który ukończył opłacony dystans i próbował dłuższego lecz nie zmieścił się → klasyfikowany na dystansie opłaconym. Zawodnik który nie ukończył opłaconego dystansu → status "nieklasyfikowany"** | ~~blocker~~ |
| H-011 | Brak zdarzenia finalizacji rejestracji ("Potwierdzono rejestrację") — luka czy strata konwersji? | niejasność | E-004, E-013 | open | wysoki |
| H-012 | Co się dzieje z uczestnikiem który nie opłaci w terminie? Brak mechanizmu anulowania/wygasania | brak wiedzy | E-014, E-015 | open | wysoki |
| H-013 | Uczestnik vs Zawodnik — ta sama osoba w różnych stanach, czy dwa różne byty w modelu systemu? | niejasność | E-004, E-059 | open | średni |
| H-014 | Relacje między kontekstami: format wymiany danych, właściciel, kierunek — nieopisane | brak wiedzy | BC-02→BC-03, BC-03→BC-04 | open | wysoki |
| H-015 | Kto może zwrócić pakiet — "zawodnik lub osoba uprawniona". Brak mechanizmu upoważnienia | niejasność | E-095 | open | niski |
| H-016 | Strona WWW do wyników real-time — ten sam system czy osobna aplikacja? | niejasność | E-080 | open | wysoki |
| H-017 | "W trakcie opłacania nie można zmienić dystansu" — jak system egzekwuje ten zakaz? | ryzyko | E-009, E-016 | open | średni |
| H-018 | Minimalna liczba uczestników X wymagana do generacji grup — wartość konfiguracyjna nieznana | brak wiedzy | E-033, E-034 | open | średni |
| H-019 | Scenariusz "nie stawił się na starcie" — brak przepływu | brak wiedzy | E-077 | open | średni |
| H-020 | Zawodnik który zakończył udział przedwcześnie — co z dyskietką RFID i wynikami? | niejasność | E-076, E-079 | open | średni |

---

## Wnioski

| ID    | Wniosek | Pewność | Źródło |
|-------|---------|---------|--------|
| W-001 | System ma co najmniej 10 odrębnych kontekstów domenowych od konfiguracji po zakończenie zawodów; złożoność wysoka (>7 BC) | wysoka | seed + weryfikacja K4/K5 |
| W-002 | Dwie integracje zewnętrzne (bank, RFID) są jedynymi punktami wyjścia/wejścia systemu; dyplomy i wyniki są generowane wewnętrznie | wysoka | potwierdzone przez orkiestratora |
| W-003 | Pivotal Events są wyraźne i pokrywają się z granicami kontekstów: PE-001 (blokada grup), PE-002 (twarda blokada), PE-003 (start = aktywacja pomiaru), PE-004 (koniec zawodów) | wysoka | derived z seeda + weryfikacja |
| W-004 | Proces opłat jest bardziej złożony niż wynika z ogólnego opisu — 5+ ścieżek (pełna, promocja 100%, niedopłata, zmiana dystansu z dopłatą, zmiana z zwrotem) | średnia | opłata_za_udział_1 + opłata_za_udział_2 (oba lossy) |
| W-005 | Brakuje co najmniej 8 zdarzeń domenowych nieobecnych w seedzie, odkrytych w weryfikacji od tyłu: potwierdzenie rejestracji, wykrycie niedopłaty, anulowanie rejestracji, odmowa zmiany grupy, ranking wyników, wygenerowanie dyplomu, publikacja wyników, upłynięcie czasu maratonu | średnia | K5 reverse walk (wniosek oparty na logice procesu, nie wprost z materiałów lossy) |
| W-006 | Obszar generacji grup startowych jest najsilniej obwarowany regułami temporalnymi (3 triggery czasowe, okno zmian, blokada). Prawdopodobnie core domain lub kluczowy supporting domain | średnia | derived z generacja grup startowych + grupy_startowe_ds (oba lossy) |
| W-007 | Przepływ zakończenia zawodów (generowanie rankingu → generowanie dyplomu → przyznanie nagrody) jest obecny tylko jako pojedyncze zdarzenia bez opisanego łańcucha przyczynowego | średnia | zakończenie_zawodów (lossy); potwierdzono tylko zdarzenie "Otrzymano dyplom" |
| W-008 | Zdyskwalifikowanie zawodnika (kwalifikacja) pojawia się w materiałach ale jest POZA ZAKRESEM systemu — decyzja sędziego, nie logika systemowa | wysoka | potwierdzone przez orkiestratora; kary-processed.md wykluczone |
| W-009 | Zmiana grupy jest jednorazowa (potwierdzony fakt domenowy #5) i ograniczona do tego samego dystansu; wymaga: wolnych miejsc, otwartego okna czasowego, braku wcześniejszej zmiany, opłaconego dystansu, przypisanego numeru startowego | wysoka | grupy_startowe_ds + zminia_grupystartowej (oba lossy, ale zbieżne) |
| W-010 | Checkpointy RFID: 10 dystansów (50–700 km), 1 pętla = 100 km, checkpointy co pętlę (50 km — jeden mniej). Wyniki real-time = biuro zawodów + strona WWW | wysoka | potwierdzone przez orkiestratora |

---

## Otwarte pytania

| ID    | Pytanie | Priorytet | Dotyczy | Przeniesione do fazy |
|-------|---------|-----------|---------|---------------------|
| Q-001 | Co się dzieje gdy system rozpocznie generowanie grup startowych a uczestnik ma jeszcze nieopłaconą niedopłatę? | ~~blocker~~ RESOLVED | BC-05 GeneracjaGrupStartowych / BC-03 OpłataZaUdział | — |
| Q-002 | Jaka jest logika zamknięcia pomiaru czasu? | ~~blocker~~ RESOLVED | BC-09 PomiarCzasu | — |
| Q-003 | Czy strona WWW wyświetlająca wyniki real-time jest częścią tego samego systemu, czy osobną aplikacją? | ~~wysoki~~ RESOLVED: ta sama baza, osobny widok — brak osobnej integracji | H-016 / granice systemu | — |
| Q-004 | Co się dzieje gdy uczestnik zarejestruje się i nie opłaci w żadnym terminie? Czy rejestracja wygasa automatycznie, czy wymaga ręcznej decyzji administratora? | wysoki | BC-02 Rejestracja / BC-03 Opłata | process-level |
| Q-005 | Jaki jest pełny przepływ generowania rankingu wyników i dyplomów? Kto jest inicjatorem, jaki trigger, jaki format dyplomu? | wysoki | BC-09→BC-10 | process-level |
| Q-006 | Uczestnik vs Zawodnik — czy to jeden byt w modelu systemu (różne stany), czy dwa odrębne agregaty? | wysoki | BC-02 Rejestracja | process-level |
| Q-007 | Relacje między kontekstami: w jaki sposób Rejestracja przekazuje dane do Opłaty? A Opłata do Przydział numeru? Jaki format wymiany, synchronicznie czy asynchronicznie? | wysoki | BC-02→BC-03→BC-04 | process-level |
| Q-008 | Czy dyskietka RFID jest częścią pakietu startowego czy wydawana osobno? Czy ma osobny numer / identyfikator? | średni | BC-07 WeryfikacjaStartu / I-002 | process-level |
| Q-009 | Jaka jest minimalna liczba uczestników X wymagana do uruchomienia generacji grup startowych? | średni | BC-05 GeneracjaGrupStartowych | process-level |
| Q-010 | Co dzieje się z zawodnikiem który nie stawił się na starcie? Czy jego slot jest automatycznie zwalniany? | średni | BC-08 StartZawodników | process-level |
| Q-011 | Co dzieje się z dyskietką RFID zawodnika który zakończył udział przedwcześnie? Czy system przestaje rejestrować jego pomiary? | średni | BC-09 PomiarCzasu | process-level |
| Q-012 | Kto i w jaki sposób może złożyć wniosek o zmianę dystansu w imieniu zawodnika — tylko zawodnik online, czy też administrator w biurze? | nice-to-have | BC-02 Rejestracja | process-level |
| Q-013 | Jakie jest kryterium "odpisania uczestnictwa" (E-029 — częściowo nieczytelne)? Czy to rezygnacja czy anulowanie przez organizatora? | nice-to-have | BC-03 OpłataZaUdział | — |

---

## Korekty i uzupełnienia (retrospektywne)

[2026-06-14]: H-003 wymaga korekty — odpowiedź użytkownika po Fazie 1.
Poprzednio: otwarty bloker — brak reguły dla konfliktu generacja grup vs niedopłata.
Aktualnie: uczestnik z nieopłaconą niedopłatą w momencie generowania grup → pozostaje na starym dystansie. Zmiana następuje dopiero po otrzymaniu potwierdzenia płatności. Generowanie nie jest blokowane — uczestnik trafia do grupy odpowiadającej opłaconemu dystansowi.
Powód: jawna odpowiedź użytkownika na bloker Q-001.

[2026-06-14]: H-010 wymaga korekty — odpowiedź użytkownika po Fazie 1.
Poprzednio: otwarty bloker — dwa triggery zamknięcia pomiaru, brak priorytetu.
Aktualnie: trigger dominujący = upłynięcie czasu zawodów (24h). Po czasie: zawodnicy na trasie → "jadą dla siebie" (wyniki nieuwzględnione w klasyfikacji). Reguła kwalifikacji: (a) zawodnik który ukończył opłacony dystans i próbował dłuższego lecz nie zmieścił się w czasie → klasyfikowany na dystansie opłaconym; (b) zawodnik który nie ukończył opłaconego dystansu w czasie → status "nieklasyfikowany", może kontynuować jazdę lub zakończyć.
Powód: jawna odpowiedź użytkownika na bloker Q-002.

[2026-06-14 — Faza 2]: Hot spoty H-001, H-004, H-008, H-016 rozwiązane przez analizę Process Level.
H-001 (brak jawnej granicy systemu): 10 BC zidentyfikowanych z wyraźnymi granicami; strona WWW = ten sam system, osobny widok — brak integracji zewnętrznej.
H-004 (real-time display): BC-009 Pomiar Czasu i Wyniki — ta sama baza, widok publiczny i biurowy jako osobne odczyty.
H-008 (brak typologii subdomen): SD-001..SD-010 sklasyfikowane (1 core, 8 supporting, 1 generic).
H-016 (strona WWW — osobna aplikacja?): potwierdzone przez użytkownika — ta sama baza, osobny widok.
Core domain: SD-005 / BC-005 Generacja Grup Startowych (spełnia T1-T4).
