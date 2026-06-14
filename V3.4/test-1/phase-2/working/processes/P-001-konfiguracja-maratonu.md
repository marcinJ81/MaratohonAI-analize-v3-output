# P-001 — Konfiguracja Maratonu — BC-001

**Wyzwalacz:** Główny organizator (A-004) — decyzja o przeprowadzeniu edycji imprezy
**Rezultat:** E-003 Opublikowano regulamin maratonu — parametry dostępne dla wszystkich BC

---

## Kroki

1. [A-004 Główny organizator] --[Zarejestruj maraton]--> [BC-001] ==> [E-001 Zarejestrowano maraton]
2. [A-004 Główny organizator] --[Skonfiguruj parametry: terminy, dystanse, limity, okna zmian, regulamin]--> [BC-001] ==> [E-002 Skonfigurowano parametry maratonu]
3. [A-004 Główny organizator] --[Opublikuj regulamin]--> [BC-001] ==> [E-003 Opublikowano regulamin maratonu]

---

## Read modele

| Krok | Aktor | Co musi widzieć |
|------|-------|----------------|
| 2 | Główny organizator | Poprzednie edycje (jeśli istnieją), wzorzec parametrów, lista dystansów |
| 3 | Główny organizator | Komplet parametrów do weryfikacji przed publikacją |

---

## Reguły biznesowe

| ID     | Reguła | Egzekwowana w kroku |
|--------|--------|---------------------|
| RB-001 | Regulamin musi zawierać: terminy rejestracji, listę dystansów (50–700 km), limity per dystans, termin generacji grup, okno czasowe zmian grupy | 2 |
| RB-002 | Dystanse: 50, 100, 150, 200, 300, 400, 500, 600, 650, 700 km — wartości stałe domenowo | 2 |
| RB-003 | Konfiguracja jest niezmieniana po opublikowaniu regulaminu (lub tylko z notyfikacją uczestników) | 3 |

---

## Wyjątki i ścieżki alternatywne

| ID     | Warunek | Akcja | Skutek |
|--------|---------|-------|--------|
| EX-001 | Parametry niekompletne | BC-001 odrzuca | Brak E-003, organizator wraca do kroku 2 |
| EX-002 | Zmiana parametrów po publikacji | Organizator zmienia parametry + notyfikuje uczestników | Brak systemowego mechanizmu notyfikacji opisanego w materiałach → HS2-007 |

---

## Przepływ odwrotny

- Brak mechanizmu cofnięcia publikacji regulaminu opisanego w materiałach
- Jeśli zmiana konieczna po publikacji → EX-002 + HS2-007

## Weryfikacja odwrotną narracją

Żeby E-003 mogło nastąpić → musiały być E-001 i E-002. Żeby E-002 → organizator musi mieć dostęp do formularza parametrów. Żeby E-001 → decyzja organizatora. Brak luk w podstawowej ścieżce. Luka: brak mechanizmu notyfikacji o zmianach regulaminu → HS2-007.
