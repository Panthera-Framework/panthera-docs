## httplib module

The name came from Python's httplib, but was builded to provide more simpler interface to cURL library.

### Using GET method

```php
$http = new httplib; // create a HTTP "session" - object will store cookies etc.
$contents = $http -> get('http://duckduckgo.com/?q=Test+from+Panthera+Framework\'s+httplib+module');
var_dump($contents);
```

### POST

```php
$http = new httplib;

$fields = array(
    'name' => 'Jan',
    'surname' => 'Kowalski'
);

$contents = $http -> post('http://example.com', $fields);
var_dump($contents);
```
