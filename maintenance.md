 
# Maintenance

- [Activation](#Activation)
- [Get Maintenance Status](#get-maintenance-status)

------------------------------------------------------------------------------------------------------------------------

<a id="Activation"></a>
## Activation

to set the application in a maintenance modus, remove the dot `.` from the file `/.maintenance` in the Base Path of your Application.

_rename from..._
~~~bash
/.maintenance
~~~
_...to_  
~~~bash
/maintenance
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="get-maintenance-status"></a>
## Get Maintenance Status

~~~php
/** @var boolean $bMaintenance */
$bMaintenance = \MVC\Application::isMaintenance();
~~~
- returns `true` if the file `/maintenance` exists in Base Path
- returns `false` if the file `/maintenance` does **not** exist in Base Path

**Events fired**

- `mvc.application.maintenance`