---
source: kary.png
converted: lossy
used-in: BP, PL
requires-verification: false
out-of-scope: true
user-confirmed: 2026-06-14
---
> **DECYZJA UŻYTKOWNIKA 2026-06-14:** Kary/dyskwalifikacja POZA ZAKRESEM SYSTEMU.
> Dyskwalifikacja to decyzja sędziego trasy — system jej nie nakłada.
> Ten plik WYKLUCZ z analizy domenowej.
# Nakładanie kar

## Opis wizualny
Uproszczony diagram Event Storming. Sekcja "Nakładanie kar". Sędzia trasy jako aktor, dwa zdarzenia domenowe, jedna polityka (różowa).

## Zdarzenia domenowe

| Zdarzenie | Typ | Uwagi |
|---|---|---|
| Nałożono karę na zawodnika za złamanie regulaminu | domenowe | główna ścieżka |
| Naliczono karę za pominięte punkty kontrolne | domenowe | ścieżka alternatywna |

## Aktorzy
- Sędzia trasy

## Polityki / Zasady (różowe)
- Kary regulamin maratonu

## Granice kontekstów
- Nakładanie kar (osobny kontekst / subdomena)

## Hot Spoty / Problemy
- Bardzo uproszczony diagram — brakuje szczegółów procesu weryfikacji kary
- Brak informacji o wpływie kar na wyniki końcowe — prawdopodobna strata konwersji, nie luka domenowa
- Brak informacji jak kara jest odwoływana lub kwestionowana

## Cross-check z opisem systemu
- Nakładanie kar: NIEOBECNE wprost w opisie systemu — obszar dodatkowy nieujęty w opisie
  → flaga requires-verification: prawdopodobna strata konwersji lub rozszerzenie zakresu nieujęte w opisie
- Generowanie wyników: pośrednio powiązane (kary wpływają na wyniki)
