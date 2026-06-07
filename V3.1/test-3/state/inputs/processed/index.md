# Inputs Index
<!-- generated-by: coverage-assessment, session: maraton-rowerowy-24h -->
<!-- złożoność: średnia -->

## Materiały źródłowe

| Plik | Typ | Konwersja | Użyty w fazach |
|---|---|---|---|
| es-processed.md | Event Storming (18 screenshotów) | lossy | phase-1, phase-2 |

*Uwaga: konwersja oznaczona jako lossy — część karteczek nieczytelna z powodu rozdzielczości. Zastosowano korektę -20% do wszystkich ocen.*

---

## Ocena pokrycia

| Faza | Pokrycie raw | Korekta lossy | Pokrycie końcowe | Status | Co brakuje |
|---|---|---|---|---|---|
| Big Picture | 90% | −18% | **72%** | częściowe | Pełna lista integracji zewnętrznych (może brakować email/SMS) |
| Process Level | 85% | −17% | **68%** | częściowe | Przepływy dla Kar i Rankingów; decyzja ws. HS-01 (niedopłata vs generacja grup) |
| Design Level | 50% | −10% | **40%** | częściowe | Modele agregatów dla większości BC; kontrakt interfejsów |

---

## Szczegóły per faza

### Big Picture

| Kryterium | Waga | Punkty | Wynik | Uwaga |
|---|---|---|---|---|
| Aktorzy | 25% | 1.0 | 25% | 8 aktorów zidentyfikowanych, w tym 2 zewnętrznych |
| Zdarzenia domenowe | 30% | 1.0 | 30% | ~50 zdarzeń — powyżej progu dla złożoności średniej (15–40) |
| Granice systemu | 25% | 1.0 | 25% | 9 BC wyraźnie widocznych, granica zewnętrzna czytelna |
| Integracje zewnętrzne | 20% | 0.5 | 10% | Bank + chip timing zidentyfikowane; możliwe brakujące (email?) |
| **Suma raw** | | | **90%** | |
| **Po korekcie lossy** | | | **72%** | Status: częściowe |

### Process Level

| Kryterium | Waga | Punkty | Wynik | Uwaga |
|---|---|---|---|---|
| Przepływy procesów | 30% | 0.5 | 15% | Rejestracja, opłaty, grupy startowe, start, zakończenie — OK; brak: Kary, Wyniki |
| Reguły biznesowe | 30% | 1.0 | 30% | Liczne policies widoczne: terminy, limity, regulamin |
| Wyjątki i hot spoty | 20% | 1.0 | 20% | 4 hot spoty, wiele ścieżek błędów |
| Bounded Contexts | 20% | 1.0 | 20% | 9 BC z nazwami i odpowiedzialnościami |
| **Suma raw** | | | **85%** | |
| **Po korekcie lossy** | | | **68%** | Status: częściowe |

### Design Level

| Kryterium | Waga | Punkty | Wynik | Uwaga |
|---|---|---|---|---|
| Agregaty / encje | 30% | 0.5 | 15% | Widoczne tylko dla GrupyStartowe (DS + DL screenshoty) |
| Kontrakty / interfejsy | 25% | 0.5 | 12.5% | Komendy widoczne (Zmień, Generuj) ale tylko dla BC-4 |
| Bounded Contexts (nazwane) | 25% | 0.5 | 12.5% | Nazwy są, pełne modele tylko dla BC-4 |
| Reguły niezmienników | 20% | 0.5 | 10% | Widoczne dla GrupyStartowe, reszta domniemana |
| **Suma raw** | | | **50%** | |
| **Po korekcie lossy** | | | **40%** | Status: częściowe (granica) |

---

## Rekomendacje dla orchestratora

### Blokery przed startem Process Level
- [blocker] Brak opisanych przepływów dla kontekstu **Kary** (tylko 2 zdarzenia, zero reguł)
- [blocker] Brak opisanych przepływów **generowania wyników / rankingów** po zakończeniu zawodów
- [blocker] Hot spot HS-01: decyzja "co z zawodnikami z niedopłatą podczas generacji grup" — wymagana odpowiedź użytkownika

### Nice-to-have
- [nice-to-have] Potwierdzenie czy istnieją integracje email/SMS (powiadomienia dla uczestnika)
- [nice-to-have] Wyjaśnienie mechanizmu "dyskietki" — typ chipa (RFID, transponder?)
- [nice-to-have] Potwierdzenie czy zawodnik może mieć wiele dystansów jednocześnie
