Users and groups
=================

All functions for users and groups management are placed in /lib/user.class.php, users management is handled by `pantheraUser` class that is able to manage user meta tags, changing password and parsing most of data.

Interface is very simple, it's all based on pantheraFetchDB, so we are able to find user by login, full name or id.

```php
$u = new pantheraUser('id', 1);
```

```php
$u = new pantheraUser('login', 'myuser');
```

#### Changing password

changePassword function is automaticaly adding salt and hashing input password with selected algorithm in Panthera configuration.
Avaliable hashing algorithms are blowfish, md5 and sha512.

```php
$u -> changePassword('test123');

if ($u -> verifyPassword('test123'))
{
    print("Password matches.");
}
```

##### Blocking user

It can't be more simpler.

```php
if (!$u -> isBanned())
{
    print("User is not banned but will be banned now.");
    $u -> isBanned(True);
}
```

##### Getting user's name (or login if full name is not avaliable)

Returns full name or login if full name is not avaliable

```php
var_dump($u -> getName());
```

#### Meta attributes

Meta attributes are provided by `metaAttributes` class, so it's used also in other objects - groups, comments, static pages etc.
Usage is very simple and is similar to cache or config management.

```php
$u -> acl -> set('admin', True); // set user to be an admin
var_dump($u -> acl -> get('admin'));

// if you want to be sure that all data was saved
$u -> acl -> save();
```

#### Editing raw data

As `pantheraUser` is a `pantheraFetchDB` based class it inherits interface that allows modyfing user data by setting class variables of created object.

```php
$u -> full_name = 'Jan Kowalski;
$u -> profile_picture = 'http://example.com/example.jpg';
$u -> save();
```

Above example is just directly editing `full_name` and `profile_picture` columns in SQL database for selected user.
