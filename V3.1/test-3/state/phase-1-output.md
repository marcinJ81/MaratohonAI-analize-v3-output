---
phase: big-picture
session: maraton-rowerowy-24h
generated: 2026-06-07
status: partial
coverage: 72%
sources:
  - state/inputs/processed/phase-1-seed.md
  - state/inputs/processed/es-processed.md
---

# Big Picture — System zarządzania maratonem rowerowym 24h

## Kontekst

System obsługuje pełny cykl życia 24-godzinnego maratonu rowerowego — od konfiguracji zasad, przez rejestrację uczestników i zarządzanie płatnościami, generację grup startowych, obsługę startu i pomiaru czasu, po zakończenie zawodów z nagrodami i dyplomami. Złożoność oceniona jako średnia (9 bounded contexts, 2 integracje zewnętrzne, liczne reguły biznesowe).

---

## Aktorzy

| ID | Nazwa | Rola | Zewnętrzny? |
|---|---|---|---|
| A-01 | Uczestnik / Zawodnik | Rejestruje się, wybiera dystans, płaci, jedzie | Nie |
| A-02 | Administrator | Konfiguruje maraton, zarządza ręcznie grupami i zapisami | Nie |
| A-03 | Główny organizator zawodów | Przyznaje nagrody, podejmuje decyzje finalne | Nie |
| A-04 | Sędzia trasy | Startuje zawodników, nakłada kary regulaminowe | Nie |
| A-05 | Obsługa punktu kontrolnego | Weryfikuje zawodników, wydaje pakiety, rejestruje zdarzenia offline | Nie |
| A-06 | Pracownik obsługi | Obsługuje zwroty, nadpłaty, płatności gotówkowe | Nie |
| A-07 | System pomiaru czasu (chip timing) | Rejestruje przejazdy przez czytniki RFID/dyskietki | Tak |
| A-08 | Interfejs banku | Obsługuje przelewy i zwroty płatności | Tak |
| A-09 | System / Timer wewnętrzny | Wyzwala zdarzenia czasowe (generacja grup, koniec zawodów) | Nie |

---

## Zdarzenia domenowe

### BC-1: Konfiguracja maratonu
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-001 | Zarejestrowano maraton | Administrator | Konfiguracja |

### BC-2: Rejestracja
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-010 | Zarejestrowano uczestnika | Uczestnik | Rejestracja |
| E-011 | Wybrano dystans | Uczestnik | Rejestracja |
| E-012 | Potwierdzono dane kontaktowe | System | Rejestracja |
| E-013 | Zweryfikowano zawodnika | System | Rejestracja |
| E-014 | Wyliczono opłatę | System | Rejestracja |
| E-015 | Złożono wniosek o zmianę dystansu | Uczestnik | Rejestracja |
| E-016 | Zmieniono dystans | System | Rejestracja |
| E-017 | Poinformowano uczestnika o zmianie opłaty | System | Rejestracja |

### BC-3: Opłata za udział
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-020 | Rozpoczęto opłacenie udziału | Uczestnik | Opłaty |
| E-021 | Zarejestrowano przelew | Interfejs banku | Opłaty |
| E-022 | Zarejestrowano opłatę (ręcznie) | Pracownik obsługi | Opłaty |
| E-023 | Opłacono uczestnictwo | System | Opłaty |
| E-024 | Zaaplikowano promocję (-100%) | System | Opłaty |
| E-025 | Wyliczono kwotę zwrotu | System | Opłaty |
| E-026 | Zwrócono nadpłatę | Pracownik obsługi | Opłaty |
| E-027 | Rozpoczęto opłacenie niedopłaty | Uczestnik | Opłaty |
| E-028 | Opłacono niedopłatę | System | Opłaty |
| E-029 | Opłacono zmianę dystansu | System | Opłaty |
| E-030 | Wyliczono opłatę do nowego dystansu | System | Opłaty |
| E-031 | Wyliczono kwotę zwrotu po zmianie dystansu | System | Opłaty |
| E-032 | Złożono wniosek o zmianę dystansu (po opłaceniu) | Uczestnik | Opłaty |

### BC-4: Grupy startowe
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-040 | Przypisano zawodnika do grupy (ręcznie) | Administrator | Grupy startowe |
| E-041 | Złożono wniosek o zmianę grupy | Zawodnik | Grupy startowe |
| E-042 | Przyjęto wniosek o zmianę grupy | Administrator | Grupy startowe |
| E-043 | Nie przyjęto wniosku o zmianę grupy | Administrator | Grupy startowe |
| E-044 | Zmieniono grupę | System | Grupy startowe |
| E-045 | Nie zmieniono grupy | System | Grupy startowe |
| E-046 | Wygenerowano grupy startowe (auto) | System / Timer | Grupy startowe |
| E-047 | Nie udało się wygenerować grup startowych | System | Grupy startowe |
| E-048 | Przypisano nieprzydzielonych zawodników (auto) | System | Grupy startowe |
| E-049 | Dołączono do grupy (po terminie) | System | Grupy startowe |
| E-050 | Złożono wniosek o zmianę dystansu (przed gen. grup) | Zawodnik | Grupy startowe |
| E-051 | Złożono wniosek o zmianę dystansu (po gen. grup) | Zawodnik | Grupy startowe |

### BC-5: Przygotowanie do startu
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-060 | Zweryfikowano tożsamość zawodnika | Obsługa pkt. kontrolnego | Przygotowanie |
| E-061 | Podpisano oświadczenie zawodnika | Zawodnik | Przygotowanie |
| E-062 | Wydano pakiet startowy | Obsługa pkt. kontrolnego | Przygotowanie |
| E-063 | Zweryfikowano zawodnika przed startem (lista startowa) | Obsługa pkt. kontrolnego | Przygotowanie |
| E-064 | Zdyskwalifikowano zawodnika (brak weryfikacji) | System | Przygotowanie |
| E-065 | Zdyskwalifikowano zawodnika (brak oświadczenia) | System | Przygotowanie |
| E-066 | Przypisano numer startowy (dyskietka) | Obsługa pkt. kontrolnego | Przygotowanie |
| E-067 | Zgubiono dyskietkę z numerem startowym | Zawodnik | Przygotowanie |
| E-068 | Przejęto kaucję za nową dyskietkę | Obsługa pkt. kontrolnego | Przygotowanie |
| E-069 | Odpięto numer startowy | Obsługa pkt. kontrolnego | Przygotowanie |
| E-070 | Wydano nową dyskietkę | Obsługa pkt. kontrolnego | Przygotowanie |

### BC-6: Start zawodników
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-080 | Zamknięto zapisy na dany dystans | Administrator | Start |
| E-081 | Wystartowano grupę | Sędzia trasy | Start |
| E-082 | Zarejestrowano czas startu grupy | System | Start |
| E-083 | Wystartowano zawodnika (indywidualnie) | Sędzia trasy | Start |
| E-084 | Zgłoszono indywidualny start | Zawodnik | Start |
| E-085 | Wyciągnięto zawodnika z grupy | Administrator | Start |
| E-086 | Zdyskwalifikowano zawodnika (przy starcie) | System | Start |
| E-087 | Zakończono udział przedwcześnie | Sędzia trasy | Start |

### BC-7: Pomiar czasu
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-090 | Zarejestrowano pomiar czasu (dyskietka) | System pomiaru czasu | Pomiar czasu |
| E-091 | Zawodnik zakończył przejazd (dekl. dystans) | System pomiaru czasu | Pomiar czasu |
| E-092 | Podliczono czasy przejazdu | System | Pomiar czasu |
| E-093 | Zakończono przejazd zawodników na danym dystansie | System | Pomiar czasu |
| E-094 | Zakończono przejazdy wszystkich dystansów | System | Pomiar czasu |
| E-095 | Zgłoszono prośbę o przedłużenie dystansu | Zawodnik | Pomiar czasu |
| E-096 | Przedłużono przejazd zawodnika | Obsługa pkt. kontrolnego | Pomiar czasu |
| E-097 | Odnotowano dodatkowy pomiar czasu (wydłużony) | System pomiaru czasu | Pomiar czasu |
| E-098 | Zawodnik zakończył przejazd (wydłużony dystans) | System pomiaru czasu | Pomiar czasu |

### BC-8: Kary
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-100 | Nałożono karę za złamanie regulaminu | Sędzia trasy | Kary |
| E-101 | Naliczono karę za pominięte punkty kontrolne | Sędzia trasy | Kary |

### BC-9: Zakończenie zawodów
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-110 | Przyznano nagrodę | Główny organizator | Zakończenie |
| E-111 | Zwrócono pakiet startowy | Zawodnik / Upoważniony | Zakończenie |
| E-112 | Kaucja zwrócona | Obsługa pkt. kontrolnego | Zakończenie |
| E-113 | Otrzymano dyplom | Zawodnik | Zakończenie |
| E-114 | Upłynął czas zwrotu pakietu | Timer | Zakończenie |
| E-115 | Nie zwrócono pakietu startowego | System | Zakończenie |
| E-116 | Kaucja zatrzymana | System | Zakończenie |

### Zdarzenia sugerowane przez agenta (not in source materials)
| ID | Zdarzenie | Uzasadnienie |
|---|---|---|
| E-S01 | Wygenerowano ranking / wyniki zawodów | Logiczny wniosek po E-094; brak w ES |
| E-S02 | Opublikowano wyniki | Typowe w podobnych systemach |
| E-S03 | Wysłano powiadomienie do uczestnika | Email/SMS po przypisaniu do grupy, starcie |
| E-S04 | Zawodnik nie stawił się na start (DNS) | Brak przepływu w ES |
| E-S05 | Anulowano rejestrację uczestnika | Brak przepływu w ES |

---

## Granice systemu

### W systemie
- Rejestracja i zarządzanie uczestnikami
- Obliczanie opłat i statusy płatności
- Generacja i zarządzanie grupami startowymi
- Weryfikacja i obsługa pakietów startowych
- Zarządzanie startem (grupy, indywidualny)
- Rejestracja i przetwarzanie pomiarów czasu
- Ewidencja kar
- Obsługa zakończenia (nagrody, dyplomy, kaucje)
- Konfiguracja maratonu i regulaminu

### Integracje zewnętrzne
| System | Kierunek | Protokół/Typ | Uwagi |
|---|---|---|---|
| Bank / system płatniczy | IN (przelewy) / OUT (zwroty) | nieznany | Rejestracja przelewów przychodzących |
| Chip timing / czytniki RFID | IN (odczyty) | fizyczny (dyskietki) | Wyzwala E-090..E-098 |

---

## Hot Spoty

| ID | Opis | Powód | Powiązane zdarzenia | Priorytet |
|---|---|---|---|---|
| HS-01 | Niedopłata vs generacja grup — wyścig stanów | Uczestnik z niedopłatą może blokować generację lub zostać pominięty | E-027, E-028, E-046 | blocker |
| HS-02 | Kontekst Kar prawie nieobecny | Tylko 2 zdarzenia; brak reguł, typów, wpływu na wynik | E-100, E-101 | blocker |
| HS-03 | Brak generowania rankingów / wyników | ES kończy się na "Zakończono przejazdy"; brak dalszego przepływu | E-092..E-094 | blocker |
| HS-04 | Zmiana dystansu po generacji grup | Wymaga koordynacji: zmiana grupy + przeliczenie opłaty | E-044, E-051, E-029 | wysoki |
| HS-05 | DNS — zawodnik nie stawia się na start | Brak zdarzenia i przepływu w materiale | E-S04 | średni |
| HS-06 | Anulowanie rejestracji — brak przepływu | Zasady zwrotów nieznane | E-S05 | średni |

---

## Wnioski

| ID | Wniosek | Pewność | Źródło |
|---|---|---|---|
| W-001 | System obsługuje pełny cykl życia maratonu w 9 odrębnych obszarach | wysoka | es-processed.md |
| W-002 | Grupy startowe to core domeny — największa liczba screenshotów, najbardziej rozbudowane reguły | wysoka | es-processed.md |
| W-003 | Dwa systemy zewnętrzne: bank i chip timing — oba są wejściem do systemu, nie wyjściem | wysoka | es-processed.md |
| W-004 | Regulamin maratonu pełni rolę konfiguracji globalnej (policy) wpływającej na wiele BC | wysoka | es-processed.md |
| W-005 | Konteksty Kar i Generowania Wyników są niekompletne — wymagają uzupełnienia przed Process Level | wysoka | coverage-assessment |
| W-006 | "Dyskietka" to fizyczny nośnik chipa; kaucja jest mechanizmem zabezpieczającym przed jej zgubieniem | średnia | es-processed.md |
| W-007 | Zmiana dystansu jest możliwa w różnych momentach (przed/po generacji grup) z różnymi konsekwencjami | wysoka | es-processed.md |

---

## Otwarte pytania

| ID | Pytanie | Priorytet | Dotyczy |
|---|---|---|---|
| Q-001 | Jak generowane są wyniki / ranking po zakończeniu zawodów? | blocker | BC-7, BC-8, BC-9 |
| Q-002 | Jakie typy kar istnieją? Jak kara wpływa na wynik (czas karny vs dyskwalifikacja)? | blocker | BC-8 |
| Q-003 | Co dzieje się z zawodnikiem z niedopłatą gdy startuje generacja grup? (HS-01) | blocker | BC-3, BC-4 |
| Q-004 | Czy zawodnik może wystartować na wielu dystansach jednocześnie? | wysoki | BC-2, BC-4 |
| Q-005 | Czy punkty kontrolne są obowiązkowe dla każdego zawodnika? Skąd system wie, że pominięto punkt? | wysoki | BC-7, BC-8 |
| Q-006 | Czy istnieje mechanizm powiadomień (email/SMS) dla uczestników? | średni | BC-2, BC-4 |
| Q-007 | Czy anulowanie rejestracji jest możliwe? Jakie zasady zwrotów? | średni | BC-2, BC-3 |
| Q-008 | Co to "DNS" (Did Not Start) — czy istnieje taki status w systemie? | średni | BC-6 |

## Korekty i uzupełnienia (retrospektywne)
<!-- wypełnia orkiestrator gdy późniejsza faza zmienia ten output -->

[przed-Phase-2]: Wniosek Q-002 wymaga korekty — uzupełniono po zebraniu wiedzy domenowej.
Poprzednio: kary mogą być czasowe lub dyskwalifikujące — nieznane
Aktualnie: **jedyna kara to dyskwalifikacja** (brak kar czasowych)
Powód: odpowiedź użytkownika przed startem Process Level

[przed-Phase-2]: Wniosek Q-001 — uzupełniono mechanizm generowania wyników.
Poprzednio: nieznany
Aktualnie: **system automatycznie generuje wyniki** na podstawie: (1) czasów zarejestrowanych na punktach pomiaru, (2) czasu ukończenia pełnego dystansu przez przekroczenie linii mety. Zawodnicy rywalizują w obrębie swojego dystansu.
Powód: odpowiedź użytkownika przed startem Process Level

[przed-Phase-2]: Wniosek Q-003 / HS-01 — uzupełniono regułę niedopłaty vs generacja grup.
Poprzednio: niejasne — czy uczestnik z niedopłatą wchodzi na listę startową
Aktualnie: **niedopłata dotyczy wyłącznie zmiany dystansu** — uczestnik, który chce zmienić dystans musi zapłacić różnicę; jeśli nie zapłaci, pozostaje na pierwotnym dystansie (za który zapłacił). Nie blokuje to uczestnictwa w maratonie.
Powód: odpowiedź użytkownika przed startem Process Level

Nowe zdarzenia potwierdzone:
- E-102: Wygenerowano wyniki / ranking (System, po E-094) → BC-9 Zakończenie zawodów
- E-103: Zdyskwalifikowano zawodnika z powodu kary (System) → BC-8 Kary
