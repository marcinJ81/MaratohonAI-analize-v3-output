# Polityki (Event → Policy → Command)

| ID | Gdy zdarzenie | Reguła / warunek | Akcja / komenda | BC |
|---|---|---|---|---|
| POL-01 | E-021 Zarejestrowano przelew | kwota = wymagana opłata | Opłacono uczestnictwo (E-023) | BC-03 |
| POL-02 | E-021 Zarejestrowano przelew | kwota > wymagana opłata | Wylicz zwrot (E-025) → Zwróć nadpłatę (E-026) → Opłacono (E-023) | BC-03 |
| POL-03 | E-021 Zarejestrowano przelew | kwota < wymagana opłata | Zarejestruj niedopłatę → czekaj na E-027 | BC-03 |
| POL-04 | E-024 Zaaplikowano promocję -100% | zawsze | Opłacono uczestnictwo (E-023) bez przelewu | BC-03 |
| POL-05 | Timer osiąga termin generacji grup | min. X uczestników z numerem + termin OK | Wygenerowano grupy startowe (E-046) | BC-04 |
| POL-06 | E-046 Wygenerowano grupy | są zawodnicy bez przydziału | Przypisano nieprzydzielonych auto (E-048) | BC-04 |
| POL-07 | E-016 Zmieniono dystans (po gen. grup) | zawsze | Złóż wniosek o zmianę grupy do BC-04 | BC-02 → BC-04 |
| POL-08 | Timer osiąga termin zmiany grup | zawsze | Przydział grup jest ostateczny (lock) | BC-04 |
| POL-09 | E-090 Zarejestrowano pomiar czasu | zawodnik ukończył wymagany dystans | Zawodnik zakończył przejazd (E-091) | BC-07 |
| POL-10 | E-090 Zarejestrowano pomiar czasu | punkt kontrolny pominięty wcześniej przez zawodnika | Nalicz karę (E-101) → Zdyskwalifikuj (E-103) | BC-07 → BC-08 |
| POL-11 | E-100 / E-101 Nałożono karę | zawsze | Zdyskwalifikowano zawodnika (E-103) | BC-08 |
| POL-12 | E-093 Zakończono przejazd na dystansie | wszystkie dystanse zakończone | Zakończono przejazdy wszystkich dystansów (E-094) | BC-07 |
| POL-13 | E-094 Zakończono przejazdy wszystkich dystansów | zawsze | Generuj ranking (E-102) | BC-07 → BC-09 |
| POL-14 | Timer osiąga czas zwrotu pakietu | pakiet nie zwrócony | Zatrzymaj kaucję (E-116) | BC-09 |
| POL-15 | E-060 Zweryfikowano tożsamość | weryfikacja negatywna | Zdyskwalifikowano zawodnika (E-064) | BC-05 |
| POL-16 | E-061 Podpisano oświadczenie | brak podpisu | Zdyskwalifikowano zawodnika (E-065) | BC-05 |
