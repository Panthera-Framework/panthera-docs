Bazy danych
==========

Używamy objektów danych PHP (PDO) do wykonywania połączeń z bazą danych. Każdy wynik jest zwracany jako object PDO, więc jeśli miałeś jakąkolwiek styczność z tą biblioteką nie powinieneś mieć problemu ze zrozumieniem jak działa `pantheraDB`.

## Konfiguracja

Konfiguracja bazy danych jest umieszczona w pliku `app.php`. Obecnie Panthera wspiera sockety SQLite3 i MySQL poprzez PDO - jeśli jesteś zainteresowany portowaniem Panthery dla innych typów baz danych, czekamy na pull request. ;)

Przykład konfiguracji:

<table>
    <tr>
        <td><b>db_socket</b></td>
        <td><b>db_file</b></td>
        <td><b>db_host</b></td>
        <td><b>db_userame</b></td>
        <td><b>db_name</b></td>
        <td><b>db_password</b></td>
        <td><b>db_autocommit</b></td>
        <td><b>db_prefix</b></td>
    </tr>
    
    <tr>
        <td>mysql</td>
        <td></td>
        <td>localhost</td>
        <td>panthera</td>
        <td>example_app</td>
        <td>test123</td>
        <td>1</td>
        <td>pa_</td>
    </tr>
    
    <tr>
        <td>sqlite3</td>
        <td>db.sqlite3</td>
        <td></td>
        <td></td>
        <td></td>
        <td></td>
        <td>1</td>
        <td>pa_</td>
    </tr>
</table>

## Kolejki

Zapytania do bazy danych są tak uproszczone jak to tylko było możliwe. Posiadamy tylko jedną funkcję do wykonywania zapytań.

Przykład pobierania jednego rekordu z bazy danych:

```php
$values = array(
    'login' => 'admin'
);

$query = $panthera -> db -> query('SELECT * FROM `{$db_prefix}users` WHERE `login` = :login', $values);
var_dump($query -> fetch(PDO::FETCH_ASSOC));
```

Funckja query() potrzebuje *tylko dwóch argumentów* - zapytania i tablicy z wartościami. 
Każdy klucz tablicy powinien istnieć w zapytaniu w formacie ":klucz".
PDO wyrzuci wyjątek jeśli będzie zbyt dużo lub zbyt mało argumentów niż jest przewidziane w zapytaniu.

#### Dlaczego podajemy argumenty w tablicy zamiast wpisywać je prosto do zapytania query?

Argumenty podane w tablicy mogą być sparsowane i sprawdzone przez bibliotekę PDO - zapewnia to większe bezpieczeństwo przed atakami SQL Injection.

## Tworzenie zapytań query z tablicy

Posiadamy kilka prostych, wbudowanych funkcji `pantheraDB` pozwalających nam tworzyć instrukcje Insert, Update, Select lub Delete.

### Dodaj pojedynczy rekord

```php
$values = array(
    'key' => 'test',
    'value' => 'aaaa'
);

$query = $panthera -> db -> buildInsertString($values, False, 'varcache');

var_dump($query);
```

### Dodaj wiele rekordów

```php
$values = array(
    array (
        'key' => 'first',
        'value' => 1
    ),
    
    array (
        'key' => 'second',
        'value' => 2
    )
);

$query = $panthera -> db -> buildInsertString($values, True, 'varcache');

var_dump($query);

# $panthera -> db -> query($query['query'], $query['values']);
```

### Uaktualnianie istniejących danych

```php
$values = array (
    'name' => 'Jan',
    'points' => 5
);

// drugi argument oznacza, że "name" kolumny nie będzie zawarte w zaktualizowanej składni, ale będzie dostępne w wartościach tablicy
// to jest bardzo użyteczne kiedy chcesz sprecyzować dodatkowe prawo w klauzuli where
$query = $panthera -> db -> buildUpdateString($values, array('name'));

var_dump($query);

# $panthera -> db -> query($query['query'], $query['values']);

```

### Select i Delete (klasa whereClause)

W większość klauzul select i delete jest `where` które może być łatwo wygenerowane.

```php
$where = new whereClause;
$where -> add('', 'login', '=', 'user74838');
$where -> add('AND', 'banned', '=', 0);
$show = $where->show();

$query = 'SELECT * FROM `{$db_prefix}users` WHERE ' .$show[0];

# $panthera -> db -> query($query, $show[1]);
```

## Grupowanie wyrażeń whereClause

Czasami musimy dodać trochę więcej bardziej skomplikowanych wyrażeń jak (`login` = "test" AND `banned` = 0) AND (`lastlogged` > 3232323 OR `lastlogin` = 0).
Każde wyrażenie może być pogrupowane, np. (`login` = "test" AND `banned` = 0) - pierwsze wyrażenie, i drugie - (`lastlogged` > 3232323 OR `lastlogin` = 0)

```php
$where = new whereClause;
$where -> add('', 'login', '=', 'test', 1);
$where -> add('AND', 'banned', '=', 0, 1);

$where -> setGroupStatement(2, 'AND');
$where -> add('', 'lastlogged', '>', 3232323, 2);
$where -> add('OR', 'lastlogged', '=', 0, 2);
$show = $where->show();

$query = 'SELECT * FROM `{$db_prefix}users` WHERE ' .$show[0];

var_dump($query);

# $panthera -> db -> query($query, $show[1]);
```

## Generowanie kolumny unique

W przypadkach kiedy dane w żadnej kolumnie nie mogą się powtarzać, możemy wygenerować klucze API poprzez funkcję createUniqueData().

```php
// generateRandomString przykładowo zwróci tekst "GbE5Bf" i kiedy createUniqueData() odkryje, że ta wartość jest już w zaznaczonej kolumnie, będzie szukała GbE5Bf1, GbE5Bf2, ..., GbE5Bf10
$check = $panthera -> db -> createUniqueData('apiserver', 'key', generateRandomString(6));
```

## Przykłady bazy danych i autoloadera

Każda tabela może być zapisana jako szablon w folderze `/content/database/templates` lub `/lib/database/templates`. Szabloney SQLite3 są przechowywane w podkatalogu `sqlite3`.
Jeśli zmienna konfiguracyjna `build_missing_tables` jest włączona w app.php każda brakująca tabela zostanie automatycznie zaimportowana z wcześniej wygenerowanego szablonu (jeśli jest dostępny).

Przykład szablonu tabeli MySQL:

```
DROP TABLE IF EXISTS `{$db_prefix}var_cache`;

CREATE TABLE `{$db_prefix}var_cache` (
  `var` varchar(128) NOT NULL,
  `value` TEXT NOT NULL,
  `expire` int(20) NOT NULL,
  UNIQUE KEY `var` (`var`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

# Objekty danych - pantheraFetchDB

`pantheraFetchDB` jest klasą, która pozwala zwrócić rekord bazy danych w odczytywalnym dla PHP objekcie.
Warte uwagi jest również to, że wszystkie objekty bazowane na tej klasie wspierają cachowanie, które poprawia wydajność, ale może powodować tworzenie problemów jeśli ktoś tego użycje bez odpowiedniej wiedzy.

## Konwertowanie tablic w objekty

Aby umożliwić konwertowanie rekordów baz danych w objekty *musisz stworzyć klasę* poszerzoną o `pantheraFetchDB`.

```php
class panel extends pantheraFetchDB
{
    protected $_tableName = 'panels'; // tutaj jest nazwa tableli bez przedrostka
    protected $_idColumn = 'id'; // `id` kolumny tej tabeli
    protected $_constructBy = array('id', 'array', 'title'); // pozwala skonstruować kolumny poprzez `id` i `title` przy użyciu tablicy z wartościami
}
```

Powyższy przykład jest bardzo prosty, definiujemy nazwę tabeli, unikalne `id` kolumny i listę kolumn które mogą być wykorzystane do stworzenia objektu.
`array` jest nie tylko kolumną, ale także metodą konstruowania objektu. Z tą metodą możesz konstruować objekt podając tylko tablicę wszystkich kolumn i wartości. Jest to bardzo pomocne przy wykonywaniu szerokiego wybierania w tabeli, a następnie konstruowania objektu poprzez podanie tablicy z wartościami pobranymi z bazy danych.

```php
$panel = new panel('id', 1);

// lub
$panel = new panel('title', 'To jest panel');

// i poprzez tablicę
$q = $panthera -> db -> query('SELECT * FROM `{$db_prefix}panels` WHERE `id` = 1');
$data = $q -> fetch(PDO::FETCH_ASSOC);

$panel = new panel('array', $data);

var_dump($panel -> exists());
```

#### Sprawdzanie czy objekt istnieje

Bardzo ważną rzeczą w `pantheraFetchDB` jest sprawdzanie czy objekt istnieje. Specjalnie stworzyliśmy odpowiednią funkcję do tego.

```php
if ($panel -> exists())
{
    // zrób coś
}
```

Jeśli nie sprawdzimy, czy objekt został zainicjowany poprawnie, możemy dostać błąd krytyczny lub niespodziewane rezultaty.

#### Pobieranie tablicy objektów np. listy użytkowników

Tutaj jest przykład:

```php
$by = new whereClause;
$by -> add('', 'login', '=', 'Jan');

// lub poprzez tablicę (ale to zawiera tylko operator "AND" w zapytaniu SQL)
# $by = array('login' => 'Jan');

$rows = $panthera -> db -> getRows('users', $by, 10, 0, 'pantheraUser', 'id', 'DESC');

// Argumenty
// 'users' - szukamy w tabeli `{$db_prefix}users`
// $by - gdzie wyrażeniem może być `profile_picture` != ''
// 10 - Limit SQL (pokazuje tylko 10 użytkowników)
// 0 - offset (startujemy z tej pozycji)
// 'pantheraUser' - zwraca rezultaty jako objekt klasy (spróbuj skonstruować to poprzez metodę `array`)
// 'id' - column to order by
// 'DESC' - kierunek sortowania
```
