Before you start
================

Before you start learning how everything works it's good to have any knowledge on how to test it's all.
So, there are two methods depending on what you preffer and what you are able to run.

### phpsh - PHP Shell

phpsh is a great interactive PHP console created by Facebook developers, and works just like Python console.
In Panthera we are using it very often to maintain our Panthera based websites and for test cases. It allows eg. to change user's password with a single command.

To start using phpsh you must first install it, I assume you'r OS package manager don't have it in repositories, so we are going to clone it from GIT.

```bash
cd /tmp
git clone https://github.com/facebook/phpsh.git
cd phpsh
sudo python setup.py install
pecl install pcntl
```

Now you can navigate to you'r Panthera application's root directory and execute:

```bash
# phpsh _phpsh.php
```

```bash
panthera@server: / $ phpsh _phpsh.php 
Starting php with extra includes: ['_phpsh.php']
Generating output
Client addr(127.0.0.1 50141 22) => CLI /usr/local/lib/python2.7/dist-packages/phpsh/phpsh.php
[0.0297930, 1.9381046ms real] [pantheraCore] Including userspace implementation of password hashing
[0.0298569, 0.0638961ms] [pantheraCore] Using "blowfish" algorithm
[0.0306069, 0.7500648ms] [pantheraCore] varCache initialized, using memcached
[0.0504329, 19.825935ms] [pantheraCore] Requested Rain\Tpl class
[0.0520169, 1.5840530ms] [pantheraLocale] setLocale(polski)
[0.0520861, 0.0691413ms] [pantheraLocale] Adding domain "messages" from /.../content/locales/polski
[0.0530040, 0.9179115ms] [pantheraLocale] Read id=locale.polski.messages from cache
[0.0532751, 0.2710819ms] [pantheraCore] Loading /.../content/plugins/... plugin
[0.0536749, 0.3998279ms] [] Registering plugin ... for file /.../content/plugins/.../plugin.php, key=...
[0.0537810, 0.1060962ms] [pantheraLogging] Generating output
[0.0541279, 0.3719329ms] [pantheraLogging] Done
type 'h' or 'help' to see instructions & features
php> $u = new pantheraUser('login', 'webnull');
query( SELECT * FROM `pa_users` WHERE `login` = :login , {"login":"webnull"} )
Clearing cache record, id=pa_users.s:2:"id";.6
Clearing cache record, id=pa_users.s:5:"login";.webnull
Clearing cache record, id=pa_users.s:9:"full_name";.Damian Kęska
Updated cache id=pa_users.s:5:"login";.webnull
pantheraUser::Found a record by ""login"" (value="webnull")
query( SELECT * FROM `pa_metas` WHERE `userid` = :objectID AND `type` = :type , {"objectID":"6","type":"u"} )
Loaded meta overlay, counting overall 5 elements
Wrote meta to cache id=meta.u.6
query( SELECT * FROM `pa_metas` WHERE `type` = :type AND `userid` = :objectID , {"type":"g","objectID":"users"} )
Wrote to cache id=meta.overlay.g.users
php> print_r($u -> getName());
Damian Kęska
```

### As CGI script

If you don't have access to shell, or you don't like terminal you can test everything in CGI mode.
A good solution to test in CGI mode is to create a front controller in main directory of your application.

```php
include 'content/app.php'; // Panthera Framework + configuration

if ($_SERVER['REMOTE_ADDR'] != '127.0.0.1')
{
    print("Only localhost allowed.");
    exit;
}

$u = new pantheraUser('login', 'webnull');
print_r($u->getName());
```

### Debugging variables

There are some functions built-in Panthera and PHP core library for easy debugging.

#### Printing arrays, strings, ints, object variables

```php
php> $a = 'test';

php> var_dump($a);
string(4) "test"

php> $b = array('a' => 1, 'b' => 2);

php> var_dump($b);
array(2) {
  ["a"]=>
  int(1)
  ["b"]=>
  int(2)
}

php> $a = r_dump($b); // save var_dump output to $a
```

#### Getting list of object/class functions

```php
php> object_info('pantheraAutoloader');
/**
  * Rebuild autoloader database                                                                                                                      
  *                                                                                                                                                  
  * @package Panthera\modules\autoloader.tools                                                                                                       
  * @author Damian Kęska                                                                                                                             
  */                                                                                                                                                 
Class [ <user> class pantheraAutoloader ] {                                                                                                          
  @@ /home/webnull/public_html/panthera/lib/modules/autoloader.tools.module.php 17-117                                                               

  - Constants [0] {                                                                                                                                  
  }                                                                                                                                                  

  - Static properties [0] {                                                                                                                          
  }                                                                                                                                                  

  - Static methods [2] {                                                                                                                             
    /**                                                                                                                                              
      * Panthera autoloader cache update cronjob                                                                                                     
      *                                                                                                                                              
      * @param string $data                                                                                                                          
      * @return mixed                                                                                                                                
      * @author Damian Kęska                                                                                                                         
      */                                                                                                                                             
    Method [ <user> static public method updateCache ] {                                                                                             
      @@ /home/webnull/public_html/panthera/lib/modules/autoloader.tools.module.php 27 - 76                                                          

      - Parameters [1] {                                                                                                                             
        Parameter #0 [ <optional> $data = '' ]                                                                                                       
      }                                                                                                                                              
    }                                                                                                                                                

    /**                                                                                                                                              
      * Get list of declared class in PHP file (without including it)
      *
      * @param string $fileName
      * @return array 
      * @author AbiusX <http://stackoverflow.com/questions/7153000/get-class-name-from-file>
      */
    Method [ <user> static public method fileGetClasses ] {
      @@ /home/webnull/public_html/panthera/lib/modules/autoloader.tools.module.php 86 - 116

      - Parameters [1] {
        Parameter #0 [ <required> $fileName ]
      }
    }
  }

  - Properties [0] {
  }

  - Methods [0] {
  }
}


php> 
```

#### Printing arrays

```php
php> $a = array(1, 2, 3, 4, 5);

php> print_r($a);
Array
(
    [0] => 1
    [1] => 2
    [2] => 3
    [3] => 4
    [4] => 5
)
```
