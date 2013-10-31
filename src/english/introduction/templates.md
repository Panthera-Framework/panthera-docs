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
