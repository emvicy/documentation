
# Cron

- [`callRoute`](#callRoute)
- [`isRunning`](#isRunning)
- [`reportOnPid`](#reportOnPid)
- [Cron config](#Cron-config)

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

_runs emvicy cron configuration_  
~~~bash
php emvicy cron:run
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Cron-config"></a>
## Cron config