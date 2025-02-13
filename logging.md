
# Logging

- [Log a message into a Logfile](#log-a-message-into-a-logfile)
- [Configuration](#configuration)
  - [Logging on/off](#Logging-on-off)
  - [Log file places](#Log-file-places)
  - [Log details](#Log-details)

------------------------------------------------------------------------------------------------------------------------

<a id="log-a-message-into-a-logfile"></a>
## Log a message into a Logfile

_default path for logfiles_  
~~~
/application/log/
~~~

_write into default logfile_ 
~~~php
\MVC\Log::write('My Message');
~~~
- writes to: `/application/log/default.log`

_write into another logfile_  
~~~php
\MVC\Log::write('My Message', 'specialLogfile.log');
~~~

All Log Entries show:  
- Date and Time
- process id (pid)
- Host
- Environment
- IP Address
- A uniqueID for the current Request
- the Session ID (if any)
- An increasing Counter for each log entry for the time Request is running
- The file and lineNr from where the log was called
- The Log Message

Extra LogInfos for Events  
- `BIND` with the Eventname and where the Event was called from.
- `RUN`	with the Eventname and where the Event was called from. No further logic is bonded to that Event actually
- `RUN+` with the Eventname and where the Event was called from. In this case some logic was bonded to that event (via "BIND") and all bonded logics are listed in detail

_Example_    
~~~bash
2025-02-12 13:11:43  12345  localhost develop  0.0.0.0  2023072516084364bfd76bbbfa8  ...........no.session...........  1  /modules/Cdm/etc/event/request.php, 45  object(MVC\DataType\DTRequestIn)#343 (18) { … }
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="configuration"></a>
## Configuration

Preferably change the settings in your module's [environment configuration file](/2.x/configuration#Modules-environment-config-file) according to your needs.

it is recommended to set a logrotate mechanism for these logfiles as they may grow quickly.

<a id="Logging-on-off"></a>
**Logging on/off**

_Example `modules/Foo/etc/config/Foo/config/develop.php`_  
~~~php
$aConfig['MVC_LOG_SQL'] = true;
$aConfig['MVC_LOG_CRON'] = true;
$aConfig['MVC_LOG_EVENT'] = false;
$aConfig['MVC_LOG_ERROR'] = true;               // recommended to set to true for ALL environments
$aConfig['MVC_LOG_QUEUE'] = true;
$aConfig['MVC_LOG_NOTICE'] = true;
$aConfig['MVC_LOG_POLICY'] = true;
$aConfig['MVC_LOG_PROCESS'] = true;
$aConfig['MVC_LOG_WARNING'] = true;
$aConfig['MVC_LOG_REQUEST'] = true;
$aConfig['MVC_LOG_DEFAULT'] = true;
$aConfig['MVC_LOG_EVENT_RUN'] = false;
$aConfig['MVC_LOG_AUTOLOADER'] = false;         // recommended to set to true for develop environments only: Log autoloader actions
$aConfig['MVC_LOG_ROUTEINTERVALL'] = false;      // recommended to set to true for develop environments only: Log Route Intervall actions
$aConfig['MVC_LOG_FORCE_LINEBREAK'] = true;     // force linebreaks in logfiles no matter what, improves readability of logs but blows up logfile
~~~

<a id="Log-file-places"></a>
**Log file places**

to change Log file places overwrite these settings from [Emvicy config](/2.x/configuration#Emvicy-config-folder) in your module's [environment configuration file](/2.x/configuration#Modules-environment-config-file):

~~~php
$aConfig['MVC_LOG_FILE_DIR'] = $aConfig['MVC_APPLICATION_PATH'] . '/log/';          # trailing slash required
$aConfig['MVC_LOG_FILE_DEFAULT'] = $aConfig['MVC_LOG_FILE_DIR'] . 'default.log';
$aConfig['MVC_LOG_FILE_ERROR'] = $aConfig['MVC_LOG_FILE_DIR'] . 'error.log';
$aConfig['MVC_LOG_FILE_WARNING'] = $aConfig['MVC_LOG_FILE_DIR'] . 'warning.log';
$aConfig['MVC_LOG_FILE_NOTICE'] = $aConfig['MVC_LOG_FILE_DIR'] . 'notice.log';
$aConfig['MVC_LOG_FILE_POLICY'] = $aConfig['MVC_LOG_FILE_DIR'] . 'policy.log';
$aConfig['MVC_LOG_FILE_EVENT'] = $aConfig['MVC_LOG_FILE_DIR'] . 'event.log';
$aConfig['MVC_LOG_FILE_EVENT_RUN'] = $aConfig['MVC_LOG_FILE_DIR'] . 'event_run.log';
$aConfig['MVC_LOG_FILE_REQUEST'] = $aConfig['MVC_LOG_FILE_DIR'] . 'request.log';
$aConfig['MVC_LOG_FILE_SQL'] = $aConfig['MVC_LOG_FILE_DIR'] . 'sql.log';
$aConfig['MVC_LOG_FILE_ROUTEINTERVALL'] = $aConfig['MVC_LOG_FILE_DIR'] . 'route_intervall.log';

// 1) make sure write access is given to the folder
// as long as the db user is going to write and not the webserver user
// 2) consider a logrotate mechanism for this logfile as it may grow quickly
$aConfig['MVC_LOG_FILE_DB_DIR'] = '/tmp/';
~~~

<a id="Log-details"></a>
**Log details**

to define control log details you could overwrite these settings from [Emvicy config](/2.x/configuration#Emvicy-config-folder) in your module's [environment configuration file](/2.x/configuration#Modules-environment-config-file):

~~~php
// control log details
$aConfig['MVC_LOG_DETAIL'] = [
    'date' => true,
    'host' => true,
    'env' => true,
    'ip' => true,
    'uniqueid' => true,
    'sessionid' => true,
    'count' => true,
    'debug' => true,
    'message' => true,
];
~~~
