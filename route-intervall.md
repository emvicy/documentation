
# RouteIntervall

Call up routes recurrently after a certain period of time. For complete automatization you need these 3 things:
- [Config yaml file](#Config-yaml-file)
- [Init Route](#Init-Route)
- [Cron Job](#Cron-Job)

---
 
- [Logging RouteIntervall Actions](#Logging-RouteIntervall-Actions)
- [pid file](#pidFile)
- [stop running the `RouteIntervall` process](#stop)

---

<a id="Config-yaml-file"></a>
## Config yaml file

in your Module's environment config folder (`/modules/{moduleName}/etc/config/{moduleName}/config/`) create the yaml file `_routeintervall.yaml`.

In this file you name your existing routes you want to be called periodically, following by intervall seconds for each route.

*config file `_routeintervall.yaml`*  
~~~bash
#-------------------------------------------
# route             # intervall in seconds
#-------------------------------------------

"/robot/autokick": 30
"/robot/sendMail": 300
~~~

So in this example you want 
- route `/robot/autokick` to be called **every 30 seconds**
- route `/robot/sendMail` to be called **every 300 seconds**

---

<a id="Init-Route"></a>
## Init Route 

you need to create an init route (see [Creating a Route](/1.x/routing#Creating-a-Route)) - you might want to call it `/routeIntervall/`.

_init route_  
~~~php
\MVC\Route::get('/routeIntervall/', '\Foo\Controller\Index::routeIntervall');
~~~

...and the corresponding controller `\Foo\Controller\Index::routeIntervall` which let `RouteIntervall` start its service.

~~~php
...
public function routeIntervall()
{
    Event::run('mvc.view.render.off');
    Event::run('mvc.view.echoOut.off');
    
    // start Route Intervall
    RouteIntervall::create(
        // name the yaml config file 
        Config::get_MVC_MODULE_PRIMARY_STAGING_CONFIG_DIR() . '/_routeintervall.yaml'
    )->run();
}
...
~~~

Once you have reached this point, you are already able to make a call.
cd into the `/public` folder and start the init route. 

~~~php
php index.php '/routeIntervall/'
~~~

---

<a id="Cron-Job"></a>
## Cron Job

Since `RouteIntervall` is being locked (see [Lock](/1.x/lock)), you can safely call the Init Route each minute via cron - there 
will only be one process of it running.

_cronjob for RouteInternvall_  
~~~bash
# route intervall; start in background
* * * * * cd /var/www/Emvicy/public; /usr/bin/php index.php '/intervall/' > /dev/null 2>/dev/null & echo $!
~~~

---

<a id="Logging-RouteIntervall-Actions"></a>
## Logging RouteIntervall Actions

create the file `routeintervall.php` in your event folder (see [Registering Event Listeners](/1.x/events#registering-event-listeners))

~~~
module/{module}/etc/event/routeintervall.php
~~~

copy the following code into that file.

All Listeners are logging into `\MVC\Config::get_MVC_LOG_FILE_ROUTEINTERVALL()`,  
which is by default `$aConfig['MVC_LOG_FILE_ROUTEINTERVALL'] = $aConfig['MVC_LOG_FILE_DIR'] . 'route_intervall.log';`

_Event Listener on RouteIntervall Actions_  
~~~php
<?php

# List of Emvicy Standard Events
# @see https://emvicy.com/1.x/events#EmvicyStandardEvents


\MVC\Event::processBindConfigStack([

    // route intervall starts
    'mvc.routeintervall.run.before' => [
        function(string $sCronYamlFile) {
            \MVC\Log::write(
                'route intervall has started (config file: `' . $sCronYamlFile . '`',
                \MVC\Config::get_MVC_LOG_FILE_ROUTEINTERVALL()
            );
        },
    ],
    // route intervall ...
    'mvc.routeintervall.intervall.after' => [
        function(\MVC\DataType\DTCronTask $oDTCronTask) {
            \MVC\Log::write(
                $oDTCronTask->get_iPid() . "\t" . $oDTCronTask->get_sRoute() . "\t" . $oDTCronTask->get_iIntervall(),
                \MVC\Config::get_MVC_LOG_FILE_ROUTEINTERVALL()
            );
        },
    ],
    // route intervall ends
    'mvc.routeintervall.intervall.end' => [
        function(string $sCronYamlFile) {
            \MVC\Log::write(
                'route intervall has ended (config file: `' . $sCronYamlFile . '`',
                \MVC\Config::get_MVC_LOG_FILE_ROUTEINTERVALL()
            );
        },
    ],
    /**
     * @warning Be careful if listening to this; it would mean a huge amount of continuous data flow
     *          strictly recommended for development environment only
     */
//    'mvc.routeintervall.intervall.before' => [
//        function(\MVC\DataType\DTCronTask $oDTCronTask) {
//            \MVC\Log::write($oDTCronTask->getPropertyJson(), \MVC\Config::get_MVC_LOG_FILE_ROUTEINTERVALL());
//        },
//    ],
    /**
     * @warning Be careful if listening to this; it would mean a huge amount of continuous data flow
     *          strictly recommended for development environment only
     */
//    'mvc.routeintervall.intervall.skip' => [
//        function(\MVC\DataType\DTCronTask $oDTCronTask) {
//            \MVC\Log::write('Skip - ' . $oDTCronTask->getPropertyJson(), \MVC\Config::get_MVC_LOG_FILE_ROUTEINTERVALL());
//        },
//    ],
]);
~~~

---

<a id="pidFile"></a>
## pid File

Once `RouteIntervall` is running, there will be a dot-file created in your Emvicy Base Path _(this is where also `emvicy.php` resides, see [directory-structure#root](/1.x/directory-structure#root))_.

Its name is `.mvc-routeintervall-run.` + `{process id}` .

_Example_  
~~~
.mvc-routeintervall-run.18787
~~~

so you could grep in your process list for that pid as long as `RouteIntervall` is running.

~~~bash
admin1@erazer:~/var/www/Emvicy$ ps u | grep 18787
admin1     18787 99.6  0.1 132584 30512 pts/3    R+   09:39   1:20 php index.php /routeIntervall/
~~~

---

<a id="stop"></a>
## stop running the `RouteIntervall` process

you can stop running the `RouteIntervall` process by simply removing the pidfile.

cd into Emvicy Base Path _(this is where also `emvicy.php` resides, see [directory-structure#root](/1.x/directory-structure#root))_ and remove the pidfile.

_Example_  
~~~bash
cd /var/www/Emvicy; \
rm .mvc-routeintervall-run.18787;
~~~



