## Writable json object - rwjson module

rwjson provides a simple class that is able to operate on JSON object with write support.
Array keys can be treated as object's variables, but there are also set() and get() methods for any weird names.

```php
$w = new writableJSON('test.json');
$w -> set('test', 'value');
var_dump($w -> get('test'));
$w -> save(); // save changes to file
```
