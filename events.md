
# Events

- [Registering Event Listeners](#registering-event-listeners)
- [`\MVC\Event::run()`](#event-run)
- [`\MVC\Event::bind()`](#event-bind)
  - [`\MVC\Event::processBindConfigStack()` - Bind to Events via config Stack](#processBindConfigStack)
- [`\MVC\Event::delete()`](#event-delete) 
- [Emvicy Standard Events](#EmvicyStandardEvents)
  - [Database Events](#database_events)

------------------------------------------------------------------------------------------------------------------------

<a id="registering-event-listeners"></a>
## Registering Event Listeners

_Location where to write Event Listeners_  
~~~
module/{module}/etc/config/event/
~~~

Listeners written here are executed right after all Configurations have been loaded. No essential classes of Emvicy have been loaded or processed at this point yet.

So this is the perfect place to influence the behavior of the starting application.

_Example: log current request object to debug.log when on develop environment_
~~~php
<?php

\MVC\Event::processBindConfigStack([

    'mvc.request.getCurrentRequest.after' => [ // Event
    
        function (\MVC\DataType\DTArrayObject $oDTArrayObject) { // Closure
        
            // get request object
            $oDTRequestCurrent = $oDTArrayObject->getDTKeyValueByKey('oDTRequestCurrent')->get_sValue();

            if ('develop' === \MVC\Config::get_MVC_ENV())
            {
                \MVC\Log::write($oDTRequestCurrent, 'debug.log');
            }
        },
    ],
]);
~~~

You can write your own event bondings into an already existing file.  

And you may add as many events as your application requires.

You can also create your own php file in that folder and note your event bondings, no problem.  
⚠ but consider the loading order of the files, which is a-z.


------------------------------------------------------------------------------------------------------------------------

<a id="event-run"></a>
## `\MVC\Event::run()` 

with `run` you initialize an event; bonded Closures to the event name get executed in order.

_Syntax_
~~~
\MVC\Event::run('eventName', {mixed} );
~~~

_Example: Run a simple Event_
~~~php
\MVC\Event::run('foo');
~~~

_Example: pass a value to the event_  
~~~php
\MVC\Event::run('foo', 123);
~~~

_Example: pass an `DTArrayObject` Object with Infos_  
~~~php
\MVC\Event::run('foo', DTArrayObject::create()
    ->add_aKeyValue(
        DTKeyValue::create()->set_sKey('foo')->set_sValue('bar')
    )
);
~~~


------------------------------------------------------------------------------------------------------------------------

<a id="event-bind"></a>
## `\MVC\Event::bind()`

with `bind` you create a Listener to an expected event;
you _bind_ a _closure_ to the expected event.

_Syntax_  
~~~
\MVC\Event::bind('event', {closure} );
~~~

**bind to an Event Name**

_regular - listens to event whose event name is 'fooName'_
~~~php
\MVC\Event::bind('fooName', function() { ... });
~~~

**bind to an Event Name with placeholder**

_with placeholder * - listens to all events whose event names match_
~~~php
\MVC\Event::bind('foo*', function() { ... });
\MVC\Event::bind('*Name', function() { ... });
\MVC\Event::bind('f*N*', function() { ... });
~~~


**bind to a Controller::method**  

_Example: bind a closure to a concrete Controller::method_ 
~~~php
\MVC\Event::bind('\{module}\Controller\Index::foo', function (\MVC\DataType\DTArrayObject $oDTArrayObject, \MVC\DataType\DTEventContext $oDTEventContext) {
    info($oDTArrayObject);
    display($oDTEventContext);
});
~~~
- 🛈 The first parameter will always be of type `\MVC\DataType\DTArrayObject $oDTArrayObject`
- 🛈 You can bind to Controller classes only which have `\MVC\MVCInterface\Controller` implemented
- 🛈 Make sure to write the event name as the method was a static one, even it is not.

---

**Additional Context Infos passed as second parameter to a bonded closure**

As you may have noticed, the object `\MVC\DataType\DTEventContext $oDTEventContext)` is always being passed as the second parameter to a bonded closure. 
It provides Infos about the context of the event.

This is helpful if your app listens to events with placeholder `*` - let's say `foo*` (with placeholder asterisk).
You can get the origin Event which matched to `foo*`:

~~~php
// returns 'fooName'
$oDTEventContext->get_sEventOrigin();
~~~

_example content of `\MVC\DataType\DTEventContext $oDTEventContext)`_
~~~
// type: object
\MVC\DataType\DTEventContext::__set_state(array(
      'sEvent' => 'foo*',
      'sEventOrigin' => 'fooName',
      'mRunPackage' => true,
      'aBonded' =>    array (
        's:53:"/modules/{module}/etc/event/default.php, 7 (65759f7544778)";' =>        \Closure::__set_state(array(
        )),
    ),
      'sBondedBy' => 's:53:"/modules/{module}/etc/event/default.php, 7 (65759f7544778)";',
      'sCalledIn' => '/modules/Foo/Controller/Index.php, 58',
      'oCallback' =>    \Closure::__set_state(array(
    )),
      'sCallbackDumped' => 'function (oDTEventContext $oDTEventContext){
                        info($oDTEventContext);
                        display($oDTEventContext->get_sEventOrigin());
                },
',
      'sMessage' => 'RUN+ (foo* [fooName]) --> called in: /modules/Foo/Controller/Index.php, 58 --> bonded by `/modules/{module}/etc/event/default.php, 7 (65759f7544778), try to run its Closure: function ($mPackage, \\MVC\\DataType\\DTEventContext $oDTEventContext) { info($oDTEventContext); display($oDTEventContext->get_sEventOrigin()); }',
))
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="processBindConfigStack"></a>
### `\MVC\Event::processBindConfigStack()` - Bind to Events via config Stack

Instead of writing one `bind` command expressure after the other you can make use of array notation and use `Event::processBindConfigStack`.

_Syntax_  
~~~
\MVC\Event::processBindConfigStack([ '{EventName}' => [ {Closure}, ], ]);
~~~

This way you can note bondings to multiple Events at once and you can even note multiple closures for each event.

This reduces complexity and improves readability.

_Example: using config Stack to bind closures to Events_  
~~~php
<?php
\MVC\Event::processBindConfigStack([ // Bondings to 2 Events

    '\Foo\Controller\Index::foo' => [ // 2 Closures bonded to this Event
    
        function (\MVC\DataType\DTArrayObject $oDTArrayObject, \MVC\DataType\DTEventContext $oDTEventContext) {      
            info($oDTArrayObject);
        },
        function (\MVC\DataType\DTArrayObject $oDTArrayObject, \MVC\DataType\DTEventContext $oDTEventContext) {      
            \MVC\Log:write($oDTArrayObject, 'debug.log');
        }
    ],
    '\Foo\Controller\Index::bar' => [ // 1 Closure bonded to this Event
    
        function (\MVC\DataType\DTArrayObject $oDTArrayObject, \MVC\DataType\DTEventContext $oDTEventContext) {      
            \MVC\Log:write($oDTArrayObject, 'debug.log');
        }
    ],    
]);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="event-delete"></a>
## `\MVC\Event::delete()`

delete one or all events. 

⚠ If this parameter not is set, *all* events are going to be deleted.

_Example: delete the certain Event named `foo.bar`_  
~~~php
\MVC\Event::delete('foo.bar');
~~~

_Example: delete *all* Events_  
~~~php
\MVC\Event::delete();
~~~


------------------------------------------------------------------------------------------------------------------------

<a id="EmvicyStandardEvents"></a>
## Emvicy Standard Events 

| Event Name                                            | `Event::bind` perforemd in                                   | `Event::run` located in                                                                                 | value passed                                                    |
|-------------------------------------------------------|--------------------------------------------------------------|---------------------------------------------------------------------------------------------------------|-----------------------------------------------------------------|
| mvc.event.init.after                                  |                                                              | `\MVC\Event::init`                                                                                      |                                                                 |
| policy.index.requestMethodHasToMatchRouteMethod.after | modules/{module}/etc/event/policy.php                        | `\{module}\Policy\Index::requestMethodHasToMatchRouteMethod`                                            | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.application.construct.after                       |                                                              | `\MVC\Application::__construct`                                                                         |                                                                 |
| mvc.application.setSession.before                     | modules/{module}/etc/event/default.php                       | `\MVC\Application::initSession`                                                                         |                                                                 |
| mvc.application.setSession.after                      |                                                              | `\MVC\Application::initSession`                                                                         | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.application.destruct.before                       |                                                              | `\MVC\Application::__destruct`                                                                          | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.controller.init.before                            | `\MVC\Request::getCurrentRequest`                            | `\MVC\Controller::init`                                                                                 |                                                                 |
| mvc.controller.init.after                             |                                                              | `\MVC\Controller::init`                                                                                 | `$bSuccess`                                                     |
| mvc.controller.runTargetClassPreconstruct.after       |                                                              | `\MVC\Controller::runTargetClassPreconstruct`                                                           | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.controller.destruct.before                        |                                                              | `\MVC\Controller::__destruct`                                                                           | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.error                                             | `\MVC\Error::init`<br>modules/{module}/etc/event/default.php | `\MVC\Controller::runTargetClassPreconstruct`<br>`\MVC\Request::getUriProtocol`<br>`\MVC\Policy::apply` | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.policy.init.before                                |                                                              | `\MVC\Policy::init`                                                                                     |                                                                 |
| mvc.policy.init.after                                 |                                                              | `\MVC\Policy::init`                                                                                     |                                                                 |
| mvc.policy.set.before                                 |                                                              | `\MVC\Policy::set`                                                                                      | `array $aPolicy` _all declared policies_                        |
| mvc.policy.set.after                                  |                                                              | `\MVC\Policy::set`                                                                                      | `array $aPolicy` _all declared policies_                        |
| mvc.policy.unset.before                               |                                                              | `\MVC\Policy::unset`                                                                                    | `array $aPolicy` _all declared policies_                        |
| mvc.policy.unset.after                                |                                                              | `\MVC\Policy::unset`                                                                                    | `array $aPolicy` _all declared policies_                        |
| mvc.policy.apply.before                               |                                                              | `\MVC\Policy::apply`                                                                                    | `array $aPolicy` _matching policy rules on the current request_ |
| mvc.policy.apply.execute                              |                                                              | `\MVC\Policy::apply`                                                                                    | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.reflex.reflect.before                             |                                                              | `\MVC\Reflex::reflect`                                                                                  | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.reflex.reflect.targetObject.before                | modules/{module}/etc/event/default.php                       | `\MVC\Reflex::reflect`                                                                                  | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.reflex.reflect.targetObject.after                 | modules/{module}/etc/event/default.php                       | `\MVC\Reflex::reflect`                                                                                  | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.reflex.destruct.before                            |                                                              | `\MVC\Reflex::__destruct`                                                                               | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.route.init.before                                 |                                                              | `\MVC\Route::init`                                                                                      |                                                                 |
| mvc.route.init.after                                  |                                                              | `\MVC\Route::init`                                                                                      |                                                                 |
| `$sControllerClassName :: $sMethod`                   |                                                              | `\MVC\Reflex::reflect`                                                                                  |                                                                 |
| mvc.request.getCurrentRequest.after                   |                                                              | `\MVC\Request::getCurrentRequest`                                                                       | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.request.redirect                                  |                                                              | `\MVC\Request::redirect`                                                                                | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.routeintervall.intervall.before                   | modules/{module}/etc/event/routeintervall.php                | `\MVC\RouteIntervall::intervall`                                                                        | `\MVC\DataType\DTCronTask $oDTCronTask`                         |
| mvc.routeintervall.intervall.after                    | modules/{module}/etc/event/routeintervall.php                | `\MVC\RouteIntervall::intervall`                                                                        | `\MVC\DataType\DTCronTask $oDTCronTask`                         |
| mvc.routeintervall.intervall.skip                     | modules/{module}/etc/event/routeintervall.php                | `\MVC\RouteIntervall::intervall`                                                                        | `\MVC\DataType\DTCronTask $oDTCronTask`                         |
| mvc.routeintervall.intervall.end                      | modules/{module}/etc/event/routeintervall.php                | `\MVC\RouteIntervall::intervall`                                                                        | `\MVC\DataType\DTCronTask $oDTCronTask`                         |
| mvc.view.render.before                                | `\MVC\InfoTool::__construct`                                 | `\MVC\View::render`                                                                                     | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.view.renderString.before                          |                                                              | `\MVC\View::renderString`                                                                               | `$sTemplateString`                                              |
| mvc.view.renderString.after                           |                                                              | `\MVC\View::renderString`                                                                               | `$sRendered`                                                    |
| mvc.view.render.after                                 |                                                              | `\MVC\View::render`                                                                                     | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.debug.stop.after                                  | modules/{module}/etc/event/default.php                       | `\MVC\Debug::stop`                                                                                      | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.lock.create                                       |                                                              | `\MVC\Lock::create`                                                                                     | `\MVC\DataType\DTArrayObject $oDTArrayObject`                   |
| mvc.view.echoOut.off                                  | `\MVC\View::__construct`                                     |                                                                                                         |                                                                 |
| mvc.view.echoOut.on                                   | `\MVC\View::__construct`                                     |                                                                                                         |                                                                 |
| mvc.view.render.off                                   | `\MVC\View::__construct`                                     |                                                                                                         |                                                                 |
| mvc.view.render.on                                    | `\MVC\View::__construct`                                     |                                                                                                         |                                                                 |

<a id="database_events"></a>  
### Database Events

| Event Name                              | `Event::bind` perforemd in          | `Event::run` located in               | value passed                                        |
|-----------------------------------------|-------------------------------------|---------------------------------------|-----------------------------------------------------|
| mvc.db.model.dbinit.construct.after     |                                     |                                       |                                                     |
| mvc.db.model.dbpdo.fetchRow.sql         | `modules/{module}/etc/event/db.php` | `\MVC\DB\Model\DbPDO::fetchRow`       | `string $sSql`                                      |
| mvc.db.model.dbpdo.fetchAll.sql         | `modules/{module}/etc/event/db.php` | `\MVC\DB\Model\DbPDO::fetchAll`       | `string $sSql`                                      |
| mvc.db.model.dbpdo.query.sql            | `modules/{module}/etc/event/db.php` | `\MVC\DB\Model\DbPDO::query`          | `string $sSql`                                      |
| mvc.db.model.db.construct.before        |                                     | `\MVC\DB\Model\Db::__construct`       | `\MVC\DataType\DTValue $oDTValue`                   |
| mvc.db.model.db.construct.saveCache     | `modules/{module}/etc/event/db.php` | `\MVC\DB\Model\Db::__construct`       |                                                     |
| mvc.db.model.db.create.before           | `modules/{module}/etc/event/db.php` | `\MVC\DB\Model\Db::create`            | `\MVC\DB\DataType\DB\TableDataType $oTableDataType` |
| mvc.db.model.db.create.sql              | `modules/{module}/etc/event/db.php` | `\MVC\DB\Model\Db::create`            | `string $sSql`                                      |
| mvc.db.model.db.create.after            |                                     | `\MVC\DB\Model\Db::create`            | `\MVC\DB\DataType\DB\TableDataType $oTableDataType` |
| mvc.db.model.db.createTable.before      |                                     | `\MVC\DB\Model\Db::create`            | `\MVC\DataType\DTValue $oDTValue`                   |
| mvc.db.model.db.createTable.sql         | `modules/{module}/etc/event/db.php` | `\MVC\DB\Model\Db::create`            | `string $sSql`                                      |
| mvc.db.model.db.insert.sql              | `modules/{module}/etc/event/db.php` | `\MVC\DB\Model\Db::synchronizeFields` | `string $sSql`                                      |
| mvc.db.model.db.retrieveTupel.before    |                                     | `\MVC\DB\Model\Db::retrieveTupel`     | `\MVC\DB\DataType\DB\TableDataType $oTableDataType` |
| mvc.db.model.db.retrieve.before         |                                     | `\MVC\DB\Model\Db::retrieve`          | `\MVC\DataType\DTValue $oDTValue`                   |
| mvc.db.model.db.retrieve.sql            | `modules/{module}/etc/event/db.php` | `\MVC\DB\Model\Db::retrieve`          | `string $sSql`                                      |
| mvc.db.model.db.retrieve.after          |                                     | `\MVC\DB\Model\Db::retrieve`          | `\MVC\DataType\DTValue $oDTValue`                   |
| mvc.db.model.db.count.before            |                                     | `\MVC\DB\Model\Db::count`             | `\MVC\DataType\DTValue $oDTValue`                   |
| mvc.db.model.db.count.sql               |                                     | `\MVC\DB\Model\Db::count`             | `string $sSql`                                      |
| mvc.db.model.db.updateTupel.before      |                                     | `\MVC\DB\Model\Db::updateTupel`       | `\MVC\DB\DataType\DB\TableDataType $oTableDataType` |
| mvc.db.model.db.updateTupel.after       |                                     | `\MVC\DB\Model\Db::updateTupel`       | `\MVC\DataType\DTValue $oDTValue`                   |
| mvc.db.model.db.updateTupel.fail        |                                     | `\MVC\DB\Model\Db::updateTupel`       | `\MVC\DataType\DTValue $oDTValue`                   |
| mvc.db.model.db.updateTupel.success     |                                     | `\MVC\DB\Model\Db::updateTupel`       | `\MVC\DataType\DTValue $oDTValue`                   |
| mvc.db.model.db.update.before           | `modules/{module}/etc/event/db.php` | `\MVC\DB\Model\Db::update`            | `\MVC\DataType\DTValue $oDTValue`                   |
| mvc.db.model.db.update.sql              | `modules/{module}/etc/event/db.php` | `\MVC\DB\Model\Db::update`            | `string $sSql`                                      |
| mvc.db.model.db.update.fail             |                                     | `\MVC\DB\Model\Db::update`            | `\MVC\DataType\DTValue $oDTValue`                   |
| mvc.db.model.db.update.success          |                                     | `\MVC\DB\Model\Db::update`            | `\MVC\DataType\DTValue $oDTValue`                   |
| mvc.db.model.db.deleteTupel.before      |                                     | `\MVC\DB\Model\Db::deleteTupel`       | `\MVC\DB\DataType\DB\TableDataType $oTableDataType` |
| mvc.db.model.db.delete.before           |                                     | `\MVC\DB\Model\Db::delete`            | `\MVC\DataType\DTValue $oDTValue`                   |
| mvc.db.model.db.delete.sql              | `modules/{module}/etc/event/db.php` | `\MVC\DB\Model\Db::delete`            | `string $sSql`                                      |
| mvc.db.model.db.synchronizeFields.after |                                     | `\MVC\DB\Model\Db::synchronizeFields` |                                                     |
| mvc.db.model.db.dropIndices.after       |                                     | `\MVC\DB\Model\Db::dropIndices`       |                                                     |


