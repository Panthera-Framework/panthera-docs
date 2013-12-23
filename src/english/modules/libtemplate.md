## libtemplate

Libtemplate extends template.class.php with rarely used functions that don't need to be included on every page refresh.

#### webrootMerge

Webroot merging mechanism is able to detect modified files in template's webroots eg. /content/templates/front/webroot
In this directory there should be stored all public files like css, js and image files.

```php
// this will copy admin panel, default site template and all its mobile views
libtemplate::webrootMerge();

// merge all templates plus "canvas" but only from content
libtemplate::webrootMerge(array('canvas' => False));

// merge all templates plus "example" from /lib and /content
libtemplate::webrootMerge(array('example' => True));
```

##### Configuration

<table>
    <tr>
        <td>key</td>
        <td>value</td>
        <td>type</td>
        <td>section</td>
        <td>comment</td>
    </tr>
    
    <tr>
        <td>webroot.templates</td>
        <td>array('example' => True)</td>
        <td>array</td>
        <td>webroot</td>
        <td>Works same as first argument of libtemplate::webrootMerge function</td>
    </tr>
    
    <tr>
        <td>webroot.excluded</td>
        <td>array('canvas' => True)</td>
        <td>array</td>
        <td>webroot</td>
        <td>Set allways to true to exclude a template</td>
    </tr>
</table>

#### listTemplates

Listing template files or getting list of all avaliable templates from /lib and /content.

##### Listing all avaliable themes from /content and /lib (templates)

```php
php> print_r(libtemplate::listTemplates());
Requested libtemplate class
Imported "libtemplate" from /lib/modules
Array
(
    [_libs_webroot] => Array
        (
            [item] => /var/www/public_html/panthera/lib/templates/_libs_webroot
            [place] => lib
        )

    [_system] => Array
        (
            [item] => /var/www/public_html/panthera/lib/templates/_system
            [place] => lib
        )

    [admin] => Array
        (
            [item] => /var/www/public_html/darbs-tools/content/templates/admin
            [place] => content
        )

    [admin_mobile] => Array
        (
            [item] => /var/www/public_html/panthera/lib/templates/admin_mobile
            [place] => lib
        )

    [installer] => Array
        (
            [item] => /var/www/public_html/panthera/lib/templates/installer
            [place] => lib
        )

    [example] => Array
        (
            [item] => /var/www/public_html/darbs-tools/content/templates/example
            [place] => content
        )

)
```

##### Listing template files (.tpl files) for "admin" theme

```php
php> print_r(libtemplate::listTemplates('admin'));
Array
(
    [_popup_jsonedit.tpl] => Array
        (
            [item] => /var/www/public_html/panthera/lib/templates/admin/templates/_popup_jsonedit.tpl
            [place] => lib
        )

    (...)

    [users_account.tpl] => Array
        (
            [item] => /var/www/public_html/panthera/lib/templates/admin/templates/users_account.tpl
            [place] => lib
        )

)
```
