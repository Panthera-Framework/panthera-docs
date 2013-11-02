Bazy danych
==========

Używamy objektów danych PHP (PDO) do wykonywania połączeń z bazą danych. Każdy wynik jest zwracany jako object PDO, więc jeśli miałeś jakąkolwiek styczność z tą biblioteką nie powinieneś mieć problemu ze zrozumieniem jak działa `pantheraDB`.

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
