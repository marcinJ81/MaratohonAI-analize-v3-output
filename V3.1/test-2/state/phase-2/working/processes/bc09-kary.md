# Proces: Zarządzanie karami — BC-09

## Główny flow: Nałożenie kary

**Wyzwalacz:** Sędzia trasy stwierdza naruszenie
**Rezultat:** Kara nałożona i przekazana do BC-08

**Kroki:**
1. Sędzia trasy identyfikuje zawodnika (numer na dyskietce)
2. Sędzia stwierdza typ naruszenia:
   2a. Złamanie regulaminu → Nałożono karę za złamanie regulaminu
   2b. Pominięcie punktu kontrolnego → Naliczono karę za pominięte punkty kontrolne
3. System zapisuje karę powiązaną z zawodnikiem
4. BC-08 zostaje poinformowany → aktualizacja pozycji w rankingu

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-029 | Typy kar i ich konsekwencje określone w regulaminie maratonu (BC-01) | 2 |
| RB-030 | Kara to nie czas dodatkowy — to zmiana pozycji (koniec listy) lub wykluczenie z wyników | 3–4 |
| RB-031 | Pominięcie punktu kontrolnego jest wykrywane przez system (brak odczytu dyskietki na danym czytnik) lub manualnie przez sędziego | 2b |

**Otwarte pytanie:**
- Czy sędzia może cofnąć/anulować nałożoną karę? Jeśli tak — jak to wpływa na wyniki?
