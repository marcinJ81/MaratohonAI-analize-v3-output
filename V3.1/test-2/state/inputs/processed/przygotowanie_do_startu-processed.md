<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture + process-level -->
<!-- used-in: phase-1, phase-2 -->
<!-- converted: lossy -->
<!-- source-files: przypisanie_zawodnika_do_listy.png -->

# Kontekst: Przygotowanie zawodnika do startu

## Zdarzenia domenowe
| Zdarzenie | Aktor | Obszar |
|---|---|---|
| Zweryfikowano tożsamość zawodnika | obsługa punktu kontrolnego | Kwalifikacja |
| zawodnik nie przeszedł weryfikacji | obsługa punktu kontrolnego | Kwalifikacja |
| Podpisano oświadczenie zawodnika | zawodnik | Kwalifikacja |
| zawodnik nie podpisał oświadczenia | zawodnik | Kwalifikacja |
| zdyskwalifikowano zawodnika | obsługa punktu kontrolnego | Kwalifikacja |
| Wydano pakiet startowy | obsługa punktu kontrolnego | Kwalifikacja |
| weryfikacja na podstawie listy startowej | obsługa punktu kontrolnego | Weryfikacja pre-start |
| Zweryfikowano zawodnika przed startem | obsługa punktu kontrolnego | Weryfikacja pre-start |
| zawodnik nie przeszedł weryfikacji (przed startem) | obsługa punktu kontrolnego | Weryfikacja pre-start |
| zdyskwalifikowano zawodnika (przed startem) | obsługa punktu kontrolnego | Weryfikacja pre-start |

## Aktorzy
| Aktor | Rola | Zewnętrzny? |
|---|---|---|
| obsługa punktu kontrolnego | weryfikacja tożsamości, wydanie pakietu | nie |
| zawodnik | podpisuje oświadczenie, odbiera pakiet | nie |

## Granice i integracje
- Rejestracja → Przygotowanie do startu: lista startowa jest wejściem do weryfikacji
- Przygotowanie do startu → Start zawodników: zawodnik zweryfikowany może wystartować

## Reguły / Polityki
- Weryfikacja odbywa się na podstawie listy startowej
- Zawodnik musi podpisać oświadczenie (jeśli nie podpisał w biurze)
- Brak weryfikacji tożsamości lub brak podpisu → dyskwalifikacja

## Subprocesy
1. **Kwalifikacja zawodnika do zawodnika startu** — weryfikacja tożsamości + podpisanie oświadczenia → wydanie pakietu startowego
2. **Weryfikacja zawodnika przed startem** — weryfikacja na podstawie listy startowej bezpośrednio przed startem

## Hot Spoty
_(brak)_

## Pytania otwarte
- Czy oba procesy (kwalifikacja + weryfikacja przed startem) zawsze są wymagane, czy jeden może zastąpić drugi?
- Jak obsługiwany jest zawodnik który był wcześniej zdyskwalifikowany — czy może odwołać?
