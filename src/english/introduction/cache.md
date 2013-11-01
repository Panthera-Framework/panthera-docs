Cache management
=================

Cache is a very simple key-value memory that aims to be ultra responsive. 
It's used to store results of many heavy operations, and of course can be used to increase database speed storing users, simple variables or comments in RAM memory.

Panthera supports many caching systems that are divided by architecture and performance.

<table>
    <tr>
        <td></td>
        <td><b>Memcached</b></td>
        <td><b>APC</b></td>
        <td><b>XCache</b></td>
        <td><b>Redis</b></td>
        <td><b>Database</b></td>
    </tr>
    
    <tr>
        <td>Architecture</td>
        <td>client-server</td>
        <td>PHP's shared memory</td>
        <td>PHP's shared memory</td>
        <td>client-server</td>
        <td>external relational database socket</td>
    </tr>
    
    <tr>
        <td>Performance</td>
        <td>fast</td>
        <td>ultra fast</td>
        <td>ultra fast</td>
        <td>fast</td>
        <td>bad</td>
    </tr>
</table>

## Configuration

There are two caching slots - `cache` and `varCache`. varCache should be used to store little, non-massive elements.
In Cache slot there should be stored elements that needs ultra-fast performance, it can't be a database type cache.

<table>
    <tr>
        <td><b>varCache</b></td>
        <td><b>cache</b></td>
    </tr>
    
    <tr>
        <td>Simple data</td>
        <td>Massive storage of any data</td>
    </tr>
    
    <tr>
        <td>Can be even a database</td>
        <td>Must be a extra responsive caching system</td>
    </tr>
</table>

If you have access to a quick cache eg. APC or Memcached use it in both slots, avoid using slower caching systems in any slots.

Example configuration (config_overlay - can be set via $panthera -> config -> setKey()):

<table>
    <tr>
        <td><b>key</b></td>
        <td><b>value</b></td>
        <td><b>type</b></td>
        <td><b>section</b></td>
    </tr>
    
    <tr>
        <td>varcache_type</td>
        <td>apc</td>
        <td>string</td>
        <td></td>
    </tr>
    
    <tr>
        <td>cache_type</td>
        <td>apc</td>
        <td>string</td>
        <td></td>
    </tr>
</table>

## Practical example of usage

In langtool - translation editor for Panthera Framework we have automatic searching for missing translations, it needs to scan all files in /content and /lib directories
to find and parse them, it usually takes ~3 seconds so it can't be executed every page refresh. Langtool is executing research only once, and then all results are stored in cache memory
and every page refresh only results are grabbed from cache, and if cache will expire or is empty it will be regenerated - files will be scanned and results will be saved to cache.

```php
if ($panthera->cache)
{
    if ($panthera -> cache -> exists('results-of-heavy-job'))
    {
        $results = $panthera -> cache -> get('results-of-heavy-job');
    }
}

if (!isset($results))
{
     $results = doAHeavyJobThatWillTookAbout10Seconds();
     
     if ($panthera->cache)
         $panthera -> cache -> set('results-of-heavy-job', $results, 86400); // 1 day in seconds
}
```
