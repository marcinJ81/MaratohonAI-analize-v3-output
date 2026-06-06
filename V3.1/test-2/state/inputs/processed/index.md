# Indeks materiałów wejściowych — ocena pokrycia faz
<!-- generated-by: orchestrator (inline coverage-assessment), session: maraton-rowerowy-24h -->
<!-- data: 2026-06-06 -->

## Materiały źródłowe

| Plik oryginalny | Typ | Konwersja | Plik processed |
|---|---|---|---|
| kontekst_rejestracja1.png | Event Storming screenshot | lossy | rejestracja-processed.md |
| ogólny_widok_kontekstu_rejestracja.png | Event Storming screenshot | lossy | rejestracja-processed.md |
| opłata_za_udział_1.png | Event Storming screenshot | lossy | oplata_za_udzial-processed.md |
| opłata_za_udział_2.png | Event Storming screenshot | lossy | oplata_za_udzial-processed.md |
| przypisanie_zawodnika_do_listy.png | Event Storming screenshot | lossy | przygotowanie_do_startu-processed.md |
| generacja grup startowych.png | Event Storming screenshot | lossy | grupy_startowe-processed.md |
| generacja_grup_poterminie.png | Event Storming screenshot | lossy | grupy_startowe-processed.md |
| grupy_startowe_ds.png | Event Storming screenshot (DL) | lossy | grupy_startowe-processed.md |
| utowrzenie_grup_ds.png | Event Storming screenshot (DL) | lossy | grupy_startowe-processed.md |
| genreacja_numeru_indywidualna.png | Event Storming screenshot | lossy | grupy_startowe-processed.md |
| zmiania_grupy_startowej_DL.png | Event Storming screenshot (DL) | lossy | grupy_startowe-processed.md |
| zminia_grupystartowej.png | Event Storming screenshot (DL) | lossy | grupy_startowe-processed.md |
| pomiar_czasu.png | Event Storming screenshot | lossy | pomiar_czasu-processed.md |
| start_indywidualny.png | Event Storming screenshot | lossy | start_zawodnikow-processed.md |
| wystartowano_zawodników.png | Event Storming screenshot | lossy | start_zawodnikow-processed.md |
| kary.png | Event Storming screenshot | lossy | kary-processed.md |
| zakończenie_2.png | Event Storming screenshot | lossy | zakonczenie_zawodow-processed.md |
| zakończenie_zawodów.png | Event Storming screenshot | lossy | zakonczenie_zawodow-processed.md |

## Pokrycie faz

| Faza | Pokrycie | Ocena |
|---|---|---|
| Big Picture (Phase 1) | **85%** | Dobra podstawa. Brakuje: kontekst Wyniki/Ranking/Publikacja. |
| Process Level (Phase 2) | **65%** | Mamy przepływy 7/8 kontekstów. Brakuje: Wyniki/Ranking (wymienione przez użytkownika jako cel), szczegóły modelu kar. |
| Design Level (Phase 3) | **15%** | Fragmenty poleceń/decyzji widoczne w kilku screenshotach DL, ale brak systematycznego pokrycia. |

## Szczegóły per kontekst — Phase 2

| Kontekst | Zdarzenia | Reguły | Przepływ | Aktorzy |
|---|---|---|---|---|
| Rejestracja | ✅ | ✅ | ✅ | ✅ |
| Opłata za udział | ✅ | ✅ | ✅ | ✅ |
| Grupy startowe | ✅ | ✅ | ✅ | ✅ |
| Przygotowanie do startu | ✅ | ✅ | ✅ | ✅ |
| Pomiar czasu | ✅ | ✅ | ✅ | ✅ |
| Start zawodników | ✅ | ✅ | ✅ | ✅ |
| Nakładanie kar | ⚠️ (ogólne) | ⚠️ (tylko nazwy) | ❌ (brak szczegółów) | ✅ |
| Zakończenie zawodów | ✅ | ✅ | ✅ | ✅ |
| **Wyniki / Ranking** | ❌ | ❌ | ❌ | ❌ |

## Blokery

- **BLOKER Phase 2**: Brak kontekstu Wyniki / Ranking / Publikacja wyników — użytkownik wymienił "publikację wyników" jako część systemu, ale nie istnieje w materiałach ES
- **Uwaga**: Kontekst Nakładanie kar jest bardzo szczątkowy (1 screenshot, brak flow)

## Rekomendacja
- Recommended start phase: **Big Picture** — dobre pokrycie, pozwoli zidentyfikować brakujące konteksty
- Po Big Picture: uzupełnić ES o kontekst Wyniki/Ranking przed przejściem do Process Level
