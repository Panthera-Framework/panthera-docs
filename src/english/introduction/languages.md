Languages
==========

Translations are stored in serialized PHP arrays splitted into `domains` like in GNU Gettext.
*Domain is a group of translated strings*, for example user management would have it's own group named "users". This prevents translatons system
from loading unneeded data for current loaded page.

Translation file is not readable for user itself, but there is an elegant class for editing those files. The class is included in `liblangtool` module.

## Creating new language

Locales are directories in /content/locales or /lib/locales eg. _/content/locales/polski_

```php
localesManagement::create('deutsh'); // create Deutsh locale
```
