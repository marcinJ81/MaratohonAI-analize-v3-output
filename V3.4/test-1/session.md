# Session: System Zarządzania 24h Maratonem Rowerowym
Data startu: 2026-06-14
Tryb: Tryb 2 (Ekspercki)
Złożoność systemu: wysoka

## Cel analizy
Audyt Event Stormingu — weryfikacja kompletności, identyfikacja brakujących zdarzeń i niespójności. Fazy: Big Picture + Process Level.

## Fazy
| Faza | Status | Plik outputu | Seed |
|---|---|---|---|
| Big Picture | completed | state/phase-1-output.md | state/inputs/processed/phase-1-seed.md |
| Process Level | completed | state/phase-2-output.md | state/inputs/processed/phase-2-seed.md |
| Design Level | — (poza zakresem) | — | — |
| Specialist | — (nie uruchomiony) | — | — |

**Status sesji: ZAKOŃCZONA 2026-06-14**

## Materiały wejściowe
<!-- kopia tabeli pokrycia z state/inputs/processed/index.md -->
Big Picture: 83% (wystarczające) | Process Level: 65% (częściowe) | 15 plików lossy

## Otwarte pytania (przeniesione między fazami)
- Q-001 RESOLVED: Generowanie grup przy niedopłacie — uczestnik zostaje na starym dystansie. Zmiana następuje dopiero po otrzymaniu potwierdzenia płatności.
- Q-002 RESOLVED: Zamknięcie pomiaru czasu — trigger: czas zawodów (24h). Zawodnicy na trasie po upływie czasu "jadą dla siebie" (wyniki nieklasyfikowane). Szczególny przypadek: zawodnik który ukończył opłacony dystans i spróbował dłuższego — liczy się na dystansie opłaconym. Zawodnik który nie ukończył opłaconego dystansu → status "nieklasyfikowany".

## Decyzje użytkownika
- Tryb pracy: 2 (Ekspercki)
- Tryb analizy: audit
- Akceptacja wsadu (B2): 2026-06-14 — pokrycie BP=83%, PL=65%, lossy: 15 plików — ZAAKCEPTOWANE
- Potwierdzone fakty domenowe:
  - "Real-time wyświetlanie = zdarzenia przejazdu przez checkpoint (zarejstrowano czas przejazdu). Dystanse: 50, 100, 150, 200, 300, 400, 500, 600, 650, 700 km. 1 pętla = 100 km. Checkpoint na każdej pętli (50 km — o jeden mniej). Wyświetlanie: biuro zawodów (ekran) + strona WWW." — potwierdził użytkownik 2026-06-14
  - "Kary/dyskwalifikacja poza zakresem systemu — decyzja sędziego, nie systemu. Plik kary.png nie jest zakresem." — potwierdził użytkownik 2026-06-14
  - "Pliki zakończenie_2.png i zakończenie_zawodów.png — duplikaty (ten sam proces, różny zoom)." — potwierdził użytkownik 2026-06-14
  - "Pliki zmiania_grupy_startowej_DL.png i zminia_grupystartowej.png — duplikaty." — potwierdził użytkownik 2026-06-14
  - "Reguła niedopłaty przy zmianie grupy: brak zapłaty różnicy = brak zmiany, zawodnik pozostaje w oryginalnej grupie." — potwierdził użytkownik 2026-06-14
  - "Integracje zewnętrzne (kompletna lista): bank (przelew) + RFID (czytnik). Dyplomy wewnętrznie. Brak zewnętrznej platformy rejestracyjnej." — potwierdził użytkownik 2026-06-14
  - "Strona WWW wyników real-time = ta sama baza co system, osobny widok (nie osobna aplikacja, nie osobna integracja)." — potwierdził użytkownik 2026-06-14
- Bleed-through Fazy 1: read modele NIE, zasady biznesowe NIE
- Bleed-through Fazy 2: wstępne reguły biznesowe TAK
- Core domain: SD-005 / BC-005 Generacja Grup Startowych

## Historia handoffów
- 2026-06-14 data-prep/prepare → completed-with-warnings, złożoność=wysoka, 18 plików lossy
- 2026-06-14 coverage-assessment → completed, BP=83%, PL=65%, brak blokerów
- 2026-06-14 data-prep/seed → completed, phase-1-seed.md + phase-2-seed.md
- 2026-06-14 process-level → completed, 10 BC, core=SD-005, 19 polityk, 30 nowych HS, 1 bloker
