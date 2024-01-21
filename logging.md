
# Logging

- [Configuration](#configuration)

------------------------------------------------------------------------------------------------------------------------

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
2023-07-25 16:08:43     foo.local       develop 127.0.0.1       2023072516084364bfd76bbbfa8     ...........no.session...........        1       /application/init/util/bootstrap.php, 111       ##########      new Request     apache2handler  GET /
~~~

---

<a id="configuration"></a>
## Configuration

Preferably change the settings in the [environment configuration file](/1.x/configuration#Modules-environment-config-file) of your module according to your needs.

_Settings for Logging_  
~~~php
/**-----------------------------------------------------------------------------------------------------------------
 * Log
 * consider a logrotate mechanism for these logfiles as they may grow quickly
 */
$aConfig['MVC_LOG_SQL'] = true;                 // logging enabled true|false
$aConfig['MVC_LOG_EVENT'] = true;               // logging enabled true|false
// logging of each simple "RUN" event into MVC_LOG_FILE_EVENT
// remember:
// - events marked as "RUN": fired events without any listener (nothing happens)
// - events marked as "RUN+": fired events with bonded listeners / closures to be executed
// be aware that setting this to "true" would produce much data in the logfile (consider using logrotate!)
// anyway this might be useful for a develop environment, as it helps debugging and understanding
$aConfig['MVC_LOG_EVENT_RUN'] = false;          // logging enabled true|false
$aConfig['MVC_EVENT_LOG_RUN'] = $aConfig['MVC_LOG_EVENT_RUN']; /** @deprecated use instead: MVC_LOG_EVENT_RUN */
$aConfig['MVC_LOG_POLICY'] = true;              // logging enabled true|false
$aConfig['MVC_LOG_ERROR'] = true;               // logging enabled true|false
$aConfig['MVC_LOG_NOTICE'] = true;              // logging enabled true|false
$aConfig['MVC_LOG_WARNING'] = true;             // logging enabled true|false
$aConfig['MVC_LOG_REQUEST'] = true;            // logging enabled true|false
$aConfig['MVC_LOG_DEFAULT'] = true;             // logging enabled true|false
$aConfig['MVC_LOG_ROUTEINTERVALL'] = true;      // logging enabled true|false

$aConfig['MVC_LOG_FORCE_LINEBREAK'] = false;    // force linebreaks in logfiles no matter what

// Log file places

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
