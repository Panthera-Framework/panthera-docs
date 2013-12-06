## Moduł httplib

Nazwa wzięła się od Pythonowej biblioteki httplib. Moduł został zbudowany w celu zapewnienia prostszego interfejsu dla cURL.

### Użucie metody GET

```php
$http = new httplib; // utworzenie sesji HTTP - objekt będzie przechowywał ciasteczka itp.
$contents = $http -> get('http://duckduckgo.com/?q=Test+from+Panthera+Framework\'s+httplib+module');
var_dump($contents);
```

### POST

Metoda POST jest bardzo użyteczna w przypadku logowania się na stronę, która wymaga autoryzacji. Objekt będzie przechowywał ciasteczka ustawione przez stronę.

```php
$http = new httplib;

$fields = array(
    'login' => 'hacker',
    'password' => '123'
);

// jeśli strona utworzy ciasteczko objekt je będzie przechowywał
$contents = $http -> post('http://example.com/login.php', $fields);

// parsowanie danych ze zmiennej $content (zawartość), utworzenie $session (sesji)

// pobranie pliku, który wymaga logowania
$file = $http -> get('http://example.com/filedownload.php?fid=542343514663&sid=' .$session);
```

### Używanie serwera proxy

httplib umożliwia wysyłanie zapytań poprzez serwer proxy. Wspierane typy serwerów proxy to: socks4, socks5 i http.

```php
$http = new httplib;
$http -> setProxy('127.0.0.1', 3128); // publiczne proxy http
$http -> setProxy('127.0.0.1', 3128, 'http', 'user:password'); // proxy z autoryzacją
$http -> setProxy('127.0.0.1', 8080, 'socks5'); // proxy socks5
```

### Zmienne konfiguracji

```php
$http -> timeout = 6; // ustawienie czasu oczekiwania na 6 sekund
httplib::userAgent = 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/32.0.1667.0 Safari/537.36';
```
