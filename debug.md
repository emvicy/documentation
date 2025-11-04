
# Debug

- [constructionTime](#constructionTime)
- [display](#display)
- [info](#info)
- [prepareBacktraceArray](#prepareBacktraceArray)
- [stop](#stop)
- [varExport](#varExport)

---

## `constructionTime` <a id="constructionTime"></a>

returns time passed from start until calling this method.

~~~
Debug::constructionTime() : float
~~~

_available shorthand function_
~~~
ct() : float
~~~

---

## `display` <a id="display"></a>

on Frontend, multiple display usages are stacked on top of each other. Each display debug has a count number for better identifying.

The source file, line and class/method infos from where the display command was called are shown.

~~~
Debug::display(mixed $mData = '', array $aDebugBacktrace = array()) : void
~~~

_available shorthand function_
~~~
display(mixed $mData = '', array $aDebugBacktrace = array()) : void
~~~

_Frontend_  
![Debug::display()](/doc/2.x/debug/debug_display.png)

_CLI_  
![Debug::display()](/doc/2.x/debug/debug_display_cli.png)

---

## `info` <a id="info"></a>

dumps Data. The source file, line and class/method infos from where the info command was called are shown.

~~~
Debug::info(mixed $mData = '', array $aDebugBacktrace = array()) : void
~~~

_available shorthand function_
~~~
info(mixed $mData = '', array $aDebugBacktrace = array()) : void
~~~

_Example_  
~~~php
info($this);
~~~

_Frontend_  
![Debug::info()](/doc/2.x/debug/debug_info.png)

_CLI_  
![Debug::info()](/doc/2.x/debug/debug_info_cli.png)

---

## `prepareBacktraceArray` <a id="prepareBacktraceArray"></a>

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
    'sFile' => '/var/www/html/application/library/MVC/Reflex.php',
    'sLine' => 140,
    'sClass' => 'MVC\\Reflex',
    'sFunction' => 'reflect',
]
~~~

## `stop` <a id="stop"></a>

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
![Debug::display()](/doc/2.x/debug/debug_stop.png)

_CLI_  
![Debug::display()](/doc/2.x/debug/debug_stop_cli.png)

---

## `varExport` <a id="varExport"></a>

~~~
Debug::varExport(mixed $mData, bool $bReturn = false, bool $bShortArraySyntax = true)
~~~

_Examples_ 

*equals `var_export()`, except that arrays are noted with square brackets `[ ]`*      
~~~php
Debug::varExport($GLOBALS['aConfig']);
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
