Databases
==========

We use PHP Data Objects (PDO) to handle database connections. Every result is returned as PDO object, so if you have already any experience with
this library you should'nt have any problem with learning how `pantheraDB` works.

## Queries

Database queries are simplified as much as possible, so we have only one function that handles query and it's optional values.

Example of getting one record from database:

```php
$values = array(
    'login' => 'admin'
);

$query = $panthera -> db -> query('SELECT * FROM `{$db_prefix}users` WHERE `login` = :login', $values);
var_dump($query -> fetch(PDO::FETCH_ASSOC));
```

query() function takes *exactly two arguments* - query and array of values. 
Every array key should be present in query in ":key" format.
PDO will throw an exception if there are too many or too few arguments than in provided query string.

#### Why put arguments in array instead of just mixing them into query string?

Arguments put in the array can be parsed by PDO library and escaped to provide more security from SQL injection attacks.

## Building query strings from array

There are some simple functions built-in `pantheraDB` allowing building Insert, Update, Select or Delete instructions.

### Inserting a single row

```php
$values = array(
    'key' => 'test',
    'value' => 'aaaa'
);

$query = $panthera -> db -> buildInsertString($values, False, 'varcache');

var_dump($query);
```

### Inserting multiple rows

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

### Updating existing data

```php
$values = array (
    'name' => 'Jan',
    'points' => 5
);

// the second argument means that "name" column will not be included in update sytax, but will be avaliable in values array
// this is very usable when you want to specify additional rules in where clause 
$query = $panthera -> db -> buildUpdateString($values, array('name'));

var_dump($query);

# $panthera -> db -> query($query['query'], $query['values']);

```

### Select and Delete (whereClause class)

In most of select and delete clauses there is a where clause that can be also easily generated.

```php
$where = new whereClause;
$where -> add('', 'login', '=', 'user74838');
$where -> add('AND', 'banned', '=', 0);
$show = $where->show();

$query = 'SELECT * FROM `{$db_prefix}users` WHERE ' .$show[0];

# $panthera -> db -> query($query, $show[1]);
```

## Grouping whereClause statements

Sometimes we have to add more a little bit more complicated statements like (`login` = "test" AND `banned` = 0) AND (`lastlogged` > 3232323 OR `lastlogin` = 0)
Every statement can be grouped, eg. (`login` = "test" AND `banned` = 0) - first statement and the second (`lastlogged` > 3232323 OR `lastlogin` = 0)

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

## Generating unique column data

In cases when data in any column can't be duplicated, eg. generated API keys we can use createUniqueData() function.

```php
// generateRandomString will example output a string "GbE5Bf" and when createUniqueData() will find out that there is already such value in selected column it will search for GbE5Bf1, GbE5Bf2, ..., GbE5Bf10
$check = $panthera -> db -> createUniqueData('apiserver', 'key', generateRandomString(6));
```

## Database templates and autoloader

Every table schema can be saved into template in `/content/database/templates` or `/lib/database/templates` directory. SQLite3 templates are stored in a `sqlite3` subdirectory.
If `build_missing_tables` configuration variable is enabled in app.php every missing table will be automaticaly imported from previously generated template (if avaliable).

Example of MySQL schema template:

```
DROP TABLE IF EXISTS `{$db_prefix}var_cache`;

CREATE TABLE `{$db_prefix}var_cache` (
  `var` varchar(128) NOT NULL,
  `value` TEXT NOT NULL,
  `expire` int(20) NOT NULL,
  UNIQUE KEY `var` (`var`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```
