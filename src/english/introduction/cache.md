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
        <td>varCache</td>
        <td>cache</td>
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
        <td>key</td>
        <td>value</td>
        <td>type</td>
        <td>section</td>
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
