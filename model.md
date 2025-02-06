
<a id="Model"></a>
# Model

- [Quick start](#quick-start)
- [Class](#Class)
- [Examples](#Examples)
    - [Controller](#Example-Controller)
    - [Master Controller](#Example-Master-Controller)

------------------------------------------------------------------------------------------------------------------------

<a id="quick-start"></a>
## Quick start

_creates Model `Bar` in the given module `Foo`_
~~~bash
php emvicy module:createModel Bar Foo
~~~
- If module `Foo` does not exist, it will be created as a primary if possible, otherwise as a secondary one.

Find out more about Models, for example on <a href="https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller#Model" target="_blank">wikipedia, "Model–view–controller#Model", 2023-12-28</a>

------------------------------------------------------------------------------------------------------------------------

<a id="Class"></a>
## Class

There are no specific restrictions regarding a model class. For example, methods can have any visibility, the class can also follow the singleton pattern, etc.

writing the Model class

- Place the Model Class inside your module's Model folder (see [/modules/{moduleName}/Model/](/2.x/directory-structure#modules-moduleName)
- Use a Pascal Case Name (see <a href="https://wiki.c2.com/?PascalCase" target="_blank">wiki.c2.com/?PascalCase</a>) for the Class file

_Illustration: Module `Foo`, Model `Bar` with static method `doSomething`_
~~~php
<?php

namespace Foo\Model;

class Bar
{
    /**
     * @param int $iValue
     * @return void
     */
    public static function doSomething(int $iValue)
    {
        // your code …
    }
}
~~~

 
