<!-- source: user-input, session: maraton-rowerowy-24h, phase: big-picture -->
<!-- used-in: big-picture, process-level -->
<!-- converted: lossless -->

# Start zawodów

## Źródła
- wystartowano_zawodników.png (Frame 47)

---

## Zdarzenia domenowe

| Zdarzenie | Aktor | Obszar |
|---|---|---|
| Wystartowano grupę | sędzia trasy | Start zawodników |
| Zarejestrowano czas startu | sędzia trasy / system | Start zawodników |
| Wystartowano zawodnika | sędzia trasy | Start zawodników |
| Zdyskwalifikowano zawodnika | sędzia trasy | Start zawodników |
| Zakończono udział przedwcześnie | sędzia trasy | Start zawodników |
| Wystartowano zawodników na danym dystansie | administrator | Zamknięcie zapisów |
| Zamknięto zapisy na dany dystans | system | Zamknięcie zapisów |

## Sub-procesy

### Zamknięcie zapisów
- Trigger: Wystartowano zawodników na danym dystansie
- Aktor: administrator
- Efekt: zamknięto zapisy na dany dystans

### Start zawodników
- Sędzia trasy startuje grupy/zawodników
- Rejestrowany jest czas startu
- Możliwa dyskwalifikacja lub przedwczesne zakończenie udziału

## Aktorzy

| Nazwa | Rola | Zewnętrzny? |
|---|---|---|
| sędzia trasy | Startuje grupy i zawodników, dyskwalifikuje | Nie |
| administrator | Zamyka zapisy po starcie | Nie |

## Granice kontekstu

- **Start zawodników**: moment fizycznego startu grup
- **Zamknięcie zapisów**: automatyczne zamknięcie po starcie dystansu

## Hot Spoty / Otwarte pytania

- Czy start grupy i start zawodnika to osobne zdarzenia czy jedno?
- Co dokładnie oznacza "zakończono udział przedwcześnie" — DNF?
- Czy sędzia trasy rejestruje czas manualnie czy jest to automatyczne?
