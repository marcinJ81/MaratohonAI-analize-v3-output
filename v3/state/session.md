# Session: maraton-rowerowy
Data startu: 2026-06-01
Tryb: Tryb 2 — Ekspercki (użytkownik jako ekspert domenowy)
Złożoność systemu: średnia-wysoka

## Cel analizy
Przegląd procesu obsługi maratonu rowerowego oraz uzupełnienie brakujących części systemu. Użytkownik chce dojść do Process Level.

## Fazy
| Faza | Status | Plik outputu | Seed |
|---|---|---|---|
| Big Picture | completed | state/phase-1-output.md | state/inputs/processed/all-materials-processed.md |
| Process Level | in-progress | state/phase-2-output.md | state/inputs/processed/phase-2-seed.md |
| Design Level | not-started | state/phase-3-output.md | — |
| Specialist | not-started | state/phase-s-output.md | — |

## Materiały wejściowe
| Plik | Typ | Konwersja | Pokrycie BP | Pokrycie PL | Pokrycie DL |
|---|---|---|---|---|---|
| 18 × PNG (Event Storming) | obraz | lossy | 80% | 60% | 15% |

Szczegóły: state/inputs/processed/index.md

## Otwarte pytania (przeniesione z Fazy 1)
- Q-001: Jak obsługiwany jest konflikt: generowanie list a nieopłacona niedopłata?
- Q-003: Czy Wyniki/Ranking to osobny BC? Jak są obliczane?
- Q-004: Kto i kiedy weryfikuje kary — czy zawodnik może się odwołać?
- Q-006: Kto i na jakiej podstawie przyznaje nagrody?

## Decyzje użytkownika
- Tryb pracy: 2 (Ekspercki)
- Cel: Process Level (przegląd + uzupełnienie)
- Core domain: do ustalenia w Fazie 2

## Historia handoffów
- 2026-06-01 Faza 1 → completed (derived z materiałów), suggested_next: process-level
- 2026-06-01 Faza 2 → in-progress
