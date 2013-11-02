Front controllers
=================

Front controllers handles client requests directly and running suitable action controllers or modules. All standard controllers should be linked from **/lib/frontpages** to root directory of an application. Copied controllers wont be up-to-date after Panthera update.

A good example could be index.php placed in your application's root directory.

```php
<?php
/**
  * Default front controller
  *
  * @package Panthera\core
  * @author Damian Kęska
  * @license GNU Affero General Public License 3, see license.txt
  */
  
  
if (!is_file('content/app.php'))
{
    header('Location: install.php');
    exit;
}

require 'content/app.php';

// include custom functions to default front controller
if (is_file('content/front.php'))
    require 'content/front.php';
    
// enable frontside panels
frontsidePanels::init();

$display = str_replace('/', '', addslashes($_GET['display']));
$template -> setTemplate($panthera->config->getKey('template'));
$path = False;

if (!defined('PAGES_DISABLE_LIB'))
{
    $path = getContentDir('/pages/' .$display. '.php');
} else {
    if (is_dir(SITE_DIR. '/content/pages/' .$display. '.php'))
        $path = SITE_DIR. '/content/pages/' .$display. '.php';
}

// here we will include site pages
if ($path)
{
    @include($path);
    pa_exit();
} else {
    define('SITE_ERROR', 404);
    @include(SITE_DIR. '/content/pages/index.php');
}

$template -> display();
$panthera -> finish();
?>
```

As you can see it's initializing the framework by including content/app.php (which contains configuration and includes Panthera library),
and then it's initializing frontSidePanels module, page content from /lib/pages or /content/pages directories.

Maybe now a more simpler example, just like Hello World. So, let's create a front controller that will show us if we are logged in or not and display user name.

```php
<?php
include('content/app.php'); // include Panthera Framework

if ($panthera->user)
{
    print("You are logged in as ".$panthera->user->getName());
} else {
    print("You are not logged in.");
}

pa_exit(); // just in case, this will finish all pending processes like saving data to database
?>
```

It isn't really complicated, it's just a bit of structural programming using fully objective framework you can learn to use by reading our manual or generated documentation, or just reading the code :-)

##  Default front controllers built-in Panthera

For some modules there are already built-in example front controllers you can use in your application. You can also create your own and place it in application root directory.

* index.php - *show pages from /lib/pages and /content/pages directory*
* pa-login.php - *sign in user (using mainly to login to admin panel, but it can be also used to sing in for normal users to appliaction)*
* _ajax.php - *show pages from ajaxpages directory*
* pa-admin.php - *admin panel*
* _crontab.php - *runs planned jobs (like sending emails)*
* install.php - *installer, could be used to move to another server or as tool to reset/repair configuration*
* _phpsh.php - *integrate phpsh tool with Panthera, which creates interactive Panthera console*
