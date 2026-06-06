# Hot Spoty — working file

| ID | Opis | Powód | Powiązane zdarzenia | Priorytet |
|---|---|---|---|---|
| H-001 | Niedopłata vs generowanie list startowych | Niejasność: co jeśli generowanie list startuje a uczestnik ma nieopłaconą niedopłatę? | E-016, E-017, E-022 | wysoki (blocker) |
| H-002 | Start indywidualny — rejestracja czasu | Niejasność: zawodnik startuje indywidualnie (np. spóźnienie) — jak system rejestruje czas startu inny niż grupy? | E-049, E-050, E-045 | wysoki |
| H-003 | Brak kontekstu Wyniki/Ranking/Publikacja | Luka: użytkownik wymienił "publikację wyników" jako cel systemu, ale ES nie zawiera tego obszaru | E-055 | wysoki (blocker) |
| H-004 | ~~Kary a wyniki końcowe~~ — WYJAŚNIONY | Kara = przeniesienie na koniec listy LUB wykluczenie z wyników. Nie jest to czas dodatkowy. | E-060, E-061, E-071, E-072 | ~~wysoki~~ → zamknięty |
| H-005 | Awaria systemu pomiaru czasu | Ryzyko: co jeśli czytnik nie zarejestruje przejazdu zawodnika (hardware failure)? Czy jest fallback? | E-051, E-052 | średni |
| H-006 | Zmiana dystansu po wygenerowaniu grup | Złożoność: zawodnik zmienia dystans po wygenerowaniu grup — zawodnik "nie przypisano do ostatecznej grupy" | E-007, E-008, E-022, E-024 | średni |
| H-007 | Zawodnik bez przypisanej grupy w dniu startu | Ryzyko: edge case — zawodnik opłacił, ale z powodu zmiany dystansu lub błędu nie ma grupy | E-024, E-022 | średni |
