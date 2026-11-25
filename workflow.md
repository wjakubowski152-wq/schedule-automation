# Power Automate – logika przepływu

## Cel

Przepływ synchronizuje dane z tabeli Excel z listą SharePoint. Uruchamia się cyklicznie, odczytuje wiersze z tabeli, sprawdza, czy wpis o danym identyfikatorze istnieje już na liście SharePoint, a następnie tworzy nowy element lub aktualizuje istniejący.

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
##Bezpieczeństwo
Użyte są fikcyjne dane. 

