<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture -->
<!-- used-in: big-picture, process-level -->
<!-- converted: lossless -->

# Pomiar czasu

## Źródła
- pomiar_czasu.png

---

## Zdarzenia domenowe

| Zdarzenie | Aktor | Obszar |
|---|---|---|
| Zarejestrowano pomiar czasu | system pomiaru czasu | Pomiar czasu |
| Zawodnik zakończył przejazd | zawodnik / system | Pomiar czasu |
| Podliczono czasy przejazdu | system | Pomiar czasu |
| Zakończono przejazd zawodników na danym dystansie | system | Pomiar czasu |
| Zakończono przejazdy wszystkich dystansów | system | Pomiar czasu |
| Zgłoszono prośbę o przedłużenie dystansu | zawodnik | Pomiar wydłużony |
| Przedłużono przejazd zawodnika | obsługa punktu kontrolnego | Pomiar wydłużony |
| Odnotowano dodatkowy pomiar czasu | system | Pomiar wydłużony |

## Sub-procesy

### Pomiar czasu zadeklarowanego dystansu
- Trigger: przyłożenie dyskietki do czytnika (zawodnik jest wyzwalaczem)
- System rejestruje pomiar
- Zawodnik kończy przejazd → system podlicza czasy
- Koniec: wszyscy dojechali LUB minął czas → zakończono przejazd na dystansie
- Reguły: Limity na dany dystans (regulamin)

### Pomiar czasu na wydłużonym dystansie
- Zawodnik przejechał zadeklarowany dystans, zgłasza chęć przejechania następnego
- Zgłoszono prośbę o przedłużenie dystansu
- Obsługa punktu kontrolnego + reguły regulaminu → przedłużono przejazd
- Odnotowano dodatkowy pomiar czasu → zawodnik zakończył przejazd

## Aktorzy

| Nazwa | Rola | Zewnętrzny? |
|---|---|---|
| zawodnik | Inicjuje pomiar (dyskietką), może prosić o wydłużenie | Nie |
| system pomiaru czasu | Rejestruje pomiary, oblicza czasy | Nie (automatyczny) |
| obsługa punktu kontrolnego | Autoryzuje wydłużenie dystansu | Nie |

## Reguły biznesowe

- Pomiar przez dyskietkę i czytnik (sprzętowy)
- Limity czasowe na dystans z regulaminu
- Wydłużenie wymaga zgody (reguły regulaminu maratonu)
- System wie, że wszyscy dojechali ALBO że minął czas → kończy dystans

## Granice kontekstu

- **Pomiar czasu**: kluczowy kontekst dla 24h maratonu
- Sprzętowa integracja: czytniki dyskietek
- Powiązany z: Zakończenie zawodów (zakończono przejazdy wszystkich dystansów)

## Hot Spoty / Otwarte pytania

- Jak system obsługuje utratę sygnału / awarię czytnika?
- Czy zawodnik może jechać wiele dystansów (wydłużać wielokrotnie)?
- Jak podliczane są kary czasowe?
