---
phase: big-picture
session: maraton-rowerowy-24h
generated: 2026-06-06
status: completed
coverage: 92%
sources:
  - state/inputs/processed/phase-1-seed.md
  - user-input
---

# Big Picture — System zarządzania i pomiaru czasu na maratonie rowerowym 24H

## Kontekst
Analizowano system obsługujący maraton rowerowy 24H — od konfiguracji zawodów i rejestracji uczestników, przez zarządzanie grupami startowymi i pomiar czasu na trasie, aż po zakończenie zawodów i publikację wyników. Cel: zrozumienie domeny i uzupełnienie procesu. Złożoność systemu: średnia — kilka bounded contexts, reguły biznesowe, integracja z bankiem i sprzętem pomiarowym.

---

## Aktorzy

| ID    | Nazwa | Rola | Zewnętrzny? | Uwagi |
|-------|-------|------|-------------|-------|
| A-001 | Administrator | Konfiguruje maraton, zarządza rejestracją, zamknięciem zapisów | nie | |
| A-002 | Uczestnik / Zawodnik | Rejestruje się, opłaca, bierze udział | nie | |
| A-003 | Obsługa punktu kontrolnego | Weryfikuje tożsamość, wydaje pakiety, rejestruje opłaty, obsługuje kaucje | nie | |
| A-004 | Interfejs banku | Rejestruje przelewy płatności | tak | system zewnętrzny |
| A-005 | Główny organizator zawodów | Przyznaje nagrody, zatwierdza operacje finansowe | nie | |
| A-006 | Sędzia trasy | Startuje grupy i zawodników, rejestruje czas startu, nakłada kary | nie | |
| A-007 | System pomiaru czasu | Urządzenie sprzętowe (czytnik + dyskietka) rejestrujące przejazdy | nie | sprzęt wewnętrzny |
| A-008 | System (automatyczny) | Oblicza opłaty, przydziela grupy, waliduje warunki, aktualizuje ranking | nie | |
| A-009 | Timer / Harmonogram | Wyzwala zdarzenia czasowe (terminy rejestracji, termin zwrotu pakietu) | nie | |
| A-010 | Pracownik obsługi | Wykonuje zwroty nadpłat, obsługuje wyjątki finansowe | nie | |

---

## Zdarzenia domenowe

| ID    | Zdarzenie | Aktor (ID) | Obszar | Hot Spot? |
|-------|-----------|------------|--------|-----------|
| E-001 | Zarejestrowano maraton | A-001 | Konfiguracja maratonu | nie |
| E-002 | Skonfigurowano regulamin maratonu | A-001 | Konfiguracja maratonu | nie |
| E-003 | Zarejestrowano uczestnika | A-002 | Rejestracja | nie |
| E-004 | Wybrano dystans | A-002 | Rejestracja | nie |
| E-005 | Potwierdzono dane kontaktowe | A-002 / A-008 | Rejestracja | nie |
| E-006 | Zweryfikowano zawodnika (w biurze) | A-003 | Rejestracja | nie |
| E-007 | Złożono wniosek o zmianę dystansu | A-002 | Rejestracja | tak |
| E-008 | Zmieniono dystans | A-008 | Rejestracja | tak |
| E-009 | Wyliczono opłatę | A-008 | Rejestracja | nie |
| E-010 | Poinformowano uczestnika o zmianie opłaty | A-008 | Rejestracja | nie |
| E-011 | Rozpoczęto opłacanie udziału | A-002 | Opłata za udział | nie |
| E-012 | Zarejestrowano przelew | A-004 | Opłata za udział | nie |
| E-013 | Zarejestrowano opłatę | A-003 | Opłata za udział | nie |
| E-014 | Opłacono uczestnictwo | A-008 | Opłata za udział | nie |
| E-015 | Zaaplikowano promocję | A-003 | Opłata za udział | nie |
| E-016 | Rozpoczęto opłacanie niedopłaty | A-002 | Opłata za udział | tak |
| E-017 | Opłacono niedopłatę | A-008 | Opłata za udział | tak |
| E-018 | Opłacono zmianę dystansu | A-008 | Opłata za udział | nie |
| E-019 | Wyliczono kwotę zwrotu | A-008 | Opłata za udział | nie |
| E-020 | Zwrócono nadpłatę | A-010 | Opłata za udział | nie |
| E-021 | Przypisano numer startowy | A-008 | Grupy startowe | nie |
| E-022 | Wygenerowano grupy startowe | A-001 / A-008 | Grupy startowe | nie |
| E-023 | Nie udało się wygenerować grup startowych | A-008 | Grupy startowe | nie |
| E-024 | Przypisano zawodnika do grupy | A-008 | Grupy startowe | tak |
| E-025 | Złożono wniosek o zmianę grupy | A-002 | Grupy startowe | nie |
| E-026 | Przyjęto wniosek o zmianę grupy | A-008 | Grupy startowe | nie |
| E-027 | Nie przyjęto wniosku o zmianę grupy | A-008 | Grupy startowe | nie |
| E-028 | Zmieniono grupę | A-008 | Grupy startowe | nie |
| E-029 | Dołączono do grupy po terminie | A-008 | Grupy startowe | nie |
| E-030 | Zweryfikowano tożsamość zawodnika | A-003 | Przygotowanie do startu | nie |
| E-031 | Zawodnik nie przeszedł weryfikacji tożsamości | A-003 | Przygotowanie do startu | nie |
| E-032 | Podpisano oświadczenie zawodnika | A-002 | Przygotowanie do startu | nie |
| E-033 | Zawodnik nie podpisał oświadczenia | A-002 | Przygotowanie do startu | nie |
| E-034 | Wydano pakiet startowy | A-003 | Przygotowanie do startu | nie |
| E-035 | Zdyskwalifikowano zawodnika (biuro) | A-003 | Przygotowanie do startu | nie |
| E-036 | Zweryfikowano zawodnika przed startem (lista) | A-003 | Przygotowanie do startu | nie |
| E-037 | Zdyskwalifikowano zawodnika (przed startem) | A-003 | Przygotowanie do startu | nie |
| E-038 | Zgubiono dyskietkę z numerem startowym | A-002 | Przygotowanie do startu | nie |
| E-039 | Przejęto kaucję za dyskietkę | A-003 | Przygotowanie do startu | nie |
| E-040 | Odpięto numer startowy | A-003 | Przygotowanie do startu | nie |
| E-041 | Przypisano nowy numer startowy | A-003 | Przygotowanie do startu | nie |
| E-042 | Wydano nową dyskietkę | A-003 | Przygotowanie do startu | nie |
| E-043 | Zamknięto zapisy na dany dystans | A-001 | Start zawodników | nie |
| E-044 | Wystartowano grupę | A-006 | Start zawodników | nie |
| E-045 | Zarejestrowano czas startu grupy | A-006 | Start zawodników | nie |
| E-046 | Wystartowano zawodnika (w grupie) | A-006 | Start zawodników | nie |
| E-047 | Zdyskwalifikowano zawodnika (przy starcie) | A-006 | Start zawodników | nie |
| E-048 | Zakończono udział przedwcześnie | A-006 | Start zawodników | nie |
| E-049 | Zgłoszono indywidualny start | A-002 | Start zawodników | tak |
| E-050 | Wyciągnięto zawodnika z grupy | A-001 | Start zawodników | nie |
| E-051 | Zarejestrowano pomiar czasu | A-007 | Pomiar czasu | nie |
| E-052 | Zawodnik zakończył przejazd (dystans std.) | A-002 / A-007 | Pomiar czasu | nie |
| E-053 | Podliczono czasy przejazdu | A-008 | Pomiar czasu | nie |
| E-054 | Zakończono przejazdy na danym dystansie | A-008 | Pomiar czasu | nie |
| E-055 | Zakończono przejazdy wszystkich dystansów | A-008 | Pomiar czasu | nie |
| E-056 | Zgłoszono prośbę o przedłużenie dystansu | A-002 | Pomiar czasu | nie |
| E-057 | Przedłużono przejazd zawodnika | A-003 | Pomiar czasu | nie |
| E-058 | Odnotowano dodatkowy pomiar czasu | A-007 | Pomiar czasu | nie |
| E-059 | Zawodnik zakończył przejazd (dystans wydł.) | A-002 / A-007 | Pomiar czasu | nie |
| E-060 | Nałożono karę za złamanie regulaminu | A-006 | Nakładanie kar | nie |
| E-061 | Naliczono karę za pominięte punkty kontrolne | A-006 | Nakładanie kar | nie |
| E-062 | Zaktualizowano wyniki na żywo | A-008 | Wyniki / Ranking | nie |
| E-063 | Sklasyfikowano zawodnika w rankingu (dystans + płeć) | A-008 | Wyniki / Ranking | nie |
| E-064 | Zawodnik przeniesiony na koniec listy wyników | A-008 | Wyniki / Ranking | nie |
| E-065 | Zawodnik wykluczony z wyników | A-008 | Wyniki / Ranking | nie |
| E-066 | Opublikowano wyniki końcowe | A-008 | Wyniki / Ranking | nie |
| E-067 | Przyznano nagrodę | A-005 | Zakończenie zawodów | nie |
| E-068 | Zwrócono pakiet startowy | A-002 / A-003 | Zakończenie zawodów | nie |
| E-069 | Kaucja została zwrócona | A-003 | Zakończenie zawodów | nie |
| E-070 | Upłynął czas zwrotu pakietu | A-009 | Zakończenie zawodów | nie |
| E-071 | Nie zwrócono pakietu startowego | A-002 | Zakończenie zawodów | nie |
| E-072 | Kaucja została zatrzymana | A-008 | Zakończenie zawodów | nie |
| E-073 | Otrzymano dyplom | A-002 | Zakończenie zawodów | nie |

---

## Granice systemu

### Opis granic
System obejmuje pełen cykl życia zawodów — od konfiguracji, przez rejestrację i opłaty, zarządzanie grupami, start, pomiar czasu i kary, aż po wyniki i zakończenie. Poza systemem pozostaje: rzeczywisty transfer bankowy (obsługuje bank), fizyczne urządzenie pomiaru czasu (hardware), oraz prezentacja wyników na stronie WWW (publikacja wychodzi do zewnętrznych kanałów).

### Integracje zewnętrzne

| ID    | System zewnętrzny | Kierunek | Typ integracji | Uwagi |
|-------|-------------------|----------|----------------|-------|
| I-001 | Interfejs banku | wchodzi | async | Rejestracja przelewów; nie jest jasne czy automatycznie czy ręcznie |
| I-002 | Strona WWW zawodów | wychodzi | push / sync | Publikacja wyników na żywo i końcowych |
| I-003 | Tablica wyników na miejscu | wychodzi | push | Wyświetlanie wyników na żywo |

---

## Hot Spoty

| ID    | Opis | Powód | Powiązane zdarzenia | Priorytet |
|-------|------|-------|---------------------|-----------|
| H-001 | Niedopłata vs generowanie list startowych | Co jeśli generowanie list startuje gdy uczestnik ma nieopłaconą niedopłatę — czy blokować generację czy uczestnika? | E-016, E-017, E-022 | wysoki — blocker |
| H-002 | Start indywidualny — rejestracja czasu | Zawodnik startuje indywidualnie (np. spóźnienie) — jak system rejestruje czas startu inny niż grupowy? Inny punkt startowy? | E-049, E-050, E-045 | wysoki |
| H-003 | Awaria systemu pomiaru czasu | Co jeśli czytnik nie zarejestruje przejazdu zawodnika (hardware failure)? Czy jest ręczny fallback? | E-051, E-052 | średni |
| H-004 | Zmiana dystansu po wygenerowaniu grup | Zawodnik zmienia dystans po wygenerowaniu grup — trafia do stanu "nie przypisano do ostatecznej grupy" | E-007, E-008, E-022, E-024 | średni |
| H-005 | Zawodnik bez przypisanej grupy w dniu startu | Edge case — opłacony zawodnik bez grupy (zmiana dystansu lub błąd systemu) | E-024, E-022 | średni |
| H-006 | Mechanizm integracji z bankiem | Czy rejestracja przelewu jest automatyczna (webhook/API) czy ręczna przez obsługę? | E-012 | niski |

---

## Wnioski

| ID    | Wniosek | Pewność | Źródło |
|-------|---------|---------|--------|
| W-001 | System ma 9 wyraźnych obszarów domenowych: Konfiguracja, Rejestracja, Opłaty, Grupy startowe, Przygotowanie do startu, Start, Pomiar czasu, Kary, Wyniki/Ranking, Zakończenie | wysoka | user-input + ES |
| W-002 | Dyskietka z numerem startowym jest centralnym identyfikatorem technicznym zawodnika na trasie — jej utrata/awaria to krytyczny scenariusz | wysoka | ES |
| W-003 | Ranking dzieli się per dystans × płeć — brak innych kategorii | wysoka | user-input |
| W-004 | Kara to nie czas dodatkowy lecz pozycja w rankingu (koniec listy lub wykluczenie) | wysoka | user-input |
| W-005 | Proces rejestracji i opłat jest ściśle powiązany — zmiana dystansu przed i po opłaceniu różni się zachowaniem | wysoka | ES |
| W-006 | Wyniki publikowane są na żywo (w trakcie) i końcowo (po zawodach) w dwóch kanałach: na miejscu i WWW | wysoka | user-input |
| W-007 | Generacja grup startowych jest krytycznym krokiem z wieloma warunkami — minimalna liczba uczestników, terminy, numery startowe | wysoka | ES |
| W-008 | Kontekst Wyniki/Ranking nie był uwzględniony w Event Stormingu — odkryty w trakcie analizy | wysoka | derived |

---

## Otwarte pytania

| ID    | Pytanie | Priorytet | Dotyczy | Przeniesione do fazy |
|-------|---------|-----------|---------|----------------------|
| Q-001 | Co się dzieje z uczestnikiem z niedopłatą gdy startuje generowanie grup? Czy blokowany czy traktowany jak nieaktywny? | blocker | Opłata za udział / Grupy startowe | process-level |
| Q-002 | Jak system rejestruje czas startu indywidualnego — czy zawodnik ma inny punkt startowy czy jest "wyciągany" z grupy i startuje z tym samym czasem? | blocker | Start zawodników / Pomiar czasu | process-level |
| Q-003 | Czy istnieje ręczna rejestracja czasu jako fallback gdy system pomiaru czasu zawiedzie? | nice-to-have | Pomiar czasu | process-level |
| Q-004 | Jak działa integracja z bankiem — automatyczny webhook/API czy ręczna rejestracja przez obsługę? | nice-to-have | Opłata za udział | process-level |
| Q-005 | Jak obliczany jest czas będący podstawą rankingu — czas od startu grupy do mety, czy od indywidualnego startu? | blocker | Wyniki / Ranking | process-level |

---

## Korekty i uzupełnienia (retrospektywne)
<!-- Sekcja wypełniana przez orkiestratora gdy późniejsza faza zmienia wnioski tej fazy. -->
