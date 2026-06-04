<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture -->
<!-- used-in: big-picture, process-level -->
<!-- converted: lossless -->

# Nakładanie kar

## Źródła
- kary.png

---

## Zdarzenia domenowe

| Zdarzenie | Aktor | Obszar |
|---|---|---|
| Nałożono karę na zawodnika za złamanie regulaminu | sędzia trasy | Kary |
| Naliczono karę za pominięte punkty kontrolne | sędzia trasy / system | Kary |

## Aktorzy

| Nazwa | Rola | Zewnętrzny? |
|---|---|---|
| sędzia trasy | Nakłada kary za złamanie regulaminu | Nie |

## Reguły biznesowe

- Kary regulamin maratonu (różowy sticky)
- Kara za złamanie regulaminu (nałożona przez sędziego)
- Kara za pominięte punkty kontrolne (może być automatyczna)

## Granice kontekstu

- **Nakładanie kar**: oddzielny kontekst zarządzania karami
- Powiązany z: Pomiar czasu (pominięte punkty = brak pomiaru w punkcie kontrolnym)
- Powiązany z: Zakończenie zawodów (kary wpływają na wyniki)

## Hot Spoty / Otwarte pytania

- Czy kary są odejmowane od czasu czy są dyskwalifikacyjne?
- Jak system wie które punkty kontrolne zostały pominięte?
- Czy sędzia nakłada kary w systemie czy jest to oddzielny proces?

## Uwaga

Ten kontekst jest najmniej opisany w materiałach — tylko 2 zdarzenia widoczne.
Może wymagać dodatkowego warsztatowania.
