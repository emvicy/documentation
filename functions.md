 
# functions

## `get()`

simplifies the use of variables. If a variable does not exist, null or a defined value is returned.

_usually_  
~~~
$mValue = (isset($aData['foo']['bar'])) ? $aData['foo']['bar'] : null;
// or
$mValue = ($aData['foo']['bar'] ?? null);
~~~

_way with get()_  
~~~php
$mValue = get($aData['foo']['bar']);
~~~

---

## `display()`

shorthand for `Debug::display()` on userland.

see [Debug::display()](/1.x/debug#display)

---

## `info()`

shorthand for `Debug::info()` on userland.

see [Debug::info()](/1.x/debug#info)


---

## `stop()`

shorthand for `Debug::stop()` on userland.

see [Debug::stop()](/1.x/debug#info)

---

## `whereis()`

locates source/binary for a specified file.

_Example_  
~~~php
$sLogId = '202401011415586592bb0e3eb50';
$sCmd = "cd " . Config::get_MVC_LOG_FILE_DIR() . "; " . whereis('grep') .  " " . $sLogId . " *.log ";
Emvicy::shellExecute($sCmd, true);
~~~


---

## `pr()`

dumps data using `print_r`.

_Example_  
~~~php
pr(get_include_path(), ':');
~~~

---

## `mvcStoreEnv()`

reads environment key/values from a given file and stores them via putenv so that they will be accessible via getenv().

---

## `mvcConfigLoader()`

loads all available configs. 

see also [Config files, -places and reading order](/1.x/configuration#Config-files-places-and-reading-order)
