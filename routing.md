
# Routing

- [Quick start](#quick-start)
- [Creating a Route](#Creating-a-Route)
    - [Routing files](#Routing-files)
    - [Writing a Route](#writing-a-Route)
      - [Standard RESTful Request Methods](#Standard-RESTful-Request-Methods)
      - [Any Request Method](#Any-Request-Method")
      - [Mixed Request Methods](#Mixed-Request-Methods)
    - [Adding additional context information to route](#adding-additional-context-information-to-route)
    - [Placeholder routing](#wildcard-routing)
    - [Routing with Path Params / Variables](#path-params)
- [Working with Routes](#Working-with-Routes)
    - [Get all routes](#Get-all-routes)
        - [Get routes array](#get-routes-array)
        - [Get array stacked by request methods](#Get-array-stacked-by-request-methods)
    - [Get a route](#Get-a-route)
        - [Get current route](#Get-current-route)
        - [Get any route](#Get-any-route)
        - [Get a Route on Tag](#Get-route-on-tag)
    - [Accessing additional context information](#Accessing-additional-context-information)
        - [Get the additional context information of the current route](#Get-the-additional-context-information-of-the-current-route)
        - [Get the additional context information of _any_ route](#Get-the-additional-context-information-of-any-route)

------------------------------------------------------------------------------------------------------------------------

<a id="quick-start"></a>
## Quick start

~~~php
\MVC\Route::get     ('/', '\Foo\Controller\Index::index');                      // expecting GET
\MVC\Route::post    ('/', '\Foo\Controller\Index::index');                      // expecting POST
\MVC\Route::put     ('/', '\Foo\Controller\Index::index');                      // expecting PUT
\MVC\Route::delete  ('/', '\Foo\Controller\Index::index');                      // expecting DELETE

\MVC\Route::any     ('/', '\Foo\Controller\Index::index');                      // be open for any request method
\MVC\Route::mix     (['GET', 'POST'], '/',    '\Foo\Controller\Index::index');  // expecting GET or POST
~~~
- write the commands in a routing file inside your module's routing folder, like `modules/Foo/etc/routing/frontend.php`

------------------------------------------------------------------------------------------------------------------------

<a id="Creating-a-Route"></a>
## Creating a Route

<a id="Routing-files"></a>
### Routing files

All routing information are written in php files in your module's routing folder. All `*.php` files in the routing folder will be load at start.

_Schema_
~~~
modules/{Module}/etc/routing/
~~~

If your module is `Foo`, then this is your routing folder
~~~
modules/Foo/etc/routing/;
~~~

per default you see these contents
~~~
modules/Foo/etc/routing/
└── frontend.php
~~~

_individual routing files_  
you can create you own `*.php` routing file, for example `service.php` and edit your routes in that file.

If you want to create routes for an API, it then makes sense to create a file called `api.php` and write all your api routes inside that file.

------------------------------------------------------------------------------------------------------------------------

<a id="writing-a-Route"></a>
### Writing a Route

<a id="Standard-RESTful-Request-Methods"></a>
**Standard RESTful Request Methods**

_You declare a route with Command `\MVC\Route`:_
~~~
\MVC\Route::{METHOD}(string path, string targetController, mixed additionalInformation)
~~~
- `{METHOD}`: one of the RESTful request methods `GET`, `POST`, `PUT`, `DELETE`; you declare which RESTful request method is expected for this route
- `path`: url path
- `targetController`: the `Module`, `Controller` and `method` the route leads to - ⚠ Requirement: It has to be an [Emvicy Controller](/2.x/controller).
- `additionalInformation` [optional]: any information you may need to process in your target controller (or elsewhere). You can pass string, array, object, bool or whatever you like.

_Example_  
~~~php
\MVC\Route::get('/foo/', '\Foo\Controller\Index::index');
~~~
- request method here is `GET`
- path is `/foo/`
- targetController: leads to Class::method `\Foo\Controller\Index::index`

<a id="Any-Request-Method"></a>
**Any Request Method**

_Using `ANY` you leave it open which request method should apply_   
~~~php
\MVC\Route::any('/foo/', '\Foo\Controller\Index::index');
~~~

<a id="Mixed-Request-Methods"></a>
**Mixed Request Methods**

_Assign more than one request method to the route with `MIX`_
~~~php
\MVC\Route::mix(['GET', 'POST'], '/foo/', '\Foo\Controller\Index::index');
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="adding-additional-context-information-to-route"></a>
### Adding additional context information to route

You can add any information of any type to the route. 

**Simple Examples**

~~~php
\MVC\Route::get('/foo/', '\Foo\Controller\Index::index', 'Hello World'); # passing string
~~~
~~~php
\MVC\Route::get('/foo/', '\Foo\Controller\Index::index', [1, 'foo', 'bar']); # passing array
~~~
~~~php
\MVC\Route::get('/foo/', '\Foo\Controller\Index::index', DTRoutingAdditional::create()->set_sTitle('Foo')); # passing object
~~~

**Get the additional context**

you can get the additonal context via the Route object. 

_example: get additonal context in Controller method_  
~~~php
public function index(DTRequestCurrent $oDTRequestCurrent, DTRoute $oDTRoute)
{
    $oDTRoutingAdditional = $oDTRoute->get_additional()
    ...
}
~~~

_You can also get the additional context by requesting Route class directly_  
~~~php
$oDTRoutingAdditional = Route::getCurrent()->get_additional()
~~~

see also: [Accessing additional context information](#Accessing-additional-context-information)

**More complex Example**

In the example `frontend.php` you see a DataType Class `$oDTRoutingAdditional` of Type `\Foo\DataType\DTRoutingAdditional`.
This class is generated by Emvicy's DataType Generator; see [/2.x/generating-datatype-classes](/2.x/generating-datatype-classes) for more Information.  
The information stored in this DataType Class will be accessed by the View Class as they are necessary for setting up the View correctly.

_Example object DTRoutingAdditional_
~~~php
$oDTRoutingAdditional = \Foo\DataType\DTRoutingAdditional::create()
    ->set_sTitle('Foo')
    ->set_sTemplate('Frontend/content/index.tpl')
    ->set_aStyle(array (
        '/Emvicy/assets/bootstrap-4.6.2-dist/css/bootstrap.min.css',
        '/Emvicy/assets/font-awesome-4.7.0/css/font-awesome.min.css',
        '/Emvicy/styles/Emvicy.min.css',
    ))
    ->set_aScript(array (
        '/Emvicy/assets/jquery/3.7.0/jquery-3.7.0.min.js',
        '/Emvicy/assets/jquery-cookie/1.4.1/jquery.cookie.min.js',
        '/Emvicy/assets/bootstrap-4.6.2-dist/js/bootstrap.min.js',
        '/Emvicy/scripts/cookieConsent.min.js',
    ));
~~~

_Examples routes with `$oDTRoutingAdditional`_
~~~php
/*
 * Routes
 */
\MVC\Route::mix(
    ['GET', 'POST'],
    '/',
    '\Foo\Controller\Index::index',
    clone $oDTRoutingAdditional // copy from above object
);
\MVC\Route::get(
    '/404/',
    '\Foo\Controller\Index::notFound',
    clone $oDTRoutingAdditional // copy from above object; but set different title, content
        ->set_sTitle('404')
        ->set_sContent('Frontend/content/404.tpl')
);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="wildcard-routing"></a>
### Placeholder routing

**per default all routings are restricitv**  

_Example_  
~~~php
\MVC\Route::get('/foo/', '\Foo\Controller\Index::foo');
~~~

you can only request  
- `/foo/`

you cannot request e.g.  
- `/foo/bar/`

**activating Placeholder routing**  
therefore, just add `*` after the path: `'/foo/*'`.  

~~~php
\MVC\Route::get('/foo/*', '\Foo\Controller\Index::foo');
~~~

now you can call the route with further paths 
- e.g. `/foo/bar/baz/`

You can get the tailing path after `/foo/` by accessing Path Params _tail.

~~~php
$aPathParam = \MVC\Request::in()->get_pathParamArray()['_tail'];
~~~

this will give you

~~~
bar/baz/
~~~

- see `Request`: [Accessing Path Params / Variables](/2.x/request#Accessing-Path-Params-Variables)

------------------------------------------------------------------------------------------------------------------------

<a id="path-params"></a>
### Routing with Path Params / Variables

You can define Routes with Variables.  
This you can do in one of two ways:

1. `:var` notation: e.g. `'/api/:id/:name/:address/'`  
2. `{var}` notation: e.g. `'/api/{id}/{name}/{address}/'`

You cannot mix these notations; Use one or the other for defining your routes.  
All variables you declare in your route are accessible by name.

_Examples_
~~~php
# :var notation
\MVC\Route::get('/api/:id/:name/:address/', '\Foo\Controller\Api::index');
# {var} notation
\MVC\Route::get('/api/{id}/{name}/{address}/', '\Foo\Controller\Api::index');
~~~

Valid Requests:
- `/api/1/Foo/Bar/`
- `/api/1969/FooBar/Somwhere%20else/`

_Example with activating wildcard routing_
~~~php
\MVC\Route::get('/api/:id/:name/:address/*', '\Foo\Controller\Api::index');
~~~

Valid Requests:
- `/api/1/Foo/Bar/what/else/`
- `/api/1969/FooBar/Somwhere%20else/what/else/`

_Access the Variables_  
- see `Request`: [Accessing Path Params / Variables](/2.x/request#Accessing-Path-Params-Variables)

------------------------------------------------------------------------------------------------------------------------

<a id="Working-with-Routes"></a>
## Working with Routes

<a id="Get-all-routes"></a>
### Get all routes

**using emvicy cli command** 

_lists available routes in a markdown table_  
~~~bash
php emvicy routes:list
~~~

_Example Result_  

~~~
# Route List

| No  | Method  | Methods assigned            | Route                           | Target                                                    | Tag                             |
|-----|---------|-----------------------------|---------------------------------|-----------------------------------------------------------|---------------------------------|
| 1   | GET     | GET                         | /                               | \Foo\Controller\Index::index                              | home                            |
| 2   | *       | *                           | /403/                           | \Foo\Controller\Index::forbidden                          | 403                             |
| 3   | *       | *                           | /404/                           | \Foo\Controller\Index::notFound                           | 404                             |
| 4   | *       | *                           | /api/                           | \Foo\Controller\Api\Api::index                            | any-api                         |
| 5   | *       | *                           | /download/                      | \Foo\Controller\Api\Api::download                         | any-download                    |
| 6   | GET     | GET                         | /imprint/                       | \Foo\Controller\Index::index                              | imprint                         |
| 7   | GET     | GET                         | /info/                          | \Foo\Controller\Index::phpinfo                            | info                            |
| 8   | GET     | GET                         | /privacy-policy/                | \Foo\Controller\Index::index                              | privacyPolicy                   |
| 9   | GET     | GET                         | /user/                          | \Foo\Controller\Index::user                               | user                            |
| 10  | GET     | GET                         | /ws/pushtest/                   | \Ws\Controller\Ws::pushtest                               | WsPushTest                      |
| 11  | GET     | GET                         | /ws/serve/                      | \Ws\Controller\Ws::serve                                  | WsServe                         |
| 12  | GET     | GET                         | /~/cron/run                     | \App\Controller\Cron::run                                 | get-cron-run                    |
| 13  | GET     | GET                         | /~/queue/run                    | \App\Controller\Queue::run                                | get-queue-run                   |
| 14  | GET     | GET                         | /~/queue/worker/Dummy::do/*     | \App\Controller\Queue::workerAutoRouteResolve             | get-queue-worker-dummy-do       |
| 15  | GET     | GET                         | /~/queue/worker/Email::new/*    | \App\Controller\Queue::workerAutoRouteResolve             | get-queue-worker-email-new      |
~~~

Available commands for the "routes" namespace:

- routes:array  [rt] `php emvicy routes:array` => lists available routes as array/var_export
- routes:json   [rtj] `php emvicy routes:json` => lists available routes in JSON format
- routes:list   [rtl] `php emvicy routes:list` => lists available routes in a markdown table

---

<a id="get-routes-array"></a>
**Get routes array**

Simply access the public property `$aRoute` of class `Route`. It will give you an associative array where its values are objects of type `MVC\DataType\DTRoute`.

_Command_
~~~php
/** @var MVC\DataType\DTRoute[] $aRoute */
$aRoute = Route::$aRoute;
~~~

_Example Result of `$aRoute` (shortened)_
~~~
// type: array, items: 15
[
    '/' => [
        \MVC\DataType\DTRoute::__set_state(array(
              'path' => '/',
              'requestMethod' => 'GET',
              'methodsAssigned' => [
                0 => 'GET',
            ],
              'query' => '\\Foo\\Controller\\Index::index',
              'module' => 'Foo',
              'class' => '\\Foo\\Controller\\Index',
              'classFile' => '/var/www/html/modules/Foo/Controller/Index.php',
              'method' => 'index',
              'additional' => [
                \Foo\DataType\DTRoutingAdditional::__set_state(array( 
                    … 
                ))
              ],
              'tag' => 'home',
        )],
    '/user/' => [ 
        … 
    ],
    …        
]
~~~

<a id="get-array-stacked-by-request-methods"></a>
**Get array stacked by request methods**

Access the public property `$aMethod` to get an array of route indeces stacked by request method.

_Command_
~~~php
$aMethod = Route::$aMethod;
~~~

_Example Result of `$aMethod`_
~~~
// type: array, items: 2
[
    'get' => [
        0 => '/',
        1 => '/user/',
        2 => '/imprint/*',
        3 => '/privacy-policy/',
        4 => '/info/',
    ],
    '*' => [
        0 => '/403/',
        1 => '/404/',
        2 => '/api/',
        3 => '/download/',
    ],
]
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Get-a-route"></a>
### Get a route

<a id="Get-current-route"></a>
#### Get current route

this will give you the current route object to the requested url path.

_Example request_
- `/api/1/Foo/Bar/what/else/`

_Command_
~~~php
$oDTRoute = \MVC\Route::getCurrent();
~~~

_Example Result of `$oDTRoute`_
~~~
// type: object
\MVC\DataType\DTRoute::__set_state(array(
      'path' => '/imprint/',
      'requestMethod' => 'GET',
      'methodsAssigned' =>    array (
        0 => 'GET',
    ),
      'query' => '\\Foo\\Controller\\Index::index',
      'module' => 'Foo',
      'class' => '\\Foo\\Controller\\Index',
      'classFile' => '/var/www/html/modules/Foo/Controller/Index.php',
      'method' => 'index',
      'additional' =>    \Foo\DataType\DTRoutingAdditional::__set_state(array(
          'sTitle' => 'Imprint',
          'sTemplate' => 'Frontend/content/imprint.tpl',
          'sContent' => '',
          'aStyle' =>        array (
            0 => '/Emvicy/assets/bootstrap-5.3.3-dist/css/bootstrap.min.css',
            1 => '/Emvicy/assets/fontawesome-free-6.7.2-web/css/all.min.css',
            2 => '/Emvicy/styles/Emvicy.min.css',
            3 => '/Ws_old/assets/pnotify.min.css',
            4 => '/Ws_old/assets/pnotify.brighttheme.min.css',
        ),
          'aScript' =>        array (
            0 => '/Emvicy/assets/jquery-3.7.1/jquery-3.7.1.min.js',
            1 => '/Emvicy/assets/jquery-cookie-1.4.1/jquery.cookie.min.js',
            2 => '/Emvicy/assets/popper-v2.11.8/popper.min.js',
            3 => '/Emvicy/assets/bootstrap-5.3.3-dist/js/bootstrap.min.js',
            4 => '/Emvicy/scripts/cookieConsent.min.js',
            5 => '/Ws_old/assets/pnotify.min.js',
            6 => '/Ws_old/assets/pnotify.desktop.min.js',
            7 => '/Ws_old/scripts/pnotify.min.js',
            8 => '/Ws/scripts/wss.domain.port.min.js',
        ),
    )),
      'tag' => 'imprint',
))
~~~

As the Result is an object of type `MVC\DataType\DTRoute` you can access all its properties by getter.

_Example_ 
~~~php
$sClass = \MVC\Route::getCurrent()->get_methodsAssigned();
~~~

_Example Result of `$sClass`_  
~~~
// type: array, items: 1
[
    0 => 'GET',
]
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Get-any-route"></a>
#### Get any route

you can get any route object by accessing public `$aRoute` from class `Route`.

_Command_
~~~php
// we want the route object of route /404/
$oRoute = \MVC\Route::$aRoute['/api/'];
~~~

_Example Result of `$oRoute`_  
~~~
// type: object
\MVC\DataType\DTRoute::__set_state(array(
      'path' => '/api/',
      'requestMethod' => '*',
      'methodsAssigned' =>    array (
        0 => '*',
    ),
      'query' => '\\Foo\\Controller\\Api\\Api::index',
      'module' => 'Foo',
      'class' => '\\Foo\\Controller\\Api\\Api',
      'classFile' => '/var/www/html/modules/Foo/Controller/Api/Api.php',
      'method' => 'index',
      'additional' => NULL,
      'tag' => 'any-api',
))
~~~

As the Result is an object of type `MVC\DataType\DTRoute` you can access all its properties by getter.

_Example_  
~~~php
$sClass = \MVC\Route::$aRoute['/api/']->get_class();
~~~

_Example Result of `$sClass`_  
~~~
// type: string
'\\Foo\\Controller\\Api\\Api'
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Get-route-on-tag"></a>
#### Get a Route on Tag

_Command_
~~~bash
Route::getOnTag('any-download')
~~~

_Example Result_  
~~~
// type: object
\MVC\DataType\DTRoute::__set_state(array(
      'path' => '/download/',
      'requestMethod' => '*',
      'methodsAssigned' =>    array (
        0 => '*',
    ),
      'query' => '\\Foo\\Controller\\Api\\Api::download',
      'module' => 'Foo',
      'class' => '\\Foo\\Controller\\Api\\Api',
      'classFile' => '/var/www/html/modules/Foo/Controller/Api/Api.php',
      'method' => 'download',
      'additional' => NULL,
      'tag' => 'any-download',
))
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Accessing-additional-context-information"></a>
### Accessing additional context information

<a id="Get-the-additional-context-information-of-the-current-route"></a>
**Get the additional context information of the current route**

_Command_
~~~php
/** @var \MVC\DataType\DTRoutingAdditional $oDTRoutingAdditional */
$oDTRoutingAdditional = \MVC\Route::getCurrent()->get_additional();
~~~

_Example Result of `$oDTRoutingAdditional`_  
~~~
// type: object
\Foo\DataType\DTRoutingAdditional::__set_state(array(
      'sTitle' => 'Home',
      'sTemplate' => 'Frontend/content/index.tpl',
      'sContent' => '',
      'aStyle' =>    array (
        0 => '/Emvicy/assets/bootstrap-5.3.3-dist/css/bootstrap.min.css',
        1 => '/Emvicy/assets/fontawesome-free-6.7.2-web/css/all.min.css',
        2 => '/Emvicy/styles/Emvicy.min.css',
    ),
      'aScript' =>    array (
        0 => '/Emvicy/assets/jquery-3.7.1/jquery-3.7.1.min.js',
        1 => '/Emvicy/assets/jquery-cookie-1.4.1/jquery.cookie.min.js',
        2 => '/Emvicy/assets/popper-v2.11.8/popper.min.js',
        3 => '/Emvicy/assets/bootstrap-5.3.3-dist/js/bootstrap.min.js',
        4 => '/Emvicy/scripts/cookieConsent.min.js',
    ),
))
~~~

<a id="Get-the-additional-context-information-of-any-route"></a>
**Get the additional context information of _any_ route**

Therefore access the public property `$aRoute` directly with the path of the route of your interest. It will give you an object of Type `MVC\DataType\DTRoute`, which allows you to access its method `get_additional()`.

_Command_
~~~php
/** @var \MVC\DataType\DTRoutingAdditional $oDTRoutingAdditional */
$oDTRoutingAdditional = \MVC\Route::$aRoute['/404/']->get_additional();
~~~

_Example Result of `$oDTRoutingAdditional`_  
~~~
// type: object
\Foo\DataType\DTRoutingAdditional::__set_state(array(
      'sTitle' => '404',
      'sTemplate' => 'Frontend/content/404.tpl',
      'sContent' => '',
      'aStyle' =>    array (
        0 => '/Emvicy/assets/bootstrap-5.3.3-dist/css/bootstrap.min.css',
        1 => '/Emvicy/assets/fontawesome-free-6.7.2-web/css/all.min.css',
        2 => '/Emvicy/styles/Emvicy.min.css',
    ),
      'aScript' =>    array (
        0 => '/Emvicy/assets/jquery-3.7.1/jquery-3.7.1.min.js',
        1 => '/Emvicy/assets/jquery-cookie-1.4.1/jquery.cookie.min.js',
        2 => '/Emvicy/assets/popper-v2.11.8/popper.min.js',
        3 => '/Emvicy/assets/bootstrap-5.3.3-dist/js/bootstrap.min.js',
        4 => '/Emvicy/scripts/cookieConsent.min.js',
    ),
))
~~~