---
source: phase-1-seed.md + weryfikacja audit K3b
generated: 2026-06-14
---

# Aktorzy

---

## Lista aktorów

| ID | Nazwa | Typ | Zewnętrzny? | Główne zdarzenia inicjowane | Uwagi |
|---|---|---|---|---|---|
| A-001 | Zawodnik | człowiek | nie | Zarejestrowano uczestnika, Wybrano dystans, Złożono wniosek o zmianę dystansu/grupy, Zgłoszono indywidualny start, Zgłoszono prośbę o przedłużenie dystansu, Zwrócono pakiet startowy | Początkowo "Uczestnik" — staje się "Zawodnikiem" po wydaniu pakietu startowego |
| A-002 | Uczestnik | człowiek | nie | Zarejestrowano uczestnika, Wybrano dystans, Potwierdzono dane kontaktowe, Złożono wniosek o zmianę dystansu, Rozpoczęto opłacenie udziału | Stan przed startem: przed wydaniem pakietu i weryfikacją; po weryfikacji = Zawodnik (A-001). Formalnie ten sam człowiek — różne stany w procesie. |
| A-003 | Administrator | człowiek | nie | Zweryfikowano zawodnika, Zmieniono dystans, Przyjęto wniosek do zmiany grupy, Wygenerowano grupy startowe, Wyciągnięto zawodnika z grupy, Przypisano numer startowy (nowy) | Rola biurowa, nie na trasie |
| A-004 | Główny organizator zawodów | człowiek | nie | Przyznano nagrodę, Zaaplikowano promocje, Przedłużono przejazd zawodnika, Opublikowano wyniki końcowe | Najwyższy autorytet; decyzje: promocje, przedłużenia, nagrody |
| A-005 | Obsługa punktu kontrolnego | człowiek | nie | Zweryfikowano tożsamość zawodnika, Wydano pakiet startowy, Przejęto kaucję, Wydano nową dyskietkę, Zwrócono pakiet startowy | Na checkpointach — weryfikacja i fizyczna obsługa |
| A-006 | Sędzia trasy | człowiek | nie | Zakończono udział przedwcześnie, Zdyskwalifikowano zawodnika | Poza zakresem systemu (kary/dyskwalifikacja POZA ZAKRESEM); może inicjować "Zakończono udział przedwcześnie" |
| A-007 | Interfejs banku | system | tak | Zarejestrowano przelew | Zewnętrzny system płatniczy; push — bank wysyła zdarzenie do systemu po przelewie uczestnika |
| A-008 | System RFID (czytnik dyskietek) | system | tak | Przyłożenie dyskietki do czytnika → Zarejestrowano pomiar czasu | Fizyczny trigger — karty RFID na checkpointach; zdarzenie zewnętrzne inicjowane fizycznie przez zawodnika |
| A-009 | System (automatyczna generacja) | system | nie | Wygenerowano grupy startowe, Przypisano numer startowy, Wyliczono opłatę, Podliczono czasy przejazdu, Wygenerowano ranking wyników, Wygenerowano dyplom | Wewnętrzny system — logika automatyczna |

---

## Uwagi o aktorach

### A-001 vs A-002 (Zawodnik vs Uczestnik)
Seed rozróżnia dwie nazwy dla tej samej osoby w różnych stanach procesu:
- "Uczestnik" — etap rejestracji i opłaty (przed wydaniem pakietu)
- "Zawodnik" — etap od weryfikacji i startu (po wydaniu pakietu)
Hot spot: czy to ten sam aktor w sensie systemu (jeden account), czy dwa różne byty?
Rekomendacja: jeden aktor A-001 "Uczestnik/Zawodnik" z rozróżnieniem przez stan rejestracji.

### A-004 vs A-003 (Główny organizator vs Administrator)
Podział ról: Administrator = biuro (rejestracja, grupy), Główny organizator = decyzje strategiczne (nagrody, promocje, przedłużenia).
Mogą to być te same osoby lub różne — wymaga potwierdzenia domenowego.

### A-006 (Sędzia trasy)
Minimalny udział w systemie — zdarzenia dyskwalifikacji POZA ZAKRESEM.
"Zakończono udział przedwcześnie" może być inicjowane przez zawodnika (rezygnacja) lub sędziego.

### A-008 (RFID)
Technicznie system zewnętrzny — fizyczny czytnik. Zawodnik fizycznie przyłoży kartę, ale zdarzenie domenowe (Zarejestrowano pomiar czasu) tworzy system. Powiązanie: dyskietka wydana w pakiecie startowym (A-005).

---

## Zdarzenia z wieloma możliwymi inicjatorami (weryfikacja K3b)

| Zdarzenie | Możliwi aktorzy | Uwagi |
|---|---|---|
| Złożono wniosek o zmianę dystansu | A-001 (Zawodnik), A-003 (Administrator) | Czy administrator może złożyć w imieniu zawodnika? |
| Zakończono udział przedwcześnie | A-001 (rezygnacja), A-006 (decyzja trasy) | Dwa triggery — różne powody |
| Przypisano numer startowy | A-003 (manualnie), A-009 (automatycznie) | Dwa tryby |
| Zwrócono pakiet startowy | A-001, lub "osoba uprawniona przez zawodnika" (z zakończenie_zawodów) | Proxy — kto może zwrócić pakiet? |
