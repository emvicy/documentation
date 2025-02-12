
# Cron

- [Cron config](#Cron-config)
- [list cron configuration](#list-cron-configuration)
- [run cron](#run-cron)
- [crontab](#crontab)

------------------------------------------------------------------------------------------------------------------------


<a id="Cron-config"></a>
## Cron config

Edit the `_cron.php` file in the [configuration folder of your module](/2.x/configuration#Modules-config-folder).

Add routes that are to be called recurrently.

*Example: `modules/Foo/etc/config/Foo/config/_cron.php`*
~~~php
<?php

/**
 * just list concrete Routes to be called
 */
$aConfig['MODULE']['Foo']['cron'] = [

    #-------------------------------------------------------------------------------------------------------------------
    # queue
    # starts all workers that are listed in config `etc/config/{module}/config/_worker.php`.

    // which is: '/~/queue/worker/run'
    $aConfig['MVC_QUEUE_RUN'],

    // start WebSocket Server
    '/ws/serve/'
];
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="list-cron-configuration"></a>
## list cron configuration


_list cron configuration_  
~~~bash
php emvicy cron:list
~~~

_Example output_  
~~~
# Cron List
# config: /var/www/html/modules/Foo/etc/config/Foo/config/


| No  | Route                                                                                                                 |
|-----|-----------------------------------------------------------------------------------------------------------------------|
| 1   | /~/queue/worker/run                                                                                                   |
| 2   | /ws/serve/                                                                                                            |
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="run-cron"></a>
## run cron

all routes listed in the Cron config are called non-blocking via [Process::callRoute()](/2.x/process#callRoute)

_runs emvicy cron configuration_  
~~~bash
php emvicy cron:run
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="crontab"></a>
## crontab

_Example crontab_  
~~~bash
#----------------------------------------
# Emvicy 2.x
* * * * * cd /var/www/html; /usr/bin/php emvicy cron:run > /dev/null 2>/dev/null;
~~~