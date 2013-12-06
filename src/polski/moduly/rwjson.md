## Zapisywalny objekt json - moduł rwjson

Moduł jest w stanie pracować na objekcie JSON z funkcją zapisu.
Klucze tablicy mogą być traktowane jako zmienne objektu, ale jest także funkcja set() i get() w przypadku dziwnych nazw.

```php
$w = new writableJSON('test.json');
$w -> set('test', 'value');
var_dump($w -> get('test'));
$w -> save(); // zapisanie zmian do pliku
```
