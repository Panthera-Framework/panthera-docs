Session
=======

Every visitor of your website is using a session and cookies to store temporary data, eg. search results or history.

pantheraSession is operating on PHP's standard sessions and cookies mechanisms, but it's a lot easier to do than in plain PHP, and of course it's possible to move all cookies to any external storage.

#### Example of operating on sessions

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

#### Cookies example usage

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

#### Browser detection

There is a very simple browser detection stored in user's session, it detects operating system, device type and software version. Detection is performed only once when a new session is started.

```php
print_r($panthera -> session -> clientInfo);
```
