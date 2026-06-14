---
source: phase-1-seed.md + weryfikacja audit K2, K4, K5
generated: 2026-06-14
---

# Timeline — oś czasu

Zdarzenia ułożone chronologicznie wzdłuż osi czasu maratonu rowerowego 24h.

---

## Legenda

- [S] — sekwencyjne
- [R] — równoległe (mogą się dziać jednocześnie)
- [C] — cykliczne / powtarzalne
- **[PE-XXX]** — Pivotal Event
- [?] — hot spot / niejasna kolejność

---

## FAZA 0 — Konfiguracja maratonu (jednorazowa, przed otwarciem zapisów)

```
[S] Zarejestrowano maraton                          ← A-006 (Główny organizator)
[S] Skonfigurowano parametry maratonu               ← A-006 [DODANO K4]
[S] Opublikowano regulamin maratonu                 ← A-006 [DODANO K5]
```

Warunek wstępny dla całej reszty procesu. Bez tej fazy nie ma rejestracji.

---

## FAZA 1 — Rejestracja uczestnika (okno przed terminem)

```
[S] Zarejestrowano uczestnika                       ← A-002 (Uczestnik)
[S] Wybrano dystans                                 ← A-002
[S] Potwierdzono dane kontaktowe                    ← A-002
[S] Zweryfikowano zawodnika                         ← A-003 (Administrator) / System
[S] Wyliczono opłatę                                ← System (A-009)
[S] Potwierdzono rejestrację                        ← System [DODANO K5 — brakujące zdarzenie finalizacji]

  ↳ [?] opcjonalnie:
    [S] Złożono wniosek o zmianę dystansu           ← A-002
    [S] Zmieniono dystans                           ← A-003
    [S] Wyliczono opłatę (zmiana dystansu)          ← System
    [S] Poinformowano użytkownika o zmianie opłaty  ← System
```

Edge case (K5):
```
  [?] Anulowano rejestrację                         ← [DODANO K5 — co jeśli uczestnik nie opłaci w terminie?]
  [?] Przekroczono termin płatności                 ← czasowe [DODANO K5]
```

---

## FAZA 2 — Opłata za udział (równoległa do rejestracji i przed generacją grup)

```
[S] Rozpoczęto opłacenie udziału                   ← A-002 / A-003
[S] Zarejestrowano przelew                         ← A-007 (Interfejs banku) — zewnętrzne
[S] Zaaplikowano opłatę                            ← System
```

Ścieżka A — pełna opłata:
```
[S] Opłacono uczestnictwo                          ← System
```

Ścieżka B — promocja 100%:
```
[S] Zaaplikowano promocje (-100%)                  ← A-006 / System
[S] Opłacono uczestnictwo                          ← System
```

Ścieżka C — niedopłata:
```
[S] Wykryto niedopłatę                             ← System [DODANO K5]
[S] Rozpoczęto opłacenie niedopłaty                ← A-002
[S] Zarejestrowano przelew                         ← A-007 — zewnętrzne
[S] Opłacono niedopłatę                            ← System
```

Ścieżka D — zmiana dystansu z dopłatą/zwrotem:
```
[S] Wyliczono opłatę za nowy dystans               ← System
  ↳ [R] dopłata: Opłacono zmianę dystansu
  ↳ [R] zwrot:   Wyliczono kwotę zwrotu → Zwrócono nadpłatę
```

Zasada (potwierdzone): "W trakcie opłacania nie można zmienić dystansu"
Hot spot: [?] "Co zrobić jak rozpocznie się generowanie list a czekamy na opłacenie niedopłaty?"

```
[?] Odpisano uczestnictwo                          ← A-006 (częściowo nieczytelne)
```

---

## FAZA 3 — Przydział numeru startowego (po opłacie, przed generacją grup)

```
[S] Przypisano numer startowy                      ← System (A-009) / A-003
```

Warunek wstępny: uczestnik opłacony (Opłacono uczestnictwo).
Warunek dla generacji grup: numer startowy musi być przypisany (potwierdzony przez genreacja_numeru_indywidualna).

---

## FAZA 4 — Generacja grup startowych (okno terminowe, przed startem)

```
[S] Wygenerowano grupy startowe                    ← A-003 (Administrator) via komenda Generuj
   ↳ warunek: przynajmniej X uczestników z numerami startowymi
   ↳ warunek: nie przekroczono ostatecznego terminu
   ↳ wynik negatywny: Nie udało się wygenerować grup startowych

**[PE-001] Wygenerowano ostateczne grupy startowe** ← punkt bez powrotu dla standardowych przypisań
   ↳ Grupy startowe zostały wygenerowane (stan systemu)
```

Zarządzanie grupami (okno po generacji, przed blokadą):
```
[S] Złożono wniosek o zmianę grupy                ← A-001 (Zawodnik)
  ↳ Przyjęto wniosek do zmiany grupy → Grupa została zmieniona → Zmieniono grupę
  ↳ Nie przyjęto wniosku o zmianę grupy → Grupa nie została zmieniona → Nie udało się zmienić grupy
  ↳ brak opłaty → Odmówiono zmiany grupy z powodu braku opłaty [DODANO K5]

[S] Dołączono do grupy (po terminie)              ← System
[S] Nadobiegnięto do grupy                        ← System (częściowo nieczytelne)
```

Zdarzenie blokujące:
```
**[PE-002] Minął czas na ewentualne zmiany grupy — przypisanie jest na stałe** ← czasowe
   ↳ Upłynął czas na zmianę grupy
   ↳ Upłynął termin zmiany grupy dystansowo
```

Scenariusz przed generacją:
```
[?] Złożono wniosek o zmianę dystansu (przed generowaniem) ← A-001
[?] Złożono wniosek o zmianę dystansu (po terminie)       ← A-001
```

---

## FAZA 5 — Weryfikacja i kwalifikacja przed startem (dzień zawodów)

```
[S] Zweryfikowano tożsamość zawodnika              ← A-005 (Obsługa punktu kontrolnego)
  ↳ pozytywna: Podpisano oświadczenie zawodnika
    ↳ Wydano pakiet startowy (zawiera dyskietkę RFID)
    ↳ [?] Wydano dyskietkę RFID zawodnikowi [DODANO K5 — niejawne]
  ↳ negatywna: Zawodnik nie przeszedł weryfikacji

[S] Zweryfikowano zawodnika przed startem          ← A-005
  ↳ negatywna: Zawodnik nie przeszedł weryfikacji (przed startem)
```

Edge case:
```
[?] Nie stawił się na starcie                     ← [DODANO K5]
```

---

## FAZA 6 — Start zawodników

Scenariusz normalny:
```
[C] Wystartowano grupę (per każda grupa)           ← A-006 / A-003 / A-008 (Sędzia)
[S] Zarejestrowano czas startu                    ← System / A-005
[C] Wystartowano zawodnika                        ← System per zawodnik w grupie

**[PE-003] Wystartowano zawodników**               ← punkt aktywacji całego pomiaru czasu
[S] Zamknięto zapisy na dany dystans              ← System (po starcie danego dystansu)
[S] Wystartowano zawodników na danym dystansie    ← System
```

Scenariusz indywidualny (spóźnienie):
```
[S] Zgłoszono indywidualny start                  ← A-001 (Zawodnik)
[S] Wyciągnięto zawodnika z grupy                 ← A-003
```

Scenariusz zgubionej dyskietki:
```
[S] Zgubiono dyskietkę z numerem startowym        ← A-001
[S] Przejęto kaucję                               ← A-005
[S] Odpięto numer startowy                        ← A-005
[S] Przypisano numer startowy (nowy)              ← A-005 / A-003
[S] Wydano nową dyskietkę                         ← A-005
```

Scenariusz przerwania:
```
[?] Zakończono udział przedwcześnie               ← A-001 / A-008
[?] Nie stawił się na starcie                     ← [DODANO K5]
```

---

## FAZA 7 — Pomiar czasu (cykliczny, przez cały czas zawodów)

```
[C] Przyłożenie dyskietki do czytnika             ← A-001 — zewnętrzny trigger RFID
[C] Zarejestrowano pomiar czasu                   ← System (A-008 RFID)
[C] Wyświetlono wyniki w czasie rzeczywistym      ← System [DODANO K4] — biuro + WWW

  ↳ ścieżka A (dystans zadeklarowany ukończony):
    [S] Zawodnik zakończył przejazd               ← System

  ↳ ścieżka B (prośba o przedłużenie):
    [S] Zgłoszono prośbę o przedłużenie dystansu  ← A-001
    [S] Przedłużono przejazd zawodnika            ← A-006 / A-003
    [C] Odnotowano dodatkowy pomiar czasu         ← System
    [S] Zawodnik zakończył przejazd (wydłużony)   ← System
```

Zamknięcie pomiaru — dwa triggery [HOT SPOT — który obowiązuje?]:
```
  ↳ trigger 1: Zakończono przejazd zawodników na danym dystansie  ← System (wszyscy dojechali)
  ↳ trigger 2: Upłynął czas maratonu                              ← czasowe [DODANO K5]

[S] Podliczono czasy przejazdu                    ← System
[S] Wygenerowano ranking wyników                  ← System [DODANO K5 — brakujący przepływ]
```

**[PE-004] Zakończono przejazdy wszystkich dystansów** ← trigger końca zawodów / przejścia do ceremonii

---

## FAZA 8 — Zakończenie zawodów (ceremonia)

```
[S] Opublikowano wyniki końcowe                   ← System [DODANO K5]
[S] Przyznano nagrodę                             ← A-006 (Główny organizator)
[R] Wygenerowano dyplom                           ← System [DODANO K5]
[R] Otrzymano dyplom                              ← A-001 (Zawodnik)

[S] Zwrócono pakiet startowy                      ← A-001 → A-005
  ↳ pozytywna: Kaucja została zwrócona
  ↳ negatywna (timer): Upłynął czas zwrotu pakietu → Nie zwrócono pakietu startowego → Kaucja zatrzymana
```

---

## Grupy zdarzeń — kandydaci na granice BC

| Grupa | Zdarzenia | Sygnał granicy |
|---|---|---|
| KonfiguracjaMaratonu | FAZA 0 | pivotal output: "Zarejestrowano maraton" |
| Rejestracja | FAZA 1 | pivot: "Potwierdzono rejestrację" → dane przechodzą do Opłaty |
| OpłataZaUdział | FAZA 2 | pivot: "Opłacono uczestnictwo" → dane przechodzą do PrzydziałNumeruStartowego |
| PrzydziałNumeruStartowego | FAZA 3 | pivot: "Przypisano numer startowy" → warunek generacji grup |
| GeneracjaGrupStartowych | FAZA 4 (pierwsze okno) | PE-001 |
| GrupyStartowe | FAZA 4 (zmiana, okno po PE-001) | PE-002 (blokada) |
| WeryfikacjaStartu | FAZA 5 | pivot: "Wydano pakiet startowy" → RFID aktywny |
| StartZawodników | FAZA 6 | PE-003 |
| PomiarCzasu | FAZA 7 | PE-004 |
| ZakończenieZawodów | FAZA 8 | koniec procesu |
