# Session: System zarządzania maratonem rowerowym 24h

Data startu: 2026-06-07
Tryb: Tryb 1 — Agentowy (Claude orkiestruje)
Złożoność systemu: średnia

## Cel analizy

Uzupełnienie procesu — pogłębiona analiza domeny systemu do zarządzania 24-godzinnym maratonem rowerowym, obejmująca rejestrację zawodników, grupy startowe, pomiar czasu i generowanie wyników. Zakres: Big Picture + Process Level.

## Fazy

| Faza | Status | Plik outputu | Seed |
|---|---|---|---|
| Big Picture | completed | state/phase-1-output.md | state/inputs/processed/phase-1-seed.md |
| Process Level | completed | state/phase-2-output.md | state/inputs/processed/phase-2-seed.md |
| Design Level | not-started | state/phase-3-output.md | — |
| Specialist | not-started | state/phase-s-output.md | — |

## Materiały wejściowe
<!-- wypełniane przez coverage-assessment — nie edytuj ręcznie -->
<!-- źródło: state/inputs/processed/index.md -->
| Plik | Typ | Konwersja | Pokrycie BP | Pokrycie PL | Pokrycie DL |
|---|---|---|---|---|---|
| es-processed.md | Event Storming (18 screenshotów) | lossy | 72% | 68% | 40% |

## Otwarte pytania (przeniesione między fazami)
- Q-001: Jak dokładnie działa generowanie rankingów / wyników po zakończeniu zawodów?
- Q-002: Jakie kary istnieją (typy, wartości) i jak wpływają na wyniki?
- Q-003: Co dzieje się z zawodnikami z niedopłatą gdy startuje generacja grup? (HS-01)
- Q-004: Czy ranking uwzględnia kary (czas karny) czy dyskwalifikację?
- Q-007: Czy zawodnik może jechać wiele dystansów czy tylko jeden?
- Q-008: Jak wyglądają punkty kontrolne — czy brak przejazdu przez punkt = kara?

## Decyzje użytkownika
- Tryb pracy: 1 — Agentowy
- Zakres: Big Picture + Process Level
- Core domain: BC-04 Grupy startowe (BC-07 Pomiar czasu → Generic)

## Historia handoffów
- 2026-06-07 coverage-assessment → completed, BP: 72%, PL: 68%, DL: 40%
- 2026-06-07 Faza 1 Big Picture → completed, 9 BC, 52 zdarzeń, 6 hot spotów
- 2026-06-07 Faza 2 Process Level → completed, 9 BC (Core: BC-04), 16 polityk, 10 zależności
