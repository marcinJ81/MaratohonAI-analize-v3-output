---
generated-by: data-prep
---
# Ocena złożoności systemu

**Poziom:** wysoka

**Uzasadnienie:** System obejmuje wiele kontekstów domenowych: rejestrację uczestników, obsługę płatności, przydział numerów startowych, generowanie grup startowych, pomiar czasu przez RFID, wyświetlanie wyników w czasie rzeczywistym oraz generowanie dyplomów. Każdy z tych obszarów posiada własne reguły biznesowe (np. weryfikacja opłaty przed przydziałem numeru, generowanie grup na podstawie dystansu i czasu) oraz integracje zewnętrzne (system kart RFID, komunikacja w czasie rzeczywistym). Złożoność jest wysoka ze względu na ilość kontekstów, wymagania czasu rzeczywistego i liczbę integracji.

## Opis systemu (źródło)
System pozwala na rejestrację uczestnika na jeden z kilku dystansów, sprawdza czy uczestnik opłacił udział, przydziela numer startowy, generuje grupy startowe, rejestruje czas przejazdu za pomocą systemu kart opartych o RFID, wyświetla w czasie rzeczywistym między czasy przejazdu, generuje wyniki i dyplomy uczestnictwa.
