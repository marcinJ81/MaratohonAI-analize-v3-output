# Polityki (Event → Policy → Command) — working file

| ID | Zdarzenie wyzwalające | Polityka / Reguła | Akcja / Komenda | BC |
|---|---|---|---|---|
| P-001 | E-014 Opłacono uczestnictwo | Przypisz uczestnikowi numer startowy | Wygeneruj numer startowy | BC-04 |
| P-002 | E-008 Zmieniono dystans + dystans nieopłacony | Uczestnik pozostaje na opłaconym dystansie | Odrzuć zmianę dystansu (bez opłaty) | BC-02, BC-03 |
| P-003 | E-022 Wygenerowano grupy + nowy opłacony uczestnik | Jeśli nie przekroczono terminu dołączenia | Dołącz do istniejącej grupy | BC-04 |
| P-004 | E-051 Zarejestrowano pomiar czasu (zawodnik na mecie) | Po każdym zakończonym przejeździe | Zaktualizuj wyniki na żywo | BC-08 |
| P-005 | E-060 Nałożono karę / E-061 Naliczono karę | Kara regulaminowa = zmiana pozycji | Przenieś na koniec listy LUB wyklucz z wyników | BC-08, BC-09 |
| P-006 | E-055 Zakończono przejazdy wszystkich dystansów | Zawody zakończone | Opublikuj wyniki końcowe | BC-08 |
| P-007 | E-055 Zakończono przejazdy wszystkich dystansów | Zawody zakończone | Uruchom timer zwrotu pakietów | BC-10 |
| P-008 | E-070 Upłynął czas zwrotu pakietu | Pakiet nie zwrócony w terminie | Zatrzymaj kaucję | BC-10 |
| P-009 | E-049 Zgłoszono indywidualny start | Start poza grupą | Zarejestruj indywidualny czas startu | BC-06 |
| P-010 | E-038 Zgubiono dyskietkę | Zawodnik bez identyfikatora | Przejmij kaucję, wydaj nową dyskietkę | BC-05 |
