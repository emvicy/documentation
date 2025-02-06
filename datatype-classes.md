
# DataType Classes

- [`DTRoute`](#DTRoute)
- [`DTRequestIn`](#DTRequestIn)
- [`DTFileinfo`](#DTFileinfo)
- [`DTEventContext`](#DTEventContext)

---

<a id="DTRoute"></a>
## `DTRoute`

- This Class is used to store Route Information.
- An object of this class gets being returned by `Route::getCurrent()` (see [/2.x/routing#Get-current-route](/2.x/routing#Get-current-route))

~~~php
/** @var \MVC\DataType\DTRoute $oDTRoute */
$oDTRoute = \MVC\Route::getCurrent();
~~~
~~~
object(MVC\DataType\DTRoute)#15 (9) {
    ["path":protected]=>string(1) "/"
    ["method":protected]=>string(3) "GET"
    ["methodsAssigned":protected]=>array(1) {[0]=> string(3) "GET"}
    ["query":protected]=>string(30) "module=Foo&c=Index&m=index"
    ["class":protected]=>string(24) "Foo\Controller\Index"
    ["classFile":protected]=>string(128) "/var/www/Emvicy/modules/Foo/Controller/Index.php"
    ["module":protected]=>string(7) "Foo"
    ["c":protected]=>string(5) "Index"
    ["m":protected]=>string(5) "index"
    ["additional":protected]=>string(0) ""
}
~~~

<a id="DTRequestIn"></a>
## `DTRequestIn`

- This Class is used to store the current incoming Request Informations.
- An object of this class gets being returned by `Request::in()` (see [/2.x/request#Request-in](/2.x/request#Request-in))

~~~php
/** @var \MVC\DataType\DTRequestIn $oDTRequestIn */
$oDTRequestIn = \MVC\Request::in();
~~~

_Example_  
~~~
// type: object
\MVC\DataType\DTRequestIn::__set_state(array(
      'requestMethod' => 'GET',
      'full' => 'https://emvicy2x.ddev.site/imprint/foo/bar/baz?a=b',
      'protocol' => 'https://',
      'scheme' => 'https',
      'requestUri' => '/imprint/foo/bar/baz?a=b',
      'path' => '/imprint/foo/bar/baz',
      'host' => 'emvicy2x.ddev.site',
      'pathArray' =>    array (
        0 => 'imprint',
        1 => 'foo',
        2 => 'bar',
        3 => 'baz',
    ),
      'pathParamArray' =>    array (
        '_tail' => 'foo/bar/baz',
    ),
      'query' => 'a=b',
      'queryArray' =>    array (
        'a' => 'b',
    ),
      'headerArray' =>    array (
        'X-Real-Ip' => '172.21.0.1',
        'X-Forwarded-Server' => '0d23c0701c03',
        'X-Forwarded-Proto' => 'https',
        'X-Forwarded-Port' => '443',
        'X-Forwarded-Host' => 'emvicy2x.ddev.site',
        'X-Forwarded-For' => '172.21.0.1',
        'Upgrade-Insecure-Requests' => '1',
        'Sec-Fetch-User' => '?1',
        'Sec-Fetch-Site' => 'none',
        'Sec-Fetch-Mode' => 'navigate',
        'Sec-Fetch-Dest' => 'document',
        'Sec-Ch-Ua-Platform' => '"Linux"',
        'Sec-Ch-Ua-Mobile' => '?0',
        'Sec-Ch-Ua' => '"Not(A:Brand";v="99", "Google Chrome";v="133", "Chromium";v="133"',
        'Priority' => 'u=0, i',
        'Cookie' => 'Emvicy_cookieConsent=true; Emvicy_secure=7t9i925cp5bendbl939ct4h6ug',
        'Cache-Control' => 'max-age=0',
        'Accept-Language' => 'de-DE,de;q=0.9,en-US;q=0.8,en;q=0.7',
        'Accept-Encoding' => 'gzip, deflate, br, zstd',
        'Accept' => 'text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8,application/signed-exchange;v=b3;q=0.7',
        'User-Agent' => 'Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/133.0.0.0 Safari/537.36',
        'Host' => 'emvicy2x.ddev.site',
    ),
      'input' => '',
      'ip' => '172.21.0.1',
      'cookieArray' =>    array (
        'Emvicy_cookieConsent' => 'true',
        'Emvicy_secure' => '7t9i925cp5bendbl939ct4h6ug',
    ),
      'isSecure' => true,
      'isCli' => false,
      'isHttp' => true,
))
~~~

<a id="DTFileinfo"></a>
## `DTFileinfo`

- This Class is used to store File Information.
- An object of this class gets being returned by [File::info()](/2.x/file#info)

~~~php
/** @var \MVC\DataType\DTFileinfo $oDTFileinfo */
$oDTFileinfo = \MVC\File::info(__FILE__);
~~~
~~~
object(MVC\DataType\DTFileinfo)#84 (16) {
  ["dirname":protected]=>string(151) "/var/www/Emvicy/modules/Doc/Controller"
  ["basename":protected]=>string(9) "Index.php"
  ["path":protected]=>string(161) "/var/www/Emvicy/modules/Doc/Controller/Index.php"
  ["is_file":protected]=>bool(true)
  ["is_dir":protected]=>bool(false)
  ["extension":protected]=>string(3) "php"
  ["filename":protected]=>string(5) "Index"
  ["name":protected]=>string(6) "admin1"
  ["passwd":protected]=>string(1) "x"
  ["uid":protected]=>int(1000)
  ["gid":protected]=>int(1000)
  ["gecos":protected]=>string(9) "admin1,,,"
  ["dir":protected]=>string(12) "/var/www"
  ["shell":protected]=>string(9) "/bin/bash"
  ["filemtime":protected]=>int(1666350030)
  ["filectime":protected]=>int(1666350030)
  ["mimetype":protected]=>string(10) "text/x-php"
}
~~~

<a id="DTEventContext"></a>
## `DTEventContext`

- This Class provides various information about the context of an event.
- An object of this class is passed to executed Closures in `Event::bind()`

~~~
object(MVC\DataType\DTEventContext)#78 (9) {
  ["sEvent":protected]=>string(0) ""
  ["sEventOrigin":protected]=> string(0) ""
  ["mRunPackage":protected]=> string(0) ""
  ["aBonded":protected]=> array(0) {}
  ["sBondedBy":protected]=>string(0) ""
  ["sCalledIn":protected]=>string(0) ""
  ["oCallback":protected]=>NULL
  ["sCallbackDumped":protected]=>string(0) ""
  ["sMessage":protected]=>string(0) ""
}
~~~

---

## `DTArrayObject`
## `DTKeyValue`
## `DTClass`
## `DTConfig`
## `DTConstant`
## `DTProperty`



