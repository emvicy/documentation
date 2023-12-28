
# RouteIntervall

Call up routes recurrently after a certain period of time. For complete automatization you need these 3 things:
- [Config yaml file](#Config-yaml-file)
- [Init Route](#Init-Route)
- [Cron Job](#Cron-Job)

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

...and the corresponding controller `\Foo\Controller\Index::routeIntervall`:

~~~php
...
public function routeIntervall()
{
    Event::run('mvc.view.render.off');
    Event::run('mvc.view.echoOut.off');
    
    // start Route Intervall
    RouteIntervall::create(
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


~~~bash
# route intervall
* * * * * cd /var/www/Emvicy/public; /usr/bin/php index.php '/intervall/' 2&>1 /dev/null
~~~