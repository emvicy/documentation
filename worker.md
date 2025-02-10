
# Worker

- [Creation](#Worker-creation)
- [Worker Config](#Worker-config)

------------------------------------------------------------------------------------------------------------------------

<a id="Worker-creation"></a>
## Creation

_creates the Worker class `Bar` in the given module `Foo`_
~~~bash
php emvicy queue:worker Bar Foo 
~~~
- creates the class `modules/Foo/Model/Worker/Bar.php` 

_`modules/Foo/Model/Worker/Bar.php`_  
~~~php
<?php

namespace Foo\Model\Worker;

use App\DataType\DTAppTableQueue;
use MVC\Log;
use MVC\WorkerTrait;

class Dummy implements \MVC\MVCInterface\InterfaceWorker
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
- the static method `work` is the one which gets called
- the Argument `$oDTAppTableQueue` contains the Job from the Queue

------------------------------------------------------------------------------------------------------------------------

<a id="Worker-config"></a>
## Worker config

the queue / worker config resides in the `_queue.php` config file in your module's [Module's config folder](/2.x/configuration#Modules-config-folder)

Here you define which worker is responsible for which queue job keys.

*Example `_queue.php`*    
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
- Here the Worker class `\Foo\Model\Worker\Bar` is responsible for Queue Jobs having the key `Bar::do`.

