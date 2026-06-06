<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture + process-level -->
<!-- used-in: phase-1, phase-2 -->
<!-- converted: lossy — bardzo mały fragment ES, mało szczegółów -->
<!-- source-files: kary.png -->

# Kontekst: Nakładanie kar

## Zdarzenia domenowe
| Zdarzenie | Aktor | Obszar |
|---|---|---|
| nałożono karę na zawodnika za złamanie regulaminu | sędzia trasy | Kary regulaminowe |
| naliczono karę za pominięte punkty kontrolne | sędzia trasy | Kary za trasy |

## Aktorzy
| Aktor | Rola | Zewnętrzny? |
|---|---|---|
| sędzia trasy | nakłada kary | nie |

## Reguły / Polityki
- Kary regulamin maratonu (policy zewnętrzna)
- Kara za pominięcie punktu kontrolnego

## Granice i integracje
- Nakładanie kar → Pomiar czasu / Wyniki: kary wpływają na wyniki końcowe
- Pomiar czasu → Nakładanie kar: system wykrywa pominięte punkty kontrolne

## Hot Spoty
_(fragment bardzo ogólny — brak szczegółów jak kary są reprezentowane w systemie)_

## Pytania otwarte
- Jak kara jest reprezentowana w systemie — czas dodatkowy, punkty, dyskwalifikacja?
- Kto może odwołać się od kary?
- Czy system automatycznie wykrywa pominięte punkty kontrolne czy to ręczna decyzja sędziego?
