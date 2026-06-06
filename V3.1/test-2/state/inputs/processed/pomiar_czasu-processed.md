<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture + process-level -->
<!-- used-in: phase-1, phase-2 -->
<!-- converted: lossy -->
<!-- source-files: pomiar_czasu.png -->

# Kontekst: Pomiar czasu

## Zdarzenia domenowe
| Zdarzenie | Aktor | Obszar |
|---|---|---|
| zarejestrowano pomiar czasu | system pomiaru czasu | Pomiar |
| Zawodnik zakończył przejazd (dystans deklarowany) | zawodnik | Pomiar dystansu |
| Podliczono czasy przejazdu | system pomiaru czasu | Obliczenia |
| Zakończono przejazd zawodników na danym dystansie | system | Zakończenie dystansu |
| zakończono przejazdy wszystkich dystansów | system | Zakończenie zawodów |
| zgłoszono prośbę o przedłużenie dystansu | zawodnik | Wydłużony dystans |
| przedłużono przejazd zawodnika | obsługa punktu kontrolnego | Wydłużony dystans |
| odnotowano dodatkowy pomiar czasu | system pomiaru czasu | Wydłużony dystans |
| Zawodnik zakończył przejazd (dystans wydłużony) | zawodnik | Wydłużony dystans |

## Aktorzy
| Aktor | Rola | Zewnętrzny? |
|---|---|---|
| zawodnik | przejeżdża, wyzwala pomiar przez przyłożenie dyskietki do czytnika | nie |
| system pomiaru czasu | rejestruje czasy, oblicza wyniki | nie (urządzenie sprzętowe) |
| obsługa punktu kontrolnego | obsługuje prośby o przedłużenie | nie |

## Mechanizm pomiaru
- Zawodnik posiada **dyskietkę** jako identyfikator
- Pomiar czasu = przyłożenie dyskietki do czytnika (wyzwalacz zdarzenia)
- System wie, że wszyscy dojechali albo że minął czas

## Subprocesy
1. **Pomiar czasu zadeklarowanego dystansu** — standardowy przejazd, zawodnik kończy po przejechanym dystansie
2. **Pomiar czasu na wydłużonym dystansie** — zawodnik zgłasza chęć przejechania kolejnego dystansu, przedłużono przejazd, dodatkowy pomiar

## Reguły / Polityki
- Limity na dany dystans zdefiniowane w regulaminie
- Zawodnik może zgłosić prośbę o przedłużenie dystansu (zgłoszono chęć przejechania następnego dystansu)

## Granice i integracje
- Start zawodników → Pomiar czasu: po wystartowaniu grupy zaczyna się pomiar
- Pomiar czasu → Nakładanie kar: pominięte punkty kontrolne są wykrywane w pomiarze
- Pomiar czasu → Zakończenie zawodów: zakończono przejazdy wszystkich dystansów

## Hot Spoty
_(brak zidentyfikowanych explicite)_

## Pytania otwarte
- Jak system obsługuje zawodnika który nie przyłożył dyskietki na mecie (awaria)?
- Czy istnieje ręczna rejestracja czasu jako fallback?
- Jak zdefiniowany jest "koniec" dla zawodów 24H — limit czasu czy limit okrążeń?
