---
source: phase-1-seed.md + weryfikacja audit K2a
generated: 2026-06-14
---

# Granice systemu i integracje zewnętrzne

---

## Granica systemu jako całości

System zarządza procesem maratonu rowerowego od konfiguracji przez rejestrację, płatności, grupy startowe, pomiar czasu aż do zakończenia zawodów i wydania dyplomów.

**W systemie:**
- UI uczestnika / zawodnika (rejestracja, zmiana grupy/dystansu)
- UI administratora (zarządzanie uczestnikami, grupami, generacja)
- UI biura zawodów (monitoring checkpointów, real-time display)
- Logika domenowa (wszystkie procesy od FAZY 0 do FAZY 8)
- Generowanie dyplomów (wewnętrzne — potwierdzone przez orkiestratora)
- Generowanie i publikacja wyników / rankingów

**Poza systemem:**
- Fizyczny czytnik RFID (hardware) — wysyła zdarzenia do systemu
- System bankowy — push zdarzeń przelewów do systemu
- Fizyczne pakiety startowe / dyskietki (hardware)
- Decyzje sędziego (kary/dyskwalifikacja POZA ZAKRESEM)

**Hot spot — brak wyraźnej granicy:**
- Strona WWW do wyświetlania wyników real-time: czy to ten sam system czy osobna aplikacja?
- UI uczestnika: aplikacja webowa, mobilna, system zewnętrzny? Nie określono w materiałach.

---

## Granice wewnętrzne — konteksty domenowe

| ID | Kontekst | Opis | Pivotal Events graniczne | Faza |
|---|---|---|---|---|
| BC-01 | KonfiguracjaMaratonu | Parametry maratonu, regulamin, dystanse | "Zarejestrowano maraton" (wejście do BC-02) | FAZA 0 |
| BC-02 | Rejestracja | Rejestracja uczestnika, wybór dystansu | "Potwierdzono rejestrację" → BC-03 | FAZA 1 |
| BC-03 | OpłataZaUdział | Obsługa płatności bankowych, niedopłaty, zwrotów, promocji | "Opłacono uczestnictwo" → BC-04 | FAZA 2 |
| BC-04 | PrzydziałNumeruStartowego | Generowanie i przypisanie numerów | "Przypisano numer startowy" → BC-05 | FAZA 3 |
| BC-05 | GeneracjaGrupStartowych | Automatyczne generowanie grup, warunki wejścia | PE-001 "Wygenerowano ostateczne grupy" | FAZA 4 (pierwsze okno) |
| BC-06 | GrupyStartowe | Zarządzanie grupami — zmiana grupy przez zawodnika | PE-002 "Minął czas na zmiany grupy" | FAZA 4 (drugie okno) |
| BC-07 | WeryfikacjaStartu | Check-in, pakiet startowy, oświadczenie | "Wydano pakiet startowy" → BC-08 | FAZA 5 |
| BC-08 | StartZawodników | Start grup, start indywidualny, obsługa zgubionej dyskietki | PE-003 "Wystartowano zawodników" | FAZA 6 |
| BC-09 | PomiarCzasu | RFID checkpointy, real-time display, przedłużenia | PE-004 "Zakończono przejazdy wszystkich dystansów" | FAZA 7 |
| BC-10 | ZakończenieZawodów | Nagrody, dyplomy, zwrot pakietów, kaucje | Koniec procesu | FAZA 8 |

---

## Integracje zewnętrzne

| ID | System zewnętrzny | Kierunek | Typ integracji | Miejsce na osi | Uwagi |
|---|---|---|---|---|---|
| I-001 | Bank (przelew bankowy) | wchodzi do systemu | async / event-push | BC-03 OpłataZaUdział (FAZA 2) | Bank pushuje zdarzenie "Zarejestrowano przelew" po wykonaniu przelewu przez uczestnika. Brak informacji o modelu API (webhook? polling?). |
| I-002 | System RFID (czytnik dyskietek) | wchodzi do systemu | sync / event-push (fizyczny) | BC-09 PomiarCzasu (FAZA 7) | Czytnik RFID przesyła do systemu sygnał przyłożenia karty. Trigger fizyczny przez zawodnika. Dyskietka wydana w pakiecie startowym (BC-07). |

**Uwaga:** Dyplomy generowane wewnętrznie — brak zewnętrznej platformy druku/wysyłki.
**Uwaga:** Brak zewnętrznej platformy rejestracyjnej — rejestracja wyłącznie w systemie.

---

## Relacje między kontekstami (znane)

```
BC-01 → BC-02    KonfiguracjaMaratonu publikuje parametry → Rejestracja korzysta z listy dystansów
BC-02 → BC-03    Rejestracja → OpłataZaUdział (dane uczestnika + wyliczona opłata)
BC-03 → BC-04    OpłataZaUdział → PrzydziałNumeruStartowego (potwierdzenie opłaty)
BC-04 → BC-05    PrzydziałNumeruStartowego → GeneracjaGrupStartowych (uczestnicy z numerami)
PE-001 → BC-06   GeneracjaGrupStartowych → GrupyStartowe (wygenerowane grupy jako wejście)
BC-06 → BC-07    GrupyStartowe → WeryfikacjaStartu (lista startowa)
BC-07 → BC-08    WeryfikacjaStartu → StartZawodników (pakiet wydany = dyskietka aktywna)
BC-08 → BC-09    StartZawodników → PomiarCzasu (czas startu jako punkt odniesienia)
PE-004 → BC-10   PomiarCzasu → ZakończenieZawodów (czasy przejazdu + ranking)
```

Hot spot: relacje między BC-02 (Rejestracja) i BC-03 (Opłata) są ściśle powiązane;
granica między nimi wymaga decyzji projektowej (czy to jeden kontekst czy dwa).

---

## Pivotal Events — podsumowanie

| ID | Pivotal Event | Dlaczego | Konteksty graniczne |
|---|---|---|---|
| PE-001 | Wygenerowano ostateczne grupy startowe | Punkt bez powrotu dla standardowego przypisania do grup; po tym zdarzeniu tylko zmiana w BC-06 z restrykcjami | BC-05 → BC-06 |
| PE-002 | Minął czas na ewentualne zmiany grupy — przypisanie jest na stałe | Blokada zmian — żadna zmiana grupy po tym zdarzeniu nie jest możliwa | BC-06 → BC-07 |
| PE-003 | Wystartowano zawodników | Aktywacja pomiaru czasu; zamknięcie zapisów; przejście z administracji do operacji | BC-08 → BC-09 |
| PE-004 | Zakończono przejazdy wszystkich dystansów | Trigger końca zawodów; przejście do ceremonii; zamknięcie pomiaru | BC-09 → BC-10 |
