---
name: deep-analysis-test-comparator
description: >
  Skill do analizy i porównywania wyników testów orkiestratora deep-analysis.
  Czyta logi, stan sesji i wygenerowane pliki z katalogów test-N, porównuje je między sobą
  i generuje raport MD z oceną spójności, podobieństwa i kompletności przebiegów.
tools: Read, Glob, LS, Write
---

# Deep Analysis — Test Comparator

## Odpowiedzialność

Czyta wyniki testów z repozytorium output, porównuje przebiegi między sobą
i generuje jeden plik `comparison-report.md`.

Nie analizuje domeny. Nie ocenia jakości wniosków merytorycznych.
Ocenia **zachowanie orkiestratora**: kolejność kroków, kompletność, tryb, zakończenie.

---

## Struktura repozytorium wejściowego

```
/MaratohonAI-analize-v3-output/V3.1/
  skill/                    ← aktualna wersja orkiestratora i plików pomocniczych
  test-1/                   ← wyniki pierwszego uruchomienia
  test-2/
  test-N/
```

Każdy katalog `test-N` zawiera pliki wygenerowane przez orkiestrator w trakcie sesji:

```
test-N/
  state/
    session.md              ← stan sesji: fazy, tryb, decyzje, historia handoffów
    orchestrator-log.md          ← log akcji orkiestratora (data, godzina, akcja)
    phase-1-output.md       ← wynik Big Picture (jeśli wygenerowany)
    phase-2-output.md       ← wynik Process Level (jeśli wygenerowany)
    phase-3-output.md       ← wynik Design Level (jeśli wygenerowany)
    inputs/
      processed/
        index.md            ← ocena pokrycia materiałów
```

---

## Algorytm

### Krok 1 — Odkrycie katalogów testowych

Przeszukaj `/MaratohonAI-analize-v3-output/V3.1/` i znajdź wszystkie katalogi
pasujące do wzorca `test-*`. Posortuj rosnąco (`test-1`, `test-2`, …).

Jeśli nie ma żadnego katalogu testowego — zakończ z komunikatem:
`"Brak katalogów testowych w [ścieżka]"`

### Krok 2 — Odczyt danych per test

Dla każdego katalogu `test-N` odczytaj:

| Plik | Co wyciągasz |
|---|---|
| `state/session.md` | tryb pracy, złożoność systemu, cel, lista faz ze statusami, decyzje użytkownika, historia handoffów |
| `state/orchestrator-log.md` | sekwencja akcji z timestampami |
| `state/phase-1-output.md` | czy istnieje, status (completed/partial), liczba wniosków, liczba otwartych pytań |
| `state/phase-2-output.md` | j.w. |
| `state/phase-3-output.md` | j.w. |
| `state/inputs/processed/index.md` | pokrycie per faza (%) |

Jeśli plik nie istnieje — odnotuj jako `MISSING`.

### Krok 3 — Profil testu

Na podstawie odczytanych danych zbuduj profil dla każdego testu:

```
test-N:
  tryb: [1/2/3/4 lub UNKNOWN]
  złożoność: [niska/średnia/wysoka lub UNKNOWN]
  fazy_zakończone: [lista: big-picture, process-level, design-level]
  fazy_pominięte: [lista]
  zakończenie: [completed / paused / error / unknown]
  ostatnia_aktywna_faza: [nazwa fazy]
  pliki_wygenerowane: [lista istniejących phase-N-output.md]
  pokrycie_bp: [X% lub MISSING]
  pokrycie_pl: [X% lub MISSING]
  pokrycie_dl: [X% lub MISSING]
  log_dostępny: [tak/nie]
  sekwencja_kroków: [lista akcji z logu, skrócona do kluczowych]
  anomalie_logu: [brakujące kroki, nieoczekiwana kolejność, luki czasowe > 30min]
```

### Krok 4 — Porównanie między testami

Porównaj wszystkie testy parami i oceń:

**A. Spójność sekwencji kroków**
- Czy kroki w logach wykonywały się w tej samej kolejności?
- Czy żaden krok nie został pominięty względem oczekiwanego pipeline?

Oczekiwana sekwencja kroków orkiestratora:
```
1. session-init
2. material_input scan
3. [A, B normalizacja] jeśli materiały istnieją
4. coverage-assessment
5. [D zasilanie wsteczne] jeśli potrzebne
6. data-prep seed
7. phase-1 (big-picture)
8. [phase-2 jeśli wybrany poziom]
9. [phase-3 jeśli wybrany poziom]
10. session close
```

**B. Tryb pracy**
- Czy wszystkie testy używały tego samego trybu?
- Jeśli różne tryby — odnotuj które i jakie to miało skutki.

**C. Kompletność faz**
- Które fazy zakończyły się (completed) w każdym teście?
- Czy testy zatrzymały się w tym samym miejscu?

**D. Wygenerowane pliki**
- Czy te same pliki wyjściowe zostały wygenerowane we wszystkich testach?
- Lista plików obecnych w jednym teście a nieobecnych w innym.

**E. Zakończenie sesji**
- Czy sesje zakończyły się w ten sam sposób (completed / paused / error)?
- Jeśli różnie — w którym miejscu się rozeszły?

**F. Podobieństwo ogólne**
Oceń w skali: `wysokie / średnie / niskie`

Kryteria:
- `wysokie` — ta sama sekwencja kroków, ten sam tryb, te same fazy, te same pliki wyjściowe
- `średnie` — drobne różnice w sekwencji lub brakujące pojedyncze pliki, ale ogólny przebieg zbliżony
- `niskie` — różne tryby, różne fazy zakończone, istotne różnice w sekwencji

---

## Format pliku wyjściowego

Zapisz wynik do:
```
/MaratohonAI-analize-v3-output/V3.1/comparison-report.md
```

Jeśli plik istnieje — nadpisz go.

---

```markdown
---
generated: [data i godzina]
orchestrator_version: V3.1
tests_compared: [lista: test-1, test-2, ...]
---

# Raport porównawczy — Deep Analysis Orchestrator V3.1

## Podsumowanie

[2–4 zdania: ogólna ocena podobieństwa między testami, czy orkiestrator zachował się
spójnie, gdzie wystąpiły odchylenia, czy testy zakończyły się w tych samych miejscach.]

Podobieństwo ogólne: **wysokie / średnie / niskie**

---

## Profile testów

### test-1
| Parametr | Wartość |
|---|---|
| Tryb pracy | [N] |
| Złożoność | [niska/średnia/wysoka] |
| Fazy zakończone | [lista] |
| Fazy pominięte | [lista lub —] |
| Ostatnia aktywna faza | [nazwa] |
| Zakończenie sesji | [completed/paused/error/unknown] |
| Pliki wygenerowane | [lista phase-N-output.md] |
| Pokrycie BP/PL/DL | [X% / X% / X%] |
| Log dostępny | [tak/nie] |

### test-2
[analogicznie]

### test-N
[analogicznie]

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

| Krok | test-1 | test-2 | test-N |
|---|---|---|---|
| session-init | ✓ / ✗ / MISSING | | |
| material_input scan | | | |
| normalizacja | | | |
| coverage-assessment | | | |
| zasilanie wsteczne | | | |
| data-prep seed | | | |
| phase-1 | | | |
| phase-2 | | | |
| phase-3 | | | |
| session close | | | |

### Anomalie w sekwencji
- test-N: [opis anomalii — pominięty krok, nieoczekiwana kolejność, luka czasowa]
- [lub: Brak anomalii we wszystkich testach]

---

## Porównanie wygenerowanych plików

| Plik | test-1 | test-2 | test-N |
|---|---|---|---|
| state/session.md | ✓ / ✗ | | |
| state/orchestrator-log.md | ✓ / ✗ | | |
| state/phase-1-output.md | ✓ / ✗ | | |
| state/phase-2-output.md | ✓ / ✗ | | |
| state/phase-3-output.md | ✓ / ✗ | | |
| state/inputs/processed/index.md | ✓ / ✗ | | |

Pliki obecne tylko w wybranych testach:
- [plik] — tylko w test-N: [możliwa przyczyna]
- [lub: Wszystkie testy wygenerowały ten sam zestaw plików]

---

## Porównanie zakończenia sesji

| Test | Ostatnia faza | Status zakończenia | Otwarte pytania przeniesione |
|---|---|---|---|
| test-1 | [nazwa] | completed/paused/error | [liczba lub —] |
| test-2 | | | |
| test-N | | | |

[1–2 zdania komentarza: czy testy zatrzymały się w tym samym miejscu i z tego samego powodu.]

---

## Wnioski i obserwacje

[3–5 punktów: konkretne obserwacje o zachowaniu orkiestratora na podstawie porównania.
Tylko fakty z logów i plików — bez oceny merytorycznej domeny.]

- [obserwacja 1]
- [obserwacja 2]
- [obserwacja N]
```

---

## Reguły działania

- Nie oceniaj treści merytorycznej wniosków — tylko strukturę i przebieg.
- Nie czytaj plików diagramów (`.mermaid`, `.svg`, `.png`) — odnotuj tylko ich istnienie.
- Jeśli `orchestrator-log.md` nie istnieje w danym teście — oznacz cały test jako `log: MISSING`
  i zaznacz w raporcie że ocena sekwencji kroków jest niepełna.
- Porównuj każdy nowy test ze wszystkimi poprzednimi — nie tylko z ostatnim.
- Nadpisuj `comparison-report.md` przy każdym uruchomieniu.
- Timestamp w nagłówku raportu = moment wygenerowania raportu, nie moment testów.
