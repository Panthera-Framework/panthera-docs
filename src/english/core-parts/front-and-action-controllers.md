Pages on my website - front and action controllers
==================================================

When your page is beign opened within a simple url like http://example.org then user is just calling index.php from application's main directory.
This file is a front controller, also pa-login.php, pa-admin.php, _ajax.php are front controllers as they are placed in main directory, public to all people.
There are also things called "action controllers". Action controllers are placed in /content/pages and /content/ajaxpages and are called by front controllers eg. when user makes a request "?display=custom&id=1" then
index.php front controller is used and /content/pages/custom.php action controller.

To overwrite a front controller you can simply make a include.

```php
<?php
unset($_GET['url_id']);
unset($_GET['unique']);
unset($_GET['id']);
unset($_GET['forceNativeLanguage']);

$_GET['url_id'] = 'rules';

include PANTHERA_DIR. '/pages/custom.php';
```

It's very stupid and simple but creates a new action controller that directly shows you a selected custom page editable from admin panel.
