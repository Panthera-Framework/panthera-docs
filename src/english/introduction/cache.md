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
