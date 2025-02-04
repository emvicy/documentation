<a id="Controller"></a>
# Controller

- [Quick start](#quick-start)
- [Controller](#Controller)
    - [Method Parameter](#Controller-method-Parameter)
    - [special method `__preconstruct`](#preconstruct)
- [Example](#Example)
  - [Controller](#Example-Controller)
  - [Master Controller](#Example-Master-Controller)

---

<a id="quick-start"></a>
## Quick start

_creates controller `Bar` in the given module `Foo`_  
~~~bash
php emvicy module:createController Bar Foo
~~~

---

*"The controller responds to the user input and performs interactions on the data model objects. The controller receives the input, optionally validates it and then passes the input to the model."  
<small><a href="https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller#Interactions" target="_blank">wikipedia, "Model–view–controller#Interactions", 2023-12-28</a>*

A Controller accepts input and converts it to commands for the model or view. Here is where Business Logic is placed.

Rerquirements
- First you need [a primary Module created](/2.x/creating-a-module#creating-a-primary-module)
- Second you need a Route leading to the Controller::method (see [Creating a Route](/2.x/routing#Creating-a-Route))

_assuming we have a Route, accepting GET Requests leading to module's `Foo` Controller `Index` with method `index`_
~~~php
\MVC\Route::get('/', '\Foo\Controller\Index::index'); // expecting GET Requests
~~~

writing the Controller class

- Place the Controller Class inside your module's Controller folder (see [/modules/{moduleName}/](/2.x/directory-structure#modules-moduleName)
- Naming Convention for the php file: Pascal Case (see <a href="https://wiki.c2.com/?PascalCase" target="_blank">wiki.c2.com/?PascalCase</a>)
- The Controller must have implement the interface `\MVC\MVCInterface\Controller`; therefore simply extend App\Controller: `class Index extends App\Controller { }` as it fullfills the required interface.

Now in the method `index` you can place your business logic.

_Illustration: Module `Foo`, Controller `Index` with method `index`, responsible for incoming requests via the corresponding Route_
~~~php
<?php

namespace Foo\Controller;
use App\Controller;
use MVC\DataType\DTRequestIn;
use MVC\DataType\DTRoute;

class Index extends Controller
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

<a id="Controller-method-Parameter"></a>
## Method Parameter

There are two DataType objects as parameters sending to a Controller method by default:

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

this method of the Target Controller class is called by `\MVC\Application`  
- **after** policy rules have been taken into account
- **before** Session has been created
- **before** the regular instantiation via the `__construct` method of the controller class

This way, preparatory work can be carried out, such as loading certain configurations, applying filters or checking any authorisations.


---

<a id="Example"></a>
# Example

<a id="Example-Controller"></a>
## Example Controller

here you find a complete Controller class extending a `_Master` Controller.

_Example Controller `/modules/Foo/Controller/Index.php`_
~~~php
<?php
/**
 * Index.php
 *
 * @package Emvicy
 * @copyright ueffing.net
 * @author Guido K.B.W. Üffing <emvicy@ueffing.net>
 * @license GNU GENERAL PUBLIC LICENSE Version 3. See application/doc/COPYING
 */

/**
 * @name $FooController
 */
namespace Foo\Controller;

use App\Controller;
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
/**
 * Master.php
 *
 * @package Emvicy
 * @copyright ueffing.net
 * @author Guido K.B.W. Üffing <emvicy@ueffing.net>
 * @license GNU GENERAL PUBLIC LICENSE Version 3. See application/doc/COPYING
 */

/**
 * @name $FooController
 */
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