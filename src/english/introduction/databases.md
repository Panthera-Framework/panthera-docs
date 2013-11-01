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
```

## Updating existing data

```php
$values = array (
    'name' => 'Jan',
    'points' => 5
);

// the second argument means that "name" column will not be included in update sytax, but will be avaliable in values array
// this is very usable when you want to specify additional rules in where clause 
$query = $panthera -> db -> buildUpdateString($values, array('name'));

var_dump($query);
```
