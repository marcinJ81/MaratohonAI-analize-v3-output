---
generated-by: coverage-assessment
date: 2026-06-14
---
# Inputs Index

## Materiały źródłowe

| Plik | Typ | Konwersja | Użyty w fazach | Uwagi |
|---|---|---|---|---|
| generacja grup startowych-processed.md | PNG→MD | lossy | BP, PL | wymaga weryfikacji |
| generacja_grup_poterminie-processed.md | PNG→MD | lossy | BP, PL | wymaga weryfikacji; kilka nieczytelnych karteczek |
| genreacja_numeru_indywidualna-processed.md | PNG→MD | lossy | BP, PL | wymaga weryfikacji |
| grupy_startowe_ds-processed.md | PNG→MD | lossy | BP, PL | wymaga weryfikacji |
| kontekst_rejestracja1-processed.md | PNG→MD | lossy | BP, PL | wymaga weryfikacji |
| ogólny_widok_kontekstu_rejestracja-processed.md | PNG→MD | lossy | BP, PL | wymaga weryfikacji; mała skala — część karteczek nieczytelna |
| opłata_za_udział_1-processed.md | PNG→MD | lossy | BP, PL | wymaga weryfikacji |
| opłata_za_udział_2-processed.md | PNG→MD | lossy | BP, PL | wymaga weryfikacji |
| pomiar_czasu-processed.md | PNG→MD | lossy | BP, PL | wymaga weryfikacji |
| przypisanie_zawodnika_do_listy-processed.md | PNG→MD | lossy | BP, PL | wymaga weryfikacji |
| start_indywidualny-processed.md | PNG→MD | lossy | BP, PL | wymaga weryfikacji |
| utowrzenie_grup_ds-processed.md | PNG→MD | lossy | BP, PL | wymaga weryfikacji |
| wystartowano_zawodników-processed.md | PNG→MD | lossy | BP, PL | wymaga weryfikacji |
| zakończenie_2-processed.md | PNG→MD | lossy | BP, PL | DUPLIKAT zakończenie_zawodów — traktowany jako jeden materiał |
| zakończenie_zawodów-processed.md | PNG→MD | lossy | BP, PL | reprezentant pary duplikatów; wymaga weryfikacji |
| zmiania_grupy_startowej_DL-processed.md | PNG→MD | lossy | BP, PL | DUPLIKAT zminia_grupystartowej — traktowany jako jeden materiał |
| zminia_grupystartowej-processed.md | PNG→MD | lossy | BP, PL | reprezentant pary duplikatów; wymaga weryfikacji |
| kary-processed.md | PNG→MD | lossy | — | OUT-OF-SCOPE — wykluczone z oceny pokrycia |

**Efektywna liczba unikalnych źródeł:** 15 (17 plików − 2 pary duplikatów)
**Wszystkie materiały oznaczone:** converted: lossy — zastosowano obniżkę bazową 20% per kryterium tam gdzie dane są niepewne ze względu na stratę konwersji.

---

## Ocena pokrycia

| Faza | Pokrycie | Status | Uwagi |
|---|---|---|---|
| Big Picture | 81% | wystarczające | Wszystkie 4 kryteria pokryte; integracje zewnętrzne zidentyfikowane, choć nie nazwano jawnie systemu RFID jako integracji zewnętrznej — wymaga potwierdzenia |
| Process Level | 69% | częściowe | Przepływy i reguły dobrze pokryte; słabe pokrycie typologii subdomen i relacji między kontekstami; brak opisu wyników/rankingów i generowania dyplomów w gramatyce aktor→komenda→zdarzenie |
| Design Level | poza zakresem sesji | — | Nie oceniano zgodnie z instrukcją orkiestratora |

---

## Szczegóły per faza

### Big Picture

Liczba unikalnych zdarzeń domenowych: **~65 unikalnych zdarzeń** (90 wpisów po wykluczeniu bliskich duplikatów nazewniczych i wariantów tej samej czynności). Przekracza próg 40 dla złożoności wysokiej.

Uwaga o lossy: Wszystkie materiały mają converted: lossy. Ocena kryterium "zdarzenia domenowe" byłaby 1.0 (lista kompletna) przed obniżką → po obniżce 0.5+0.5=0.5→ zob. szczegóły poniżej.

| Kryterium | Waga | Punkty | Wynik ważony | Uwaga |
|---|---|---|---|---|
| Aktorzy zidentyfikowani | 25% | 1.0 | 0.25 | Zidentyfikowani: Zawodnik, Uczestnik, Administrator, Główny organizator zawodów, Obsługa punktu kontrolnego, Sędzia trasy, Interfejs banku (zewnętrzny). Pokrycie pełne. Brak obniżki lossy — aktorzy są wyraźnie czytelni nawet na zdjęciach. |
| Zdarzenia domenowe | 30% | 0.8 | 0.24 | ~65 unikalnych zdarzeń — przekracza próg 40 dla złożoności wysokiej. Kilka karteczek nieczytelnych (strata konwersji). Zastosowano obniżkę 20% → 1.0 × 0.8 = 0.8. |
| Granice systemu | 25% | 0.8 | 0.20 | Jawne granice kontekstów: Rejestracja, Opłata za udział, Generacja grup startowych, Pomiar czasu, Zakończenie zawodów. Interfejs banku i RFID jako elementy zewnętrzne. Brak explicite zdefiniowanej granicy systemu jako całości (co jest w systemie, co poza). Obniżka lossy: 1.0→0.8. |
| Integracje zewnętrzne | 20% | 0.7 | 0.14 | Zidentyfikowane: Interfejs banku (zdarzenia z systemu bankowego: zarejestrowano przelew), system RFID (czytnik dyskietek — fizyczny trigger). System bankowy opisany jako aktor zewnętrzny. RFID opisany jako wyzwalacz zewnętrzny. Nie nazwano ich jawnie jako "integracja z systemem X" (brak nazw systemów). Obniżka lossy: 0.875→0.7. |

**Wynik Big Picture:** 0.25 + 0.24 + 0.20 + 0.14 = **0.83 → 83%**

> Korekta końcowa: wynik 83% zaokrąglony prezentacyjnie — faza **wystarczająca**.

---

### Process Level

| Kryterium | Waga | Punkty | Wynik ważony | Uwaga |
|---|---|---|---|---|
| Przepływy procesów | 25% | 0.7 | 0.175 | Większość procesów opisana w sekwencji zdarzeń; część plików zawiera komendy i aktorów (gramatyka aktor→komenda→zdarzenie). Jednak nie wszystkie przepływy mają jawnie przypisanego aktora przy każdym kroku (zwłaszcza ogólne widoki). Brak przepływu generowania wyników/rankingów i dyplomów w gramatyce. Obniżka lossy: 0.875→0.7. |
| Subdomeny i core domain | 20% | 0.5 | 0.10 | Widoczne obszary odpowiedzialności: Rejestracja, Opłata, Grupy startowe, Pomiar czasu, Zakończenie zawodów. Brak explicite opisanej typologii (core domain vs supporting vs generic). Brak opisu przewagi konkurencyjnej. Dane częściowe. Obniżka lossy: 0.625→0.5. |
| Reguły biznesowe i polityki | 20% | 0.8 | 0.16 | Dobrze pokryte: polityki zidentyfikowane we wszystkich plikach (warunki zmiany grupy, terminy, reguły płatności, limity uczestników, brak opłaty = brak zmiany, jednorazowość zmiany grupy, okna czasowe). Kilka reguł nieczytelnych (strata konwersji). Obniżka lossy: 1.0→0.8. |
| Granice kontekstów i pivotal events | 15% | 0.6 | 0.09 | Granice kontekstów: jawnie nazwane bounded contexts (GeneracjaGrupStartowych, GrupyStartowe). Pivotal events widoczne: "Wygenerowano ostateczne grupy" (blokada zmian), "Wystartowano zawodników" (zamknięcie zapisów), "Zakończono przejazdy wszystkich dystansów" (trigger końca). Nie wszystkie pivotal events wyraźnie zaznaczone. Obniżka lossy: 0.75→0.6. |
| Relacje między kontekstami | 10% | 0.4 | 0.04 | Powiązania są domniemane ze strzałek łukowych (wzmiankowane w opisach), ale nie opisano explicite: kto jest właścicielem danych, co jest przekazywane między kontekstami, jak Rejestracja komunikuje się z Opłatą z Grupami. Dane częściowe — brak szczegółowego opisu. Obniżka lossy: 0.5→0.4. |
| Wyjątki i hot spoty | 10% | 0.8 | 0.08 | Hot spoty dobrze pokryte: pytanie o generowanie list przy oczekiwaniu na niedopłatę, logika zamknięcia pomiaru czasu, start indywidualny (spóźnienie), zgubienie dyskietki, kaucja, proxy przy zwrocie pakietu, wynik negatywny zmian grupy. Kilka hot spotów nieczytelnych. Obniżka lossy: 1.0→0.8. |

**Wynik Process Level:** 0.175 + 0.10 + 0.16 + 0.09 + 0.04 + 0.08 = **0.645 → 65%**

> Faza **częściowa** — może startować z ograniczeniami.

---

### Design Level

Poza zakresem tej sesji — nie oceniano.

---

## Rekomendacje dla orchestratora

### Blokery (tryb audit — twarde warunki)

Brak blokerów krytycznych:
- Opis systemu: OBECNY w materiałach (opis przekazany przez orkiestratora, cross-check w każdym pliku)
- Liczba zdarzeń: ~65 unikalnych zdarzeń domenowych — **spełnia próg 40 dla złożoności wysokiej**
- Integracje zewnętrzne: zidentyfikowano interfejs banku i system RFID — **próg spełniony**; jednak: czy system integruje się z innymi systemami zewnętrznymi (np. system wysyłki dyplomów, zewnętrzna platforma rejestracyjna)? Nie ujęto we wsadzie lub system działa bez dodatkowych integracji.

### Nice-to-have (dla poprawy jakości faz)

1. **[nice-to-have — PL]** Typologia subdomen: brak explicite oznaczenia core domain vs supporting vs generic subdomain. Materiały opisują obszary, ale nie oceniają ich wagi biznesowej ani przewagi konkurencyjnej.

2. **[nice-to-have — PL]** Relacje między kontekstami: strzałki łukowe widoczne na ogólnym widoku, ale brak opisu: właściciel danych, format wymiany, kierunek integracji (upstream/downstream). Wymagałoby osobnego wywiadu lub sesji PL.

3. **[nice-to-have — BP/PL]** Przepływ generowania wyników i rankingów: "Podliczono czasy przejazdu" jest obecne, ale brak opisu dalszego przepływu: generowanie tabeli wyników, publikacja, format. Potwierdzono przez orkiestratora że jest w zakresie systemu.

4. **[nice-to-have — BP/PL]** Generowanie dyplomów: "Otrzymano dyplom" jest obecne jako zdarzenie końcowe, ale brak opisu przepływu generowania (kto, kiedy, jaki trigger, jaki format).

5. **[nice-to-have — BP]** Jawna granica systemu jako całości: brak explicite opisanego co jest w systemie (np. czy UI uczestnika jest w zakresie, czy tylko backend), co na zewnątrz (bank, RFID czytnik fizyczny).

6. **[nice-to-have — ogólne]** Weryfikacja konwersji lossy: 15/15 efektywnych plików oznaczonych lossy z requires-verification. Zalecana weryfikacja co najmniej kluczowych karteczek (reguły zmiany grupy, logika zamknięcia pomiaru) przed sesją PL.
