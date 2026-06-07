# Bounded Contexts — Process Level

| ID | Nazwa | Odpowiedzialność | Typ | Zdarzenia |
|---|---|---|---|---|
| BC-01 | Konfiguracja maratonu | Definiuje zasady, regulamin i parametry zawodów (terminy, limity) | Supporting | E-001 |
| BC-02 | Rejestracja | Zarządza cyklem życia uczestnika: od zgłoszenia do gotowości do startu | Supporting | E-010..E-017 |
| BC-03 | Opłaty | Obsługuje płatności, zwroty, niedopłaty i status finansowy uczestnictwa | Generic | E-020..E-032 |
| BC-04 | Grupy startowe | Generuje i zarządza grupami startowymi; obsługuje zmiany w obrębie reguł regulaminu | **Core** | E-040..E-051 |
| BC-05 | Przygotowanie do startu | Weryfikuje gotowość zawodnika i wydaje wyposażenie (pakiet, dyskietka) | Supporting | E-060..E-070 |
| BC-06 | Start zawodników | Zarządza momentem startu grup i indywidualnym; zamyka zapisy | Supporting | E-080..E-087 |
| BC-07 | Pomiar czasu | Rejestruje przejazdy przez punkty i linię mety; generuje dane do rankingu | **Core** | E-090..E-098 |
| BC-08 | Kary | Rejestruje naruszenia regulaminu i dyskwalifikuje zawodników | Supporting | E-100..E-103 |
| BC-09 | Zakończenie zawodów | Generuje wyniki, przyznaje nagrody, obsługuje zwrot wyposażenia | Supporting | E-102, E-110..E-116 |
