Sesja
=======

Każdy kto przegląda Twoją stronę używa sesji oraz ciasteczek - pozwala to na zapisywanie tymczasowych danych, np. rezultatów wyszukiwań czy historii.

pantheraSession zarządza standardowymi mechanizmami do zarządzania sesją i ciasteczkami, które są wbudowane w PHP. Oczywiście w imię prostoty, stworzyliśmy narzędzie bardzo proste w użyciu. Pozwala ono nawet na przeniesienie wszystkich zmiennych do zewnętrznej pamięci.

### Przykłady użycia sesji

Kod do zarządzanie sesją domyślnie używa zmiennej $_SESSION. Lepiej nie używać sesji ręcznie - pantheraSession sprawdza poprawność, szuka błędów i dba o bezpieczeństwo.

_**NOTATKA:** Każda informacja przetrzymywana w sesji jest także zapisana na serwerze._

```php
// definiujemy zmienną "name" i ustawiamy jej wartość - string "John"
$panthera -> session -> set('name', 'John');

// sprawdzamy, czy zmienna rzeczywiście jest zdefiniowana
if ($panthera -> session -> exists('name'))
{
    var_dump($panthera -> session -> get('name')); // pobieramy wartość zmiennej "name" z sesji
}

// wypisuje wszystko, co jest przetrzymywane w sesji
print_r($panthera -> session -> dump());

// usuwa zmienną
$panthera -> session -> remove('name');
```

### Ciasteczka i ich użycie

Ciasteczka są przetrzymywane w przeglądarce użytkownika. Przeglądający może zarządzać wartościami zapisanymi w ciasteczku, więc nie jest to metoda, którą chciałbyś użyć do przetrzymywania ważnych informacji jak hasła, czy dane autoryzacyjne.

_**NOTATKA:** Ciasteczka są przetrzymywane na komputerze użytkownika i mogą zostać podglądnięte, zedytowane a nawet usunięte!_

```php
// ustawienie ciasteczka
$panthera -> session -> cookies -> set('name', 'John', time()+60); // przetrzymaj zmienną "name" => "John" przez 60 sekund

// pobierz wartość ciasteczka
var_dump($panthera -> session -> cookies -> get('name'));

// usuń ciasteczko
$panthera -> session -> cookies -> remove('name');

// pobierz wszystkie dane przetrzymywane w ciasteczkach (zwraca tablicę)
var_dump($panthera -> session -> cookies -> getAll());
```

#### Szyfrowanie

Klasa pantheraCookie daje możliwość szyfrowania zapisywanych przez użytkownika ciasteczek. W tym celu używa ona 256 bitowego AES w trybie CBC.
Aby włączyć wyżej wspomniane szyfrowanie AES, ustaw wartość zmiennej `cookie_encrypt` na `1`.

### Wykrywanie przeglądarki

Wykrywanie przeglądarki jest przeprowadzane tylko raz - podczas startu nowej sesji. Informacje pobrane podczas tego działania są przetrzymywane w nowo wystartowanej sesji. Możemy się z nich dowiedzieć o systemie operacyjnym, typie urządzenia czy wersji oprogramowania przeglądającego.

```php
print_r($panthera -> session -> clientInfo);
```
