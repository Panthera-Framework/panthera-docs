Facebook
========

We are using Facebook PHP SDK to provide access to Graph API and FQL. 
Facebook module is integrating Facebook SDK with Panthera configuration, functions and fixing common errors.

#### Authenticating user with Facebook

```php
$facebook = new FacebookWrapper;

// the redirection will be made by header "Location" to http://mywebsite.org/login
$facebook -> loginUser(array('publish_stream', 'user_likes'), 'header', 'http://mywebsite.org/login');
```

So, was it hard?

#### Checking if user likes our fanpage

There is a simple method that uses FQL query and returns boolean.

```php
var_dump($facebook -> userLikesPage('1234567890')); // you have to put page id here
```

#### Making custom API calls

FacebookWrapper is implementing Panthera Logging to allow debugging and benchmarking of API calls.

```php
var_dump($facebook -> api ('/me'));
var_dump($facebook -> fql ('SELECT page_id FROM page_fan WHERE uid=me() AND page_id=123'));
```

#### Caching API calls - making a performance boost

Facebook API isn't enough fast to call it everytime, sometimes we can just save results to memory and restore it later if we are sure that nothing was changed.
So, every API call supports caching, it's just an additional boolean argument to api() method.

```php
// check if we have already serialized state of FacebookWrapper object somewhere in the user session
if ($panthera -> session -> exists('facebookCache'))
{
    $facebook = new FacebookWrapper('', '', $panthera -> session -> get('facebookCache'));
} else {
    $facebook = new FacebookWrapper;
    $facebook -> loginUser(array('publish_stream', 'user_likes'), 'header', 'http://mywebsite.org/login');
}

$facebook -> api('/me', True); // we don't want to download user personal informations every page refresh, it can be downloaded once, huh?
$cache = $facebook -> serializeState(); // this is a serialized FacebookWrapper data, it can be stored everywhere now
$panthera -> session -> set('facebookCache', $cache);
```
