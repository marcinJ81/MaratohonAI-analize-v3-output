---
source: pomiar_czasu.png
converted: lossy
used-in: BP, PL
requires-verification: true
---
# Pomiar czasu

## Opis wizualny
Diagram Event Storming z dwiema sekcjami: (1) Pomiar czasu zadeklarowanego dystansu oraz (2) Pomiar czasu na wydłużonym dystansie. Widoczne hot spoty (różowe karteczki) i wyzwalacze zewnętrzne (RFID — przyłożenie dyskietki).

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Zarejestrowano pomiar czasu | domenowe | wynik odczytu RFID |
| Zawodnik zakończył przejazd | domenowe | ukończenie zadeklarowanego dystansu |
| Podliczono czasy przejazdu | domenowe | agregacja wyników |
| Zakończono przejazd zawodników na danym dystansie | domenowe | koniec dystansu |
| Zakończono przejazdy wszystkich dystansów | domenowe | koniec zawodów |
| Zgłoszono prośbę o przedłużenie dystansu | domenowe | inicjowane przez zawodnika |
| Przedłużono przejazd zawodnika | domenowe | akceptacja prośby |
| Odnotowano dodatkowy pomiar czasu | domenowe | rejestracja czasu na dłuższym dystansie |
| Zawodnik zakończył przejazd (wydłużony) | domenowe | ukończenie dłuższego dystansu |

## Zdarzenia zewnętrzne / wyzwalacze
| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Przyłożenie dyskietki do czytnika (zawodnik jest wyzwalaczem) | zewnętrzne | RFID — fizyczny trigger pomiaru |

## Aktorzy
- Zawodnik
- System pomiaru czasu (RFID)
- Obsługa punktu kontrolnego

## Polityki / Zasady (różowe/hot spoty)
- Przyłożenie dyskietki do czytnika, zawodnik jest wyzwalaczem
- Limity na dany dystans — regulamin
- Reguły regulamin maratonu (wydłużony dystans)
- System wie, że wszyscy dojechali albo że minął czas

## Granice kontekstów / sekcje
- Pomiar czasu zadeklarowanego dystansu
- Pomiar czasu na wydłużonym dystansie

## Hot Spoty / Problemy
- Hot spot: "system wie, że wszyscy dojechali albo że minął czas" — logika zamknięcia pomiaru
- Hot spot: "limity na dany dystans — regulamin" — reguły konfiguracyjne nieznane

## Cross-check z opisem systemu
- Rejestracja czasu przejazdu (RFID): POKRYTE
- Wyświetlanie czasu w czasie rzeczywistym: NIEOBECNE bezpośrednio — prawdopodobna strata konwersji, nie luka domenowa
- Generowanie wyników: częściowo (podliczono czasy)
- Dyplomy: NIEOBECNE — obsługiwane w osobnych plikach
