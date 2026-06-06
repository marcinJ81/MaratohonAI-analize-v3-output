<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture + process-level -->
<!-- used-in: phase-1, phase-2 -->
<!-- converted: lossy -->
<!-- source-files: generacja grup startowych.png, generacja_grup_poterminie.png, grupy_startowe_ds.png, utowrzenie_grup_ds.png, genreacja_numeru_indywidualna.png, zmiania_grupy_startowej_DL.png, zminia_grupystartowej.png -->

# Kontekst: Grupy startowe

## Zdarzenia domenowe
| Zdarzenie | Aktor | Obszar |
|---|---|---|
| Przypisano zawodnika do grupy | system / administrator | Generacja grup |
| Wygenerowano grupy startowe | system | Generacja grup |
| Nie udało się wygenerować grup startowych | system | Generacja grup |
| złożono wniosek o zmianę grupy | zawodnik | Zmiana grupy |
| Przyjęto wniosek o zmianę grupy | system | Zmiana grupy |
| Nie przyjęto wniosku o zmianę grupy | system | Zmiana grupy |
| Zmieniono grupę | system | Zmiana grupy |
| nie udało się zmienić grupy | system | Zmiana grupy |
| nie przypisano do ostatecznej grupy | system | Zmiana grupy |
| dołączono do grupy (po terminie) | system | Generacja po terminie |
| Zmiana dystansu przed wygenerowaniem grup | zawodnik | Zmiana dystansu |
| Zmiana dystansu po wygenerowaniu grup | zawodnik | Zmiana dystansu |

## Aktorzy
| Aktor | Rola | Zewnętrzny? |
|---|---|---|
| administrator | ręczna generacja, obsługa wniosków | nie |
| zawodnik | składa wnioski o zmianę grupy | nie |
| system | automatyczna generacja, walidacja warunków | nie |

## Reguły generacji grup (polityki)
- Minimalna liczba uczestników musi być spełniona
- Musi być przynajmniej X uczestników z numerami startowymi
- Nie przekroczono ostatecznego terminu generowania grup
- Generowanie grup odbywa się w określonym regulaminem terminie

## Reguły zmiany grupy (polityki)
- Nie przekroczono terminu zmiany grup
- Grupa docelowa ma wolne miejsca
- Zawodnik nie zmieniał wcześniej grupy
- Grupa docelowa jest z tego samego dystansu
- Grupy startowe zostały wygenerowane
- Zmiana grupy jest możliwa tylko w pewnym oknie czasowym
- Reguły: typ limit, dystans

## Specjalne scenariusze
- **Generacja po terminie**: zawodnik dołącza po opłaceniu dystansu gdy grupy już wygenerowane → system dołącza do grupy jeśli nie przekroczono terminu dołączenia do dystansu
- **Automatyczna generacja**: system przydziela zawodnika do grupy (mniej czasu na ewentualne zmiany)
- **Zmiana dystansu przed generacją**: złożono wniosek o zmianę dystansu → normalny flow
- **Zmiana dystansu po generacji**: złożono wniosek o zmianę dystansu → bardziej złożony flow (nie był przypisany do ostatecznej grupy)

## Granice i integracje
- Rejestracja → Grupy startowe: po przypisaniu numeru startowego
- Opłata za udział → Grupy startowe: po opłaceniu uczestnictwa
- Grupy startowe → Start zawodników: wygenerowane grupy startowe są wejściem do startu

## Hot Spoty
_(brak zidentyfikowanych explicite — ale zmiana dystansu po wygenerowaniu grup jest scenariuszem wymagającym uwagi)_

## Pytania otwarte
- Czy zawodnik może złożyć wniosek o zmianę grupy przed wygenerowaniem grup?
- Jak system obsługuje zawodnika bez przypisanej grupy w dniu startu?
