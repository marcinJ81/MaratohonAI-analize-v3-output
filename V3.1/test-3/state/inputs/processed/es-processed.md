<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture + process-level -->
<!-- used-in: phase-1, phase-2 -->
<!-- converted: lossy (screenshoty Event Stormingu — niektóre kartki nieczytelne z powodu rozdzielczości) -->

# Event Storming — System zarządzania maratonem rowerowym 24h

## Zidentyfikowane obszary (Bounded Contexts)

| Obszar | Nazwa na diagramie | Pliki źródłowe |
|---|---|---|
| BC-1 | Konfiguracja maratonu | kontekst_rejestracja1.png, ogólny_widok_kontekstu_rejestracja.png |
| BC-2 | Rejestracja | kontekst_rejestracja1.png, ogólny_widok_kontekstu_rejestracja.png |
| BC-3 | Opłata za udział | opłata_za_udział_1.png, opłata_za_udział_2.png, ogólny_widok_kontekstu_rejestracja.png |
| BC-4 | Grupy startowe | generacja grup startowych.png, generacja_grup_poterminie.png, genreacja_numeru_indywidualna.png, grupy_startowe_ds.png, zminia_grupystartowej.png, zmiania_grupy_startowej_DL.png, utowrzenie_grup_ds.png |
| BC-5 | Przygotowanie do startu | przypisanie_zawodnika_do_listy.png, start_indywidualny.png |
| BC-6 | Start zawodników | wystartowano_zawodników.png, start_indywidualny.png |
| BC-7 | Pomiar czasu | pomiar_czasu.png |
| BC-8 | Kary | kary.png |
| BC-9 | Zakończenie zawodów | zakończenie_zawodów.png, zakończenie_2.png |

---

## Aktorzy

| ID | Nazwa | Rola | Zewnętrzny? |
|---|---|---|---|
| A-01 | Uczestnik / Zawodnik | Rejestruje się, płaci, startuje, jedzie | Nie |
| A-02 | Administrator | Konfiguruje maraton, zarządza systemem | Nie |
| A-03 | Główny organizator zawodów | Przyznaje nagrody, zatwierdza decyzje | Nie |
| A-04 | Sędzia trasy | Startuje zawodników, nakłada kary | Nie |
| A-05 | Obsługa punktu kontrolnego | Weryfikuje zawodników, wydaje pakiety, rejestruje zdarzenia | Nie |
| A-06 | Pracownik obsługi | Obsługuje zwroty, nadpłaty | Nie |
| A-07 | System pomiaru czasu | Rejestruje odczyty chipów (dyskietki/czytniki) | Tak (integracja) |
| A-08 | Interfejs banku | Rejestruje przelewy, obsługuje płatności | Tak (integracja) |

---

## Zdarzenia domenowe

### BC-1: Konfiguracja maratonu
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-001 | Zarejestrowano maraton | Administrator | Konfiguracja maratonu |

*Uwaga: bardzo mało zdarzeń — kontekst widoczny tylko jako tło w Frame 54. Reguły konfiguracji maratonu widoczne jako policy (różowe kartki).*

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
| E-017 | Poinformowano użytkownika o zmianie opłaty | System | Rejestracja |

### BC-3: Opłata za udział
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-020 | Rozpoczęto opłacenie udziału | Uczestnik | Opłata za udział |
| E-021 | Zarejestrowano przelew | Interfejs banku | Opłata za udział |
| E-022 | Zarejestrowano opłatę | Obsługa pkt kontrolnego | Opłata za udział |
| E-023 | Opłacono uczestnictwo | System | Opłata za udział |
| E-024 | Zaaplikowano promocje (-100%) | System | Opłata za udział |
| E-025 | Wyliczono kwotę zwrotu | System | Opłata za udział |
| E-026 | Zwrócono nadpłatę | Pracownik obsługi / Administrator | Opłata za udział |
| E-027 | Rozpoczęto opłacenie niedopłaty | Uczestnik | Opłata za udział |
| E-028 | Opłacono niedopłatę | System | Opłata za udział |
| E-029 | Opłacono zmianę dystansu | System | Opłata za udział |
| E-030 | Wyliczono opłatę do nowego dystansu | System | Opłata za udział |
| E-031 | Wyliczono kwotę zwrotu (zmiana dystansu) | System | Opłata za udział |
| E-032 | Złożono wniosek o zmianę dystansu (po opłaceniu) | Uczestnik | Opłata za udział |

*Hot spot: "co zrobić jak rozpocznie się generowanie list a czekamy na opłacenie niedopłaty?"*

### BC-4: Grupy startowe
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-040 | Przypisano zawodnika do grupy | Administrator | Grupy startowe |
| E-041 | Złożono wniosek o zmianę grupy | Zawodnik | Grupy startowe |
| E-042 | Przyjęto wniosek o zmianę grupy | Administrator | Grupy startowe |
| E-043 | Nie przyjęto wniosku o zmianę grupy | Administrator | Grupy startowe |
| E-044 | Grupa została zmieniona | System | Grupy startowe |
| E-045 | Grupa nie została zmieniona | System | Grupy startowe |
| E-046 | Wygenerowano grupy startowe | System | Grupy startowe |
| E-047 | Nie udało się wygenerować grup startowych | System | Grupy startowe |
| E-048 | Przypisano nieprzydzielonych zawodników do grupy (auto) | System | Grupy startowe |
| E-049 | Dołączono do grupy | System | Grupy startowe |
| E-050 | Złożono wniosek o zmianę dystansu (przed gen. grup) | Zawodnik | Grupy startowe |
| E-051 | Złożono wniosek o zmianę dystansu (po gen. grup) | Zawodnik | Grupy startowe |
| E-052 | Zmieniono grupę | System | Grupy startowe |
| E-053 | Nie przypisano do ostatecznej grupy | System | Grupy startowe |

*Reguły biznesowe widoczne:*
- Nie przekroczono terminu zmiany grup
- Grupa docelowa ma wolne miejsca
- Zawodnik nie zmieniał wcześniej grupy
- Grupa docelowa jest z tego samego dystansu
- Grupy startowe zostały wygenerowane
- Minimalna liczba uczestników spełniona
- Nie przekroczono ostatecznego terminu generowania grup
- Zmiana grupy jest możliwa tylko w pewnym oknie czasowym

### BC-5: Przygotowanie do startu
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-060 | Zweryfikowano tożsamość zawodnika | Obsługa pkt kontrolnego | Przygotowanie do startu |
| E-061 | Podpisano oświadczenie zawodnika | Zawodnik | Przygotowanie do startu |
| E-062 | Wydano pakiet startowy | Obsługa pkt kontrolnego | Przygotowanie do startu |
| E-063 | Zweryfikowano zawodnika przed startem (lista startowa) | Obsługa pkt kontrolnego | Przygotowanie do startu |
| E-064 | Zdyskwalifikowano zawodnika (brak weryfikacji) | System | Przygotowanie do startu |
| E-065 | Zdyskwalifikowano zawodnika (brak oświadczenia) | System | Przygotowanie do startu |
| E-066 | Przypisano numer startowy | Obsługa pkt kontrolnego | Przygotowanie do startu |
| E-067 | Zgubiono dyskietkę z numerem startowym | Zawodnik | Przygotowanie do startu |
| E-068 | Przejęto kaucję | Obsługa pkt kontrolnego | Przygotowanie do startu |
| E-069 | Odpięto numer startowy | Obsługa pkt kontrolnego | Przygotowanie do startu |
| E-070 | Wydano nową dyskietkę | Obsługa pkt kontrolnego | Przygotowanie do startu |

### BC-6: Start zawodników
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-080 | Zamknięto zapisy na dany dystans | Administrator | Start zawodników |
| E-081 | Wystartowano grupę | Sędzia trasy | Start zawodników |
| E-082 | Zarejestrowano czas startu | System | Start zawodników |
| E-083 | Wystartowano zawodnika | Sędzia trasy | Start zawodników |
| E-084 | Zgłoszono indywidualny start | Zawodnik | Start zawodników |
| E-085 | Wyciągnięto zawodnika z grupy (start indywidualny) | Administrator | Start zawodników |
| E-086 | Zdyskwalifikowano zawodnika (przy starcie) | System | Start zawodników |
| E-087 | Zakończono udział przedwcześnie | System | Start zawodników |

### BC-7: Pomiar czasu
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-090 | Zarejestrowano pomiar czasu | System pomiaru czasu | Pomiar czasu |
| E-091 | Zawodnik zakończył przejazd (deklarowany dystans) | System pomiaru czasu | Pomiar czasu |
| E-092 | Podliczono czasy przejazdu | System | Pomiar czasu |
| E-093 | Zakończono przejazd zawodników na danym dystansie | System | Pomiar czasu |
| E-094 | Zakończono przejazdy wszystkich dystansów | System | Pomiar czasu |
| E-095 | Zgłoszono prośbę o przedłużenie dystansu | Zawodnik | Pomiar czasu |
| E-096 | Przedłużono przejazd zawodnika | Obsługa pkt kontrolnego | Pomiar czasu |
| E-097 | Odnotowano dodatkowy pomiar czasu | System pomiaru czasu | Pomiar czasu |
| E-098 | Zawodnik zakończył przejazd (wydłużony dystans) | System pomiaru czasu | Pomiar czasu |

*Wyzwalacz: przyłożenie dyskietki do czytnika (chip timing)*
*Reguły: limity na dany dystans — regulamin*
*System wie, że wszyscy dojechali albo że minął czas (24h)*

### BC-8: Kary
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-100 | Nałożono karę na zawodnika za złamanie regulaminu | Sędzia trasy | Kary |
| E-101 | Naliczono karę za pominięte punkty kontrolne | Sędzia trasy | Kary |

*Uwaga: kontekst bardzo słabo rozwinięty w Event Stormingu — tylko 2 zdarzenia, brak przepływów.*

### BC-9: Zakończenie zawodów
| ID | Zdarzenie | Aktor | Obszar |
|---|---|---|---|
| E-110 | Przyznano nagrodę | Główny organizator zawodów | Zakończenie zawodów |
| E-111 | Zwrócono pakiet startowy | Zawodnik / Osoba upoważniona | Zakończenie zawodów |
| E-112 | Kaucja została zwrócona | Obsługa pkt kontrolnego | Zakończenie zawodów |
| E-113 | Otrzymano dyplom | Zawodnik | Zakończenie zawodów |
| E-114 | Upłynął czas zwrotu pakietu | System (timer) | Zakończenie zawodów |
| E-115 | Nie zwrócono pakietu startowego | System | Zakończenie zawodów |
| E-116 | Kaucja została zatrzymana | System | Zakończenie zawodów |

*Reguły: czas do zwrotu ustalony w regulaminie*

---

## Granice systemu i integracje zewnętrzne

| Integracja | Typ | Kierunek | Uwagi |
|---|---|---|---|
| Interfejs banku | System płatności | Dwukierunkowy (przelew IN, ewentualnie zwrot OUT) | Zewnętrzny |
| System pomiaru czasu (chip timing) | Czytnik dyskietek/chipów | IN → system | Zewnętrzny; wyzwalacz: przyłożenie dyskietki do czytnika |

---

## Hot Spoty

| ID | Opis | Powód | Powiązane zdarzenia |
|---|---|---|---|
| HS-01 | Co zrobić jak rozpocznie się generowanie list a czekamy na opłacenie niedopłaty? | Wyścig między stanem opłat a generacją grup startowych | E-027, E-028, E-046 |
| HS-02 | Kontekst Kar jest bardzo słabo opisany | Tylko 2 zdarzenia, brak przepływów i reguł | E-100, E-101 |
| HS-03 | Brak jasnych przepływów generowania wyników / rankingów po zakończeniu zawodów | BC-9 opisuje głównie ceremonię, brak rankingów | E-092, E-093, E-094 |
| HS-04 | Zmiana dystansu po wygenerowaniu grup — niejasny przepływ | Zapis grupy i opłaty muszą być skoordynowane | E-044, E-051, E-029 |

---

## Pytania otwarte wynikające z analizy materiałów

| ID | Pytanie | Priorytet | Dotyczy |
|---|---|---|---|
| Q-001 | Jak dokładnie działa generowanie rankingów / wyników po zakończeniu zawodów? | blocker | BC-9, BC-7 |
| Q-002 | Jakie kary istnieją (typy, wartości) i jak wpływają na wyniki? | blocker | BC-8 |
| Q-003 | Co dzieje się z zawodnikami z niedopłatą gdy startuje generacja grup? | blocker | BC-3, BC-4 (HS-01) |
| Q-004 | Czy ranking uwzględnia kary (czas karny) czy dyskwalifikację? | wysoki | BC-7, BC-8 |
| Q-005 | Czy istnieje mechanizm płatności online czy tylko przelew tradycyjny? | średni | BC-3 |
| Q-006 | Czym dokładnie jest "dyskietka" — chip RFID, transponder? | niski | BC-7 |
| Q-007 | Czy zawodnik może jechać wiele dystansów czy tylko jeden? | średni | BC-2, BC-4 |
| Q-008 | Jak wyglądają punkty kontrolne — czy brak przejazdu przez punkt = kara? | wysoki | BC-8 |
