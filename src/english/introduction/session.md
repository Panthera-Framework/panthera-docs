Session
=======

Every visitor of your website is using a session and cookies to store temporary data, eg. search results or history.

pantheraSession is operating on PHP's standard sessions and cookies mechanisms. It's interface is a lot easier to use than in plain PHP, and of course there's a possibility to move all variables to any external storage.

### Example of operating on sessions

Session management is operating on $_SESSION variable by default, it is important not to do this manually as pantheraSessinon is also taking care about possible errors and session security.

_**NOTE:** Every data kept in session is stored on server._

```php
// define a variable "name" and set it's value to string type "John"
$panthera -> session -> set('name', 'John');

// check if variable was defined
if ($panthera -> session -> exists('name'))
{
    var_dump($panthera -> session -> get('name')); // get "name" from session
}

// print all variables stored in session
print_r($panthera -> session -> dump());

// remove a variable
$panthera -> session -> remove('name');
```

### Cookies example usage

Cookies are stored in user's browser. Please note that user is able to manage cookies itself, so it's not a secure method to store sensitive informations such as passwords, complete authentication data.

_**NOTE:** Cookies are stored on user's computer and can be viewed, edited or removed!_

```php
// set a cookie
$panthera -> session -> cookies -> set('name', 'John', time()+60); // set a cookie "name" => "John" for 60 seconds

// get a cookie value
var_dump($panthera -> session -> cookies -> get('name'));

// remove a cookie
$panthera -> session -> cookies -> remove('name');

// get all cookies (returns array)
var_dump($panthera -> session -> cookies -> getAll());
```

#### Encryption

pantheraCookie class also provides a possibility to encrypt user-stored cookies using 256 bit AES in CBC mode.
To enable AES encryption please set `cookie_encrypt` variable to `1`.

### Browser detection

There is a very simple browser detection stored in user's session, it detects operating system, device type and software version. Detection is performed only once when a new session is started.

```php
print_r($panthera -> session -> clientInfo);
```
