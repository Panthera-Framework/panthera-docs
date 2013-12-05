## httplib module

The name came from Python's httplib, but was builded to provide more simpler interface to cURL library.

### Using GET method

```php
$http = new httplib; // create a HTTP "session" - object will store cookies etc.
$contents = $http -> get('http://duckduckgo.com/?q=Test+from+Panthera+Framework\'s+httplib+module');
var_dump($contents);
```

### POST

Post method can be very useful to login to a website that requires authentication, object will store all cookies set by website.

```php
$http = new httplib;

$fields = array(
    'login' => 'hacker',
    'password' => '123'
);

// object will store cookies if website will create any
$contents = $http -> post('http://example.com/login.php', $fields);

// ... parse data from $contents, create $session ...

// download a file that requires login
$file = $http -> get('http://example.com/filedownload.php?fid=542343514663&sid=' .$session);
```

### Using proxy server

httplib is able to send request through a proxy server as cURL do. Supported proxy server types are socks4, socks5 and http.

```php
$http = new httplib;
$http -> setProxy('127.0.0.1', 3128); // public http proxy
$http -> setProxy('127.0.0.1', 3128, 'http', 'user:password'); // proxy with authentication
$http -> setProxy('127.0.0.1', 8080, 'socks5'); // socks5 proxy
```
