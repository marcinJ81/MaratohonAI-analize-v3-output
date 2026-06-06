---
generated: 2026-06-07 (aktualna sesja)
orchestrator_version: V3.1
tests_compared: test-1, test-2
---

# Raport porównawczy — Deep Analysis Orchestrator V3.1

## Podsumowanie

Oba testy operowały na tym samym zbiorze materiałów wejściowych (18 plików ES/DS) i tym samym trybie pracy (Tryb 1 — Agentowy) przy identycznej złożoności systemu (średnia). Testy różnią się zasadniczo pod względem kompletności: test-1 zatrzymał się po fazie coverage-assessment bez wygenerowania żadnego phase output, natomiast test-2 przeszedł pełen pipeline do końca Process Level (Big Picture + Process Level completed). W obu testach brakuje pliku `session-log.md`, co uniemożliwia pełną weryfikację sekwencji kroków. Pokrycie BP wzrosło z 78% (test-1) do 85% (test-2), natomiast DL spadło z 50% do 15% — najprawdopodobniej wskutek inaczej sklasyfikowanych materiałów (test-1: `clean`, test-2: `lossy`).

Podobieństwo ogólne: **niskie**

---

## Profile testów

### test-1

| Parametr | Wartość |
|---|---|
| Tryb pracy | 1 (Agentowy) |
| Złożoność | średnia |
| Fazy zakończone | — (żadna) |
| Fazy pominięte | Big Picture, Process Level, Design Level, Specialist |
| Ostatnia aktywna faza | coverage-assessment (poza pipeline faz) |
| Zakończenie sesji | paused / unknown |
| Pliki wygenerowane | — (brak phase-N-output.md) |
| Pokrycie BP/PL/DL | 78% / 75% / 50% |
| Log dostępny | nie (MISSING) |

### test-2

| Parametr | Wartość |
|---|---|
| Tryb pracy | 1 (Agentowy) |
| Złożoność | średnia |
| Fazy zakończone | Big Picture, Process Level |
| Fazy pominięte | Design Level (poza zakresem), Specialist (poza zakresem) |
| Ostatnia aktywna faza | Process Level |
| Zakończenie sesji | completed |
| Pliki wygenerowane | state/phase-1-output.md, state/phase-2-output.md |
| Pokrycie BP/PL/DL | 85% / 65% / 15% |
| Log dostępny | nie (MISSING) |

---

## Porównanie sekwencji kroków

### Oczekiwana sekwencja
```
1. session-init
2. material_input scan
3. normalizacja materiałów (jeśli dostarczone)
4. coverage-assessment
5. zasilanie wsteczne (jeśli potrzebne)
6. data-prep seed
7. phase-1 big-picture
8. phase-2 process-level (jeśli wybrany)
9. phase-3 design-level (jeśli wybrany)
10. session close
```

### Wykonana sekwencja per test

| Krok | test-1 | test-2 |
|---|---|---|
| session-init | ✓ (session.md wygenerowany) | ✓ (session.md wygenerowany) |
| material_input scan | ✓ (18 plików w index.md) | ✓ (18 plików w index.md) |
| normalizacja | ✓ clean (type: Event Storming / Domain Story) | ✓ lossy (typ: ES screenshot) |
| coverage-assessment | ✓ (BP 78%, PL 75%, DL 50%) | ✓ (BP 85%, PL 65%, DL 15%) |
| zasilanie wsteczne | UNKNOWN (log MISSING) | UNKNOWN (log MISSING) |
| data-prep seed | ✓ (phase-1-seed.md wzmiankowany w session.md) | ✓ (phase-1-seed.md wzmiankowany) |
| phase-1 | ✗ (plik MISSING, status: not-started) | ✓ (completed, status: completed) |
| phase-2 | ✗ (plik MISSING, status: not-started) | ✓ (completed, status: completed) |
| phase-3 | ✗ (poza zakresem) | ✗ (poza zakresem — deklaracja) |
| session close | ✗ (sesja zawieszona) | ✓ (historia handoffów finalna) |

### Anomalie w sekwencji

- **test-1**: Sesja zatrzymała się po coverage-assessment — seedy zostały przygotowane (`phase-1-seed.md`, `phase-2-seed.md` wzmiankowane w session.md), jednak żadna faza analityczna nie została uruchomiona. Brak session-log uniemożliwia określenie przyczyny zatrzymania. Sesja pozostaje w stanie `not-started` dla wszystkich faz.
- **test-2**: Orkiestrator wykonał pełen pipeline do poziomu Process Level. Anomalia: tryb pracy w sekcji nagłówkowej session.md opisany jako `(do wyboru przez użytkownika)` — dopiero w sekcji `Decyzje użytkownika` widnieje `Tryb 1 — Agentowy`, co sugeruje późniejsze uzupełnienie pola, nie initialization-time assignment.
- **Oba testy**: Brak `session-log.md` — ocena sekwencji kroków jest niepełna (oparta wyłącznie na plikach stanu i historii handoffów).

---

## Porównanie wygenerowanych plików

| Plik | test-1 | test-2 |
|---|---|---|
| state/session.md | ✓ | ✓ |
| state/session-log.md | ✗ | ✗ |
| state/phase-1-output.md | ✗ | ✓ |
| state/phase-2-output.md | ✗ | ✓ |
| state/phase-3-output.md | ✗ | ✗ |
| state/inputs/processed/index.md | ✓ | ✓ |

Pliki obecne tylko w wybranych testach:
- `state/phase-1-output.md` — tylko w test-2: Big Picture zostało uruchomione i ukończone
- `state/phase-2-output.md` — tylko w test-2: Process Level zostało uruchomione i ukończone
- `state/session-log.md` — MISSING w obu testach: log akcji orkiestratora nie był generowany w żadnym przebiegu

---

## Porównanie zakończenia sesji

| Test | Ostatnia faza | Status zakończenia | Otwarte pytania przeniesione |
|---|---|---|---|
| test-1 | coverage-assessment | paused / unknown | 2 (szczegóły regulaminu, kontekst Kary) |
| test-2 | Process Level | completed | 3 (Wyniki/Ranking, kary a wyniki, wyniki na żywo) |

Test-1 zatrzymał się przed uruchomieniem jakiejkolwiek fazy analitycznej — seed był gotowy, ale fazy pozostały `not-started`. Test-2 przeszedł pełny zakres (BP + PL) i zamknął się poprawnie, przenosząc otwarte pytania dotyczące obszarów nieobecnych w materiałach ES.

---

## Wnioski i obserwacje

- **Kompletność pipeline silnie różnicuje testy**: test-1 wykonał tylko preprocessing (material scan + coverage-assessment), test-2 przeszedł cały zadeklarowany zakres. Orkiestrator w obu przypadkach poprawnie zbudował seed, ale tylko w test-2 sesja była kontynuowana do faz analitycznych.
- **Brak session-log.md w obu testach** jest istotną luką — bez pliku logu nie można zweryfikować kolejności kroków ani wykryć luk czasowych. Ocena sekwencji opiera się wyłącznie na plikach stanu i historii handoffów w session.md.
- **Różnica w klasyfikacji konwersji materiałów**: test-1 oznaczył materiały jako `clean`, test-2 jako `lossy`. Przy identycznym zestawie wejść to wskazuje na zmianę logiki coverage-assessment między przebiegami lub różne decyzje agenta. Przekłada się to na różne % pokrycia (szczególnie DL: 50% vs 15%).
- **Tryb pracy (Tryb 1) był spójny** w obu testach, ale w test-2 pole trybu w nagłówku session.md nie zostało wypełnione przy inicjalizacji sesji — orkiestrator uzupełnił je dopiero w sekcji decyzji.
- **Liczba otwartych pytań wzrosła** z 2 (test-1, przed analizą) do 3 (test-2, po analizie) — co jest oczekiwanym zachowaniem: analiza domeny generuje nowe pytania, nie zamyka wszystkich istniejących.
