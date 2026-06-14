---
seed-for: process-level
session: System Zarządzania 24h Maratonem Rowerowym
coverage: 65%
sources:
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

# Seed: Process Level

## Kontekst domeny

System pozwala na rejestrację uczestnika na jeden z kilku dystansów, sprawdza czy uczestnik opłacił udział, przydziela numer startowy, generuje grupy startowe, rejestruje czas przejazdu za pomocą systemu kart opartych o RFID, wyświetla w czasie rzeczywistym między-czasy przejazdu, generuje wyniki i dyplomy uczestnictwa.

---

## Przepływy procesów (gramatyka aktor → komenda → zdarzenie)

### Przepływ 1: Konfiguracja maratonu

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | Główny organizator | Zarejestruj maraton | Zarejestrowano maraton |

**Warunki:** parametry regulaminu (terminy, dystanse, limity uczestników, okna czasowe zmian grupy)

---

### Przepływ 2: Rejestracja uczestnika

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | Uczestnik | Zarejestruj uczestnika | Zarejestrowano uczestnika |
| 2 | Uczestnik | Wybierz dystans | Wybrano dystans |
| 3 | Uczestnik | Potwierdź dane kontaktowe | Potwierdzono dane kontaktowe |
| 4 | System | Zweryfikuj zawodnika | Zweryfikowano zawodnika |
| 5 | System | Wylicz opłatę | Wyliczono opłatę |

**Ścieżka alternatywna — zmiana dystansu:**

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| A1 | Uczestnik | Złóż wniosek o zmianę dystansu | Złożono wniosek o zmianę dystansu |
| A2 | System | Zmień dystans | Zmieniono dystans |
| A3 | System | Wylicz opłatę za nowy dystans | Wyliczono opłatę (zmiana dystansu) |
| A4 | System | Poinformuj o zmianie opłaty | Poinformowano użytkownika o zmianie opłaty |

---

### Przepływ 3: Opłata za udział (pełna)

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | Uczestnik / Administrator | Rozpocznij opłacenie | Rozpoczęto opłacenie udziału |
| 2 | Interfejs banku (zewnętrzny) | [przelew bankowy] | Zarejestrowano przelew [zewnętrzne] |
| 3 | System | Zaaplikuj opłatę | Zaaplikowano opłatę |
| 4 | System | — | Opłacono uczestnictwo |

**Ścieżka alternatywna — promocja:**

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 3a | System | Zaaplikuj promocję | Zaaplikowano promocje (-100%) |
| 4a | System | — | Opłacono uczestnictwo |

**Reguła:** W trakcie opłacania nie można zmienić dystansu.

---

### Przepływ 4: Opłata niedopłaty

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | Administrator | Rozpocznij opłacenie niedopłaty | Rozpoczęto opłacenie niedopłaty |
| 2 | Interfejs banku (zewnętrzny) | [przelew bankowy] | Zarejestrowano przelew (niedopłata) [zewnętrzne] |
| 3 | System | — | Opłacono niedopłatę |

**Ścieżka z nadpłatą:**

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | System | Wylicz kwotę zwrotu | Wyliczono kwotę zwrotu |
| 2 | System / Administrator | Zwróć nadpłatę | Zwrócono nadpłatę |

**Ścieżka zmiany dystansu po opłaceniu:**

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | Uczestnik | Złóż wniosek o zmianę dystansu | Złożono wniosek o zmianę dystansu |
| 2 | System | Wylicz opłatę za nowy dystans | Wyliczono opłatę za nowy dystans |
| 3 | System | — | Opłacono zmianę dystansu |

---

### Przepływ 5: Generacja grup startowych

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | Administrator | Generuj grupy startowe | [walidacja warunków] |
| 2 (sukces) | System | — | Wygenerowano grupy startowe |
| 2 (porażka) | System | — | Nie udało się wygenerować grup startowych |

**Warunki konieczne (polityki):**
- Grupy startowe nie zostały jeszcze wygenerowane
- Jest przynajmniej X uczestników z przypisanym numerem startowym
- Nie przekroczono ostatecznego terminu generowania grup zapisanego w regulaminie
- Generowanie grup odbywa się w określonym w regulaminie terminie

**Konsekwencja:**
- Po wygenerowaniu: Wygenerowano ostateczne grupy → [trigger czasowy] Minął czas na ewentualne zmiany grupy → przypisanie jest na stałe

---

### Przepływ 6: Zmiana grupy startowej (przez zawodnika)

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | Zawodnik | Zmień grupę | [walidacja warunków] |
| 2 (sukces) | System | — | Zmieniono grupę |
| 2 (porażka) | System | — | Nie udało się zmienić grupy |
| 2 (porażka alt.) | System | — | Nie przypisano do ostatecznej grupy |

**Warunki konieczne (polityki):**
- Nie przekroczono terminu zmiany grup
- Grupa docelowa ma wolne miejsca
- Zawodnik nie zmieniał wcześniej grupy (jednorazowość)
- Grupa docelowa jest z tego samego dystansu
- Grupy startowe zostały wygenerowane

**Reguła domenowa:** Zmiana grupy jest możliwa tylko w pewnym oknie czasowym. Brak zapłaty różnicy przy zmianie grupy = brak zmiany.

---

### Przepływ 7: Dołączenie do grupy po terminie

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | System | — | Opłacono dystans [warunek] |
| 2 | System | — | Przyznano numer startowy [warunek] |
| 3 | System | Dołącz do grupy | Dołączono do grupy |

**Warunek blokujący:**
- Upłynął czas na zmianę grupy [czasowe] → brak możliwości dołączenia

---

### Przepływ 8: Weryfikacja i kwalifikacja zawodnika przed startem

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | Obsługa punktu kontrolnego | Zweryfikuj tożsamość | Zweryfikowano tożsamość zawodnika |
| 1b (porażka) | — | — | Zawodnik nie przeszedł weryfikacji |
| 2 | Zawodnik | Podpisz oświadczenie | Podpisano oświadczenie zawodnika |
| 2b (porażka) | — | — | Zawodnik nie podpisał oświadczenia |
| 3 (sukces) | Obsługa punktu kontrolnego | Wydaj pakiet startowy | Wydano pakiet startowy |
| 3 (porażka) | Obsługa punktu kontrolnego | Zdyskwalifikuj zawodnika | Zdyskwalifikowano zawodnika |

**Weryfikacja na starcie (odrębna od biurowej):**

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | Obsługa / Sędzia | Zweryfikuj zawodnika przed startem | Zweryfikowano zawodnika przed startem |
| 1b (porażka) | — | — | Zawodnik nie przeszedł weryfikacji (przed startem) |
| 2 (porażka) | Sędzia trasy | Zdyskwalifikuj | Zdyskwalifikowano zawodnika (przed startem) |

**Reguły:** Weryfikacja na podstawie listy startowej. Jeśli nie zweryfikowany w biurze → nie dopuszczony do startu.

---

### Przepływ 9: Start indywidualny (spóźnienie)

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | Zawodnik | Zgłoś indywidualny start | Zgłoszono indywidualny start |
| 2 | Administrator | Wyciągnij zawodnika z grupy | Wyciągnięto zawodnika z grupy |

**Trigger:** spóźnienie lub inna przyczyna startu poza grupą.

---

### Przepływ 10: Zgubienie dyskietki RFID

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | Zawodnik | Zgłoś zgubienie dyskietki | Zgubiono dyskietkę z numerem startowym |
| 2 | Obsługa punktu kontrolnego | Przejmij kaucję | Przejęto kaucję |
| 3 | Administrator | Odepnij numer startowy | Odpięto numer startowy |
| 4 | Administrator | Przypisz nowy numer | Przypisano numer startowy (nowy) |
| 5 | Obsługa punktu kontrolnego | Wydaj nową dyskietkę | Wydano nową dyskietkę |

---

### Przepływ 11: Start zawodników i zamknięcie zapisów

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | Administrator / Sędzia | Wystartuj zawodników | Wystartowano zawodników |
| 2 | System | Zamknij zapisy na dystans | Zamknięto zapisy na dany dystans |
| 3 | Administrator / Sędzia | Wystartuj grupę | Wystartowano grupę |
| 4 | System | Zarejestruj czas startu | Zarejestrowano czas startu |
| 5 | Administrator / Sędzia | Wystartuj zawodnika | Wystartowano zawodnika |
| 5b (porażka) | Sędzia trasy | Zdyskwalifikuj | Zdyskwalifikowano zawodnika |
| 5c (porażka) | — | — | Zakończono udział przedwcześnie |

**Pivotal event:** Wystartowano zawodników → zamknięcie zapisów (bez powrotu).

---

### Przepływ 12: Pomiar czasu przez RFID

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | Zawodnik | [przyłożenie dyskietki do czytnika] | Przyłożenie dyskietki do czytnika [zewnętrzne — RFID] |
| 2 | System RFID | — | Zarejestrowano pomiar czasu |
| 3 | System | — | Zawodnik zakończył przejazd (po ukończeniu dystansu) |
| 4 | System | Podlicz czasy | Podliczono czasy przejazdu |
| 5 | System | — | Zakończono przejazd zawodników na danym dystansie |
| 6 | System | — | Zakończono przejazdy wszystkich dystansów |

**Ścieżka wydłużonego dystansu:**

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | Zawodnik | Zgłoś prośbę o przedłużenie | Zgłoszono prośbę o przedłużenie dystansu |
| 2 | Administrator / Sędzia | Przedłuż przejazd | Przedłużono przejazd zawodnika |
| 3 | System RFID | [odczyt] | Odnotowano dodatkowy pomiar czasu |
| 4 | System | — | Zawodnik zakończył przejazd (wydłużony) |

**Fakty domenowe:**
- Dystanse: 50, 100, 150, 200, 300, 400, 500, 600, 650, 700 km
- 1 pętla = 100 km; checkpointy na każdej pętli (co 50 km — o jeden mniej)
- Wyniki real-time: biuro zawodów + strona WWW

---

### Przepływ 13: Zakończenie zawodów

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | System | — | Zakończono przejazdy wszystkich dystansów [pivotal event] |
| 2 | Główny organizator | Przyznaj nagrodę | Przyznano nagrodę |
| 3 | Zawodnik / osoba uprawniona | Zwróć pakiet startowy | Zwrócono pakiet startowy |
| 4 | System | — | Kaucja została zwrócona |
| 5 | Obsługa | Wydaj dyplom | Otrzymano dyplom |

**Ścieżka braku zwrotu pakietu:**

| Krok | Aktor | Komenda | Zdarzenie domenowe |
|---|---|---|---|
| 1 | System (zegar) | — | Upłynął czas zwrotu pakietu [czasowe] |
| 2 | System | — | Nie zwrócono pakietu startowego |
| 3 | System | — | Kaucja zatrzymana |

**Reguła:** Zawodnik lub osoba uprawniona przez zawodnika może zwrócić pakiet.

---

## Reguły biznesowe i polityki

| Reguła | Obszar | Źródło |
|---|---|---|
| W trakcie opłacania nie można zmienić dystansu | Opłata | opłata_za_udział_1 |
| Brak zapłaty różnicy przy zmianie grupy = brak zmiany | Grupy startowe | potwierdzone przez orkiestratora |
| Zmiana grupy jest możliwa tylko w pewnym oknie czasowym | Grupy startowe | zminia_grupystartowej, grupy_startowe_ds |
| Zmiana grupy jednorazowa — zawodnik nie może zmienić grupy więcej niż raz | Grupy startowe | grupy_startowe_ds, zminia_grupystartowej |
| Zmiana grupy możliwa tylko w obrębie tego samego dystansu | Grupy startowe | grupy_startowe_ds |
| Generowanie grup wymaga min. X uczestników z przypisanym numerem | Generacja grup | genreacja_numeru_indywidualna, utowrzenie_grup_ds |
| Generowanie grup odbywa się w terminie określonym w regulaminie | Generacja grup | genreacja_numeru_indywidualna, utowrzenie_grup_ds |
| Po wygenerowaniu grup i upływie okna czasowego — przypisanie jest stałe | Generacja grup | generacja grup startowych |
| Zawodnik lub osoba uprawniona może zwrócić pakiet startowy | Zakończenie | zakończenie_zawodów |
| Weryfikacja przed startem na podstawie listy startowej | Start | przypisanie_zawodnika_do_listy |
| Kaucja za dyskietkę RFID pobierana przy zgłoszeniu zgubienia | Start | start_indywidualny |
| Kary/dyskwalifikacja przez sędziego — poza zakresem systemu | Kary | potwierdzone przez orkiestratora |

---

## Bounded Contexts i granice kontekstów

| Bounded Context | Nazwa jawna | Obszar odpowiedzialności | Powiązania (domniemane) |
|---|---|---|---|
| Konfiguracja maratonu | Frame 54 | Parametry regulaminu, terminy, dystanse | → Rejestracja (reguły), → Generacja grup (terminy) |
| Rejestracja | Frame 52 | Rejestracja uczestnika, wybór dystansu, zmiana dystansu | → Opłata za udział (wymagana po rejestracji), → Generacja grup (uczestnik z numerem) |
| Opłata za udział | Frame 51 | Płatności, niedopłaty, zwroty, promocje | → Bank (przelew), ← Rejestracja (trigger), → Przydział numeru (warunek: opłacono) |
| Przydział numeru startowego | tak (obszar) | Generowanie i przypisanie numeru startowego | ← Opłata (warunek: opłacono), → Generacja grup (warunek: numer przypisany) |
| GeneracjaGrupStartowych | TAK (jawnie nazwany) | Automatyczna generacja grup, warunki generowania | ← Przydział numeru (warunek wejściowy), → GrupyStartowe (dane grup) |
| GrupyStartowe | TAK (jawnie nazwany) | Zmiana grupy przez zawodnika, obsługa wniosków | ← GeneracjaGrupStartowych (grupy muszą istnieć), → Start (lista grupy) |
| Weryfikacja i kwalifikacja | tak (obszar) | Check-in zawodnika przed startem, wydanie pakietu | ← Rejestracja/Opłata (opłacony i zarejestrowany) |
| Start indywidualny / Zamknięcie zapisów | tak (obszar) | Start zawodników, zamknięcie listy, start poza grupą | ← GrupyStartowe, → Pomiar czasu |
| Pomiar czasu | tak (obszar) | Rejestracja RFID, real-time, wyniki | ← RFID (zewnętrzny), → Zakończenie zawodów |
| Zakończenie zawodów | Frame 46 | Nagrody, dyplomy, zwrot pakietów, kaucja | ← Pomiar czasu (trigger: zakończono przejazdy) |

**Relacje między kontekstami (domniemane ze strzałek łukowych):**
- Strzałki łukowe widoczne w ogólnym widoku kontekstu rejestracja sugerują przepływ: Rejestracja → Opłata → Przydział numeru → Generacja grup
- Brak explicite opisanych relacji (właściciel danych, format wymiany, upstream/downstream)
- Wymaga osobnego wywiadu lub sesji PL dla pełnego opisu

---

## Pivotal Events (zdarzenia bez powrotu / zmiany właściciela)

| Zdarzenie | Znaczenie | Konsekwencje |
|---|---|---|
| Wygenerowano ostateczne grupy | Blokada zmian grupy po upływie okna czasowego | Po terminie: zmiana grupy niemożliwa; przypisanie stałe |
| Wystartowano zawodników | Zamknięcie zapisów na dany dystans | Brak możliwości dołączenia do listy startowej |
| Zakończono przejazdy wszystkich dystansów | Trigger zakończenia zawodów | Wejście w tryb zakończenia: nagrody, dyplomy, zwroty |

---

## Hot Spoty i wyjątki

| Hot Spot / Wyjątek | Obszar | Źródło |
|---|---|---|
| Pytanie: co zrobić gdy rozpocznie się generowanie list a czekamy na opłacenie niedopłaty? | Opłata / Generacja grup | opłata_za_udział_1, opłata_za_udział_2 |
| Start indywidualny — np. spóźnienie (trigger niejawny) | Start indywidualny | start_indywidualny |
| Logika zamknięcia pomiaru: "system wie że wszyscy dojechali albo że minął czas" | Pomiar czasu | pomiar_czasu |
| Limity na dany dystans — reguły konfiguracyjne nieznane | Pomiar czasu | pomiar_czasu |
| Proxy przy zwrocie pakietu: "zawodnik lub osoba uprawniona przez zawodnika" | Zakończenie | zakończenie_zawodów |
| Kaucja za dyskietkę RFID — mechanizm finansowy powiązany z kartą | Start / RFID | start_indywidualny |
| Dwie odrębne weryfikacje zawodnika: biurowa (kwalifikacja) i na starcie — różnica procesowa | Weryfikacja | przypisanie_zawodnika_do_listy |
| Reguła: minimalna liczba X uczestników — konkretna wartość konfiguracyjna nieznana | Generacja grup | genreacja_numeru_indywidualna |
| Wyświetlanie real-time nieobecne w materiałach (prawdopodobna strata konwersji) | Pomiar czasu | pomiar_czasu |
| Brak opisanego przepływu generowania wyników/rankingów w gramatyce aktor→komenda→zdarzenie | Wyniki | index.md |
| Brak opisanego przepływu generowania dyplomów (tylko zdarzenie "Otrzymano dyplom") | Dyplomy | index.md |

---

## Obszary słabego pokrycia (coverage 65%)

| Obszar | Status | Co brakuje |
|---|---|---|
| Typologia subdomen | słabe | Brak oznaczenia core domain vs supporting vs generic; brak opisu przewagi konkurencyjnej |
| Relacje między kontekstami | słabe | Tylko domniemane ze strzałek; brak właściciela danych, formatu wymiany, kierunku upstream/downstream |
| Generowanie wyników i rankingów | brak | "Podliczono czasy przejazdu" obecne, ale brak dalszego przepływu |
| Generowanie dyplomów | brak przepływu | Tylko zdarzenie "Otrzymano dyplom"; brak opisu kto, kiedy, jaki trigger, format |
| Real-time display między-czasów | brak w materiałach | Potwierdzone domenowo, ale brak procesu w plikach (strata konwersji) |
