Templates
==========

Every website looks in some way, many of them have more than one theme. It's all done because of separation between logical code,
data modell and a view.

All Panthera templates are stored in `/lib/templates` and `/content/templates`. Functions for template management are placed in templates.class.php and libtemplate module.

## Structure of templates

<table>
    
        <tr><td>configs</td><td>Contains configuration files in JSON format, every template file have it's own eg. main.tpl.json</td></tr>
        <tr><td>templates</td><td>Here are stored all template files ready to compile</td></tr>
        <tr><td>webroot</td><td>This directory contains all files to copy to application's main directory, eg. CSS styles, javascripts etc.</td></tr>
        <tr><td>config.json</td><td>Meta file, describes main template file, mobile and tablet template</td></tr>
        <tr><td>thumbnail.png</td><td>Thumbnail to display in Admin Panel</td></tr>
</table>

## Web root

Template webroot directory contains all files that may be copied to application root directory (directory where are placed public files like index.php).
Best practice is to keep this directory clean, so the CSS styles should be in their "css" directory, javascript and images also should be separated.

## Practical usage

Basic usage of template system is to assign variables and just... display it. But of course in Panthera we have scripts, styles, meta tags management.

```php
$panthera -> template -> setTitle('Wow, this site has title, but i didn\'t use any title tag yet...');
$panthera -> template -> push('text', 'hello :-)');
$panthera -> template -> display('text_demo.tpl');
```

text_demo.tpl

```html
<!DOCTYPE html>
<html>
    <head>
    {$site_header}
    </head>
    
    <body>
    {$text|strtolower}<br>
    "Yes" in current language: {function="localize('Yes')"}
    </body>
</html>
```

For more syntax examples please visit [RainTPL official website](http://www.raintpl.com/Documentation/). 

### Environment

There is a very useful variable that is used in every template file - it's called `{$PANTHERA_URL}` and is pointing directly to URL to your website's main directory.
