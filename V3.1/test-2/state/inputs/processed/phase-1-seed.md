# Seed: Phase 1 — Big Picture
<!-- generated-by: orchestrator, session: maraton-rowerowy-24h -->
<!-- coverage: 85% -->
<!-- sources: rejestracja-processed.md, oplata_za_udzial-processed.md, grupy_startowe-processed.md, przygotowanie_do_startu-processed.md, pomiar_czasu-processed.md, start_zawodnikow-processed.md, kary-processed.md, zakonczenie_zawodow-processed.md -->

## Kontekst sesji
- **System**: System zarządzania i pomiaru czasu na maratonie rowerowym 24H
- **Cel analizy**: Zrozumienie domeny i uzupełnienie procesu
- **Złożoność**: średnia

## Zidentyfikowane obszary systemu (z Event Stormingu)

1. **Konfiguracja maratonu** — administrator konfiguruje maraton (reguły, dystanse, regulamin)
2. **Rejestracja** — uczestnik rejestruje się, wybiera dystans, weryfikacja danych
3. **Opłata za udział** — płatność przez przelew bankowy, promocje, zwroty, niedopłaty
4. **Grupy startowe** — generacja grup, przypisanie zawodników, zmiany grup
5. **Przygotowanie do startu** — kwalifikacja zawodnika, weryfikacja tożsamości, wydanie pakietu
6. **Start zawodników** — zamknięcie zapisów, start grupowy i indywidualny, obsługa dyskietki
7. **Pomiar czasu** — dyskietka + czytnik, dystans standardowy i wydłużony
8. **Nakładanie kar** — sędzia trasy, kary za regulamin i punkty kontrolne
9. **Zakończenie zawodów** — nagrody, zwrot pakietu startowego, dyplom
10. ⚠️ **Wyniki / Ranking / Publikacja** — wymienione przez użytkownika, **BRAK w materiałach ES**

## Kluczowi aktorzy
administrator, uczestnik/zawodnik, obsługa punktu kontrolnego, interfejs banku (zewnętrzny), główny organizator zawodów, sędzia trasy, system pomiaru czasu

## Integracje zewnętrzne
- **Interfejs banku** — rejestracja przelewów

## Hot Spoty z ES
- Niedopłata vs generowanie list startowych (bloker sekwencji)
- Start indywidualny (spóźnienie) — jak rejestrować czas

## Mechanizm techniczny kluczowy
- Dyskietka jako identyfikator zawodnika — przyłożenie do czytnika = zapis pomiaru czasu

## Co wymaga uzupełnienia (od użytkownika w fazie Big Picture)
- Kontekst Wyniki / Ranking — czy istnieje? Jak działa?
- Szczegóły nakładania kar — jak kara wpływa na wyniki?
- Czy system publikuje wyniki na żywo czy po zakończeniu?
