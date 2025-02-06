
<a id="View"></a>
# View

- [Quick start](#quick-start)
- [Class](#Class)
- [Template Engine Smarty](#Template-Engine-Smarty)
- [Shortcut function `view()`](#Shortcut-function)
- [Assigning Variables](#Assigning-Vars)
- [Output](#Output)
- [Accessing Class methods and objects in smarty template](#Accessing-Class-methods-and-objects-in-smarty-template)

------------------------------------------------------------------------------------------------------------------------

<a id="quick-start"></a>
## Quick start

_creates View `Index` in the given module `Foo`_
~~~bash
php emvicy module:createView Index Foo
~~~
- If module `Foo` does not exist, it will be created as a primary if possible, otherwise as a secondary one.

Find out more about Views, for example on <a href="https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller#View" target="_blank">wikipedia, "Model–view–controller#View", 2023-12-28</a>

------------------------------------------------------------------------------------------------------------------------

<a id="Class"></a>
## Class

writing the View class

- Place the View Class inside your module's View folder (see [/modules/{moduleName}/View/](/2.x/directory-structure#modules-moduleName)
- Use a Pascal Case Name (see <a href="https://wiki.c2.com/?PascalCase" target="_blank">wiki.c2.com/?PascalCase</a>) for the Class file
- The Controller must extend `\App\View`

_Illustration: Module `Foo`, View class `Index` with method `doSomething`_
~~~php
<?php

namespace Foo\View;

use App\View;
use MVC\MVCTrait\TraitViewInit;

class Index extends View
{
    use TraitViewInit;

    /**
     * @throws \ReflectionException
     */
    protected function __construct()
    {
        parent::__construct();

        $this->caching = false;
        $this->registerForSmarty();
    }

    /**
     * @param int $iValue
     * @return void
     */
    public function doSomething(int $iValue)
    {
        display('the value is: ' . $iValue);
    }
    
    /**
     * @return void
     * @throws \SmartyException
     */
    protected function registerForSmarty()
    {
        // classes
        $this->registerClass('MVC\Strings', 'MVC\Strings');
 
        // modifiers
        $this->registerPlugin('modifier','floor', 'floor');
    }
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Template-Engine-Smarty"></a>
## Template Engine Smarty

- Emvicy makes use of the Template Engine `Smarty` Version 4.
- All Templates you define in your module's template folder.
- Emvicy provides a standard set of template files if you create your module via `php emvicy` command (see: [Creating a Module](/2.x/creating-a-module)).
- See the directory structure of the standard set of templates: [/2.x/directory-structure#modules-moduleName-templates](/2.x/directory-structure#modules-moduleName-templates)

_Smarty_  
For more Information about how to code templates with powerful Smarty Template Engine please visit the official Website <a href="https://www.smarty.net/" target="_blank">www.smarty.net</a>

------------------------------------------------------------------------------------------------------------------------

<a id="Shortcut-function"></a>
## Shortcut function `view()`

instead of calling your View class complete

~~~bash
\Foo\View\Index::init()
~~~

you can make use of the shortcut function

~~~bash
view()
~~~

which is defined in `modules/Foo/etc/config/Foo/config/_function.php`



------------------------------------------------------------------------------------------------------------------------

<a id="Assigning-Vars"></a>
## Assigning Variables

**assign any variable to your template**

_assign any variable to your template (assuming module is `Foo`)_
~~~php
view()->assign('myFrontendVariable', 'Any Content');
~~~

In your template you can access that assigned variable this way:

~~~html
{$myFrontendVariable}
~~~

**autoAssign variables**

If you created your module via Emvicy.phar (see: [Creating a Module](/2.x/creating-a-module)) or you added additional
context information to your route by yourself, you can easily auto assign all [additional route infos](/2.x/routing#adding-additional-context-information-to-route):

_autoAssign variables to template (assuming module is `Foo`)_
~~~php
view()->autoAssign(
    Route::getCurrent()
);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Output"></a>
## Output

**render**

_render the template_
~~~php
view()->render();
~~~

**controlling rendering**

switch on/off rendering templates

_render `off`_
~~~php
Event::run('mvc.view.render.off');
~~~

_render `on`_
~~~php
Event::run('mvc.view.render.on');
~~~

**controlling echo out**

switch on/off  to echo out the rendered result

_echoOut `off`_
~~~php
Event::run('mvc.view.echoOut.off');
~~~

_echoOut `on`_
~~~php
Event::run('mvc.view.echoOut.on');
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Accessing-Class-methods-and-objects-in-smarty-template"></a>
## Accessing Class methods and objects in smarty template

You can access any Emvicy functions and Classes  as well as any of your module's functions and classes directly in the templates.

_Examples_
~~~php
// access BasePath
{MVC\Config::get_MVC_BASE_PATH()}

// display a text
{display('this is just a test')}

// shows additional info of the current route object
{info(MVC\Route::getCurrent()->get_additional())}
~~~

Also you can access methods of assigned Objects:

_Example_
~~~php
{$oDTRoutingAdditional->get_sContent()}
~~~