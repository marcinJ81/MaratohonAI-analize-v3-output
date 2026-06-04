# Inputs Index
<!-- generated-by: coverage-assessment, session: maraton-rowerowy-24h -->
<!-- complexity: średnia -->

## Materiały źródłowe

| Plik | Typ | Konwersja | Użyty w fazach |
|---|---|---|---|
| kontekst_rejestracja1.png | Event Storming | clean | phase-1, phase-2 |
| ogólny_widok_kontekstu_rejestracja.png | Event Storming | clean | phase-1, phase-2 |
| generacja grup startowych.png | Event Storming | clean | phase-1, phase-2 |
| generacja_grup_poterminie.png | Event Storming | clean | phase-1, phase-2 |
| genreacja_numeru_indywidualna.png | Event Storming | clean | phase-1, phase-2 |
| grupy_startowe_ds.png | Domain Story | clean | phase-2, phase-3 |
| kary.png | Event Storming | clean | phase-1, phase-2 |
| opłata_za_udział_1.png | Event Storming | clean | phase-1, phase-2 |
| opłata_za_udział_2.png | Event Storming | clean | phase-1, phase-2 |
| pomiar_czasu.png | Event Storming | clean | phase-1, phase-2 |
| przypisanie_zawodnika_do_listy.png | Event Storming | clean | phase-1, phase-2 |
| start_indywidualny.png | Event Storming | clean | phase-1, phase-2 |
| utowrzenie_grup_ds.png | Domain Story | clean | phase-2, phase-3 |
| wystartowano_zawodników.png | Event Storming | clean | phase-1, phase-2 |
| zakończenie_2.png | Event Storming | clean | phase-1, phase-2 |
| zakończenie_zawodów.png | Event Storming | clean | phase-1, phase-2 |
| zmiania_grupy_startowej_DL.png | Design Level | clean | phase-2, phase-3 |
| zminia_grupystartowej.png | Event Storming | clean | phase-1, phase-2 |

---

## Ocena pokrycia

| Faza | Pokrycie | Status | Uwagi |
|---|---|---|---|
| Big Picture | 78% | **wystarczające** | Integracje zewnętrzne tylko częściowo opisane |
| Process Level | 75% | **wystarczające** | Reguły biznesowe często referencjonują "regulamin" bez szczegółów |
| Design Level | 50% | **częściowe** | Tylko 1 diagram DL (zmiana grupy); brak modeli danych |

---

## Szczegóły per faza

### Big Picture

| Kryterium | Waga | Punkty | Wynik ważony | Uwaga |
|---|---|---|---|---|
| Aktorzy zidentyfikowani | 25% | 1.0 | 25% | 7 aktorów zidentyfikowanych |
| Zdarzenia domenowe | 30% | 1.0 | 30% | ~70 zdarzeń; powyżej progu 15-40 dla złożoności średniej |
| Granice systemu | 25% | 0.5 | 12.5% | Konteksty widoczne, ale granice nie wszędzie wyraźne |
| Integracje zewnętrzne | 20% | 0.5 | 10% | Interfejs banku + czytniki dyskietek — brak szczegółów technicznych |
| **SUMA** | 100% | — | **77.5%** | |

### Process Level

| Kryterium | Waga | Punkty | Wynik ważony | Uwaga |
|---|---|---|---|---|
| Przepływy procesów | 30% | 1.0 | 30% | Szczegółowe flows dla wszystkich 9 obszarów |
| Reguły biznesowe | 30% | 0.5 | 15% | Reguły są, ale często jako "regulamin" bez konkretów |
| Wyjątki i hot spoty | 20% | 0.5 | 10% | Hot spoty zidentyfikowane (4 szt.), edge cases częściowe |
| Bounded Contexts (wstępne) | 20% | 1.0 | 20% | 9 kontekstów wyraźnie widocznych |
| **SUMA** | 100% | — | **75%** | |

### Design Level

| Kryterium | Waga | Punkty | Wynik ważony | Uwaga |
|---|---|---|---|---|
| Agregaty / encje | 30% | 0.5 | 15% | GrupyStartowe i ZmianyGrup z jednego diagramu DL |
| Kontrakty / interfejsy | 25% | 0.5 | 12.5% | Komendy i eventy widoczne, ale nieformalne |
| Bounded Contexts (nazwane) | 25% | 0.5 | 12.5% | Częściowo — nazwy widoczne, odpowiedzialności nieformalne |
| Reguły niezmienników | 20% | 0.5 | 10% | Warunki widoczne (np. zmiana grupy), ale nie jako inwarianty |
| **SUMA** | 100% | — | **50%** | |

---

## Rekomendacje dla orchestratora

### Blockery (do Big Picture i Process Level)

- **[nice-to-have]** Szczegóły reguł regulaminowych — często pojawia się "regulamin maratonu" bez konkretnych wartości (limity czasowe, kaucja, terminy)
- **[nice-to-have]** Kontekst **Kary** jest najsłabiej pokryty (2 zdarzenia) — może wymagać dodatkowego pytania

### Blockery (do Design Level)

- **[blocker]** Brak modeli danych / agregat dla większości kontekstów
- **[blocker]** Brak formalnych kontraktów między kontekstami
- **[nice-to-have]** Tylko 1 diagram Design Level (zmiana grupy) — pozostałe konteksty bez DL

### Identyfikowane konteksty (wstępnie)

1. Konfiguracja maratonu
2. Rejestracja
3. Opłata za udział
4. Generacja grup startowych
5. Zmiana grupy startowej
6. Przygotowanie zawodnika do startu
7. Start zawodów
8. Pomiar czasu
9. Nakładanie kar
10. Zakończenie zawodów
