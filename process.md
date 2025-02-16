 
# Process

- [`callRoute`](#callRoute)
- [`isRunning`](#isRunning)
- [`reportOnPid`](#reportOnPid)
- [`getPidFileFolder`](#getPidFileFolder)
- [`getAmountProcessesMax`](#getAmountProcessesMax)
- [`getAmountProcessesRecorded`](#getAmountProcessesRecorded)
- [`getAmountProcessesAvailable`](#getAmountProcessesAvailable)
- [`savePid`](#savePid)
- [`hasPidFile`](#hasPidFile)
- [`deletePidFile`](#deletePidFile)
- [`getRunningPidFileArray`](#getRunningPidFileArray)
- [`getZombiePidFileArray`](#getZombiePidFileArray)
- [`deleteZombieFiles`](#deleteZombieFiles)
- [`destruct`](#destruct)
- [Process config](#Process-config)
- [Process Event Listener](#Process-Event-Listener)

------------------------------------------------------------------------------------------------------------------------

<a id="callRoute"></a>
## `callRoute`

_calls a Route non-blocking_
~~~php
/** @var int $iPid */
$iPid = Process::callRoute('/404/');
~~~
- calls a Route non-blocking
- saves the (int) id of the process (the pid) into the pid file folder
- returns the (int) id of the process (the pid)

_`$iPid`_  
~~~
// type: int
12345
~~~

**Events**

- `mvc.process.callRoute.before`; containing the route `$sRoute`
- `mvc.process.callRoute.after`; containing `array('sRoute' => $sRoute, 'iPid' => $iPid)`

------------------------------------------------------------------------------------------------------------------------

<a id="isRunning"></a>
## `isRunning`

_checks whether a process identified by its process id is running_  
~~~php
/** @var boolean $bIsRunning */
$bIsRunning = Process::isRunning(12345);
~~~

_`$bIsRunning`_  
~~~
// type: boolean
true
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

_echo output_  
~~~
⚙ Running: 274196 since 2025-01-03 10:03:47
⚙ Running: 274198 since 2025-01-03 10:03:47
⚙ Running: 274200 since 2025-01-03 10:03:47
️️☠️ Zombie: 274202 since 2025-01-03 10:01:00
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="getPidFileFolder"></a>
## `getPidFileFolder`

_returns the absolute path to the pid file folder_
~~~php
/** @var string $sPidFileFolder */
$sPidFileFolder = Process::getPidFileFolder();
~~~

_`$sPidFileFolder`_  
~~~
// type: string
'/var/www/html/application/pid/'
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="getAmountProcessesMax"></a>
## `getAmountProcessesMax`

_returns the maximum number of all job processes allowed_
~~~php
/** @var int $iMaxProcesses */
$iMaxProcesses = Process::getAmountProcessesMax();
~~~

_`$iMaxProcesses`_  
~~~
// type: integer
30
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="getAmountProcessesRecorded"></a>
## `getAmountProcessesRecorded`

_detect running processes (=== amount of current pidfiles in pid file folder)_
~~~php
/** @var int $iAmount */
$iAmount = Process::getAmountProcessesRecorded();
~~~

_`$iAmount`_
~~~
// type: integer
2
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="getAmountProcessesAvailable"></a>
## `getAmountProcessesAvailable`

_returns the amount of available processes_
~~~php
/** @var int $iAmount */
$iAmount = Process::getAmountProcessesAvailable();
~~~

_`$iAmount`_
~~~
// type: integer
28
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="savePid"></a>
## `savePid`

_saves pid from `getmypid()` as a file into the pid file folder_
~~~php
/** @var boolean $bSave */
$bSave = Process::savePid();
~~~

_saves the given pid as a file into the pid file folder_  
~~~php
/** @var boolean $bSave */
$bSave = Process::savePid(iPid: 12345);
~~~

_saves the given pid as a file into the pid file folder, plus the pid file contains the argument `$mContent` JSON encoded_
~~~php
/** @var boolean $bSave */
$bSave = Process::savePid(
    iPid: 12345, 
    mContent: array(
        'foo' => 'bar'
    ) 
);
~~~

_`$bSave`_
~~~
// type: boolean
true
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="hasPidFile"></a>
## `hasPidFile`

_checks whether there exists a pid file in the pid file folder relating to the current pid_
~~~php
/** @var boolean $bHasPidFile */
$bHasPidFile = Process::hasPidFile();
~~~
- if no pid ist given, the current pid from `getmypid()` is automatically taken

_checks whether there exists a pid file in the pid file folder relating to the given pid_
~~~php
/** @var boolean $bHasPidFile */
$bHasPidFile = Process::hasPidFile(12345);
~~~

_`$bHasPidFile`_
~~~
// type: boolean
true
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="deletePidFile"></a>
## `deletePidFile`

_deletes pid file in the pid file folder relating to the current pid_
~~~php
/** @var boolean $bDelete */
$bDelete = Process::deletePidFile();
~~~
- if no pid ist given, the current pid from `getmypid()` is automatically taken

_deletes pid file in the pid file folder relating to the given pid_
~~~php
/** @var boolean $bDelete */
$bDelete = Process::deletePidFile(12345);
~~~

_`$bDelete`_
~~~
// type: boolean
true
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="getRunningPidFileArray"></a>
## `getRunningPidFileArray`

_returns array with absolute path to running pidfiles_
~~~php
/** @var array $aRunning */
$aRunning = Process::getRunningPidFileArray();
~~~

_`$aRunning`_
~~~
// type: array, items: 1
[
    0 => '/var/www/html/application/pid/1841',
]
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="getZombiePidFileArray"></a>
## `getZombiePidFileArray`

_returns detected real zombies as array containing absolute path to zombie pidfiles_
~~~php
/** @var array $aZombie */
$aZombie = Process::getZombiePidFileArray();
~~~

_`$aZombie`_
~~~
// type: array, items: 0
[
]
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="deleteZombieFiles"></a>
## `deleteZombieFiles`

_deletes all zombie pid files_
~~~php
Process::deleteZombieFiles();
~~~

**Event**

- `mvc.process.deleteZombieFiles.after`; containing the deleted pid file `$sPidFile`


------------------------------------------------------------------------------------------------------------------------

<a id="destruct"></a>
## `destruct`

this method is called at each event that matches the pattern `*.controller.*.__destruct`. See [Process Event Listener ](/2.x/process#Process-Event-Listener)

_deletes pid-files on shutdown_
~~~php
Process::destruct();
~~~

**Event**

- `mvc.process.destruct.after`; containing the process id (the pid) of the ended process in `$iPid`

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

------------------------------------------------------------------------------------------------------------------------

<a id="Process-Event-Listener"></a>
## Process Event Listener 

_`modules/Foo/etc/event/process.php`_  
~~~php
<?php

\MVC\Event::processBindConfigStack([

    /*
     * Events for which the process destructor should be called
     * '*.controller.*.__destruct',
     */
    '*.__destruct' => [
        function() {
            \MVC\Process::destruct();
        }
    ],
    'mvc.process.callRoute.before' => [
        function (string $sRoute) {
//            \MVC\Log::write($sRoute, \MVC\Config::get_MVC_LOG_FILE_PROCESS());
        },
    ],
    'mvc.process.callRoute.after' => [
        function (array $aInfo) {
            \MVC\Log::write(json_encode($aInfo), \MVC\Config::get_MVC_LOG_FILE_PROCESS());
        },
    ],
    'mvc.process.deleteZombieFiles.after' => [
        function (string $sPidFile) {
            \MVC\Log::write('pidfile: ' . $sPidFile . "\tDELETE ZOMBIE", \MVC\Config::get_MVC_LOG_FILE_PROCESS());
        },
    ],
    'mvc.process.destruct.after' => [
        function (int $iPid) {
            \MVC\Log::write('pid: ' . $iPid . "\tDELETE", \MVC\Config::get_MVC_LOG_FILE_PROCESS());
        },
    ],
]);
~~~