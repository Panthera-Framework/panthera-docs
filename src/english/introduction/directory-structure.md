Structure of directories
========================

There are three the most principal directories in Panthera Framework: **/lib**, his path is inside code store variable PANTHERA_DIR, **main 
directory of appliaction** define inside as SITE_DIR and **content** is SITE_DIR/content.

There is basic rule - __we are never modifing libraries of Panthera, we are working only on files which belongs to application__. Panthera is framework 
which maybe extended, but not serve to modifing his source code for needs one appliaction.

**Tip:** One /lib directory can be shared between multiple applications

Main directory of appliaction
========================

In main directory of appliaction there are front controllers - files with PHP code which are executed directly from web browser. That is inter 
alios: index.php, _ajax.php whether install.php

To get full path to main directory of appliaction you need to read value of variable *SITE_DIR*.

```
php> var_dump(SITE_DIR);
string(42) "/home/myuser/public_html"
```

/lib and /content
=================

First of this directories - **/lib** keep files of Panthera, which we shouldn't touch. Also there are default templates of admin panel, which of 
course we can overwrite creating files with the same names in directory **/content**.

* ajaxpages - *pages shown by _ajax.php front controller*
* backups - *place for backups, mainly databases*
* database - there are files of SQLite3 and templates of database, which are ready to import
