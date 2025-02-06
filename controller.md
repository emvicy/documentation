
<a id="Controller"></a>
# Controller

- [Quick start](#quick-start)
- [Class](#Class)
    - [Method Arguments](#Controller-method-Arguments)
    - [special method `__preconstruct`](#preconstruct)
- [Examples](#Examples)
  - [Controller](#Example-Controller)
  - [Master Controller](#Example-Master-Controller)

---

<a id="quick-start"></a>
## Quick start

_creates controller `Bar` in the given module `Foo`_  
~~~bash
php emvicy module:createController Bar Foo
~~~
- If module `Foo` does not exist, it will be created as a primary if possible, otherwise as a secondary one

Find out more about Controllers, for example on <a href="https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller#Controller" target="_blank">wikipedia, "Model–view–controller#Controller", 2023-12-28</a>

---

<a id="Class"></a>
## Class

writing the Controller class

- Place the Controller Class inside your module's Controller folder (see [/modules/{moduleName}/Controller/](/2.x/directory-structure#modules-moduleName)
- Use a Pascal Case Name (see <a href="https://wiki.c2.com/?PascalCase" target="_blank">wiki.c2.com/?PascalCase</a>) for the Class file
- The Controller must have implement the interface `\MVC\MVCInterface\Controller`; therefore simply extend App\Controller: `class Index extends App\Controller { }` as it fullfills the required interface.

Now in any methods of that class you can place your business logic.

_Illustration: Module `Foo`, Controller `Bar` with method `index`, responsible for incoming requests via the corresponding Route_
~~~php
<?php

namespace Foo\Controller;
use App\Controller;
use MVC\DataType\DTRequestIn;
use MVC\DataType\DTRoute;

class Bar extends Controller
{    
    /**
     * @return void
     * @throws \ReflectionException
     */
    public static function __preconstruct()
    {
        parent::__preconstruct();
    }
    
    /**
     * @param \MVC\DataType\DTRequestIn $oDTRequestIn
     * @param \MVC\DataType\DTRoute     $oDTRoute
     * @throws \ReflectionException
     */
    public function __construct(DTRequestIn $oDTRequestIn, DTRoute $oDTRoute)
	{
        parent::__construct($oDTRequestIn, $oDTRoute);
    }    
    
    /**
     * @param \MVC\DataType\DTRequestIn $oDTRequestIn
     * @param \MVC\DataType\DTRoute     $oDTRoute
     * @return void
     * @throws \ReflectionException
     */
    public function index(DTRequestIn $oDTRequestIn, DTRoute $oDTRoute)
	{
	    ;    
	}
}
~~~

---

<a id="Controller-method-Arguments"></a>
## Method Arguments

There are two DataType objects as Arguments sending to a Controller method by default:

`DTRequestIn $oDTRequestIn`    
- this DataType Object represents the Incoming Request
- see [DTRequestIn](/2.x/datatype-classes#DTRequestIn)

`DTRoute $oDTRoute`  
- this DataType Object represents the Responsible Route
- see [DTRoute](/2.x/datatype-classes#DTRoute)

_Example Usage of parameters_
~~~php
public function index(DTRequestIn $oDTRequestIn, DTRoute $oDTRoute)
{
    // get potential data sent
    $mInput = $oDTRequestIn->get_input();
    
    // get additional object
    /** @var \MVC\DataType\DTRoutingAdditional $oDTRoutingAdditional */
    $oDTRoutingAdditional = $oDTRoute->get_additional();

    // get title 
    $sTitle = $oDTRoutingAdditional->get_sTitle();        
}
~~~

---

<a id="preconstruct"></a>
## special method `__preconstruct`

this static method is called by `\MVC\Application`  
- **after** policy rules have been taken into account
- **before** Session has been created
- **before** the regular instantiation via the `__construct` method of the controller class

This way, preparatory work can be carried out, such as loading certain configurations, applying filters or checking any authorisations.


---

<a id="Examples"></a>
# Examples

<a id="Example-Controller"></a>
## Example Controller

here you find a complete Controller class extending a `_Master` Controller.

_Example Controller `/modules/Foo/Controller/Index.php`_
~~~php
<?php

namespace Foo\Controller;

use App\Controller;
use Foo\Controller\Regular\Master;
use MVC\DataType\DTRequestIn;
use MVC\DataType\DTRoute;
use MVC\Http\Status_Forbidden_403;
use MVC\Http\Status_Not_Found_404;

class Index extends Master
{
    /**
     * @return void
     * @throws \ReflectionException
     */
    public static function __preconstruct()
    {
        parent::__preconstruct();
    }

    /**
     * @param \MVC\DataType\DTRequestIn $oDTRequestIn
     * @param \MVC\DataType\DTRoute     $oDTRoute
     * @throws \ReflectionException
     */
    public function __construct(DTRequestIn $oDTRequestIn, DTRoute $oDTRoute)
    {
        parent::__construct($oDTRequestIn, $oDTRoute);
    }

    /**
     * @param \MVC\DataType\DTRequestIn $oDTRequestIn
     * @param \MVC\DataType\DTRoute     $oDTRoute
     * @return void
     * @throws \ReflectionException
     */
    public function index(DTRequestIn $oDTRequestIn, DTRoute $oDTRoute)
    {
        view()->autoAssign();
    }

    /**
     * @param \MVC\DataType\DTRequestIn $oDTRequestIn
     * @param \MVC\DataType\DTRoute     $oDTRoute
     * @return void
     * @throws \ReflectionException
     */
    public function forbidden(DTRequestIn $oDTRequestIn, DTRoute $oDTRoute)
    {
        Status_Forbidden_403::header();
        view()->autoAssign();
    }
    
    /**
     * @param \MVC\DataType\DTRequestIn $oDTRequestIn
     * @param \MVC\DataType\DTRoute     $oDTRoute
     * @return void
     * @throws \ReflectionException
     */
    public function notFound(DTRequestIn $oDTRequestIn, DTRoute $oDTRoute)
    {
        Status_Not_Found_404::header();
        view()->autoAssign();
    }

    /**
     * @throws \ReflectionException
     * @throws \SmartyException
     */
    public function __destruct ()
    {
        parent::__destruct();
        view()->render();
    }
}
~~~

<a id="Example-Master-Controller"></a>
## Example Master Controller

*Example Master Controller `/modules/Foo/Controller/_Master`*
~~~php
<?php

namespace Foo\Controller\Regular;

use App\Controller;
use MVC\DataType\DTRequestIn;
use MVC\DataType\DTRoute;
use MVC\Http\Header;
use MVC\MVCTrait\TraitDataType;

/**
 * @extends Controller
 */
class Master extends Controller
{
    use TraitDataType;

    /**
     * @return void
     * @throws \ReflectionException
     */
    public static function __preconstruct()
    {
        parent::__preconstruct();
    }

    /**
     * @param \MVC\DataType\DTRequestIn $oDTRequestIn
     * @param \MVC\DataType\DTRoute     $oDTRoute
     * @throws \ReflectionException
     */
    public function __construct(DTRequestIn $oDTRequestIn, DTRoute $oDTRoute)
    {
        parent::__construct($oDTRequestIn, $oDTRoute);
        view();
        Header::init()->ContentSecurityPolicy();
    }

    public function __destruct()
    {
        parent::__destruct();
    }
}
~~~