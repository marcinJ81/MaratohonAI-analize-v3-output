# Session: 24h Maraton Rowerowy

Data startu: 2026-06-04
Tryb: Tryb 1 (Agentowy)
Złożoność systemu: średnia

## Cel analizy

Zrozumienie domeny systemu zarządzania 24-godzinnym maratonem rowerowym. System obejmuje rejestrację uczestników, generowanie grup startowych, pomiar czasu, obsługę startu i zakończenia zawodów. Cel: pełne zrozumienie procesów biznesowych do poziomu Process Level.

## Fazy

| Faza | Status | Plik outputu | Seed |
|---|---|---|---|
| Big Picture | not-started (seed gotowy) | state/phase-1-output.md | state/inputs/processed/phase-1-seed.md |
| Process Level | not-started | state/phase-2-output.md | state/inputs/processed/phase-2-seed.md |
| Design Level | not-started | state/phase-3-output.md | — |
| Specialist | not-started | state/phase-s-output.md | — |

## Materiały wejściowe
<!-- wypełniane przez coverage-assessment — nie edytuj ręcznie -->
<!-- źródło: state/inputs/processed/index.md -->

| Plik | Typ | Konwersja | Pokrycie BP | Pokrycie PL | Pokrycie DL |
|---|---|---|---|---|---|
| kontekst_rejestracja1.png | Event Storming | clean | tak | tak | nie |
| ogólny_widok_kontekstu_rejestracja.png | Event Storming | clean | tak | tak | nie |
| generacja grup startowych.png | Event Storming | clean | tak | tak | nie |
| generacja_grup_poterminie.png | Event Storming | clean | tak | tak | nie |
| genreacja_numeru_indywidualna.png | Event Storming | clean | tak | tak | nie |
| grupy_startowe_ds.png | Domain Story | clean | częściowo | tak | tak |
| kary.png | Event Storming | clean | tak | tak | nie |
| opłata_za_udział_1.png | Event Storming | clean | tak | tak | nie |
| opłata_za_udział_2.png | Event Storming | clean | tak | tak | nie |
| pomiar_czasu.png | Event Storming | clean | tak | tak | nie |
| przypisanie_zawodnika_do_listy.png | Event Storming | clean | tak | tak | nie |
| start_indywidualny.png | Event Storming | clean | tak | tak | nie |
| utowrzenie_grup_ds.png | Domain Story | clean | częściowo | tak | tak |
| wystartowano_zawodników.png | Event Storming | clean | tak | tak | nie |
| zakończenie_2.png | Event Storming | clean | tak | tak | nie |
| zakończenie_zawodów.png | Event Storming | clean | tak | tak | nie |
| zmiania_grupy_startowej_DL.png | Design Level | clean | nie | tak | tak |
| zminia_grupystartowej.png | Event Storming | clean | tak | tak | nie |

## Otwarte pytania (przeniesione między fazami)

- Szczegóły reguł regulaminowych (limity czasowe, kaucja, terminy)
- Kontekst Kary — słabo pokryty, może wymagać doprecyzowania

## Decyzje użytkownika

- Tryb pracy: 1 (Agentowy)
- Poziom analizy: Process Level (Big Picture + Process Level)
- Przedmiot: proces biznesowy
- Cel: zrozumienie domeny

## Historia handoffów

- 2026-06-04 coverage-assessment → completed, BP: 78%, PL: 75%, DL: 50%
