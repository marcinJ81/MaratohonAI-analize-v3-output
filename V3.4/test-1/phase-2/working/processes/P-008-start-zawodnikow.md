# P-008 — Start Zawodników — BC-008

**Wyzwalacz:** A-004 Główny organizator / A-003 Administrator / A-006 Sędzia — decyzja o starcie
**Rezultat:** PE-003 (E-073 Wystartowano zawodników) → aktywacja BC-009 (Pomiar Czasu)

---

## Kroki — Start grupowy (ścieżka główna)

1. [A-004/A-003/A-006] --[Wystartuj zawodników (sygnał startowy)]--> [BC-008] ==> [E-070 Wystartowano grupę]
2. [A-009 System] --[Zarejestruj czas startu]--> [BC-008] ==> [E-071 Zarejestrowano czas startu]
3. [A-009 System] --[Oznacz zawodników jako wystartowanych]--> [BC-008] ==> [E-072 Wystartowano zawodnika] (per zawodnik)
4. [A-009 System] --[Zamknij zapisy na dystans]--> [BC-008] ==> [E-075 Zamknięto zapisy na dany dystans]
5. [A-009 System] --[Potwierdź start wszystkich dystansów]--> [BC-008] ==> [E-073 Wystartowano zawodników] → **PE-003**
5b. [A-009 System] --[per dystans]--> [BC-008] ==> [E-074 Wystartowano zawodników na danym dystansie]

---

## Ścieżka B — Start indywidualny (spóźnienie)

B1. [A-001 Zawodnik] --[Zgłoś indywidualny start]--> [BC-008] ==> [E-063 Zgłoszono indywidualny start]
B2. [A-003 Administrator] --[Wyciągnij zawodnika z grupy]--> [BC-008] ==> [E-064 Wyciągnięto zawodnika z grupy]
B3. → Czas startu indywidualnego rejestrowany osobno

**Uwaga:** "Wyciągnięto z grupy" może oznaczać że zawodnik startuje z innym czasem odniesienia niż jego grupa. Czy wyniki są wciąż porównywalne? → HS2-023.

---

## Ścieżka C — Zgubienie dyskietki RFID przed startem

C1. [A-001 Zawodnik] --[Zgłoś zgubienie dyskietki]--> [BC-008] ==> [E-065 Zgubiono dyskietkę z numerem startowym]
C2. [A-005 Obsługa] --[Przejmij kaucję]--> [BC-008] ==> [E-066 Przejęto kaucję]
C3. [A-003 Administrator] --[Odepnij numer startowy]--> [BC-004] ==> [E-067 Odpięto numer startowy]
C4. [A-005/A-003] --[Przypisz nowy numer]--> [BC-004] ==> [E-068 Przypisano numer startowy (nowy)]
C5. [A-005 Obsługa] --[Wydaj nową dyskietkę]--> [BC-008] ==> [E-069 Wydano nową dyskietkę]

**Koordynacja BC-008 ↔ BC-004:** kroki C3-C4 wymagają akcji w BC-004 (Przydział Numeru). To cross-context flow.

---

## Ścieżka D — Zakończenie udziału przedwcześnie (w trakcie startu)

D1. [A-001 Zawodnik / A-006 Sędzia trasy] --[Zakończ udział przedwcześnie]--> [BC-008 → BC-009] ==> [E-076 Zakończono udział przedwcześnie]

**UWAGA:** E-076 jest przypisane do BC-008/BC-009 w Fazie 1. Czy to zdarzenie należy do BC-008 (decyzja o zakończeniu) czy BC-009 (skutek dla pomiaru)? Prawdopodobnie inicjowane w BC-008, efekt w BC-009. → Q-011 (dyskietka) otwarte → HS2-005.

---

## Ścieżka E — Nie stawił się na starcie

E1. [Trigger czasowy] --[PE-003 minęło, zawodnik nie wystartował]--> [BC-008] ==> [E-077 Nie stawił się na starcie]
→ Q-010: czy slot zwalniany automatycznie? → HS2-022

---

## Read modele

| Krok | Aktor | Co musi widzieć |
|------|-------|----------------|
| 1 | Organizator/Administrator/Sędzia | Lista grup startowych z godzinami startów, kto jest dopuszczony (z BC-007) |
| 2 | System | Czas systemowy, lista grup z godzinami |
| 3 | System | Lista zawodników z danej grupy |
| B1 | Zawodnik | Informacja o możliwości startu indywidualnego |
| B2 | Administrator | Lista aktywnych grup, informacja o zawodniku |
| C1-C5 | Obsługa/Administrator | Stan numeru startowego, pula wolnych dyskietek |

---

## Reguły biznesowe

| ID     | Reguła | Egzekwowana w kroku |
|--------|--------|---------------------|
| RB-031 | Start następuje w godzinie określonej w harmonogramie grup (z BC-001) | 1 |
| RB-032 | Lista startowa zamknięta po PE-002 — nowi uczestnicy nie mogą być dodani | 4 |
| RB-033 | Kaucja pobierana przy zgubieniu dyskietki | C2 |
| RB-034 | Nowy numer musi być odczepiony ze starej dyskietki przed przypisaniem nowej | C3 przed C4 |

---

## Wyjątki i ścieżki alternatywne

| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-023 | Zawodnik spóźniony | Start indywidualny (ścieżka B) | Inny czas odniesienia |
| EX-024 | Zgubiona dyskietka | Ścieżka C + kaucja | Nowa dyskietka + koordynacja z BC-004 |
| EX-025 | Zawodnik nie stawił się | E-077 + Q-010 open | HS2-022 |
| EX-026 | Zawodnik rezygnuje podczas startu | E-076 | Brak pomiaru czasu po tym zdarzeniu |

---

## Przepływ odwrotny

- PE-003 jest nieodwracalne — nie ma "cofnięcia startu"
- E-075 (zamknięcie zapisów) jest nieodwracalne po PE-003

## Weryfikacja odwrotną narracją

Żeby PE-003 → E-073 (Wystartowano zawodników). Żeby E-073 → E-074 (per dystans) + E-070 (per grupa) + E-071 (czasy startu). Żeby E-070 → zawodnicy z listy startowej (z BC-007: E-059/E-060 wydane). Żeby zawodnicy na liście → PE-002 (zamrożona lista, z BC-006).

**Luki:**
1. Start indywidualny — jak czas jest rejestrowany względem czasu grupy? → HS2-023
2. Czy zawodnik który nie stawił się traci "miejsce" w grupie (Q-010)? → HS2-022
3. Koordynacja BC-008 ↔ BC-004 przy zgubionej dyskietce (kroki C3-C4) → cross-context, wymaga opisu relacji (D-007 w context map)
