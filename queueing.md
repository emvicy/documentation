 
# Queueing

- [`DTAppTableQueue`](#DTAppTableQueue)
- [`push`](#push)
- [`pop`](#pop)
- [`popOnId`](#popOnId)
- [`popAll`](#popAll)
- [`next`](#next)
- [`getAllKeys`](#getAllKeys)
- [`keyExists`](#keyExists)
- [`getAmount`](#getAmount)
- [`expire`](#expire)
- [Queue Config](#Queue-Config)

🛈 This requires a [Database](/2.x/database) setup.
       
------------------------------------------------------------------------------------------------------------------------

<a id="DTAppTableQueue"></a>
## `DTAppTableQueue`

Queue Jobs are represented by the DataType class `DTAppTableQueue`.

_full example_
~~~php
/** @var \App\DataType\DTAppTableQueue $oDTAppTableQueue */
$oDTAppTableQueue = DTAppTableQueue::create()

        // sets the Job with the key (string) 'foo'
        ->set_key('foo')
        
        // sets an optional second key (string) with the job
        ->set_key2('foo2')
        
        // sets a value (string) within the job
        ->set_value(json_encode(\MVC\Convert::objectToArray($oExample)))
        
        // sets expiry time in (int) seconds
        ->set_expirySeconds(60)
        
        // sets expiry timestamp in (int)
        ->set_expiryStamp(1739178905)
        
        // sets a description (string) text 
        ->set_description('an ordinary foo example')

        #--------------------------------------------
        
        // this has no effect as it gets used by `Queue` when Job gets saved into table 
        ->set_valueMd5()        
        ;
~~~

**`expirySeconds` and `expiryStamp`**

Normally, it is perfectly sufficient to use only the `set_expirySeconds()` method and specify the seconds here.

- if `expiryStamp` is not set, it gets set automatically when job gets saved into table
- if you set both, the one wins which get reached first
- the job expires after the given amounts of seconds, means it gets deleted from the table

------------------------------------------------------------------------------------------------------------------------

<a id="push"></a>
## `push`

_pushes a Job to the Queue_
~~~php
/** @var \App\DataType\DTAppTableQueue $oDTAppTableQueue */
$oDTAppTableQueue = \MVC\Queue::push(
    oDTAppTableQueue: \App\DataType\DTAppTableQueue::create()
        ->set_key('foo')
        ->set_value('bar'),
    bPreventMultipleCreation: true
);
~~~
- `bPreventMultipleCreation`
  - default setting is: `false`
  - if set to `true` no further similar job having same key and value can be added.

------------------------------------------------------------------------------------------------------------------------

<a id="pop"></a>
## `pop`

dequeues ONE (the oldest) job along to the given key; it returns the tuple represented by the DataType class `DTAppTableQueue`, also deletes the tuple

_takes a Job from queue_  
~~~php
/** @var \App\DataType\DTAppTableQueue $oDTAppTableQueue */
$oDTAppTableQueue = \MVC\Queue::pop('foo');
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="popOnId"></a>
## `popOnId`

dequeues ONE job along to the given `id`; it returns the tuple represented by the DataType class `DTAppTableQueue`, also deletes the tuple

_takes a specific Job from queue identified by `id` (field `id` in table)_
~~~php
/** @var \App\DataType\DTAppTableQueue $oDTAppTableQueue */
$oDTAppTableQueue = \MVC\Queue::popOnId(1182);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="popAll"></a>
## `popAll`

dequeues all jobs according to the specified keys; it returns all tuples represented by an array of DataType class `DTAppTableQueue`, also deletes all tuples

_takes all Jobs from queue matching the given key_
~~~php
/** @var \App\DataType\DTAppTableQueue[] $aDTAppTableQueue */
$aDTAppTableQueue = \MVC\Queue::popAll('foo');
~~~

_takes all Jobs from queue matching both of the given keys_
~~~php
/** @var \App\DataType\DTAppTableQueue[] $aDTAppTableQueue */
$aDTAppTableQueue = \MVC\Queue::popAll('foo', 'key2');
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="next"></a>
## `next`

just informs about the next jobs. It does not take a job from the queue (like `pop` does) and 
returns all matching tuples represented by an array of DataType class `DTAppTableQueue`

_gives the next 5 Jobs_
~~~php
/** @var \App\DataType\DTAppTableQueue[] $aDTAppTableQueue */
$aDTAppTableQueue = \MVC\Queue::next(5);
~~~

_get next 2 jobs with key='foo'_
~~~php
/** @var \App\DataType\DTAppTableQueue[] $aDTAppTableQueue */
$aDTAppTableQueue = \MVC\Queue::next(
    2,  [
        DTDBWhere::create()->set_sKey(DTAppTableQueue::getPropertyName_key())->set_sValue('foo')
    ]
)
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="getAllKeys"></a>
## `getAllKeys`

_returns all Job keys Jobs from queue as a simple array_
~~~php
/** @var array $aKey */
$aKey = \MVC\Queue::getAllKeys();
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="keyExists"></a>
## `keyExists`

_checks if a job with the given key exists in queue_
~~~php
/** @var boolean $bExists */
$bExists = \MVC\Queue::keyExists('foo');
~~~

_checks if a job having both of the given keys exists in queue_
~~~php
/** @var boolean $bExists */
$bExists = \MVC\Queue::keyExists('foo', 'key2');
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="getAmount"></a>
## `getAmount`

_get amount of jobs relating to the given key_
~~~php
/** @var int $iAmount */
$iAmount = \MVC\Queue::getAmount('foo');
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="expire"></a>
## `expire`

_deletes all jobs which are expired_
~~~php
\MVC\Queue::expire();
~~~
- a job is considered expired, if `expiryStamp` (which is a timestamp) in the job table entry has been reached
- affected jobs are deleted from the table

------------------------------------------------------------------------------------------------------------------------

<a id="Queue-Config"></a>
## Queue config

the queue main config resides in `config/_mvc.php`. Override to your needs in your module's config files.

_config_  
~~~php
MVC_QUEUE: {

    // prefix for AutoRoutes
    $aConfig['MVC_QUEUE_ROUTE_PREFIX'] = $aConfig['MVC_ROUTE_PREFIX'] . '/queue';

    // Route for running Queue; calling Worker on Jobs
    // @see modules/{module}/etc/config/{module}/config/_queue.php
    // @see modules/{module}/etc/routing/service.php
    $aConfig['MVC_QUEUE_RUN'] = $aConfig['MVC_QUEUE_ROUTE_PREFIX'] . '/worker/run';

    // Class::method responsible for running Queue
    $aConfig['MVC_QUEUE_RUN_CLASSMETHOD'] = '\App\Controller\Queue::workerRun';

    // Worker Route Structure
    $aConfig['MVC_QUEUE_WORKER_AUTO_ROUTE_PREFIX'] = $aConfig['MVC_QUEUE_ROUTE_PREFIX'] . '/worker';

    // Class::method responsible for resolving Worker Routes
    $aConfig['MVC_QUEUE_WORKER_AUTO_ROUTE_RESOLVE_CLASSMETHOD'] = '\App\Controller\Queue::workerAutoRouteResolve';

    // Max processing time of an async process in seconds; cancellation if reached.
    $aConfig['MVC_QUEUE_RUNTIME_SECONDS'] = 300;
}
~~~
