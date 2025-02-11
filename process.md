 
# Process

- [`callRoute`](#callRoute)
- [`isRunning`](#isRunning)
- [`reportOnPid`](#reportOnPid)
- [Process config](#Process-config)

------------------------------------------------------------------------------------------------------------------------

<a id="callRoute"></a>
## `callRoute`

_calls a Route non-blocking_
~~~php
/** @var int $iPid */
$iPid = Process::callRoute('/404/');
~~~
- returns the (int) id of the process

------------------------------------------------------------------------------------------------------------------------

<a id="isRunning"></a>
## `isRunning`

_checks whether a process identified by its process id is running_  
~~~php
/** @var boolean $bIsRunning */
$bIsRunning = Process::isRunning(123);
~~~

------------------------------------------------------------------------------------------------------------------------

## `reportOnPid`

_get a report on processes_
~~~php
// default
echo nl2br(
  Process::reportOnPid()
);

// with modified symbols
echo nl2br(
  Process::reportOnPid(
    sRunningSymbol: '<i class="fa fa-gear text-success"></i>',
    sZombieSymbol: '<i class="fa fa-skull-crossbones text-danger"></i>'
  )
);
~~~
~~~
⚙ Running: 274196 since 2025-01-03 10:03:47
⚙ Running: 274198 since 2025-01-03 10:03:47
⚙ Running: 274200 since 2025-01-03 10:03:47
️️☠️ Zombie: 274202 since 2025-01-03 10:01:00
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Process-Config"></a>
## Process config

the queue main config resides in `config/_mvc.php`. Override to your needs in your module's config files.

_config_  
~~~php
MVC_PROCESS: {

    // Maximum number of all job processes allowed in parallel
    $aConfig['MVC_PROCESS_MAX_PROCESSES_OVERALL'] = 30;

    // pidFiles directory
    $aConfig['MVC_PROCESS_PID_FILE_DIR'] = $aConfig['MVC_APPLICATION_PATH'] . '/pid/';
}
~~~