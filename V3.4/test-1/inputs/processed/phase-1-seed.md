---
seed-for: big-picture
session: System Zarządzania 24h Maratonem Rowerowym
coverage: 83%
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

# Seed: Big Picture

## Kontekst domeny

System pozwala na rejestrację uczestnika na jeden z kilku dystansów, sprawdza czy uczestnik opłacił udział, przydziela numer startowy, generuje grupy startowe, rejestruje czas przejazdu za pomocą systemu kart opartych o RFID, wyświetla w czasie rzeczywistym między-czasy przejazdu, generuje wyniki i dyplomy uczestnictwa.

---

## Aktorzy

| Aktor | Typ | Źródło |
|---|---|---|
| Zawodnik | wewnętrzny / główny | ogólny widok, grupy startowe, pomiar czasu, start_indywidualny, zminia_grupystartowej, przypisanie_zawodnika, zakończenie_zawodów |
| Uczestnik | wewnętrzny (przed startem) | kontekst_rejestracja1, ogólny_widok |
| Administrator | wewnętrzny | generacja grup startowych, generacja_grup_poterminie, start_indywidualny, kontekst_rejestracja1 |
| Główny organizator zawodów | wewnętrzny | ogólny_widok, opłata_za_udział_1, opłata_za_udział_2, zakończenie_zawodów |
| Obsługa punktu kontrolnego | wewnętrzny | ogólny_widok, pomiar_czasu, przypisanie_zawodnika, start_indywidualny, zakończenie_zawodów |
| Sędzia trasy | wewnętrzny | wystartowano_zawodników |
| Interfejs banku | zewnętrzny | ogólny_widok, opłata_za_udział_1, opłata_za_udział_2 |
| System RFID (czytnik dyskietek) | zewnętrzny | pomiar_czasu, start_indywidualny |
| System (automatyczna generacja grup) | systemowy | generacja grup startowych, genreacja_numeru_indywidualna, utowrzenie_grup_ds |

---

## Zdarzenia domenowe

### Obszar: Konfiguracja maratonu

| Zdarzenie | Typ | Źródło |
|---|---|---|
| Zarejestrowano maraton | domenowe | kontekst_rejestracja1, ogólny_widok |

### Obszar: Rejestracja uczestnika

| Zdarzenie | Typ | Źródło |
|---|---|---|
| Zarejestrowano uczestnika | domenowe | kontekst_rejestracja1, ogólny_widok |
| Wybrano dystans | domenowe | kontekst_rejestracja1, ogólny_widok |
| Potwierdzono dane kontaktowe | domenowe | kontekst_rejestracja1, ogólny_widok |
| Zweryfikowano zawodnika | domenowe | kontekst_rejestracja1, ogólny_widok |
| Wyliczono opłatę | domenowe | kontekst_rejestracja1, ogólny_widok |
| Złożono wniosek o zmianę dystansu | domenowe | kontekst_rejestracja1, ogólny_widok, opłata_za_udział_2 |
| Zmieniono dystans | domenowe | kontekst_rejestracja1, ogólny_widok |
| Wyliczono opłatę (zmiana dystansu) | domenowe | kontekst_rejestracja1 |
| Poinformowano użytkownika o zmianie opłaty | domenowe | kontekst_rejestracja1 |

### Obszar: Opłata za udział

| Zdarzenie | Typ | Źródło |
|---|---|---|
| Rozpoczęto opłacenie udziału | domenowe | ogólny_widok, opłata_za_udział_1 |
| Zarejestrowano przelew | zewnętrzne | opłata_za_udział_1, opłata_za_udział_2, ogólny_widok — zdarzenie z systemu bankowego |
| Zaaplikowano opłatę | domenowe | ogólny_widok, opłata_za_udział_1 |
| Opłacono uczestnictwo | domenowe | ogólny_widok, opłata_za_udział_1, opłata_za_udział_2 |
| Zaaplikowano promocje (-100%) | domenowe | ogólny_widok, opłata_za_udział_1 |
| Rozpoczęto opłacenie niedopłaty | domenowe | ogólny_widok, opłata_za_udział_1, opłata_za_udział_2 |
| Opłacono niedopłatę | domenowe | ogólny_widok, opłata_za_udział_1, opłata_za_udział_2 |
| Opłacono zmianę dystansu | domenowe | ogólny_widok, opłata_za_udział_1, opłata_za_udział_2 |
| Wyliczono kwotę zwrotu | domenowe | ogólny_widok, opłata_za_udział_2 |
| Zwrócono nadpłatę | domenowe | ogólny_widok, opłata_za_udział_2 |
| Wyliczono opłatę za nowy dystans | domenowe | ogólny_widok |
| Odpisano uczestnictwo | domenowe | ogólny_widok (częściowo nieczytelne) |
| Opłacono dystans | domenowe | zminia_grupystartowej, generacja_grup_poterminie |

### Obszar: Przydział numeru startowego

| Zdarzenie | Typ | Źródło |
|---|---|---|
| Przypisano numer startowy | domenowe | ogólny_widok, zminia_grupystartowej, generacja_grup_poterminie, start_indywidualny |

### Obszar: Generacja grup startowych

| Zdarzenie | Typ | Źródło |
|---|---|---|
| Przypisano zawodnika do grupy | domenowe | generacja grup startowych |
| Złożono wniosek o zmianę grupy | domenowe | generacja grup startowych |
| Nie złożono wcześniej wniosku | domenowe | generacja grup startowych |
| Nie przypisano do ostatecznej grupy | domenowe | generacja grup startowych, zminia_grupystartowej |
| Przyjęto wniosek do zmiany grupy | domenowe | generacja grup startowych |
| Nie przyjęto wniosku o zmianę grupy | domenowe | generacja grup startowych |
| Grupa została zmieniona | domenowe | generacja grup startowych |
| Grupa nie została zmieniona | domenowe | generacja grup startowych |
| Wygenerowano ostateczne grupy | domenowe | generacja grup startowych |
| Złożono wniosek o zmianę dystansu (przed generowaniem) | domenowe | generacja grup startowych |
| Złożono wniosek o zmianę dystansu (po terminie) | domenowe | generacja grup startowych |
| Przypisano zawodnika do grupy (automatycznie) | domenowe | generacja grup startowych |
| Minął czas na ewentualne zmiany grupy — przypisanie jest na stałe | czasowe | generacja grup startowych |
| Wygenerowano grupy startowe | domenowe | genreacja_numeru_indywidualna, utowrzenie_grup_ds |
| Nie udało się wygenerować grup startowych | domenowe | genreacja_numeru_indywidualna, utowrzenie_grup_ds |
| Grupy startowe zostały wygenerowane | domenowe | generacja_grup_poterminie |
| Dołączono do grupy | domenowe | generacja_grup_poterminie |
| Nie przekroczono terminu dołączenia do dystansu | domenowe | generacja_grup_poterminie |
| Upłynął czas na zmianę grupy | czasowe | generacja_grup_poterminie |
| Upłynął termin zmiany grupy dystansowo | czasowe | generacja_grup_poterminie |
| Nadobiegnięto do grupy (częściowo nieczytelne) | domenowe | generacja_grup_poterminie |
| Przekazano opłatę za zmianę grupy (częściowo nieczytelne) | domenowe | generacja_grup_poterminie |
| Zmieniono grupę | domenowe | grupy_startowe_ds, zminia_grupystartowej |
| Nie udało się zmienić grupy | domenowe | grupy_startowe_ds, zminia_grupystartowej |

### Obszar: Weryfikacja i kwalifikacja przed startem

| Zdarzenie | Typ | Źródło |
|---|---|---|
| Zweryfikowano tożsamość zawodnika | domenowe | przypisanie_zawodnika_do_listy |
| Zawodnik nie przeszedł weryfikacji | domenowe | przypisanie_zawodnika_do_listy |
| Podpisano oświadczenie zawodnika | domenowe | przypisanie_zawodnika_do_listy |
| Zawodnik nie podpisał oświadczenia | domenowe | przypisanie_zawodnika_do_listy |
| Wydano pakiet startowy | domenowe | przypisanie_zawodnika_do_listy |
| Zdyskwalifikowano zawodnika (kwalifikacja) | domenowe | przypisanie_zawodnika_do_listy, wystartowano_zawodników |
| Zweryfikowano zawodnika przed startem | domenowe | przypisanie_zawodnika_do_listy |
| Zawodnik nie przeszedł weryfikacji (przed startem) | domenowe | przypisanie_zawodnika_do_listy |

### Obszar: Start zawodników

| Zdarzenie | Typ | Źródło |
|---|---|---|
| Zgłoszono indywidualny start | domenowe | start_indywidualny |
| Wyciągnięto zawodnika z grupy | domenowe | start_indywidualny |
| Zgubiono dyskietkę z numerem startowym | domenowe | start_indywidualny |
| Przejęto kaucję | domenowe | start_indywidualny |
| Odpięto numer startowy | domenowe | start_indywidualny |
| Przypisano numer startowy (nowy) | domenowe | start_indywidualny |
| Wydano nową dyskietkę | domenowe | start_indywidualny |
| Wystartowano zawodników | domenowe | wystartowano_zawodników |
| Wystartowano zawodników na danym dystansie | domenowe | wystartowano_zawodników |
| Zamknięto zapisy na dany dystans | domenowe | wystartowano_zawodników |
| Wystartowano grupę | domenowe | wystartowano_zawodników |
| Zarejestrowano czas startu | domenowe | wystartowano_zawodników |
| Wystartowano zawodnika | domenowe | wystartowano_zawodników |
| Zakończono udział przedwcześnie | domenowe | wystartowano_zawodników |

### Obszar: Pomiar czasu i wyniki

| Zdarzenie | Typ | Źródło |
|---|---|---|
| Przyłożenie dyskietki do czytnika (zawodnik jest wyzwalaczem) | zewnętrzne | pomiar_czasu — fizyczny trigger RFID |
| Zarejestrowano pomiar czasu | domenowe | pomiar_czasu |
| Zawodnik zakończył przejazd | domenowe | pomiar_czasu |
| Podliczono czasy przejazdu | domenowe | pomiar_czasu |
| Zakończono przejazd zawodników na danym dystansie | domenowe | pomiar_czasu |
| Zakończono przejazdy wszystkich dystansów | domenowe | pomiar_czasu, zakończenie_zawodów |
| Zgłoszono prośbę o przedłużenie dystansu | domenowe | pomiar_czasu |
| Przedłużono przejazd zawodnika | domenowe | pomiar_czasu |
| Odnotowano dodatkowy pomiar czasu | domenowe | pomiar_czasu |
| Zawodnik zakończył przejazd (wydłużony) | domenowe | pomiar_czasu |

### Obszar: Zakończenie zawodów

| Zdarzenie | Typ | Źródło |
|---|---|---|
| Przyznano nagrodę | domenowe | zakończenie_zawodów |
| Zwrócono pakiet startowy | domenowe | zakończenie_zawodów |
| Kaucja została zwrócona | domenowe | zakończenie_zawodów |
| Otrzymano dyplom | domenowe | zakończenie_zawodów |
| Upłynął czas zwrotu pakietu | czasowe | zakończenie_zawodów |
| Nie zwrócono pakietu startowego | domenowe | zakończenie_zawodów |
| Kaucja zatrzymana (częściowo nieczytelne) | domenowe | zakończenie_zawodów |

---

## Granice kontekstów i obszary domenowe

| Kontekst | Opis | Źródło |
|---|---|---|
| Konfiguracja maratonu (Frame 54) | Parametry i regulamin maratonu | kontekst_rejestracja1, ogólny_widok |
| Rejestracja (Frame 52) | Rejestracja uczestnika na dystans, weryfikacja danych | kontekst_rejestracja1, ogólny_widok |
| Opłata za udział (Frame 51) | Obsługa płatności, niedopłaty, zwrotów | ogólny_widok, opłata_za_udział_1, opłata_za_udział_2 |
| Przydział numeru startowego | Generowanie i przypisanie numerów startowych | ogólny_widok |
| GeneracjaGrupStartowych | Automatyczne generowanie grup startowych (bounded context jawnie nazwany) | genreacja_numeru_indywidualna, utowrzenie_grup_ds |
| GrupyStartowe | Zarządzanie grupami startowymi — zmiana grupy przez zawodnika (bounded context jawnie nazwany) | grupy_startowe_ds, zminia_grupystartowej |
| Weryfikacja i kwalifikacja przed startem | Check-in zawodnika, wydanie pakietu startowego | przypisanie_zawodnika_do_listy |
| Start indywidualny | Obsługa spóźnionych i startujących poza grupą | start_indywidualny |
| Zamknięcie zapisów | Zamknięcie listy startowej po starcie | wystartowano_zawodników |
| Pomiar czasu | Rejestracja RFID, real-time wyniki między-czasów | pomiar_czasu |
| Zakończenie zawodów (Frame 46) | Nagrody, dyplomy, zwrot pakietów | zakończenie_zawodów |

---

## Integracje zewnętrzne (kompletna lista — potwierdzone przez orkiestratora)

| System zewnętrzny | Typ | Zdarzenia | Uwagi |
|---|---|---|---|
| Bank (przelew bankowy) | zewnętrzny — płatności | Zarejestrowano przelew | Interfejs banku jako aktor zewnętrzny; zdarzenia inicjowane przez bank po wykonaniu przelewu przez uczestnika |
| RFID (czytnik dyskietek) | zewnętrzny — fizyczny trigger | Przyłożenie dyskietki do czytnika | System RFID rejestruje czas przejazdu przez checkpoint; dyskietka = karta RFID |

**Uwaga:** Dyplomy generowane wewnętrznie (brak zewnętrznej platformy). Brak zewnętrznej platformy rejestracyjnej.

---

## Potwierdzone fakty domenowe

1. **Real-time display:** zdarzenia przejazdu przez checkpoint. Dystanse: 50, 100, 150, 200, 300, 400, 500, 600, 650, 700 km. 1 pętla = 100 km. Checkpointy na każdej pętli (50 km — o jeden mniej). Wyświetlanie: biuro zawodów + strona WWW.
2. **Kary/dyskwalifikacja:** POZA ZAKRESEM — decyzja sędziego (plik kary-processed.md wykluczony).
3. **Duplikaty (traktowane jako jeden scenariusz):**
   - zakończenie_2 = zakończenie_zawodów
   - zmiania_grupy_startowej_DL = zminia_grupystartowej
4. **Reguła płatności przy zmianie grupy:** brak zapłaty różnicy przy zmianie grupy = brak zmiany.
5. **Zmiana grupy jednorazowa:** zawodnik nie może zmienić grupy więcej niż raz.

---

## Hot Spoty dotyczące Big Picture

| Hot Spot | Obszar | Źródło |
|---|---|---|
| Brak explicite zdefiniowanej granicy systemu jako całości (co jest w systemie, co poza) | Granice systemu | index.md |
| Nota: "grupy mogą istnieć tylko po pewnych krokach jako reprezentacje list startowych..." (częściowo nieczytelna) | Generacja grup | generacja grup startowych |
| Pytanie: co zrobić gdy rozpocznie się generowanie list a czekamy na opłacenie niedopłaty? | Opłata / Grupy | opłata_za_udział_1, opłata_za_udział_2 |
| Wyświetlanie w czasie rzeczywistym nieobecne w materiałach — prawdopodobna strata konwersji | Pomiar czasu | pomiar_czasu, wystartowano_zawodników |
| Brak opisanego przepływu generowania wyników/rankingów i dyplomów (za wyjątkiem zdarzenia "Otrzymano dyplom") | Zakończenie / Wyniki | index.md |
| Różowa karteczka przy Frame 51 (treść nieczytelna) | Opłata za udział | ogólny_widok |

---

## Uwagi o pokryciu (coverage 83%)

- Aktorzy: pokrycie pełne (1.0)
- Zdarzenia domenowe: ~65 unikalnych — pokrycie 0.8 po obniżce lossy
- Granice systemu: jawne granice kontekstów obecne; brak ogólnej granicy systemu — pokrycie 0.8
- Integracje zewnętrzne: bank + RFID zidentyfikowane, choć nie nazwane explicite jako "integracja z systemem X" — pokrycie 0.7
- Wszystkie pliki oznaczone converted: lossy — możliwa częściowa strata konwersji karteczek
