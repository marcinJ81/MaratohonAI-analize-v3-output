---
generated: 2026-06-07
orchestrator_version: V3.1
tests_compared: [test-1, test-2, test-3]
---

# Raport porównawczy — Deep Analysis Orchestrator V3.1

## Podsumowanie

Wszystkie trzy testy dotyczyły tej samej domeny (24h maraton rowerowy) i używały tego samego trybu pracy (Tryb 1 Agentowy). test-1 przerwał działanie po fazie przygotowawczej (data-prep/seed), zanim uruchomiono jakąkolwiek fazę analityczną. Testy test-2 i test-3 osiągnęły ten sam zakres (Big Picture + Process Level), lecz różniły się strategią normalizacji materiałów, sposobem logowania i statusem plików wyjściowych. Główne odchylenia: brak faz w test-1, anomalia lokalizacji logu w test-3 oraz różne formaty wpisów w logach.

Podobieństwo ogólne: **średnie**

---

## Profile testów

### test-1

| Parametr | Wartość |
|---|---|
| Tryb pracy | 1 (Agentowy) |
| Złożoność | średnia |
| Fazy zakończone | — (żadna) |
| Fazy pominięte | Big Picture, Process Level, Design Level, Specialist |
| Ostatnia aktywna faza | brak (sesja zakończona po data-prep/seed) |
| Zakończenie sesji | paused (przez użytkownika, po phase-1-seed.md) |
| Pliki wygenerowane | state/orchestrator-log.md, state/session.md, state/inputs/processed/index.md, state/inputs/processed/phase-1-seed.md |
| Pokrycie BP/PL/DL | 78% / 75% / 50% |
| Log dostępny | tak (state/orchestrator-log.md) |

### test-2

| Parametr | Wartość |
|---|---|
| Tryb pracy | 1 (Agentowy) |
| Złożoność | średnia |
| Fazy zakończone | Big Picture, Process Level |
| Fazy pominięte | Design Level (poza zakresem), Specialist (poza zakresem) |
| Ostatnia aktywna faza | Process Level |
| Zakończenie sesji | completed |
| Pliki wygenerowane | state/orchestrator-log.md, state/session.md, state/inputs/processed/index.md, state/inputs/processed/phase-1-seed.md, state/phase-1-output.md, state/phase-2-output.md, state/output/mermaid/*.md (3 pliki) |
| Pokrycie BP/PL/DL | 85% / 65% / 15% |
| Log dostępny | tak (state/orchestrator-log.md) |

### test-3

| Parametr | Wartość |
|---|---|
| Tryb pracy | 1 — Agentowy |
| Złożoność | średnia |
| Fazy zakończone | Big Picture, Process Level |
| Fazy pominięte | Design Level (not-started), Specialist (not-started) |
| Ostatnia aktywna faza | Process Level |
| Zakończenie sesji | completed |
| Pliki wygenerowane | orchestrator-log.md (**poza state/** — anomalia), state/session.md, state/inputs/processed/index.md, state/inputs/processed/phase-1-seed.md, state/inputs/processed/phase-2-seed.md, state/phase-1-output.md, state/phase-2-output.md |
| Pokrycie BP/PL/DL | 72% / 68% / 40% |
| Log dostępny | tak (orchestrator-log.md — **poza state/**) |

---

## Porównanie sekwencji kroków

### Oczekiwana sekwencja
```
1.  session-init
2.  material_input scan
3.  normalizacja materiałów (jeśli dostarczone)
4.  coverage-assessment
5.  zasilanie wsteczne (jeśli potrzebne)
6.  data-prep seed
7.  phase-1 big-picture
8.  phase-2 process-level (jeśli wybrany)
9.  phase-3 design-level (jeśli wybrany)
10. session close
```

### Wykonana sekwencja per test

| Krok | test-1 | test-2 | test-3 |
|---|---|---|---|
| session-init | ✓ | ✓ | ✓ |
| material_input scan | ✓ (18 PNG) | ✓ (18 screenshotów, Q4) | ✓ (18 obrazów) |
| normalizacja | ✓ (9 plików MD) | ✓ (18 → 7 plików processed) | ✓ (18 → 1 plik es-processed.md) |
| coverage-assessment | ✓ | ✓ | ✓ |
| zasilanie wsteczne | ✗ (nie potrzebne) | ✗ (nie potrzebne) | ✗ (nie potrzebne) |
| data-prep seed | ✓ (phase-1-seed.md) | ✓ (phase-1-seed.md, krok-E) | ✓ (phase-1-seed.md + phase-2-seed.md) |
| phase-1 big-picture | ✗ (sesja przerwana) | ✓ | ✓ |
| phase-2 process-level | ✗ | ✓ | ✓ |
| phase-3 design-level | ✗ | ✗ (poza zakresem) | ✗ (poza zakresem) |
| session close | ✓ (przez użytkownika) | ✓ | ✓ |

### Anomalie w sekwencji

- **test-1**: sesja zamknięta przez użytkownika po data-prep/seed, bez uruchomienia żadnej fazy analitycznej. Logi nie zawierają timestampów (wszystkie wpisy oznaczone jako „start").
- **test-2**: po wpisie „sesja zamknięta przez użytkownika" wykonano dodatkowe kroki (generowanie 3 diagramów mermaid). Aktywność post-session close — brak w oczekiwanej sekwencji.
- **test-3**: log orkiestratora zapisany w `test-3/orchestrator-log.md` zamiast w `test-3/state/orchestrator-log.md`. Narusza oczekiwaną strukturę katalogów. Jako jedyny test posiada `phase-2-seed.md` — seed dla Process Level.
- **wszystkie testy**: brak kroku „zasilanie wsteczne" (krok D) — poprawnie, ponieważ pokrycie materiałów było wystarczające do startu faz.

---

## Porównanie wygenerowanych plików

| Plik | test-1 | test-2 | test-3 |
|---|---|---|---|
| state/session.md | ✓ | ✓ | ✓ |
| state/orchestrator-log.md | ✓ | ✓ | ✗ (plik istnieje poza state/) |
| state/inputs/processed/index.md | ✓ | ✓ | ✓ |
| state/inputs/processed/phase-1-seed.md | ✓ | ✓ | ✓ |
| state/inputs/processed/phase-2-seed.md | ✗ | ✗ | ✓ |
| state/phase-1-output.md | ✗ | ✓ (status: completed, coverage: 92%) | ✓ (status: partial, coverage: 72%) |
| state/phase-2-output.md | ✗ | ✓ (status: completed, coverage: 88%) | ✓ (status: partial, coverage: 68%) |
| state/phase-3-output.md | ✗ | ✗ | ✗ |
| state/phase-1/working/*.md | ✗ | ✓ (4 pliki) | ✓ (4 pliki) |
| state/phase-2/working/*.md | ✗ | ✓ (4 pliki + 10 procesów) | ✓ (4 pliki + 9 procesów) |
| state/output/mermaid/*.md | ✗ | ✓ (3 pliki) | ✗ |

**Pliki obecne tylko w wybranych testach:**

- `state/inputs/processed/phase-2-seed.md` — tylko w test-3; możliwa przyczyna: orkiestrator w test-3 generował seed specyficznie dla PL, co nie było robione w test-1 i test-2
- `state/output/mermaid/*.md` (3 pliki) — tylko w test-2; możliwa przyczyna: diagram generation był aktywnością post-session, nie wchodził w standardowy pipeline
- `state/phase-1/working/*.md` i `state/phase-2/working/*.md` — obecne w test-2 i test-3; brak w test-1 (fazy nie były uruchomione)
- `orchestrator-log.md` w root katalogu testu (nie w state/) — tylko w test-3; anomalia lokalizacji

---

## Porównanie zakończenia sesji

| Test | Ostatnia faza | Status zakończenia | Otwarte pytania przeniesione |
|---|---|---|---|
| test-1 | brak (pre-faza) | paused | 2 |
| test-2 | Process Level | completed | 3 |
| test-3 | Process Level | completed | 6 |

Test-1 był jedynym testem z przerwaniem przed fazami analitycznymi — sesja zakończona przez użytkownika bezpośrednio po phase-1-seed.md. Testy test-2 i test-3 zakończyły się w tym samym punkcie (po Process Level), lecz różniły się liczbą otwartych pytań: test-3 wygenerował dwukrotnie więcej nierozwiązanych kwestii (6 vs 3).

---

## Wnioski i obserwacje

- **Tryb i złożoność są stabilne** — we wszystkich trzech testach orkiestrator poprawnie ocenił złożoność jako średnią i wybrał Tryb 1 (Agentowy). Parametry sesji są spójne niezależnie od daty i przebiegu testu.

- **Strategia normalizacji różni się między testami** — test-1 i test-2 generowały wiele plików processed (9 i 7 odpowiednio), test-3 skonsolidował 18 screenshotów do jednego pliku `es-processed.md`. Różnica nie wpłynęła na kompletność faz, ale zmienia strukturę katalogu processed i może wpływać na granularność kontekstu dla specjalistów.

- **Lokalizacja logu jest niespójna w test-3** — `orchestrator-log.md` zapisany w root katalogu testu zamiast w `state/`. Jest to jedyna strukturalna anomalia między testami; wskazuje na brak wymuszenia lokalizacji pliku przez orkiestrator.

- **test-2 wykonał aktywność post-session** — po wpisie o zamknięciu sesji w logu pojawiają się kroki generowania diagramów mermaid. Testy 1 i 3 nie miały tej aktywności. Wskazuje to na niespójne zarządzanie momentem „session close" w pipeline.

- **Statusy plików wyjściowych różnią się mimo zbliżonego zakresu** — test-2 oznaczył phase-1-output i phase-2-output jako `completed`, test-3 jako `partial`. Różnica wynika z metody oceny pokrycia: test-3 zastosował korektę `-20%` za jakość lossy screenshotów (z 90% do 72% dla BP), test-2 tego nie robił mimo identycznego zestawu materiałów.

- **Format logów jest niejednolity** — test-1 nie zawiera timestampów (tylko etykieta „start"), test-2 używa etykiet relatywnych (Q1, krok-A, krok-B), test-3 używa pełnych znaczników czasowych HH:MM:SS. Brak standaryzacji formatu logu utrudnia mechaniczne porównanie czasu trwania kroków między sesjami.
