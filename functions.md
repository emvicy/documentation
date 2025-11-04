 
# functions

- [ct](#ct)
- [dct](#dct)
- [display](#display)
- [get](#get)
- [info](#info)
- [mvcStoreEnv](#mvcStoreEnv)
- [mvcConfigLoader](#mvcConfigLoader)
- [phpinfo_array](#phpinfo_array)
- [pr](#pr)
- [stop](#stop)
- [whereis](#whereis)

---

## `ct()` <a id="ct"></a>

returns time passed from start until calling this method.  
see [Debug::constructionTime()](/2.x/debug#constructionTime)

---

## `dct()` <a id="dct"></a>

displays the time passed from start until calling this method.

---

## `display()` <a id="display"></a>

shorthand for `Debug::display()` on userland.

see [Debug::display()](/2.x/debug#display)

---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## `get()` <a id="get"></a>

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

------------------------------------------------------------------------------------------------------------------------

## `info()` <a id="info"></a>

shorthand for `Debug::info()` on userland.

see [Debug::info()](/2.x/debug#info)


------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## `mvcStoreEnv()` <a id="mvcStoreEnv"></a>

reads environment key/values from a given file and stores them via putenv so that they will be accessible via getenv().

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## `mvcConfigLoader()` <a id="mvcConfigLoader"></a>

loads all available configs.

see also [Config files, -places and reading order](/2.x/configuration#Config-files-places-and-reading-order)

---

## `phpinfo_array` <a id="phpinfo_array"></a>

returns an assoc array based on phpinfo information.  
Alle keys are CASE_LOWER for easier access.

~~~
\phpinfo_array();
~~~

_Example_
~~~php
dump(
    phpinfo_array()
);
~~~
~~~
 array:69 [▼
  "general" => array:26 [▶]
  "apcu" => array:21 [▶]
  "bcmath" => array:2 [▶]
  "bz2" => array:4 [▶]
  "calendar" => array:1 [▶]
  "cgi-fcgi" => array:10 [▶]
  "core" => array:96 [▶]
  "ctype" => array:1 [▶]
  "curl" => array:38 [▶]
  "date" => array:10 [▶]
  "dom" => array:8 [▶]
  "exif" => array:11 [▶]
  "ffi" => array:3 [▶]
  "fileinfo" => array:2 [▶]
  "filter" => array:3 [▶]
  "ftp" => array:2 [▶]
  "gd" => array:17 [▶]
  "gettext" => array:1 [▶]
  "hash" => array:4 [▶]
  "iconv" => array:6 [▶]
  "igbinary" => array:5 [▶]
  "imagick" => array:14 [▶]
  "intl" => array:8 [▶]
  "json" => array:1 [▶]
  "ldap" => array:7 [▶]
  "libxml" => array:4 [▶]
  "mbstring" => array:17 [▶]
  "memcached" => array:37 [▶]
  "msgpack" => array:10 [▶]
  "mysqli" => array:16 [▶]
  "mysqlnd" => array:13 [▶]
  "openssl" => array:6 [▶]
  "pcre" => array:8 [▶]
  "pdo" => array:2 [▶]
  "pdo_mysql" => array:3 [▶]
  "pdo_pgsql" => array:2 [▶]
  "pdo_sqlite" => array:2 [▶]
  "pgsql" => array:11 [▶]
  "phar" => array:11 [▶]
  "posix" => array:1 [▶]
  "random" => array:1 [▶]
  "readline" => array:4 [▶]
  "redis" => array:38 [▶]
  "reflection" => array:1 [▶]
  "session" => array:33 [▶]
  "shmop" => array:1 [▶]
  "simplexml" => array:2 [▶]
  "soap" => array:7 [▶]
  "sockets" => array:1 [▶]
  "sodium" => array:3 [▶]
  "spl" => array:3 [▶]
  "sqlite3" => array:4 [▶]
  "standard" => array:16 [▶]
  "sysvmsg" => array:1 [▶]
  "sysvsem" => array:1 [▶]
  "sysvshm" => array:1 [▶]
  "tokenizer" => array:1 [▶]
  "uploadprogress" => array:4 [▶]
  "xml" => array:3 [▶]
  "xmlreader" => array:1 [▶]
  "xmlrpc" => array:5 [▶]
  "xmlwriter" => array:1 [▶]
  "xsl" => array:5 [▶]
  "yaml" => array:9 [▶]
  "zend opcache" => array:76 [▶]
  "zip" => array:9 [▶]
  "zlib" => array:8 [▶]
  "environment" => array:103 [▶]
  "php variables" => array:367 [▶]
]
~~~

_Example: requesting `xml` Infos_
~~~php
$sVersion = phpinfo_array()['xml']
~~~
~~~
 array:3 [▼
  "xml support" => "active"
  "xml namespace support" => "active"
  "libxml2 version" => "2.9.14"
]
~~~

## `pr()` <a id="pr"></a>

dumps data using `print_r`.

_Example_
~~~php
pr(get_include_path(), ':');
~~~

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## `stop()` <a id="stop"></a>

shorthand for `Debug::stop()` on userland.

see [Debug::stop()](/2.x/debug#info)

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

## `whereis()` <a id="whereis"></a>

locates source/binary for a specified file.

_Example_  
~~~php
$sGrep = whereis('grep');
~~~

_`$sGrep`_  
~~~
// type: string
'/usr/bin/grep'
~~~

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------
