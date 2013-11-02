Languages
==========

Translations are stored in serialized PHP arrays splitted into `domains` like in GNU Gettext.
*Domain is a group of translated strings*, for example user management would have it's own group named "users". This prevents translatons system
from loading unneeded data for current loaded page.

Translation file is not readable for user itself, but there is an elegant class for editing those files. The class is included in `liblangtool` module.

## Creating a new language

Locales are just directories in `/content/locales` or `/lib/locales` eg. _/content/locales/polski_ but remember - don't create any files in /lib directory.

```php
localesManagement::create('deutsh'); // create Deutsh locale
```

There is also possible to create a language from built-in page in Admin Panel.

## Making a new language domain

To create new domain you can also use a single line of code and execute it in CGI or shell.

```php
localesManagement::createDomain('deutsh', 'welcome'); // messages - it's a domain name
```

## Translating a string

Untranslated strings are strings placed inside of PHP or template code in application's native language (usually it's English).

Example:

```html
<div class="body">{function="localize('Welcome to my first website based on Panthera Framework', 'welcome');"}</div>
```

In above exapmle there is one untranslated string - "Welcome to my first website based on Panthera", we are going to translate it to Deutsh.

```php
$domain = new localeDomain('deutsh', 'welcome');
$domain -> setString('Welcome to my first website based on Panthera Framework', 'Willkommen auf meiner ersten Website auf Panthera Framework');
$domain -> save();

// make a test
$panthera -> locale -> setLocale('deutsh');
var_dump(localize('Welcome to my first website based on Panthera Framework', 'welcome'));
```
