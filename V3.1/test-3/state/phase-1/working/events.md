# Zdarzenia domenowe — Big Picture (Tryb 1 — propozycja agenta)
<!-- source: es-processed.md + seed phase-1 -->
<!-- status: do weryfikacji użytkownika -->

## BC-1: Konfiguracja maratonu
- E-001 Zarejestrowano maraton

## BC-2: Rejestracja
- E-010 Zarejestrowano uczestnika
- E-011 Wybrano dystans
- E-012 Potwierdzono dane kontaktowe
- E-013 Zweryfikowano zawodnika
- E-014 Wyliczono opłatę
- E-015 Złożono wniosek o zmianę dystansu
- E-016 Zmieniono dystans
- E-017 Poinformowano uczestnika o zmianie opłaty

## BC-3: Opłata za udział
- E-020 Rozpoczęto opłacenie udziału
- E-021 Zarejestrowano przelew (bank)
- E-022 Zarejestrowano opłatę (ręcznie przez obsługę)
- E-023 Opłacono uczestnictwo
- E-024 Zaaplikowano promocję (-100%)
- E-025 Wyliczono kwotę zwrotu
- E-026 Zwrócono nadpłatę
- E-027 Rozpoczęto opłacenie niedopłaty
- E-028 Opłacono niedopłatę
- E-029 Opłacono zmianę dystansu
- E-030 Wyliczono opłatę do nowego dystansu
- E-031 Wyliczono kwotę zwrotu po zmianie dystansu
- E-032 Złożono wniosek o zmianę dystansu (po opłaceniu)

## BC-4: Grupy startowe
- E-040 Przypisano zawodnika do grupy (ręcznie)
- E-041 Złożono wniosek o zmianę grupy
- E-042 Przyjęto wniosek o zmianę grupy
- E-043 Nie przyjęto wniosku o zmianę grupy
- E-044 Zmieniono grupę
- E-045 Nie zmieniono grupy
- E-046 Wygenerowano grupy startowe (automatycznie)
- E-047 Nie udało się wygenerować grup startowych
- E-048 Przypisano nieprzydzielonych zawodników (auto)
- E-049 Dołączono do grupy (po terminie)
- E-050 Złożono wniosek o zmianę dystansu (przed gen. grup)
- E-051 Złożono wniosek o zmianę dystansu (po gen. grup)

## BC-5: Przygotowanie do startu
- E-060 Zweryfikowano tożsamość zawodnika
- E-061 Podpisano oświadczenie zawodnika
- E-062 Wydano pakiet startowy
- E-063 Zweryfikowano zawodnika przed startem (lista startowa)
- E-064 Zdyskwalifikowano zawodnika (brak weryfikacji)
- E-065 Zdyskwalifikowano zawodnika (brak oświadczenia)
- E-066 Przypisano numer startowy (dyskietka)
- E-067 Zgubiono dyskietkę z numerem startowym
- E-068 Przejęto kaucję za nową dyskietkę
- E-069 Odpięto numer startowy
- E-070 Wydano nową dyskietkę

## BC-6: Start zawodników
- E-080 Zamknięto zapisy na dany dystans
- E-081 Wystartowano grupę
- E-082 Zarejestrowano czas startu grupy
- E-083 Wystartowano zawodnika (indywidualnie)
- E-084 Zgłoszono indywidualny start
- E-085 Wyciągnięto zawodnika z grupy (start indywidualny)
- E-086 Zdyskwalifikowano zawodnika (przy starcie)
- E-087 Zakończono udział przedwcześnie

## BC-7: Pomiar czasu
- E-090 Zarejestrowano pomiar czasu (przyłożenie dyskietki)
- E-091 Zawodnik zakończył przejazd (deklarowany dystans)
- E-092 Podliczono czasy przejazdu
- E-093 Zakończono przejazd zawodników na danym dystansie
- E-094 Zakończono przejazdy wszystkich dystansów
- E-095 Zgłoszono prośbę o przedłużenie dystansu
- E-096 Przedłużono przejazd zawodnika
- E-097 Odnotowano dodatkowy pomiar czasu (wydłużony dystans)
- E-098 Zawodnik zakończył przejazd (wydłużony dystans)

## BC-8: Kary
- E-100 Nałożono karę za złamanie regulaminu
- E-101 Naliczono karę za pominięte punkty kontrolne
<!-- UWAGA: obszar słabo opisany — brak dalszych zdarzeń w materiałach -->

## BC-9: Zakończenie zawodów
- E-110 Przyznano nagrodę
- E-111 Zwrócono pakiet startowy
- E-112 Kaucja zwrócona
- E-113 Otrzymano dyplom
- E-114 Upłynął czas zwrotu pakietu (timer)
- E-115 Nie zwrócono pakietu startowego
- E-116 Kaucja zatrzymana

## Brakujące zdarzenia — sugestie agenta (suggested — not in source materials)
- E-S01 Wygenerowano ranking / wyniki zawodów [suggested]
- E-S02 Opublikowano wyniki [suggested]
- E-S03 Wysłano powiadomienie do uczestnika o przypisaniu do grupy [suggested]
- E-S04 Zawodnik nie stawił się na start [suggested]
- E-S05 Anulowano rejestrację uczestnika [suggested]
