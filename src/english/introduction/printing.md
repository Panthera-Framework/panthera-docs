Generating printable pages
==========================

Printing module is a part of "boot" modules that are triggered by GET parameters. 
The code is placed in /modules/boot/print.module.php and contains simple HTML and PDF exporting functions.

To make a page printable you have to create a directory "printable" for HTML prints or "printable-pdf" for PDF export inside
of your template directory.

Example of template directory structure:

```
/content/templates/mysite/templates/here-is-my-template.tpl
/content/templates/mysite/templates/printable/here-is-my-template.tpl
/content/templates/mysite/templates/printable-pdf/here-is-my-template.tpl
```

Now if you created a template file in HTML format and placed it in a printable directory with same name used to display normal page
you can invoke a page with additional parameter "\__print=pdf" or "\__print=plain"

## Manually generating PDF documents

There is no need to use a GET parameter as there is possibility to maunally invoke static method of printingModule.

**pdf-template.tpl**
```html
<h2>{$document_title}</h2>
{$description}
```

**Example PHP code**
```php
$panthera -> template -> push('document_title', 'This is a page title');
$panthera -> template -> push('description', 'Description');

printingModule::$printPDFName = "test.pdf";

// will print a file from eg. /content/templates/mysite/templates/printable-pdf/pdf-template.tpl
// where: mysite - is your base theme name. eg. in admin panel its just "admin"
printingModule::printPDF('pdf-template.tpl', array());

