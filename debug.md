
# Debug

- [info](#info)
- [display](#display)
- [stop](#stop)
- [varExport](#varExport)
- [prepareBacktraceArray](#prepareBacktraceArray)

---

<a id="info"></a>
## `info`

dumps Data. The source file, line and class/method infos from where the info command was called are shown.

_available shorthand function_
~~~
info(mixed $mData = '', array $aDebugBacktrace = array()) : void
~~~

_Example_  
~~~php
info($this);
~~~

_Frontend_  
![Debug::info()](/doc/1.x/debug/debug_info.png)

_CLI_  
![Debug::info()](/doc/1.x/debug/debug_info_cli.png)

---

<a id="display"></a>
## `display`

on Frontend, multiple display usages are stacked on top of each other. Each display debug has a count number for better identifying.

The source file, line and class/method infos from where the display command was called are shown.

_available shorthand function_
~~~
display(mixed $mData = '', array $aDebugBacktrace = array()) : void
~~~

_Frontend_  
![Debug::display()](/doc/1.x/debug/debug_display.png)

_CLI_  
![Debug::display()](/doc/1.x/debug/debug_display_cli.png)

---

<a id="stop"></a>
## `stop`

_available shorthand function_ 
~~~
stop();
~~~
_Non-shorthand, offering options_
~~~
Debug::stop(mixed $mData = '', bool $bShowWhereStop = true, bool $bDump = true, array $aBacktrace = array()) : void
~~~

_Example_  
~~~php
stop();
~~~
~~~
stop at:
- File: /var/www/htdocs/Emvicy/modules/Foo/Controller/Index.php
- Line: 100
- Method: Foo\Controller\Index::index
~~~

_Example_  
~~~php
Debug::stop('here i stopped');
~~~

_Frontend_  
![Debug::display()](/doc/1.x/debug/debug_stop.png)

_CLI_  
![Debug::display()](/doc/1.x/debug/debug_stop_cli.png)

---

<a id="varExport"></a>
## `varExport`

~~~
Debug::varExport(mixed $mData, bool $bReturn = false, bool $bShortArraySyntax = true)
~~~

_Examples_ 

*equals `var_export()`, except that arrays are noted with square brackets `[ ]`*      
~~~php
Debug::varExport($GLOBALS['aConfig'];
~~~

*equals `var_export()`*  
~~~php
Debug::varExport($GLOBALS['aConfig'], bShortArraySyntax: false);
~~~

_returns output_  
~~~php
$aExport = Debug::varExport($GLOBALS['aConfig'], true);
~~~

---

<a id="prepareBacktraceArray"></a>
## `prepareBacktraceArray`

returns an array containing information about the source of the call which leads to this place.

~~~
Debug::prepareBacktraceArray(array $aBacktrace = array()) : array
~~~

_Example_  
~~~php
$aDebug = Debug::prepareBacktraceArray(
    debug_backtrace()
);
~~~

_Result of `$aDebug`_  
~~~
// type: array, items: 4
[
    'sFile' => '/var/www/htdocs/Emvicy/application/library/MVC/Reflex.php',
    'sLine' => 153,
    'sClass' => 'MVC\\Reflex',
    'sFunction' => 'reflect',
]
~~~