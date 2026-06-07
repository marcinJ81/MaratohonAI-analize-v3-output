# Seed: Phase 2 — Process Level
<!-- generated-by: orchestrator, session: maraton-rowerowy-24h -->
<!-- coverage: 68% -->
<!-- sources: state/phase-1-output.md, state/inputs/processed/es-processed.md -->

## Kontekst sesji
System: zarządzanie 24-godzinnym maratonem rowerowym
Tryb: Tryb 1 — Agentowy
Złożoność: średnia

## Wyniki Big Picture
- 9 BC zidentyfikowanych
- 54 zdarzenia domenowe (52 + 2 nowe potwierdzone)
- Core Domain: TBD
- 6 hot spotów (3 blokery uzupełnione)

## Uzupełnienia blokerów z Big Picture
- Kary: jedyna kara to dyskwalifikacja (nie ma kar czasowych)
- Wyniki: system automatycznie generuje ranking na podstawie czasów z punktów pomiaru + czas ukończenia dystansu (linia mety); rywalizacja w obrębie dystansu
- Niedopłata: blokuje wyłącznie zmianę dystansu; uczestnik z niedopłatą pozostaje na pierwotnym dystansie i może startować

## Otwarte pytania z Big Picture do zbadania
- Q-004: Czy zawodnik może startować na wielu dystansach jednocześnie?
- Q-005: Ile punktów kontrolnych, czy każdy obowiązkowy?
- Q-006: Czy istnieje mechanizm powiadomień email/SMS?
- Q-007: Czy anulowanie rejestracji jest możliwe?
- Q-008: DNS (Did Not Start) — czy istnieje ten status?
