---
source: phase-1-seed.md + weryfikacja audit K3a, K4, K5
generated: 2026-06-14
---

# Hot Spoty

Hot Spot = miejsce konfliktu, niejasności, ryzyka lub brakującej wiedzy domenowej.
Rozwiązane oznaczone [resolved: powód] — zachowane dla tracku decyzji.

---

## Lista hot spotów

| ID | Opis | Powód | Powiązane zdarzenia | Status | Priorytet |
|---|---|---|---|---|---|
| H-001 | Brak explicite zdefiniowanej granicy systemu jako całości (co jest w systemie, co poza) | niejasność | — | open | wysoki |
| H-002 | Nota: "grupy mogą istnieć tylko po pewnych krokach jako reprezentacje list startowych..." (częściowo nieczytelna) | brak wiedzy / strata konwersji | Wygenerowano grupy startowe, Przypisano zawodnika do grupy | open | średni |
| H-003 | Co zrobić gdy rozpocznie się generowanie list a czekamy na opłacenie niedopłaty? | konflikt — opłata vs termin generacji | Wygenerowano grupy startowe, Wykryto niedopłatę, Opłacono niedopłatę | open | **blocker** |
| H-004 | Wyświetlanie w czasie rzeczywistym nieobecne w materiałach źródłowych — ale potwierdzone przez orkiestratora | strata konwersji | Zarejestrowano pomiar czasu | resolved: potwierdzone przez orkiestratora — checkpointy wyświetlane w biurze + WWW | niski |
| H-005 | Brak opisanego przepływu generowania wyników/rankingów po "Podliczono czasy przejazdu" | brak wiedzy | Podliczono czasy przejazdu, Wygenerowano ranking wyników (dodano K5) | open | wysoki |
| H-006 | Brak opisanego przepływu generowania dyplomów (za wyjątkiem "Otrzymano dyplom") | brak wiedzy | Otrzymano dyplom, Wygenerowano dyplom (dodano K5) | open | wysoki |
| H-007 | Różowa karteczka przy Frame 51 (treść nieczytelna) | strata konwersji | Obszar opłaty za udział | open | średni |
| H-008 | Dwie odrębne weryfikacje zawodnika: biurowa (kwalifikacja) i na starcie — różnica procesowa niejasna | niejasność | Zweryfikowano tożsamość zawodnika, Zweryfikowano zawodnika przed startem | open | średni |
| H-009 | Powiązanie pakietu startowego z dyskietką RFID — czy dyskietka jest częścią pakietu czy osobnym wydaniem? | niejasność | Wydano pakiet startowy, Wydano dyskietkę RFID (dodano K5) | open | średni |
| H-010 | Logika zamknięcia pomiaru czasu — dwa możliwe triggery: "wszyscy dojechali" OR "minął czas". Który obowiązuje, co jeśli obydwa? | konflikt / niejasność | Zakończono przejazd zawodników na danym dystansie, Upłynął czas maratonu (dodano K5) | open | **blocker** |
| H-011 | Brak zdarzenia finalizacji rejestracji ("Potwierdzono rejestrację") w materiałach — możliwa strata konwersji lub luka | niejasność | Zarejestrowano uczestnika, Wyliczono opłatę | open | wysoki |
| H-012 | Co się dzieje z uczestnikiem który nie opłaci w terminie? Brak mechanizmu anulowania/wygasania rejestracji | brak wiedzy | Przekroczono termin płatności (dodano K5) | open | wysoki |
| H-013 | Uczestnik vs Zawodnik — ta sama osoba w różnych stanach, czy dwa różne byty w modelu systemu? | niejasność | Zarejestrowano uczestnika, Wydano pakiet startowy | open | średni |
| H-014 | Relacje między kontekstami: strzałki łukowe widoczne w ogólnym widoku ale brak opisu formatu wymiany danych, właściciela, kierunku | brak wiedzy | BC-02→BC-03, BC-03→BC-04, BC-04→BC-05 | open | wysoki |
| H-015 | Kto może zwrócić pakiet startowy — "zawodnik lub osoba uprawniona przez zawodnika". Brak mechanizmu upoważnienia w systemie | niejasność | Zwrócono pakiet startowy | open | niski |
| H-016 | Czy strona WWW do wyświetlania wyników real-time to ten sam system czy osobna aplikacja? | niejasność | Wyświetlono wyniki w czasie rzeczywistym | open | wysoki |
| H-017 | "W trakcie opłacania nie można zmienić dystansu" — jak system egzekwuje ten zakaz? Stan blokady? | ryzyko | Złożono wniosek o zmianę dystansu, Rozpoczęto opłacenie udziału | open | średni |
| H-018 | Minimalna liczba uczestników X wymagana do generacji grup — wartość konfiguracyjna nieznana | brak wiedzy | Wygenerowano grupy startowe, Nie udało się wygenerować grup startowych | open | średni |
| H-019 | Scenariusz "nie stawił się na starcie" — zawodnik zarejestrowany i opłacony, ale nie pojawia się. Brak przepływu | brak wiedzy | Wystartowano zawodnika (K5) | open | średni |
| H-020 | Zawodnik który zakończył udział przedwcześnie — co dzieje się z jego dyskietką RFID? Czy wyniki są liczone? | niejasność | Zakończono udział przedwcześnie, Zarejestrowano pomiar czasu | open | średni |

---

## Podsumowanie

- Razem hot spotów: 20
- Blockerów: 2 (H-003, H-010)
- Wysoki priorytet: 7 (H-001, H-005, H-006, H-011, H-012, H-014, H-016)
- Średni priorytet: 9
- Niski priorytet: 1
- Resolved: 1 (H-004)
