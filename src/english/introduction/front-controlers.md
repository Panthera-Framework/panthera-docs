Front controllers
=================

Front controllers handles client requests directly and running suitable action controllers or modules. All standard controllers should be linked from **/lib/frontpages** to root directory of an application. Copied controllers wont be up-to-date after Panthera update.

##  Default front controllers built-in Panthera

For some modules there are already built-in example front controllers you can use in your application. You can also create your own and place it in application root directory.

* index.php - *show pages from /lib/pages and /content/pages directory*
* pa-login.php - *sign in user (using mainly to login to admin panel, but it can be also used to sing in for normal users to appliaction)*
* _ajax.php - *show pages from ajaxpages directory*
* pa-admin.php - *admin panel*
* _crontab.php - *runs planned jobs (like sending emails)*
* install.php - *installer, could be used to move to another server or as tool to reset/repair configuration*
* _phpsh.php - *integrate phpsh tool with Panthera, which creates interactive Panthera console*
