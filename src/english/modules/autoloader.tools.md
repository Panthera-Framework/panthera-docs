## Panthera Autoloader tools

Autoloader is collecting list of modules and it's classes to simply allow dynamic loading of classes.

#### Example of autoloading class on demand

```php
// we don't have googlepr module loaded, but it will be loaded on demand
GooglePR::getRank('http://duckduckgo.com');
```

#### Reloading autoloader cache

If there was added any class recently you should reload autoloader cache to add it to list of all classes.

```php
pantheraAutoloader::updateCache();
```
