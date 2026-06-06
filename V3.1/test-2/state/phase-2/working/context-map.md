# Context Map — zależności między BC — working file

| ID | Z (upstream) | Do (downstream) | Co jest przekazywane | Typ relacji | Kierunek |
|---|---|---|---|---|---|
| D-001 | BC-01 Konfiguracja | BC-02 Rejestracja | Terminy rejestracji, dostępne dystanse | upstream-downstream | async (config) |
| D-002 | BC-01 Konfiguracja | BC-04 Grupy startowe | Terminy generacji, term. zmiany, min. liczba uczestników | upstream-downstream | async (config) |
| D-003 | BC-01 Konfiguracja | BC-07 Pomiar czasu | Limity czasu per dystans | upstream-downstream | async (config) |
| D-004 | BC-01 Konfiguracja | BC-09 Kary | Typy kar i ich konsekwencje | upstream-downstream | async (config) |
| D-005 | BC-02 Rejestracja | BC-03 Opłata | Kwota opłaty per dystans | upstream-downstream | synchroniczny |
| D-006 | BC-03 Opłata | BC-02 Rejestracja | Potwierdzenie opłacenia dystansu | upstream-downstream | async (event) |
| D-007 | BC-03 Opłata | BC-04 Grupy startowe | Informacja o opłaconym dystansie uczestnika | upstream-downstream | async (event) |
| D-008 | BC-02 Rejestracja | BC-04 Grupy startowe | Uczestnik z przypisanym numerem startowym | upstream-downstream | async (event) |
| D-009 | BC-04 Grupy startowe | BC-06 Start zawodników | Wygenerowane grupy startowe per dystans | upstream-downstream | async (event) |
| D-010 | BC-05 Przygot. do startu | BC-06 Start zawodników | Lista zweryfikowanych zawodników | upstream-downstream | synchroniczny |
| D-011 | BC-06 Start zawodników | BC-07 Pomiar czasu | Czas startu grupy / indywidualny czas startu | upstream-downstream | synchroniczny |
| D-012 | BC-07 Pomiar czasu | BC-08 Wyniki i ranking | Czasy przejazdów (per zawodnik, per pomiar) | upstream-downstream | async (event) |
| D-013 | BC-09 Kary | BC-08 Wyniki i ranking | Kara per zawodnik (typ: koniec listy / wykluczenie) | upstream-downstream | async (event) |
| D-014 | BC-08 Wyniki i ranking | I-002 Strona WWW | Wyniki na żywo i końcowe | OHS (Open Host Service) | async (push) |
| D-015 | BC-08 Wyniki i ranking | I-003 Tablica wyników | Wyniki na żywo | OHS | async (push) |
| D-016 | BC-07 Pomiar czasu | BC-10 Zakończenie | Zakończono przejazdy wszystkich dystansów | upstream-downstream | async (event) |
| D-017 | I-001 Interfejs banku | BC-03 Opłata | Rejestracja przelewu | ACL (Anti-Corruption Layer) | async |
