# Power Automate – logika przepływu

## Cel

Przepływ synchronizuje dane z tabeli Excel z listą SharePoint. Uruchamia się cyklicznie, odczytuje wiersze z tabeli, sprawdza, czy wpis o danym identyfikatorze istnieje już na liście SharePoint, a następnie tworzy nowy element lub aktualizuje istniejący.

## Konfiguracja potwierdzona na zrzutach ekranu

- Wyzwalacz: `Recurrence`.
- Interwał: co 1 minutę.
- Lokalizacja skoroszytu: biblioteka dokumentów SharePoint.
- Plik: `tblGrafik.xlsx` (w publicznej dokumentacji nazwa została uogólniona; nie publikuj prywatnej ścieżki witryny).
- Tabela Excel: `Tabela1`.
- Akcja Excel: `Wyświetl listę wierszy w tabeli`.
- Akcja SharePoint wewnątrz pętli: `Pobierz elementy 1`.
- Zapytanie filtru porównuje pole `Kluczwpisu` z wartością dynamiczną `ID_Wpisu` z bieżącego wiersza Excel.
- Warunek wykorzystuje wyrażenie:
  `length(outputs('Pobierz_elementy_1')?['body/value'])`
- Gałąź `Prawda`: `Utwórz element`.
- Gałąź `Fałsz`: `For each` → `Aktualizuj element`.

## Przebieg krok po kroku

1. **Recurrence** – uruchamia przepływ co minutę.
2. **Wyświetl listę wierszy w tabeli** – odczytuje rekordy z tabeli `Tabela1` w skoroszycie Excel.
3. **Pobierz elementy** – pobiera elementy listy SharePoint.
4. **Zastosuj do wszystkich** – iteruje po wierszach zwróconych z Excela.
5. **Pobierz elementy 1** – wyszukuje na liście SharePoint wpis, którego pole `Kluczwpisu` odpowiada `ID_Wpisu` z aktualnie przetwarzanego wiersza.
6. **Warunek** – sprawdza liczbę znalezionych rekordów za pomocą funkcji `length(...)`.
7. **Prawda → Utwórz element** – tworzy nowy element SharePoint.
8. **Fałsz → For each → Aktualizuj element** – aktualizuje znaleziony element SharePoint.

## Schemat logiki

```text
Recurrence (co 1 minutę)
        |
        v
Odczytaj wiersze z tabeli Excel
        |
        v
Pobierz elementy SharePoint
        |
        v
Dla każdego wiersza Excel:
        |
        v
Znajdź rekord po Kluczwpisu = ID_Wpisu
        |
        v
Oblicz length(body/value)
        |
        +---- Prawda ----> Utwórz element
        |
        +---- Fałsz -----> For each → Aktualizuj element
```

**Uwaga dotycząca interpretacji warunku:** zrzut pokazuje wyrażenie `length(outputs('Pobierz_elementy_1')?['body/value'])` oraz rozmieszczenie akcji w gałęziach, ale nie pokazuje całego operatora i wartości porównania. Dlatego nie opisuję tu, czy warunek sprawdza dokładnie `= 0`, `> 0` czy inną wartość. Przed publikacją dokumentacji technicznej warto dopisać pełne porównanie z edytora warunku.

## Dane

Widoczny arkusz zawiera kolumny m.in. `ID_Wpisu`, `Termin`, `Sala`, `Zabieg`, `Pacjent`, `Operator`, `Typ`, `Uwagi` i `Status`. W publicznym repozytorium należy używać wyłącznie przykładowych, fikcyjnych danych.

## Kwestie do doprecyzowania przed publikacją

- Pełny operator i wartość porównania w warunku.
- Czy przepływ ma dodatkową obsługę błędów, ponowień lub duplikatów.
- Czy aktualizacja w gałęzi `Fałsz` zawsze zwraca dokładnie jeden rekord.
- Czy pobieranie wszystkich elementów SharePoint w kroku przed pętlą jest wykorzystywane w dalszej części przepływu.

## Prywatność i bezpieczeństwo

Nie publikuj eksportu przepływu ani zrzutów zawierających nazwę organizacji, adres witryny, identyfikatory środowiska, identyfikatory list, nazwy połączeń, dane pacjentów lub personelu. Używaj demonstracyjnego środowiska i fikcyjnych rekordów.
