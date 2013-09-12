Panthera installation
=====================

Project is hosted on Github, we are using GIT to manage code so it can be easily downloaded from shell, dependencies are installable via [composer](http://getcomposer.org/).

1. Downloading source files and dependences from GIT
  
  ```
  git clone https://github.com/webnull/panthera -b master
  cd panthera
  ./install.sh
  ```

  After execution above mentioned commands, every file and dependences should be downloaded (templates parser RainTPL, whether library for sending emails - phpMailer).

  ```
  mv example-app give-me-name
  ```

  In example-app directory is example application available to install. It's the best moment to change the name, because later it could be a little bit harder.


2. Installation through built-in installer

  Now it's the time to run the installer in web browser. Navigate to __**/give-me-name/install.php**__, if installer returns a message about absence possibility to write configuration, we must give appropiate permissions to all directories in WWW content. It's needed, because every module of Panthera must have possibility to write over there in update destination, installation new modules or change site content.


3. Repeating installation process

  For some reason sometimes we need to repeat installation process - for instance it could short-circuit moving application on other server.
  In this case we are deleting every files from **installer**, directory, which is in main directory of appliaction and we are creating configuration variable "**__requires_instalation__**" with value **True** and "**__preconfigured__**" with value **False**.
  
  We can delete app.php file, it cause deleting every configuration of appliaction.
  Next we can execute installator one time yet from we browser as earlier.
