 
# Queueing

- [`push`](#push)
- [`pop`](#pop)
- [`popOnId`](#popOnId)
- [Declare a Job](queue-job)
- [Config](#Config)

🛈 This requires a Database setup.
       
------------------------------------------------------------------------------------------------------------------------

<a id="push"></a>
## `push`

_pushes a Job to the Queue_
~~~php
/** @var \App\DataType\DTAppTableQueue $oDTAppTableQueue */
$oDTAppTableQueue = \MVC\Queue::push(
    oDTAppTableQueue: \App\DataType\DTAppTableQueue::create()->set_key('foo')->set_value('bar'),
    bPreventMultipleCreation: true
);
~~~
- `bPreventMultipleCreation`
  - default setting is: `false`
  - if set to `true` no further similar job can be added.

------------------------------------------------------------------------------------------------------------------------

<a id="pop"></a>
## `pop`

_takes a Job from queue_  
~~~php
/** @var \App\DataType\DTAppTableQueue $oDTAppTableQueue */
$oDTAppTableQueue = \MVC\Queue::pop('foo');
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="popOnId"></a>
## `popOnId`

_takes a specific Job from queue identified by `id` (field `id` in table)_
~~~php
/** @var \App\DataType\DTAppTableQueue $oDTAppTableQueue */
$oDTAppTableQueue = \MVC\Queue::popOnId(1182);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="next"></a>
## `next`

next does not pop (take) a job from the queue; it just informs.

_gives the next 5 Jobs_
~~~php
/** @var \App\DataType\DTAppTableQueue $oDTAppTableQueue */
$oDTAppTableQueue = \MVC\Queue::next(5);
~~~


_get next 2 jobs with key='foo'_
~~~php
/** @var \App\DataType\DTAppTableQueue $oDTAppTableQueue */
$oDTAppTableQueue = \MVC\Queue::next(
    2,  [
        DTDBWhere::create()->set_sKey(DTAppTableQueue::getPropertyName_key())->set_sValue('foo')
    ]
)
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="queue-job"></a>
## Declare a Job

_create a DT Queue Object_
~~~php
/** @var \App\DataType\DTAppTableQueue $oDTAppTableQueue */
$oDTAppTableQueue = \App\DataType\DTAppTableQueue::create()
    ->set_key('foo')
    ->set_value('bar')
;
~~~

_adding a second key_
~~~php
/** @var \App\DataType\DTAppTableQueue $oDTAppTableQueue */
$oDTAppTableQueue = \App\DataType\DTAppTableQueue::create()
    ->set_key('foo')
    ->set_value('bar')
    ->set_key2('optional')
;
~~~

_adding description_
~~~php
/** @var \App\DataType\DTAppTableQueue $oDTAppTableQueue */
$oDTAppTableQueue = \App\DataType\DTAppTableQueue::create()
    ->set_key('foo')
    ->set_value('bar')
    ->set_description('this is a description')
;
~~~

_adding expirySeconds_
~~~php
/** @var \App\DataType\DTAppTableQueue $oDTAppTableQueue */
$oDTAppTableQueue = \App\DataType\DTAppTableQueue::create()
    ->set_key('foo')
    ->set_value('bar')
    ->set_expirySeconds(3600)            
;
~~~
- the job expires after the given amounts of seconds and cannot be taken by `Queue::pop` then.

------------------------------------------------------------------------------------------------------------------------

<a id="Config"></a>
## Config

the main config resides in `config/_mvc.php`. override to your needs in your module's config files.

_config_  
~~~php
MVC_QUEUE: {

    // prefix for AutoRoutes
    $aConfig['MVC_QUEUE_ROUTE_PREFIX'] = $aConfig['MVC_ROUTE_PREFIX'] . '/queue';

    // Route for running Queue; calling Worker on Jobs
    // @see modules/{module}/etc/config/{module}/config/_queue.php
    // @see modules/{module}/etc/routing/service.php
    $aConfig['MVC_QUEUE_RUN'] = $aConfig['MVC_QUEUE_ROUTE_PREFIX'] . '/run';

    // Class::method responsible for running Queue
    $aConfig['MVC_QUEUE_RUN_CLASSMETHOD'] = '\App\Controller\Queue::run';

    // Worker Route Structure
    $aConfig['MVC_QUEUE_WORKER_AUTO_ROUTE_PREFIX'] = $aConfig['MVC_QUEUE_ROUTE_PREFIX'] . '/worker';

    // Class::method responsible for resolving Worker Routes
    $aConfig['MVC_QUEUE_WORKER_AUTO_ROUTE_RESOLVE_CLASSMETHOD'] = '\App\Controller\Queue::workerAutoRouteResolve';

    // Max processing time of an async process in seconds; cancellation if reached.
    $aConfig['MVC_QUEUE_RUNTIME_SECONDS'] = 300;
}
~~~