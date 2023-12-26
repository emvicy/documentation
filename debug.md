
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
- File: /home/admin1/htdocs/_dev/Emvicy/Emvicy/Emvicy_1.x/modules/Foo/Controller/Index.php
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
   
---

<a id="prepareBacktraceArray"></a>
## `prepareBacktraceArray`

~~~
Debug::prepareBacktraceArray(array $aBacktrace = array()) : array
~~~