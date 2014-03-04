Pages on my website - front and action controllers
==================================================

When your page is beign opened within a simple url like http://example.org then user is just calling **index.php from application's main directory**.
This file is a front controller, **also pa-login.php, pa-admin.php, _ajax.php are front controllers** as they are placed in main directory, public to all people.
There are also things called "action controllers". **Action controllers are placed in /content/pages** and /content/ajaxpages and are called by front controllers eg. when user makes a request "?display=custom&id=1" then
index.php front controller is used and /content/pages/custom.php action controller.

To overwrite a front controller you can simply make a include.

```php
<?php
unset($_GET['url_id']);
unset($_GET['unique']);
unset($_GET['id']);
unset($_GET['forceNativeLanguage']);

$_GET['url_id'] = 'rules';

include PANTHERA_DIR. '/pages/custom.php';
```

It's very stupid and simple but creates a new action controller that directly shows you a selected custom page editable from admin panel.

### Objective action controllers

The best programming practice is to write objective code, in Panthera there are two ways - structural or objective controllers.

Advantages of objective controllers:
1. Readability
2. Additional functions built-in class
3. Any class function can be easily extended by creating new class that extends existing one

Every front controller should contain a display() method, it's like main() in C/C++. In return there should be given a string with rendered HTML code. Another way is to use $this -> template -> display() and pa_exit() functions.

**Example:**

```php
class testAjaxControllerCore extends frontController
{
    // if page will be using ui.Titlebar (it's required in pages created for admin panel)
    protected $uiTitlebar = array(
        'Test page', 'languagedomain'
    );
    
    // list of modules we will use in our code (this is optional)
    protected $requirements = array(
        'googlepr',
    );
    
    public function doSomething()
    {
        if ($this -> panthera -> user)
        {
            $this -> panthera -> template -> push('userName', $this -> panthera -> user -> getName());
        }
        
        $this -> panthera -> template -> push('github_rank', GooglePR::getRank('github.com'));
    }

    // our core function
    public function display()
    {
        $this -> doSomething();
        return $this -> panthera -> template -> compile('test.tpl');
    }
}
```

Above code will work under url ?display=test&cat=admin if placed in /content/ajaxpages/admin/test.php
**Class name should contain "AjaxController" or "AjaxControllerCore".**

#### Handling actions

Actions such as edit, delete, hide content, etc. can be handled by class methods.

If you add a single line $this -> dispatchAction() to your display() function you will be able to create methods with action names.

```php
(...)

public function createNewAction()
{
    // i will be displayed when "action=createNew" GET parameter will appear
    
    $this -> panthera -> template -> display('createNewForm.tpl');
    pa_exit();
}

public function showAction()
{
    // i will be displayed when "action=show" GET parameter will appear
    ajax_exit(array('status' => 'success', 'result' => 'This is a test'));
}

public function display()
{
    (...)
    $this -> dispatchAction();
    (...)
}
(...)
