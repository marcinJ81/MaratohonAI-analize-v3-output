# Proces: Wyniki i ranking — BC-08

## Główny flow: Aktualizacja rankingu na żywo

**Wyzwalacz:** Zawodnik zakończył przejazd (zdarzenie z BC-07)
**Rezultat:** Ranking zaktualizowany i opublikowany na żywo

**Kroki:**
1. BC-07 informuje: Zawodnik zakończył przejazd + czas pomiaru
2. System oblicza czas przejazdu zawodnika:
   - [start grupowy] → czas = meta − czas startu grupy
   - [start indywidualny] → czas = meta − indywidualny czas startu
3. System sprawdza kary zawodnika (integracja z BC-09):
   - [brak kar] → normalny zapis do rankingu
   - [kara: koniec listy] → pozycja: koniec listy rankingu per dystans × płeć
   - [kara: wykluczenie] → zawodnik nie pojawia się w wynikach
4. Sklasyfikowano zawodnika w rankingu (dystans + płeć)
5. Zaktualizowano wyniki na żywo
6. System publikuje na tablicy i stronie WWW → Opublikowano wyniki cząstkowe

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-025 | Ranking dzieli się per dystans × płeć — brak innych kategorii | 4 |
| RB-026 | Podstawa czasu: czas startu grupy do mety (lub indywidualny czas startu) | 2 |
| RB-027 | Kara pozycyjna (koniec listy) ma pierwszeństwo nad wynikiem czasowym | 3 |
| RB-028 | Wykluczenie z wyników = zawodnik niewidoczny w rankingu | 3 |

---

## Flow: Wyniki końcowe

**Wyzwalacz:** Zakończono przejazdy wszystkich dystansów (zdarzenie z BC-07)
**Rezultat:** Opublikowane wyniki końcowe (tablica + WWW)

**Kroki:**
1. BC-07 wysyła: Zakończono przejazdy wszystkich dystansów
2. System zamraża ranking — brak dalszych aktualizacji
3. System generuje finalną klasyfikację z uwzględnieniem wszystkich kar
4. Opublikowano wyniki końcowe (tablica na miejscu + strona WWW)
