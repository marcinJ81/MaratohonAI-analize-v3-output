<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture + process-level -->
<!-- used-in: phase-1, phase-2 -->
<!-- converted: lossy -->
<!-- source-files: wystartowano_zawodników.png, start_indywidualny.png -->

# Kontekst: Start zawodników

## Zdarzenia domenowe
| Zdarzenie | Aktor | Obszar |
|---|---|---|
| Wystartowano zawodników na danym dystansie | administrator | Zamknięcie zapisów |
| zamknięto zapisy na dany dystans | administrator | Zamknięcie zapisów |
| Wystartowano grupę | sędzia trasy | Start grupowy |
| Zarejestrowano czas startu | sędzia trasy | Start grupowy |
| Wystartowano zawodnika | sędzia trasy | Start grupowy |
| zdyskwalifikowano zawodnika (przy starcie) | sędzia trasy | Start grupowy |
| zakończono udział przedwcześnie | sędzia trasy | Start grupowy |
| Zgłoszono indywidualny start | zawodnik | Start indywidualny |
| wyciągnięto zawodnika z grupy | administrator | Start indywidualny |
| zgubiono dyskietkę z numerem startowym | zawodnik | Utrata dyskietki |
| przejęto kaucję | obsługa punktu kontrolnego | Utrata dyskietki |
| odpięto numer startowy | obsługa punktu kontrolnego | Utrata dyskietki |
| przypisano numer startowy (nowy) | obsługa punktu kontrolnego | Utrata dyskietki |
| wydano nową dyskietkę | obsługa punktu kontrolnego | Utrata dyskietki |

## Aktorzy
| Aktor | Rola | Zewnętrzny? |
|---|---|---|
| administrator | zamyka zapisy, obsługuje start indywidualny | nie |
| sędzia trasy | startuje grupy i zawodników, rejestruje czas startu | nie |
| zawodnik | zgłasza start indywidualny, może zgubić dyskietkę | nie |
| obsługa punktu kontrolnego | obsługuje utratę dyskietki | nie |

## Subprocesy
1. **Zamknięcie zapisów** — administrator zamyka zapisy na dany dystans przed startem
2. **Start grupowy** — sędzia trasy startuje grupę, rejestruje czas, może zdyskwalifikować zawodnika
3. **Start indywidualny** — zawodnik (np. spóźniony) zgłasza indywidualny start, wyciągany z grupy
4. **Zgubienie dyskietki z numerem startowym** — kaucja, nowa dyskietka + nowy numer

## Reguły / Polityki
- Start indywidualny możliwy np. w przypadku spóźnienia
- Dyskietka = identyfikator zawodnika, utrata wymaga kaucji i wymiany

## Granice i integracje
- Grupy startowe → Start zawodników: wygenerowane grupy wchodzą do procesu startu
- Przygotowanie do startu → Start zawodników: zweryfikowani zawodnicy
- Start zawodników → Pomiar czasu: po rejestracji czasu startu zaczyna się pomiar

## Hot Spoty
| Opis | Priorytet |
|---|---|
| Start indywidualny np. spóźnienie — jak rejestrować czas? | nice-to-have |

## Pytania otwarte
- Czy zawodnik zdyskwalifikowany przy starcie może odwołać decyzję?
- Jak działa start indywidualny w kontekście pomiaru czasu — inny punkt startu?
