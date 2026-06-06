# Proces: Opłata za udział — BC-03

## Główny flow: Opłacenie uczestnictwa

**Wyzwalacz:** Zarejestrowano uczestnika (z BC-02) — potrzeba opłacenia
**Rezultat:** Opłacono uczestnictwo → uczestnik aktywny

**Kroki:**
1. Uczestnik inicjuje płatność → Rozpoczęto opłacanie udziału
2. Uczestnik wykonuje przelew bankowy
3. Interfejs banku rejestruje przelew → Zarejestrowano przelew
4. Obsługa punktu kontrolnego lub system potwierdza → Zarejestrowano opłatę
5. System weryfikuje kwotę:
   5a. [pełna kwota] → Opłacono uczestnictwo
   5b. [promocja -100%] → Zaaplikowano promocję → Opłacono uczestnictwo (bez przelewu)
   5c. [nadpłata] → Wyliczono kwotę zwrotu → Zwrócono nadpłatę

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-008 | W trakcie sesji opłacania nie można zmienić dystansu | 1–5 |
| RB-009 | Promocja (-100%) może być aplikowana manualnie przez obsługę | 5b |

---

## Flow alternatywny: Niedopłata

**Wyzwalacz:** Uczestnik opłacił mniej niż wymagana kwota
**Rezultat:** Niedopłata uregulowana LUB uczestnik pozostaje na opłaconym dystansie

**Kroki:**
1. System lub obsługa stwierdza niedopłatę
2. Uczestnik inicjuje opłatę niedopłaty → Rozpoczęto opłacanie niedopłaty
3. Interfejs banku rejestruje przelew → Zarejestrowano przelew
4. System potwierdza → Opłacono niedopłatę → Opłacono zmianę dystansu

**Reguły biznesowe:**
| ID | Reguła | Krok |
|---|---|---|
| RB-010 | Niedopłata za zmianę dystansu: uczestnik pozostaje na opłaconym dystansie do czasu uregulowania — generacja grup nie jest blokowana (Q-001) | 1 |

---

## Flow alternatywny: Zwrot nadpłaty

**Kroki:**
1. Wyliczono kwotę zwrotu
2. Pracownik obsługi / administrator wykonuje zwrot → Zwrócono nadpłatę
