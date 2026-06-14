---
source: phase-1-seed.md + weryfikacja audit
generated: 2026-06-14
---

# Raw Events — Surowy materiał

Wszystkie zdarzenia ze wszystkich obszarów, klasyfikowane typem.
Zdarzenia uzupełnione w Kroku 4/5 oznaczone [DODANO: powód].

---

## Obszar: Konfiguracja maratonu

| Zdarzenie | Typ | Źródło | Uwagi |
|---|---|---|---|
| Zarejestrowano maraton | domenowe | kontekst_rejestracja1, ogólny_widok | |
| Skonfigurowano parametry maratonu | domenowe | kontekst_rejestracja1 | [DODANO: K4 — brak zdarzenia konfiguracji dystansów, grup, regulaminu przed rejestracją uczestników] |
| Opublikowano regulamin maratonu | domenowe | kontekst_rejestracja1 | [DODANO: K5 — warunek wstępny: zawodnicy muszą zaakceptować regulamin] |

---

## Obszar: Rejestracja uczestnika

| Zdarzenie | Typ | Źródło | Uwagi |
|---|---|---|---|
| Zarejestrowano uczestnika | domenowe | kontekst_rejestracja1, ogólny_widok | |
| Wybrano dystans | domenowe | kontekst_rejestracja1, ogólny_widok | |
| Potwierdzono dane kontaktowe | domenowe | kontekst_rejestracja1, ogólny_widok | |
| Zweryfikowano zawodnika | domenowe | kontekst_rejestracja1, ogólny_widok | |
| Wyliczono opłatę | domenowe | kontekst_rejestracja1, ogólny_widok | |
| Złożono wniosek o zmianę dystansu | domenowe | kontekst_rejestracja1, ogólny_widok, opłata_za_udział_2 | |
| Zmieniono dystans | domenowe | kontekst_rejestracja1, ogólny_widok | |
| Wyliczono opłatę (zmiana dystansu) | domenowe | kontekst_rejestracja1 | |
| Poinformowano użytkownika o zmianie opłaty | domenowe | kontekst_rejestracja1 | |
| Potwierdzono rejestrację | domenowe | kontekst_rejestracja1 | [DODANO: K5 — brakujące zdarzenie finalizacji; hot spot w procesowanym pliku: "brak zdarzenia potwierdzono rejestrację"] |
| Anulowano rejestrację | domenowe | — | [DODANO: K5 — edge case: co jeśli uczestnik nie opłaci w terminie i rejestracja wygasa?] |

---

## Obszar: Opłata za udział

| Zdarzenie | Typ | Źródło | Uwagi |
|---|---|---|---|
| Rozpoczęto opłacenie udziału | domenowe | ogólny_widok, opłata_za_udział_1 | |
| Zarejestrowano przelew | zewnętrzne | opłata_za_udział_1, opłata_za_udział_2, ogólny_widok | zdarzenie z systemu bankowego |
| Zaaplikowano opłatę | domenowe | ogólny_widok, opłata_za_udział_1 | |
| Opłacono uczestnictwo | domenowe | ogólny_widok, opłata_za_udział_1, opłata_za_udział_2 | |
| Zaaplikowano promocje (-100%) | domenowe | ogólny_widok, opłata_za_udział_1 | |
| Rozpoczęto opłacenie niedopłaty | domenowe | ogólny_widok, opłata_za_udział_1, opłata_za_udział_2 | |
| Opłacono niedopłatę | domenowe | ogólny_widok, opłata_za_udział_1, opłata_za_udział_2 | |
| Opłacono zmianę dystansu | domenowe | ogólny_widok, opłata_za_udział_1, opłata_za_udział_2 | |
| Wyliczono kwotę zwrotu | domenowe | ogólny_widok, opłata_za_udział_2 | |
| Zwrócono nadpłatę | domenowe | ogólny_widok, opłata_za_udział_2 | |
| Wyliczono opłatę za nowy dystans | domenowe | ogólny_widok | |
| Odpisano uczestnictwo | domenowe | ogólny_widok (częściowo nieczytelne) | |
| Opłacono dystans | domenowe | zminia_grupystartowej, generacja_grup_poterminie | |
| Wykryto niedopłatę | domenowe | opłata_za_udział_1 | [DODANO: K5 — warunek wstępny: skąd system wie że jest niedopłata? Brakujące zdarzenie między "Zarejestrowano przelew" a "Rozpoczęto opłacenie niedopłaty"] |
| Przekroczono termin płatności | czasowe | — | [DODANO: K5 — edge case: co jeśli uczestnik nie zapłaci w ogóle?] |

---

## Obszar: Przydział numeru startowego

| Zdarzenie | Typ | Źródło | Uwagi |
|---|---|---|---|
| Przypisano numer startowy | domenowe | ogólny_widok, zminia_grupystartowej, generacja_grup_poterminie, start_indywidualny | |

---

## Obszar: Generacja grup startowych

| Zdarzenie | Typ | Źródło | Uwagi |
|---|---|---|---|
| Przypisano zawodnika do grupy | domenowe | generacja grup startowych | |
| Złożono wniosek o zmianę grupy | domenowe | generacja grup startowych | |
| Nie złożono wcześniej wniosku | domenowe | generacja grup startowych | |
| Nie przypisano do ostatecznej grupy | domenowe | generacja grup startowych, zminia_grupystartowej | |
| Przyjęto wniosek do zmiany grupy | domenowe | generacja grup startowych | |
| Nie przyjęto wniosku o zmianę grupy | domenowe | generacja grup startowych | |
| Grupa została zmieniona | domenowe | generacja grup startowych | |
| Grupa nie została zmieniona | domenowe | generacja grup startowych | |
| Wygenerowano ostateczne grupy | domenowe | generacja grup startowych | PIVOTAL EVENT KANDYDAT |
| Złożono wniosek o zmianę dystansu (przed generowaniem) | domenowe | generacja grup startowych | |
| Złożono wniosek o zmianę dystansu (po terminie) | domenowe | generacja grup startowych | |
| Przypisano zawodnika do grupy (automatycznie) | domenowe | generacja grup startowych | |
| Minął czas na ewentualne zmiany grupy — przypisanie jest na stałe | czasowe | generacja grup startowych | PIVOTAL EVENT KANDYDAT |
| Wygenerowano grupy startowe | domenowe | genreacja_numeru_indywidualna, utowrzenie_grup_ds | |
| Nie udało się wygenerować grup startowych | domenowe | genreacja_numeru_indywidualna, utowrzenie_grup_ds | |
| Grupy startowe zostały wygenerowane | domenowe | generacja_grup_poterminie | |
| Dołączono do grupy | domenowe | generacja_grup_poterminie | |
| Nie przekroczono terminu dołączenia do dystansu | domenowe | generacja_grup_poterminie | |
| Upłynął czas na zmianę grupy | czasowe | generacja_grup_poterminie | |
| Upłynął termin zmiany grupy dystansowo | czasowe | generacja_grup_poterminie | |
| Nadobiegnięto do grupy | domenowe | generacja_grup_poterminie (częściowo nieczytelne) | |
| Przekazano opłatę za zmianę grupy | domenowe | generacja_grup_poterminie (częściowo nieczytelne) | |
| Zmieniono grupę | domenowe | grupy_startowe_ds, zminia_grupystartowej | |
| Nie udało się zmienić grupy | domenowe | grupy_startowe_ds, zminia_grupystartowej | |
| Odmówiono zmiany grupy z powodu braku opłaty | domenowe | — | [DODANO: K5 — wynika z reguły: "brak zapłaty = brak zmiany"; brakujące wyraźne zdarzenie] |

---

## Obszar: Weryfikacja i kwalifikacja przed startem

| Zdarzenie | Typ | Źródło | Uwagi |
|---|---|---|---|
| Zweryfikowano tożsamość zawodnika | domenowe | przypisanie_zawodnika_do_listy | |
| Zawodnik nie przeszedł weryfikacji | domenowe | przypisanie_zawodnika_do_listy | |
| Podpisano oświadczenie zawodnika | domenowe | przypisanie_zawodnika_do_listy | |
| Zawodnik nie podpisał oświadczenia | domenowe | przypisanie_zawodnika_do_listy | |
| Wydano pakiet startowy | domenowe | przypisanie_zawodnika_do_listy | zawiera dyskietkę RFID |
| Zdyskwalifikowano zawodnika (kwalifikacja) | domenowe | przypisanie_zawodnika_do_listy, wystartowano_zawodników | POZA ZAKRESEM — decyzja sędziego |
| Zweryfikowano zawodnika przed startem | domenowe | przypisanie_zawodnika_do_listy | |
| Zawodnik nie przeszedł weryfikacji (przed startem) | domenowe | przypisanie_zawodnika_do_listy | |
| Wydano dyskietkę RFID zawodnikowi | domenowe | przypisanie_zawodnika_do_listy | [DODANO: K5 — niejawne powiązanie: pakiet startowy zawiera chip RFID; czy jest osobnym zdarzeniem?] |

---

## Obszar: Start zawodników

| Zdarzenie | Typ | Źródło | Uwagi |
|---|---|---|---|
| Zgłoszono indywidualny start | domenowe | start_indywidualny | |
| Wyciągnięto zawodnika z grupy | domenowe | start_indywidualny | |
| Zgubiono dyskietkę z numerem startowym | domenowe | start_indywidualny | |
| Przejęto kaucję | domenowe | start_indywidualny | |
| Odpięto numer startowy | domenowe | start_indywidualny | |
| Przypisano numer startowy (nowy) | domenowe | start_indywidualny | |
| Wydano nową dyskietkę | domenowe | start_indywidualny | |
| Wystartowano zawodników | domenowe | wystartowano_zawodników | PIVOTAL EVENT KANDYDAT |
| Wystartowano zawodników na danym dystansie | domenowe | wystartowano_zawodników | |
| Zamknięto zapisy na dany dystans | domenowe | wystartowano_zawodników | |
| Wystartowano grupę | domenowe | wystartowano_zawodników | |
| Zarejestrowano czas startu | domenowe | wystartowano_zawodników | |
| Wystartowano zawodnika | domenowe | wystartowano_zawodników | |
| Zakończono udział przedwcześnie | domenowe | wystartowano_zawodników | |
| Nie stawił się na starcie | domenowe | — | [DODANO: K5 — edge case: zawodnik zarejestrowany ale nie pojawił się na starcie] |

---

## Obszar: Pomiar czasu i wyniki

| Zdarzenie | Typ | Źródło | Uwagi |
|---|---|---|---|
| Przyłożenie dyskietki do czytnika | zewnętrzne | pomiar_czasu | RFID — fizyczny trigger |
| Zarejestrowano pomiar czasu | domenowe | pomiar_czasu | zdarzenie checkpoint |
| Zawodnik zakończył przejazd | domenowe | pomiar_czasu | |
| Podliczono czasy przejazdu | domenowe | pomiar_czasu | |
| Zakończono przejazd zawodników na danym dystansie | domenowe | pomiar_czasu | |
| Zakończono przejazdy wszystkich dystansów | domenowe | pomiar_czasu, zakończenie_zawodów | PIVOTAL EVENT KANDYDAT |
| Zgłoszono prośbę o przedłużenie dystansu | domenowe | pomiar_czasu | |
| Przedłużono przejazd zawodnika | domenowe | pomiar_czasu | |
| Odnotowano dodatkowy pomiar czasu | domenowe | pomiar_czasu | |
| Zawodnik zakończył przejazd (wydłużony) | domenowe | pomiar_czasu | |
| Wyświetlono wyniki w czasie rzeczywistym | domenowe | potwierdzone-orkiestrator | [DODANO: K4 — zdarzenie real-time display; checkpointy są wyświetlane w biurze + WWW] |
| Wygenerowano rankig wyników | domenowe | — | [DODANO: K5 — brak przepływu generowania rankingu po "Podliczono czasy przejazdu"] |
| Upłynął czas maratonu | czasowe | pomiar_czasu | [DODANO: K5 — trigger zamknięcia: "system wie że minął czas" (hot spot z pomiar_czasu)] |

---

## Obszar: Zakończenie zawodów

| Zdarzenie | Typ | Źródło | Uwagi |
|---|---|---|---|
| Przyznano nagrodę | domenowe | zakończenie_zawodów | |
| Zwrócono pakiet startowy | domenowe | zakończenie_zawodów | |
| Kaucja została zwrócona | domenowe | zakończenie_zawodów | |
| Otrzymano dyplom | domenowe | zakończenie_zawodów | |
| Upłynął czas zwrotu pakietu | czasowe | zakończenie_zawodów | |
| Nie zwrócono pakietu startowego | domenowe | zakończenie_zawodów | |
| Kaucja zatrzymana | domenowe | zakończenie_zawodów (częściowo nieczytelne) | |
| Wygenerowano dyplom | domenowe | — | [DODANO: K5 — brak przepływu: "Otrzymano dyplom" to wynik, ale brak zdarzenia wygenerowania] |
| Opublikowano wyniki końcowe | domenowe | — | [DODANO: K5 — brak jawnego zdarzenia publikacji wyników] |
