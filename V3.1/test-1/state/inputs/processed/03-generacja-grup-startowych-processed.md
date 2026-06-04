<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture -->
<!-- used-in: big-picture, process-level -->
<!-- converted: lossless -->

# Generacja grup startowych

## Źródła
- generacja grup startowych.png
- genreacja_numeru_indywidualna.png
- generacja_grup_poterminie.png
- utowrzenie_grup_ds.png

---

## Zdarzenia domenowe

| Zdarzenie | Aktor | Obszar |
|---|---|---|
| Przypisano numer startowy | system | Generacja grup |
| Przypisano zawodników do grupy | administrator | Generacja grup |
| Złożono wniosek o zmianę grupy | zawodnik | Generacja grup |
| Przyjęto wniosek o zmianę grupy | administrator | Generacja grup |
| Nie przyjęto wniosku o zmianę grupy | administrator | Generacja grup |
| Wygenerowano ostateczne grupy | system | Generacja grup |
| Nie przypisano do ostatecznej grupy | system | Generacja grup |
| Grupy startowe wygenerowane | system | Generacja grup |
| Nie udało się wygenerować grup startowych | system | Generacja grup |
| Dołączono do grupy (po terminie) | system | Generacja grup |

## Aktorzy

| Nazwa | Rola | Zewnętrzny? |
|---|---|---|
| administrator | Uruchamia generację, zarządza wnioskami | Nie |
| zawodnik | Składa wnioski o zmianę grupy | Nie |
| system | Automatyczna generacja grup | Nie (automatyczny) |

## Reguły biznesowe

- Grupy generowane dla zawodników z **przypisanym numerem startowym**
- Minimalna liczba uczestników wymagana do wygenerowania grupy
- Ostateczny termin generowania grup zapisany w regulaminie
- Generowanie odbywa się w określonym w regulaminie terminie
- Zmiana dystansu przed wygenerowaniem grup → inny przepływ
- Zmiana dystansu po wygenerowaniu grup → jeszcze inny przepływ
- Zawodnicy bez grupy po generacji → automatycznie przypisani przez system
- Scenariusz po terminie: płacenie i dołączanie do grup po standardowym terminie

## Granice kontekstu

- **Generacja grup startowych**: główny kontekst grupowania zawodników
- Powiązany z: Rejestracja (numer startowy), Opłata za udział (opłacony dystans)

## Hot Spoty / Otwarte pytania

- Co się dzieje z zawodnikami którzy zapłacili ale nie zostali przypisani do grupy?
- Jak działa generacja po terminie — czy to osobny flow czy rozszerzenie głównego?
- Ilu zawodników to minimalna liczba uczestników?
