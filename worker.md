
# Worker

- [Creation of a worker class](#Worker-creation)
- [Registering a worker class for a queue job](#Registering)
- [Auto-Routes](#Auto-Routes)
- [Worker run](#Worker-run)

------------------------------------------------------------------------------------------------------------------------

<a id="Worker-creation"></a>
## Creation of a worker class

_creates the Worker class `Bar` in the given module `Foo`_
~~~bash
php emvicy worker:create Bar Foo 
~~~
- creates the class `modules/Foo/Model/Worker/Bar.php` 

_`modules/Foo/Model/Worker/Bar.php`_  
~~~php
<?php

namespace Foo\Model\Worker;

use App\DataType\DTAppTableQueue;
use MVC\Log;
use MVC\WorkerTrait;

class Bar implements \MVC\MVCInterface\InterfaceWorker
{
    use WorkerTrait;

    /**
     * @param \App\DataType\DTAppTableQueue|null $oDTAppTableQueue
     * @return void
     * @throws \ReflectionException
     */
    public static function work(?DTAppTableQueue $oDTAppTableQueue = null) : void
    {
        Log::write($oDTAppTableQueue, 'queue.log');
    }
}
~~~
- the static method `work` is the one which gets called; so here in this method you write your worker logic.
- the Argument `$oDTAppTableQueue` (see [`DTAppTableQueue`](/2.x/queueing#DTAppTableQueue)) contains the Job from the Queue.

------------------------------------------------------------------------------------------------------------------------

<a id="Registering"></a>
## Registering a working class for a queue job

The queue / worker config resides in the `_queue.php` config file in your module's [Module's config folder](/2.x/configuration#Modules-config-folder)

Here you define which worker is responsible for which queue job keys.

*Example `modules/Foo/etc/config/Foo/config/_queue.php`*    
~~~php
<?php

/**
 * queue key
 * - Do not use special characters, spaces
 * - Underscore `_`, full stop `.` and colon `:` are ok
 *
 * worker class
 * - absolute address for the method (as for routing; see /etc/routing/*.php files)
 */
$aConfig['MODULE']['Foo']['queue']['worker'] = [

    // queue job key
    //                                 responsible worker class
    'Bar::do'                        => '\Foo\Model\Worker\Bar',
    
];
~~~
- Now the Worker class `\Foo\Model\Worker\Bar` is responsible for Queue Jobs having the key `Bar::do`

------------------------------------------------------------------------------------------------------------------------

<a id="Auto-Routes"></a>
## Auto-Routes for Worker

Let's have a look at the routing file `etc/routing/service.php` in your module's routing folder.

_`etc/routing/service.php`_  
~~~php
<?php

#-------------------------------------------------------------------------------------------------------------------
# queue / worker

// automatically builds callable, unique `\MVC\Route` routes for each queue job key
\MVC\Worker::workerAutoRoute();

// adds a route for calling worker getting queue jobs done
//              '/~/queue/worker/run'             '\App\Controller\Queue::run'
\MVC\Route::GET(\MVC\Config::get_MVC_QUEUE_RUN(), \MVC\Config::get_MVC_QUEUE_RUN_CLASSMETHOD());

#-------------------------------------------------------------------------------------------------------------------
# cron

// adds a route for calling cron jobs
//              '/~/cron/run'                     '\App\Controller\Cron::run'
\MVC\Route::GET(\MVC\Config::get_MVC_CRON_ROUTE(), \MVC\Config::get_MVC_CRON_RUN_CLASSMETHOD());
~~~

**method `\MVC\Worker::workerAutoRoute()`**

this method automatically builds callable, unique `\MVC\Route` routes for each queue job key, which are defined in config - see previous section [Registering a working class for a queue job](/2.x/worker#Registering).  
e.g.: the `\MVC\Route` Route `/~/queue/worker/Bar::do/*` is created for queue key `Bar::do`; so each queue key could be called with its own route.  
The route Syntax is: `/~/queue/worker/{QUEUE KEY}/*`. Each created route points to `\App\Controller\Queue::workerAutoRouteResolve`

_Example output of `php emvicy routes:list`_

| No  | Method  | Methods assigned | Route                           | Target                                           | Tag                         |
|-----|---------|------------------|---------------------------------|--------------------------------------------------|-----------------------------|
| 14  | GET     | GET              | `/~/queue/worker/Bar::do/*`   | `\App\Controller\Queue::workerAutoRouteResolve`  | get-queue-worker-dummy-do   |

**special routes**

there are two other routes defined
- `/~/queue/worker/run`: adds a route for calling worker getting queue jobs done
- `/~/cron/run`: adds a route for calling cron jobs

------------------------------------------------------------------------------------------------------------------------

<a id="Worker-run"></a>
## Worker run

runs (processes) the queue by calling the responsible workers according to configuration - see previous section [Registering a working class for a queue job](/2.x/worker#Registering).

🛈 all ways shown here are non-blocking.

**call via cron job**

- make sure the variable `$aConfig['MVC_QUEUE_RUN']` is added to the cron config. This variable contains the queue worker route (see [Queue-Config](/2.x/queueing#Queue-Config)).
- see Section [Cron](/2.x/cron) for more Info

**Explicit call by route**

~~~bash
cd public; \
php index.php /~/queue/worker/run
~~~
- If Queue config not changed, the route to call is `/~/queue/worker/run`. 
- This route is saved to the variable `$aConfig['MVC_QUEUE_RUN']` (see [Queue-Config](/2.x/queueing#Queue-Config)). 


**Explicit call by emvicy cli tool**

~~~bash
php emvicy worker:run
~~~
