# Proces: Kary (BC-08)

## Wyzwalacz
Sędzia trasy stwierdza naruszenie regulaminu.

## Przepływ: kara za złamanie regulaminu
1. Sędzia trasy stwierdza naruszenie → E-100 Nałożono karę za złamanie regulaminu
2. System rejestruje naruszenie
3. → E-103 Zdyskwalifikowano zawodnika

## Przepływ: kara za pominięty punkt kontrolny
1. BC-07 wykrywa brak odczytu na punkcie kontrolnym dla zawodnika
2. Sędzia trasy zatwierdza → E-101 Naliczono karę za pominięte punkty kontrolne
3. → E-103 Zdyskwalifikowano zawodnika

## Reguły biznesowe
- RB-080: Jedyna kara to dyskwalifikacja (brak kar czasowych)
- RB-081: Typy naruszeń prowadzących do dyskwalifikacji: złamanie regulaminu, pominięty punkt kontrolny
- RB-082: Dyskwalifikacja jest zatwierdzona przez sędziego trasy (nie automatyczna)
- RB-083: Zdyskwalifikowany zawodnik nie wchodzi do rankingu

## Otwarte pytania
- Czy jeden pominięty punkt = dyskwalifikacja, czy musi być ich kilka?
- Kto i jak wykrywa pominięcie punktu kontrolnego — automatycznie z BC-07 czy ręcznie?
- Czy dyskwalifikacja jest odwołalna?
