<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture + process-level -->
<!-- used-in: phase-1, phase-2 -->
<!-- converted: lossy — dwa screenshoty tego samego kontekstu (Frame 46 i inny widok) -->
<!-- source-files: zakończenie_2.png, zakończenie_zawodów.png -->

# Kontekst: Zakończenie zawodów

## Zdarzenia domenowe
| Zdarzenie | Aktor | Obszar |
|---|---|---|
| zakończono przejazdy wszystkich dystansów | system | Trigger zakończenia |
| przyznano nagrodę | główny organizator zawodów | Nagrody |
| Zwrócono pakiet startowy | zawodnik / osoba uprawniona | Zwrot pakietu |
| kaucja została zwrócona | obsługa punktu kontrolnego | Zwrot pakietu |
| upłynął czas zwrotu pakietu (timer) | system (timer) | Zwrot pakietu — ścieżka alternatywna |
| nie zwrócono pakietu startowego | zawodnik / osoba uprawniona | Zwrot pakietu — ścieżka alternatywna |
| kaucja została zatrzymana | system / obsługa | Zwrot pakietu — ścieżka alternatywna |
| Otrzymano dyplom | zawodnik | Dyplom |

## Aktorzy
| Aktor | Rola | Zewnętrzny? |
|---|---|---|
| główny organizator zawodów | przyznaje nagrody | nie |
| zawodnik / osoba uprawniona przez zawodnika | zwraca pakiet, odbiera dyplom | nie |
| obsługa punktu kontrolnego | przyjmuje pakiet, zwalnia kaucję | nie |
| system (timer) | wyzwala termin zwrotu pakietu | nie |

## Reguły / Polityki
- Czas do zwrotu pakietu startowego ustalony w regulaminie
- Nagrody przyznawane na podstawie reguł regulaminu maratonu
- Brak zwrotu pakietu w terminie → kaucja zatrzymana

## Subprocesy
1. **Nagrody** — główny organizator przyznaje nagrody według regulaminu
2. **Zwrot pakietu startowego** — zawodnik zwraca pakiet, obsługa zwalnia kaucję; jeśli nie zwróci w terminie — kaucja zatrzymana
3. **Dyplom** — zawodnik otrzymuje dyplom (po zakończeniu)

## Granice i integracje
- Pomiar czasu → Zakończenie zawodów: `zakończono przejazdy wszystkich dystansów` jest triggerem
- Przygotowanie do startu (kaucja za dyskietkę) → Zakończenie zawodów: kaucja zwracana przy oddaniu pakietu

## Hot Spoty
_(brak)_

## Pytania otwarte
- Kto/co wystawia dyplom — system automatycznie czy ręcznie?
- Jak obsługiwane są wyniki i ranking — gdzie jest ten kontekst w ES?
- Jak działa przyznanie nagrody jeśli zawodnik ma nałożoną karę?
