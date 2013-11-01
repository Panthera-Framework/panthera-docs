Databases
==========

We use PHP Data Objects (PDO) to handle database connections. Every result is returned as PDO object, so if you have already any experience with
this library you should'nt have any problem with learning how `pantheraDB` works.

## Queries

Database queries are simplified as much as possible, so we have only one function that handles query and it's optional values.

Example of getting one record from database:

```php
$query = $panthera -> db -> query('SELECT * FROM `{$db_prefix}users` WHERE `login` = :login', array('login' => 'admin'));
var_dump($query -> fetch(PDO::FETCH_ASSOC));
```

query() function takes *exactly two arguments* - query and array of values. Every array key should be present in query in format ":key".
PDO will throw an exception if there are too many or too few arguments than in provided string query.

