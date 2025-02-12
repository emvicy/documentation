
# DataType Classes

- [`DTArrayObject`](#DTArrayObject)
- [`DTKeyValue`](#DTKeyValue)
- [`DTClass`](#DTClass)
- [`DTConfig`](#DTConfig)
- [`DTConstant`](#DTConstant)
- [`DTCronTask`](#DTCronTask)
- [`DTDBOption`](#DTDBOption)
- [`DTDBSet`](#DTDBSet)
- [`DTDBWhere`](#DTDBWhere)
- [`DTDBWhereRelation`](#DTDBWhereRelation)
- [`DTEventContext`](#DTEventContext)
- [`DTFileinfo`](#DTFileinfo)
- [`DTFileUpload`](#DTFileUpload)
- [`DTProperty`](#DTProperty)
- [`DTRequestIn`](#DTRequestIn)
- [`DTRequestOut`](#DTRequestOut)
- [`DTResponse`](#DTResponse)
- [`DTRoute`](#DTRoute)
- [`DTRoutingAdditional`](#DTRoutingAdditional)

all these classes resides in `\MVC\DataType\`

------------------------------------------------------------------------------------------------------------------------

<a id="DTArrayObject"></a>
## `DTArrayObject`

~~~bash
 MVC\DataType\DTArrayObject {#91 ▼
  #aKeyValue: []
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="DTKeyValue"></a>
## `DTKeyValue`

~~~bash
 MVC\DataType\DTKeyValue {#91 ▼
  #sKey: ""
  #iIndex: null
  #sValue: null
  #mOptional1: null
  #mOptional2: null
  #mOptional3: null
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="DTClass"></a>
## `DTClass`

~~~bash
 MVC\DataType\DTClass {#91 ▼
  #name: ""
  #file: ""
  #extends: ""
  #namespace: ""
  #trait: []
  #constant: []
  #property: []
  #createHelperMethods: true
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="DTConfig"></a>
## `DTConfig`

~~~bash
 MVC\DataType\DTConfig {#91 ▼
  #dir: ""
  #unlinkDir: false
  #class: []
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="DTConstant"></a>
## `DTConstant`

~~~bash
 MVC\DataType\DTConstant {#91 ▼
  #key: ""
  #value: null
  #visibility: ""
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="DTCronTask"></a>
## `DTCronTask`

~~~bash
 MVC\DataType\DTCronTask {#91 ▼
  #sRoute: ""
  #iIntervall: 60
  #sStaging: ""
  #sCommand: ""
  #iPid: 0
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="DTDBOption"></a>
## `DTDBOption`

~~~bash
 MVC\DataType\DTDBOption {#91 ▼
  #sValue: ""
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="DTDBSet"></a>
## `DTDBSet`

~~~bash
 MVC\DataType\DTDBSet {#91 ▼
  #sKey: ""
  #sValue: null
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="DTDBWhere"></a>
## `DTDBWhere`

~~~bash
 MVC\DataType\DTDBWhere {#91 ▼
  #sKey: ""
  #sRelation: "="
  #sValue: ""
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="DTDBWhereRelation"></a>
## `DTDBWhereRelation`

~~~bash
 MVC\DataType\DTDBWhereRelation {#91}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="DTEventContext"></a>
## `DTEventContext`

- This Class provides various information about the context of an event.
- An object of this class is passed to executed Closures in `Event::bind()`

~~~bash
MVC\DataType\DTEventContext {#91 ▼
  #sEvent: ""
  #sEventOrigin: ""
  #mRunPackage: ""
  #aBonded: []
  #sBondedBy: ""
  #sCalledIn: ""
  #oCallback: null
  #sCallbackDumped: ""
  #sMessage: ""
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="DTFileinfo"></a>
## `DTFileinfo`

- This Class is used to store File Information.
- An object of this class gets being returned by [File::info()](/2.x/file#info)

~~~php
/** @var \MVC\DataType\DTFileinfo $oDTFileinfo */
$oDTFileinfo = \MVC\File::info(__FILE__);
~~~

~~~bash
 MVC\DataType\DTFileinfo {#91 ▼
  #dirname: ""
  #basename: ""
  #path: ""
  #is_file: false
  #is_dir: false
  #extension: ""
  #filename: ""
  #name: ""
  #passwd: ""
  #uid: 0
  #gid: 0
  #filemtime: 0
  #filectime: 0
  #filesize: 0
  #gecos: ""
  #dir: ""
  #shell: ""
  #mimetype: ""
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="DTFileUpload"></a>
## `DTFileUpload`

~~~bash
 MVC\DataType\DTFileUpload {#91 ▼
  #name: []
  #full_path: []
  #type: []
  #tmp_name: []
  #error: []
  #size: []
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="DTProperty"></a>
## `DTProperty`

~~~bash
 MVC\DataType\DTProperty {#91 ▼
  #key: ""
  #var: "string"
  #value: null
  #visibility: "protected"
  #static: false
  #setter: true
  #getter: true
  #explicitMethodForValue: false
  #listProperty: true
  #createStaticPropertyGetter: true
  #setValueInConstructor: true
  #forceCasting: false
  #required: false
  #addMyMVCEvents: true
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="DTRequestIn"></a>
## `DTRequestIn`

- This Class is used to store the current incoming Request Informations.
- An object of this class gets being returned by `Request::in()` (see [/2.x/request#Request-in](/2.x/request#Request-in))

~~~php
/** @var \MVC\DataType\DTRequestIn $oDTRequestIn */
$oDTRequestIn = \MVC\Request::in();
~~~

_`$oDTRequestIn`_  
~~~bash
 MVC\DataType\DTRequestIn {#91 ▼
  #requestMethod: ""
  #full: ""
  #protocol: ""
  #scheme: ""
  #requestUri: ""
  #path: ""
  #host: ""
  #pathArray: []
  #pathParamArray: []
  #query: ""
  #queryArray: []
  #headerArray: []
  #input: ""
  #ip: ""
  #cookieArray: []
  #isSecure: false
  #isCli: false
  #isHttp: false
}
~~~

_Example_
~~~
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

------------------------------------------------------------------------------------------------------------------------

<a id="DTRequestOut"></a>
## `DTRequestOut`

~~~bash
 MVC\DataType\DTRequestOut {#91 ▼
  #eRequestMethod: null
  #sUrl: ""
  #aHeader: []
  #aData: []
  #aOption: []
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="DTResponse"></a>
## `DTResponse`

~~~bash
 MVC\DataType\DTResponse {#91 ▼
  #body: ""
  #raw: ""
  #headers: []
  #status_code: 0
  #protocol_version: 0
  #success: false
  #redirects: 0
  #url: ""
  #history: []
  #cookies: []
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="DTRoute"></a>
## `DTRoute`

- This Class is used to store Route Information.
- An object of this class gets being returned by `Route::getCurrent()` (see [/2.x/routing#Get-current-route](/2.x/routing#Get-current-route))

~~~php
/** @var \MVC\DataType\DTRoute $oDTRoute */
$oDTRoute = \MVC\Route::getCurrent();
~~~

_`$oDTRoute`_  
~~~bash
 MVC\DataType\DTRoute {#91 ▼
  #path: ""
  #requestMethod: ""
  #methodsAssigned: []
  #query: ""
  #module: ""
  #class: ""
  #classFile: ""
  #method: ""
  #additional: null
  #tag: ""
}
~~~

_Example_  
~~~
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

------------------------------------------------------------------------------------------------------------------------

<a id="DTRoutingAdditional"></a>
## `DTRoutingAdditional`

~~~bash
 MVC\DataType\DTRoutingAdditional {#91 ▼
  #sTitle: ""
  #sTemplate: ""
  #sContent: ""
  #aStyle: []
  #aScript: []
}
~~~

_Example_  
~~~
\Foo\DataType\DTRoutingAdditional::__set_state(array(
      'sTitle' => 'Home',
      'sTemplate' => 'Frontend/content/index.tpl',
      'sContent' => '',
      'aStyle' =>    array (
        0 => '/Emvicy/assets/bootstrap-5.3.3-dist/css/bootstrap.min.css',
        1 => '/Emvicy/assets/fontawesome-free-6.7.2-web/css/all.min.css',
        2 => '/Emvicy/styles/Emvicy.min.css',
        3 => '/Ws_old/assets/pnotify.min.css',
        4 => '/Ws_old/assets/pnotify.brighttheme.min.css',
    ),
      'aScript' =>    array (
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
))
~~~