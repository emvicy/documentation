
# MVC <small>[Model View Controller]</small>

The so called MVC Pattern is commonly named in this order, but the order of the commands is different, with **C** (Controller) in first place.

- [Controller](#Controller)
  - [Method Parameter](#Controller-method-Parameter)
- [Examples](#Examples)
    - [Example Controller](#Example-Controller)
    - [Example Master Controller](#Example-Master-Controller)
  
---

<a id="Controller"></a>
## Controller

~~~
Route => Controller
~~~

*"The controller responds to the user input and performs interactions on the data model objects. The controller receives the input, optionally validates it and then passes the input to the model."  
<small>[wikipedia, "Model–view–controller#Interactions", 2023-12-28](https://en.wikipedia.org/wiki/Model%E2%80%93view%E2%80%93controller#Interactions)</small>*

Accepts input and converts it to commands for the model or view. Here is where Business Logic is placed.

Rerquirements  
- First you need [a primary Module created](/1.x/creating-a-module#creating-a-primary-module)
- Second you need a Route leading to the Controller::method (see [Creating a Route](/1.x/routing#Creating-a-Route))

_assuming we have a Route, accepting GET Requests leading to module's `Foo` Controller `Index` with method `index`_    
~~~php
\MVC\Route::get('/', '\Foo\Controller\Index::index'); // expecting GET Requests
~~~

writing the Controller class

- The Controller Class has to placed inside your module's Controller folder (see [/modules/{moduleName}/](/1.x/directory-structure#modules-moduleName)
- Naming Convention for the php file: Pascal Case (see <a href="https://wiki.c2.com/?PascalCase" target="_blank">wiki.c2.com/?PascalCase</a>)
- The Controller must have implement the interface `\MVC\MVCInterface\Controller`; therefore simply extend App\Controller: `class Index extends App\Controller { }` as it fullfills the required interface.

Now in the method `index` you can place your business logic. 

_Illustration: Module `Foo`, Controller `Index` with method `index`, responsible for incoming requests via the corresponding Route_    
~~~php
<?php

namespace Foo\Controller;
use App\Controller;
use MVC\DataType\DTRequestCurrent;
use MVC\DataType\DTRoute;

class Index extends Controller
{    
    /**
     * @param \MVC\DataType\DTRequestCurrent $oDTRequestCurrent
     * @param \MVC\DataType\DTRoute          $oDTRoute
     * @throws \ReflectionException
     */
    public function __construct(DTRequestCurrent $oDTRequestCurrent, DTRoute $oDTRoute)
	{
        parent::__construct($oDTRequestCurrent, $oDTRoute);
    }    
    
    /**
     * @param \MVC\DataType\DTRequestCurrent $oDTRequestCurrent
     * @param \MVC\DataType\DTRoute          $oDTRoute
     * @return void
     * @throws \ReflectionException
     */
	public function index(DTRequestCurrent $oDTRequestCurrent, DTRoute $oDTRoute)
	{
	    ;    
	}
}
~~~

<a id="Controller-method-Parameter"></a>
### Method Parameter

There are two DataType objects as parameters sending to a Controller method by default:  
- `DTRequestCurrent $oDTRequestCurrent` (see [DTRequestCurrent](/1.x/datatype-classes#DTRequestCurrent))
- `DTRoute $oDTRoute` (see [DTRoute](/1.x/datatype-classes#DTRoute))


_Example Usage of parameters_  
~~~php
public function index(DTRequestCurrent $oDTRequestCurrent, DTRoute $oDTRoute)
{
    // get potential data sent
    $mInput = $oDTReuestCurrent->get_input();
    
    // get additional object
    /** @var \MVC\DataType\DTRoutingAdditional $oDTRoutingAdditional */
    $oDTRoutingAdditional = $oDTRoute->get_additional();

    // get title 
    $sTitle = $oDTRoutingAdditional->get_sTitle();        
}
~~~

---

<a id="Model"></a>
## Model

~~~
Controller => Model => Controller
~~~
  
The "Worker". Here you are receiving commands from Controller and do things with data. Try to avoid implementing business logic here - this always belongs to a Controller.

---

<a id="View"></a>
## View

~~~
Controller => View
~~~

The view renders presentation in a particular format, receiving commands from Controller. 



---

<a id="Examples"></a>
## Examples

here you find a complete Controller class extending a `_Master` Controller.

<a id="Example-Controller"></a>
### Example Controller

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

use MVC\DataType\DTRequestCurrent;
use MVC\DataType\DTRoute;

/**
 * @extends \Foo\Controller\_Master
 */
class Index extends _Master
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
     * @param \MVC\DataType\DTRequestCurrent $oDTRequestCurrent
     * @param \MVC\DataType\DTRoute          $oDTRoute
     * @throws \ReflectionException
     */
    public function __construct(DTRequestCurrent $oDTRequestCurrent, DTRoute $oDTRoute)
    {
        parent::__construct($oDTRequestCurrent, $oDTRoute);
    }

    /**
     * @param \MVC\DataType\DTRequestCurrent $oDTRequestCurrent
     * @param \MVC\DataType\DTRoute          $oDTRoute
     * @return void
     * @throws \ReflectionException
     */
	public function index(DTRequestCurrent $oDTRequestCurrent, DTRoute $oDTRoute)
	{
        view()->autoAssign();
	}

    /**
     * @param \MVC\DataType\DTRequestCurrent $oDTRequestCurrent
     * @param \MVC\DataType\DTRoute          $oDTRoute
     * @return void
     * @throws \ReflectionException
     */
    public function notFound(DTRequestCurrent $oDTRequestCurrent, DTRoute $oDTRoute)
    {
        http_response_code(404);
        view()->autoAssign();
    }

    /**
     * @throws \ReflectionException
     * @throws \SmartyException
     */
    public function __destruct ()
    {
        view()->render();
    }
}
~~~

<a id="Example-Master-Controller"></a>
### Example Master Controller

*Example Master Controller `/modules/Foo/Controller/_Master`*  
~~~php
<?php
/**
 * _Master.php
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
use Foo\Model\DB;
use MVC\DataType\DTRequestCurrent;
use MVC\DataType\DTRoute;
use MVC\MVCTrait\TraitDataType;

/**
 * @extends Controller
 */
class _Master extends Controller
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
     * @param \MVC\DataType\DTRequestCurrent $oDTRequestCurrent
     * @param \MVC\DataType\DTRoute          $oDTRoute
     * @throws \ReflectionException
     */
    public function __construct(DTRequestCurrent $oDTRequestCurrent, DTRoute $oDTRoute)
	{
        parent::__construct($oDTRequestCurrent, $oDTRoute);

        // View
        if (true === $this->isPrimary())
        {
            function view() {return \Foo\View\Index::init();}
        }
        else
        {
            \Foo\View\Index::init();
        }

        // Init Database
        DB::init();
    }

    public function __destruct()
    {
        ;
    }
}
~~~
