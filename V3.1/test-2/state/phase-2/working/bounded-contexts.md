# Bounded Contexts — working file

## Blokery z Fazy 1 — wyjaśnione
- Q-001: Uczestnik z niedopłatą za zmianę dystansu → pozostaje na opłaconym dystansie. Generacja grup nie jest blokowana — taki uczestnik trafia do grup na swoim OPŁACONYM dystansie.
- Q-002: Start indywidualny → zawodnik ma OSOBNY czas startu (nie dziedziczy czasu grupy)
- Q-005: Podstawa rankingu = czas STARTU GRUPY do przekroczenia mety (nie indywidualny start, wyjątek: start indywidualny = własny czas startu)

## Bounded Contexts

| ID | Nazwa | Odpowiedzialność | Typ |
|---|---|---|---|
| BC-01 | Konfiguracja maratonu | Definiuje parametry zawodów: dystanse, regulamin, terminy, reguły kar | Supporting |
| BC-02 | Rejestracja | Zarządza cyklem życia uczestnika: zapis, weryfikacja danych, wybór i zmiana dystansu | Supporting |
| BC-03 | Opłata za udział | Obsługuje płatności, promocje, niedopłaty i zwroty nadpłat | Generic |
| BC-04 | Grupy startowe | Generuje i zarządza grupami startowymi per opłacony dystans | Supporting |
| BC-05 | Przygotowanie do startu | Weryfikuje zawodników przed startem, wydaje pakiety i dyskietki | Supporting |
| BC-06 | Start zawodników | Rejestruje moment startu grupy lub indywidualny, zamyka zapisy | Supporting |
| BC-07 | Pomiar czasu | Rejestruje przejazdy zawodników przez punkty kontrolne i metę (dyskietka + czytnik) | Core |
| BC-08 | Wyniki i ranking | Oblicza i publikuje ranking na żywo i końcowy (per dystans × płeć, z karami) | Core |
| BC-09 | Zarządzanie karami | Nakłada kary regulaminowe wpływające na pozycję w rankingu | Supporting |
| BC-10 | Zakończenie zawodów | Obsługuje nagrody, zwrot pakietów startowych i dyplomy | Supporting |
