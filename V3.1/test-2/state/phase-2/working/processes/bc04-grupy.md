# Proces: Grupy startowe — BC-04

## Główny flow: Generacja grup startowych

**Wyzwalacz:** Administrator inicjuje generację LUB harmonogram (ostateczny termin generowania)
**Rezultat:** Wszyscy opłaceni zawodnicy z numerami startowymi przypisani do grup per dystans

**Kroki:**
1. System sprawdza warunki generacji:
   - Przynajmniej X uczestników z numerami startowymi
   - Nie przekroczono ostatecznego terminu generowania (z BC-01)
2. [warunki spełnione] → System grupuje zawodników i przypisuje do grup
3. Przypisano zawodnika do grupy (per uczestnik)
4. Wygenerowano grupy startowe
5. [warunki niespełnione] → Nie udało się wygenerować grup startowych

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-011 | Minimalna liczba uczestników musi być spełniona | 1 |
| RB-012 | Tylko uczestnicy z przypisanym numerem startowym wchodzą do generacji | 1 |
| RB-013 | Uczestnik z nieopłaconą zmianą dystansu → trafia do grup na dotychczasowym opłaconym dystansie | 2 |
| RB-014 | Generowanie grup odbywa się w terminie określonym w regulaminie | 1 |

**Wyjątki:**
| Warunek | Akcja | Skutek |
|---|---|---|
| Za mało uczestników z numerami | Zablokowanie generacji | Nie udało się wygenerować grup |

---

## Flow alternatywny: Zmiana grupy przez zawodnika

**Wyzwalacz:** Zawodnik składa wniosek o zmianę grupy
**Rezultat:** Zmieniono grupę LUB odmowa

**Warunki konieczne (wszystkie muszą być spełnione):**
1. Nie przekroczono terminu zmiany grup
2. Grupa docelowa ma wolne miejsca
3. Zawodnik nie zmieniał wcześniej grupy
4. Grupa docelowa jest z tego samego dystansu
5. Grupy startowe zostały już wygenerowane

**Wyniki:**
- [wszystkie spełnione] → Zmieniono grupę
- [termin minął] → nie udało się zmienić grupy
- [brak miejsc] → nie udało się zmienić grupy
- [zmiana wcześniej] → nie udało się zmienić grupy
- [inny dystans] → nie udało się zmienić grupy
- [grupy nie wygenerowane] → nie udało się zmienić grupy (jeszcze)

---

## Flow alternatywny: Dołączenie do grupy po terminie

**Wyzwalacz:** Uczestnik opłacił dystans po wygenerowaniu grup
**Rezultat:** Dołączono do istniejącej grupy LUB brak możliwości

**Kroki:**
1. System wykrywa: grupy wygenerowane + nowy opłacony uczestnik
2. System sprawdza: nie przekroczono terminu dołączenia do dystansu
3. [OK] → Dołączono do grupy (po terminie)
4. [termin minął] → brak przypisania do grupy — wymaga manualnej obsługi
