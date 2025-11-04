
# Convert

- [boolToString](#boolToString)
- [constValueToKey](#constValueToKey)
- [objectToArray](#objectToArray)
- [serialize](#serialize)
- [unserialize](#unserialize)

---

<a id="boolToString"></a>
## `boolToString`

converts `true` into `'true'` and `false` into `'false'`. Well of course it does not really convert the original value, but gives you a string representation of that value.

~~~
Convert::boolToString(bool $bValue) : string
~~~

~~~php
$sBool = Convert::boolToString(
    true
);

// type: string
// 'true'
var_dump(
    $aObject
);
~~~

---

<a id="constValueToKey"></a>
## `constValueToKey`

returns constant name on its integer value - works for `php` constants.

~~~
Convert::constValueToKey(int $iValue, array $aConstant = array()) : string
~~~

_Example_
~~~php
$sLevel = Convert::constValueToKey(1024); # E_USER_NOTICE
~~~

_Result_
~~~
// type: string
'E_USER_NOTICE'
~~~

---

<a id="objectToArray"></a>
## `objectToArray`

converts any object into array.

~~~
Convert::objectToArray(mixed $mObject) : mixed
~~~

_Example_  
~~~php
$aObject = Convert::objectToArray(
    $oObject
);

// array
var_dump(
    gettype($aObject)
);
~~~

---

<a id="serialize"></a>
## `serialize`

allows serialization of objects, too.  
It wraps `\Opis\Closure\serialize()` function, makes it systemwide usable (see <a href="https://opis.io/closure" target="_blank">https://opis.io/closure</a>).

~~~
Convert::serialize(mixed $mValue) : string
~~~

_Example_  
~~~php
/** \MVC\DataType\DTRoute $oDTRoute */
$sSerialized = Convert::serialize($oDTRoute)
~~~
_Content of `$sSerialized`_    
~~~
O:16:"Opis\\Closure\\Box":2:{i:0;i:3;i:1;a:2:{i:0;s:20:"MVC\\DataType\\DTRoute";i:1;a:11:{s:4:"path";s:1:"/";s:13:"requestMethod";s:3:"GET";s:15:"methodsAssigned";a:1:{i:0;s:3:"GET";}s:5:"query";s:28:"\\Foo\\Controller\\Index::index";s:6:"module";s:3:"Foo";s:5:"class";s:21:"\\Foo\\Controller\\Index";s:9:"classFile";s:46:"/var/www/html/modules/Foo/Controller/Index.php";s:6:"method";s:5:"index";s:10:"additional";O:16:"Opis\\Closure\\Box":2:{i:0;i:3;i:1;a:2:{i:0;s:32:"Foo\\DataType\\DTRoutingAdditional";i:1;a:6:{s:6:"sTitle";s:4:"Home";s:9:"sTemplate";s:26:"Frontend/content/index.tpl";s:8:"sContent";s:0:"";s:6:"aStyle";a:5:{i:0;s:57:"/Emvicy/assets/bootstrap-5.3.3-dist/css/bootstrap.min.css";i:1;s:57:"/Emvicy/assets/fontawesome-free-6.7.2-web/css/all.min.css";i:2;s:29:"/Emvicy/styles/Emvicy.min.css";i:3;s:30:"/Ws_old/assets/pnotify.min.css";i:4;s:42:"/Ws_old/assets/pnotify.brighttheme.min.css";}s:7:"aScript";a:9:{i:0;s:47:"/Emvicy/assets/jquery-3.7.1/jquery-3.7.1.min.js";i:1;s:55:"/Emvicy/assets/jquery-cookie-1.4.1/jquery.cookie.min.js";i:2;s:43:"/Emvicy/assets/popper-v2.11.8/popper.min.js";i:3;s:55:"/Emvicy/assets/bootstrap-5.3.3-dist/js/bootstrap.min.js";i:4;s:36:"/Emvicy/scripts/cookieConsent.min.js";i:5;s:29:"/Ws_old/assets/pnotify.min.js";i:6;s:37:"/Ws_old/assets/pnotify.desktop.min.js";i:7;s:30:"/Ws_old/scripts/pnotify.min.js";i:8;s:34:"/Ws/scripts/wss.domain.port.min.js";}s:3:"' . "\0" . '?' . "\0" . '";N;}}}s:3:"tag";s:4:"home";s:3:"' . "\0" . '?' . "\0" . '";N;}}}
~~~

---

<a id="unserialize"></a>
## `unserialize`

It wraps `\Opis\Closure\unserialize()` function, makes it systemwide usable (see <a href="https://opis.io/closure" target="_blank">https://opis.io/closure</a>).

~~~
Convert::unserialize(string $mValue)
~~~

_Example_
~~~php
$mUnserialized = Convert::unserialize(
    'O:16:"Opis\\Closure\\Box":2:{i:0;i:3;i:1;a:2:{i:0;s:20:"MVC\\DataType\\DTRoute";i:1;a:11:{s:4:"path";s:1:"/";s:13:"requestMethod";s:3:"GET";s:15:"methodsAssigned";a:1:{i:0;s:3:"GET";}s:5:"query";s:28:"\\Foo\\Controller\\Index::index";s:6:"module";s:3:"Foo";s:5:"class";s:21:"\\Foo\\Controller\\Index";s:9:"classFile";s:46:"/var/www/html/modules/Foo/Controller/Index.php";s:6:"method";s:5:"index";s:10:"additional";O:16:"Opis\\Closure\\Box":2:{i:0;i:3;i:1;a:2:{i:0;s:32:"Foo\\DataType\\DTRoutingAdditional";i:1;a:6:{s:6:"sTitle";s:4:"Home";s:9:"sTemplate";s:26:"Frontend/content/index.tpl";s:8:"sContent";s:0:"";s:6:"aStyle";a:5:{i:0;s:57:"/Emvicy/assets/bootstrap-5.3.3-dist/css/bootstrap.min.css";i:1;s:57:"/Emvicy/assets/fontawesome-free-6.7.2-web/css/all.min.css";i:2;s:29:"/Emvicy/styles/Emvicy.min.css";i:3;s:30:"/Ws_old/assets/pnotify.min.css";i:4;s:42:"/Ws_old/assets/pnotify.brighttheme.min.css";}s:7:"aScript";a:9:{i:0;s:47:"/Emvicy/assets/jquery-3.7.1/jquery-3.7.1.min.js";i:1;s:55:"/Emvicy/assets/jquery-cookie-1.4.1/jquery.cookie.min.js";i:2;s:43:"/Emvicy/assets/popper-v2.11.8/popper.min.js";i:3;s:55:"/Emvicy/assets/bootstrap-5.3.3-dist/js/bootstrap.min.js";i:4;s:36:"/Emvicy/scripts/cookieConsent.min.js";i:5;s:29:"/Ws_old/assets/pnotify.min.js";i:6;s:37:"/Ws_old/assets/pnotify.desktop.min.js";i:7;s:30:"/Ws_old/scripts/pnotify.min.js";i:8;s:34:"/Ws/scripts/wss.domain.port.min.js";}s:3:"' . "\0" . '?' . "\0" . '";N;}}}s:3:"tag";s:4:"home";s:3:"' . "\0" . '?' . "\0" . '";N;}}}'
)
~~~
_Content of `$mUnserialized`_
~~~
// type: object
\MVC\DataType\DTRoute::__set_state(array(
      'path' => '/',
      'requestMethod' => 'GET',
      'methodsAssigned' =>    array (
        0 => 'GET',
    ),
      'query' => '\\Foo\\Controller\\Index::index',
      'module' => 'Foo',
      'class' => '\\Foo\\Controller\\Index',
      'classFile' => '/var/www/html/modules/Foo/Controller/Index.php',
      'method' => 'index',
      'additional' =>    \Foo\DataType\DTRoutingAdditional::__set_state(array(
          'sTitle' => 'Home',
          'sTemplate' => 'Frontend/content/index.tpl',
          'sContent' => '',
          'aStyle' =>        array (
            0 => '/Emvicy/assets/bootstrap-5.3.3-dist/css/bootstrap.min.css',
            1 => '/Emvicy/assets/fontawesome-free-6.7.2-web/css/all.min.css',
            2 => '/Emvicy/styles/Emvicy.min.css',
            3 => '/Ws_old/assets/pnotify.min.css',
            4 => '/Ws_old/assets/pnotify.brighttheme.min.css',
        ),
          'aScript' =>        array (
            0 => '/Emvicy/assets/jquery-3.7.1/jquery-3.7.1.min.js',
            1 => '/Emvicy/assets/jquery-cookie-1.4.1/jquery.cookie.min.js',
            2 => '/Emvicy/assets/popper-v2.11.8/popper.min.js',
            3 => '/Emvicy/assets/bootstrap-5.3.3-dist/js/bootstrap.min.js',
            4 => '/Emvicy/scripts/cookieConsent.min.js',
            5 => '/Ws_old/assets/pnotify.min.js',
            6 => '/Ws_old/assets/pnotify.desktop.min.js',
            7 => '/Ws_old/scripts/pnotify.min.js',
            8 => '/Ws/scripts/wss.domain.port.min.js',
        ),
    )),
      'tag' => 'home',
))
~~~