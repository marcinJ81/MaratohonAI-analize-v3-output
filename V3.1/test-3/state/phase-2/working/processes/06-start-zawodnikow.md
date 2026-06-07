# Proces: Start zawodników (BC-06)

## Wyzwalacz
Sędzia trasy ogłasza start.

## Przepływ: zamknięcie zapisów
1. Administrator potwierdza start zawodników na danym dystansie → E-080 Zamknięto zapisy na dany dystans
   - Po zamknięciu zapisów nowi uczestnicy nie mogą się zapisać na ten dystans

## Przepływ: start grupowy
1. Sędzia trasy startuje grupę → E-081 Wystartowano grupę
2. System rejestruje czas startu grupy → E-082 Zarejestrowano czas startu
   - Grupy startują kolejno (w odstępach — szczegóły nieznane)
   - [Przy starcie] obsługa weryfikuje ostatecznie zawodników:
     - [Dyskwalifikacja] → E-086 Zdyskwalifikowano zawodnika
     - [Rezygnacja] → E-087 Zakończono udział przedwcześnie

## Przepływ: start indywidualny (np. spóźnienie)
1. Zawodnik zgłasza chęć startu indywidualnego → E-084 Zgłoszono indywidualny start
2. Administrator wyciąga zawodnika z grupy → E-085 Wyciągnięto zawodnika z grupy
3. Sędzia trasy startuje zawodnika → E-083 Wystartowano zawodnika
4. System rejestruje czas startu indywidualnego → E-082 Zarejestrowano czas startu

## Reguły biznesowe
- RB-060: Start indywidualny wymaga wyciągnięcia zawodnika z grupy (zmiana stanu grupy)
- RB-061: Czas startu jest podstawą do obliczenia czasu przejazdu (BC-07)
- RB-062: Zamknięcie zapisów następuje gdy zawodnicy tego dystansu zostali wystartowani

## Wyjątki
- DNS (Did Not Start) — brak przepływu w materiałach; otwarte pytanie Q-008
