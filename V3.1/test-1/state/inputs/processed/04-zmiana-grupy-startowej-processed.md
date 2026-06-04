<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture -->
<!-- used-in: big-picture, process-level -->
<!-- converted: lossless -->

# Zmiana grupy startowej

## Źródła
- zmiania_grupy_startowej_DL.png (Design Level diagram)
- zminia_grupystartowej.png
- grupy_startowe_ds.png (Domain Story)

---

## Zdarzenia domenowe

| Zdarzenie | Aktor | Obszar |
|---|---|---|
| Złożono wniosek o zmianę grupy | zawodnik | Zmiana grupy |
| Zmieniono grupę | system | Zmiana grupy |
| Nie udało się zmienić grupy | system | Zmiana grupy |
| Nie przypisano do ostatecznej grupy | system | Zmiana grupy |

## Warunki wymagane do zmiany grupy (wszystkie muszą być spełnione)

1. Nie przekroczono terminu zmiany grup (ostateczny termin zmiany grupy)
2. Grupa docelowa ma wolne miejsca
3. Zawodnik nie zmieniał wcześniej grupy
4. Grupa docelowa jest z tego samego dystansu
5. Grupy startowe zostały wygenerowane

## Stany wejściowe (pre-conditions)

- Opłacono dystans
- Przypisano numer startowy

## Reguły biznesowe

- Zmiana grupy możliwa tylko w pewnym oknie czasowym (różowy sticky)
- Reguły zmiany grupy: typ, limit, dystans
- Zawodnik może zmienić grupę tylko raz

## Granice kontekstu

- **GrupyStartowe** (agregat): przechowuje informacje o grupach, terminach, miejscach
- **Zmiany grup** (agregat): rejestruje historię zmian

## Hot Spoty / Otwarte pytania

- Co się dzieje jeśli zawodnik zmienił dystans po przypisaniu grupy?
- Czy jest możliwość zmiany grupy przez organizatora (override)?
