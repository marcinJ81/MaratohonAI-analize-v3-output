<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture -->
<!-- used-in: big-picture, process-level -->
<!-- converted: lossless -->

# Przygotowanie zawodnika do startu

## Źródła
- przypisanie_zawodnika_do_listy.png
- start_indywidualny.png (Frame 50)
- wystartowano_zawodników.png (Frame 47)

---

## Zdarzenia domenowe

| Zdarzenie | Aktor | Obszar |
|---|---|---|
| Zweryfikowano tożsamość zawodnika | obsługa punktu kontrolnego | Przygotowanie do startu |
| Zawodnik nie przeszedł weryfikacji | obsługa punktu kontrolnego | Przygotowanie do startu |
| Podpisano oświadczenie zawodnika | zawodnik | Przygotowanie do startu |
| Zawodnik nie podpisał oświadczenia | zawodnik | Przygotowanie do startu |
| Wydano pakiet startowy | obsługa punktu kontrolnego | Przygotowanie do startu |
| Zdyskwalifikowano zawodnika | obsługa punktu kontrolnego | Przygotowanie do startu |
| Zweryfikowano zawodnika przed startem | obsługa punktu kontrolnego | Weryfikacja startowa |
| Zgłoszono indywidualny start | zawodnik | Start indywidualny |
| Wyciągnięto zawodnika z grupy | administrator | Start indywidualny |
| Zgubiono dyskietkę z numerem startowym | zawodnik | Zgubienie dyskietki |
| Przejęto kaucję | obsługa punktu kontrolnego | Zgubienie dyskietki |
| Odpięto numer startowy | obsługa punktu kontrolnego | Zgubienie dyskietki |
| Przypisano nowy numer startowy | obsługa punktu kontrolnego | Zgubienie dyskietki |
| Wydano nową dyskietkę | obsługa punktu kontrolnego | Zgubienie dyskietki |

## Sub-procesy

### Kwalifikacja zawodnika do zawodnika startu
- Weryfikacja na podstawie listy startowej
- Jeśli nie zweryfikowany w biurze → ścieżka alternatywna
- Wymagane podpisanie oświadczenia
- Wydanie pakietu startowego

### Weryfikacja zawodnika (przy starcie)
- Weryfikacja na podstawie listy startowej
- Możliwa dyskwalifikacja

### Start indywidualny
- Zawodnik może zgłosić indywidualny start (np. spóźnienie)
- Administrator wyciąga zawodnika z grupy

### Zgubienie dyskietki z numerem startowym
- Pobranie kaucji
- Wymiana numeru i dyskietki

## Aktorzy

| Nazwa | Rola | Zewnętrzny? |
|---|---|---|
| obsługa punktu kontrolnego | Weryfikuje, wydaje pakiety, obsługuje incydenty | Nie |
| zawodnik | Przechodzi weryfikację, podpisuje oświadczenia | Nie |
| administrator | Zarządza startem indywidualnym | Nie |

## Granice kontekstu

- **Przygotowanie do startu**: kwalifikacja i weryfikacja zawodnika
- **Obsługa incydentów startowych**: zgubienie dyskietki, start indywidualny

## Hot Spoty / Otwarte pytania

- Co jeśli zawodnik odmawia podpisania oświadczenia?
- Jaka jest wysokość kaucji za dyskietkę?
- Czy lista startowa jest generowana z systemu czy ręcznie?
