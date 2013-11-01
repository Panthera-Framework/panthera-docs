Konfiguracja
=============

Konfiguracja Panthery dzieli się na core i overlay.
Pierwsza jest położona w pliku generowanym przez instalator Panthery - app.php - w postaci tablicy.
Jest także overlay, która zmienia tablicę z pliku app.php i dodaje nowe zmienne.
Overlay is położona w bazie SQLite albo MySQL i jest podzielona na sekcje, aby uniknąć ładowowania tysiąca niepotrzebnych zmiennych.

###Przykład app.php

```php
$config = array (
  'build_missing_tables' => true,
  'db_socket' => 'mysql',
  'db_file' => 'db.sqlite3',
  'SITE_DIR' => '/var/www/panthera/example-app',
  'disable_overlay' => false,
  'debug_to_varcache' => true,
  'debug_to_file' => true
  'url' => 'http://localhost/panthera/example-app',
  'upload_dir' => 'content/uploads',
  'db_prefix' => 'pa_',
  'timezone' => 'Europe/Warsaw',
  'lib' => '/var/www/panthera/lib/',
  'db_username' => 'panthera',
  'db_password' => 'myHardc0rePAssWD',
  'db_host' => 'localhost',
  'db_name' => 'example
);
```

###Zrozumieć overlay

Wszystko jest przechowywane w bazie danych w tabeli config_overlay. Każdy klucz posiada wartość, typ i sekcję (która jest opcjonalna, ale zalecana do użycia).
Wartością może być wszystko - tekst, tablica, liczba. W celu podniesienia wydajności, nie używamy zserializowanych liczb i tekstu - to jest przyczyna dla której preferujemy wpisywanie typu zmiennej ręcznie.

Przykład poniżej tworzy zmienną nazwaną "dieta" z domyślną wartością "wegetariańska", która jest tekstem.

```php
$panthera -> config -> setKey('dieta', 'wegetariańska', 'string');
var_dump($panthera -> config -> getKey('dieta'));
```

<table>
    <tr>
        <td>id</td>
        <td>key</td>
        <td>value</td>
        <td>type</td>
        <td>section</td>
    </tr>
    
    <tr>
        <td>1</td>
        <td>dieta</td>
        <td>wegetariańska</td>
        <td>string</td>
        <td></td>
    </tr>
</table>

#### Typy danych
<table>
    <tr>
        <td>int</td>
        <td>string</td>
        <td>bool</td>
        <td>e-mail</td>
        <td>url</td>
        <td>ip</td>
        <td>regexp</td>
        <td>array</td>
        <td>json</td>
    </tr>
</table>

#### Sekcje

Sekcje są grupami zmiennych, które są ładowane tylko na żądanie. Powinniśmy grupować zmienne, aby uniknąć ładowania ogromnych ilości informacji za jednym razem.

```php
$panthera -> config -> setKey('dash.maxItems', 16, 'int', 'dash');
$panthera -> config -> loadOverlay('dash');
var_dump($panthera -> config -> getKey('dash.maxItems'));
```

<table>
    <tr>
        <td>id</td>
        <td>key</td>
        <td>value</td>
        <td>type</td>
        <td>section</td>
    </tr>
    
    <tr>
        <td>1</td>
        <td>dash.maxItems</td>
        <td>16</td>
        <td>int</td>
        <td>dash</td>
    </tr>
</table>

## Domyślna wartość zmiennych

Nie ma żadnej potrzeby, aby tworzyć skomplikowany kod aby przypisać domyślną wartość do zmiennych.
Podczas czytania zmiennych, na początku jeśli zmienna nie istnieje to będzie stworzona automatycznie z domyślną wartością - proste, czyż nie?

```php
$panthera -> config -> getKey('dash.maxItems', 16, 'int', 'dash'); # wygląda jak setKey, no nie? To jest i get i set!
```

Jeśli `dash.maxItems` nigdy wcześniej nie istniało to będzie stworzone automatycznie z domyślną wartością "16" i typem "int" w sekcji "dash". Proste i praktyczne? To Panthera!

## Wsparcie cache w sekcji globalnej

Dla zaawansowanych użytkowników szukających wydajności, stworzyliśmy możliwość przetrzymywania config_overlay skopiowanego do cache (w pamięci RAM), więc żadne zapytania do bazy danych nie będą wysłane. Tylko jeden warunek musi być spełniony - cache i varCache muszą być skonfigurowane w app.php, nie w config_overlay. :-)
