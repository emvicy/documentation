
# Request

- [`in()` - get Information about incoming Request](#Request-in)
  - [Check request method against route method](#check-request-method-against-route-method)
  - [Get data from header of current Request](#Get-data-from-header-of-current-Request)
    - [Get all headers](#Get-all-headers)
    - [Get a certain header](#Get-a-certain-header)
  - [Get data from body of current Request](#Get-data-from-body-of-current-Request)
  - [Accessing Path Params / Variables](#Accessing-Path-Params-Variables)
  - [Get Path Info](#get-path-info)
  - [Sanitizing](#Sanitizing)   
- [`out()` - perform an outgoing Request](#perform-an-outgoing-Request)


------------------------------------------------------------------------------------------------------------------------

<a id="Request-in"></a>
## `in()` - get Information about incoming Request

_Example **GET** Request_
~~~
http://mymvc.ueffing.local/foo/bar/?a=1;b=2;c=3
~~~

_`in()` returns `DTRequestIn` object representing the incoming Request_  
~~~php
$oDTRequestIn = \MVC\Request::in();
~~~
- see [/2.x/datatype-classes#DTRequestIn](/2.x/datatype-classes#DTRequestIn)

As it gives you an object of type `MVC\DataType\DTRequestIn`, you can access all key/values by getter and setter.

_For example_  
~~~php
$sPath = \MVC\Request::in()->get_path();
$sQuery = \MVC\Request::in()->get_query();
~~~

<a id="check-request-method-against-route-method"></a>
**Check request method against route method**

Check if request method equals the expecting one you declared in your route.

_Check_    
~~~php 
$bMethodMatch = (
    // any request method is allowed
    '*' === \MVC\Route::getCurrent()->get_method() ||
    // request method does match route method
    \MVC\Request::in()->get_requestMethod() === \MVC\Route::getCurrent()->get_method()
) ? true : false;
~~~

_Evaluate and React_  
~~~php
if (false === $bMethodMatch)
{
    die('wrong request method `' 
        . \MVC\Request::getServerRequestMethod() 
        . '`. It has to be one of: `' 
        . implode('|', Route::getCurrent()->get_methodsAssigned()) . '`'
    );
}
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Get-data-from-header-of-current-Request"></a>
## Get data from header of current Request

<a id="Get-all-headers"></a>
**Get all headers** 

_Command_
~~~php
$aHeader = \MVC\Request::in()->get_headerArray();
~~~

_Example Result of `$aHeader`_  
~~~
// type: array, items: 22
[
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
]
~~~

<a id="Get-a-certain-header"></a>
**Get a certain header**

_Command_
~~~php
$aHeader = \MVC\Request::in()->getHeaderValueOnKey('User-Agent');
~~~

_Example Result_
~~~
string(10) "Mozilla/5.0 (X11; Linux x86_64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/133.0.0.0 Safari/537.36"
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Get-data-from-body-of-current-Request"></a>
## Get data from body of current Request

_Example **PUT** Request_
~~~bash
curl -X PUT http://mymvc.ueffing.local/api/1.0.0/user/1969/ -H "Content-Type: application/json" -d '{"key": "value"}'
~~~

_Command_
~~~php
$sInput = \MVC\Request::in()->get_input();
~~~

_Example Result_
~~~
{"key": "value"}
~~~
- As you can see here, `input` contains the values we PUT (`{"key": "value"}`)

------------------------------------------------------------------------------------------------------------------------

<a id="Accessing-Path-Params-Variables"></a>
## Accessing Path Params / Variables

_Example route_
~~~php
\MVC\Route::get('/api/:id/:name/:address/*', '\Foo\Controller\Api::index');
~~~
- _for more Information about setting up such routes, see [Routing with Path Params / Variables](/2.x/routing#path-params)_

_Example Request_
- `/api/1/Foo/Bar/what/else/`

<a id="Get-all-Variables"></a>
**Get all Variables**

_Command_
~~~php
$aPathParam = \MVC\Request::in()->get_pathParamArray();
~~~

_Example Result of `$aPathParam`_
~~~
// type: array, items: 4
[
    'id' => '1',
    'name' => 'Foo',
    'address' => 'Bar',
    '_tail' => 'what/else/',
]
~~~

<a id="Get-a-certain-Variable"></a>
**Get a certain Variable**

_Command_
~~~php
$aPathParam = \MVC\Request::in()->get_pathParamArray()['name']
~~~

_Example Result of `$sPathParam`_
~~~
Foo
~~~

<a id="Get-the-overlapping-string-on-wildcard-route-paths"></a>
**Get the overlapping string on wildcard route paths**

say you have a wildcard route and you want to get the overlapping path string after `*`.

_Example route_
~~~php
\MVC\Route::get('/foo/*', '\Foo\Controller\Index::foo');
~~~
- _see [Wildcard routing](/2.x/routing#wildcard-routing)_

_Example Request_
- `/foo/bar/baz/`

_Command_
~~~php
$sTail = \MVC\Request::in()->get_pathParamArray()['_tail'];
~~~

_Result of `$sTail`_
~~~
bar/baz/
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="get-path-info"></a>
## Get Path Info

<a id="Get-path-as-array"></a>
**Get requested path as array**

say the incoming Request is `https://emvicy2x.ddev.site/imprint/foo/bar/baz`

~~~php
$aPath = Request::in()->get_pathArray();
~~~

_Result of `$aPath`_
~~~
// type: array, items: 4
[
    0 => 'imprint',
    1 => 'foo',
    2 => 'bar',
    3 => 'baz',
]
~~~

**Enquiry with any url**

~~~php
$aPath = RequestHelper::getPathArrayOnUrl('https://www.example.com/Imprint/')
~~~

_Result of `$aPath`_
~~~
// type: array, items: 1
[
    0 => 'Imprint',
]
~~~
------------------------------------------------------------------------------------------------------------------------

<a id="Sanitizing"></a>
## Sanitizing

_sanitizing input (e.g. `PUT`)_  
~~~php 
$oDTRequestIn = \MVC\Request::in();

// sanitizing
$oDTRequestIn->set_input(
    preg_replace(
        // sanitizing by regex rule
        "/[^\\p{L}\\p{M}\\p{Z}\\p{S}\\p{N}\\p{P}\|']+/u",
        '',
        // sanitizing by string length
        substr($oDTRequestIn->get_input(), 0, 256)
    )
);

// sanitized
$sInput = $oDTRequestIn->get_input();  
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="perform-an-outgoing-Request"></a>
## `out()` - perform an outgoing Request

_perform a local `GET` request on `/api/`_
~~~php
/** @var \MVC\DataType\DTResponse $oDTResponse */
$oDTResponse = Request::out(
    DTRequestOut::create()
        ->set_eRequestMethod(EnumRequestMethod::GET)
        ->set_sUrl('/api/')
);
~~~

_Example Response `\MVC\DataType\DTResponse $oDTResponse`_
~~~
\MVC\DataType\DTResponse::__set_state(array(
      'body' => '{"requestMethod":"GET", ...',
      'raw' => 'HTTP/1.1 200 OK
          Content-Security-Policy: default-src \'self\'; ...
          Content-Type: application/json
          Date: Thu, 06 Feb 2025 13:17:44 GMT
          Server: Apache/2.4.62 (Debian)
          Strict-Transport-Security: max-age=63072000
          X-Content-Security-Policy: default-src \'self\'; ...
          X-Frame-Options: allow-from \'none\'
          X-Webkit-Csp: default-src \'self\'; ...
          X-Xss-Protection: 1; mode=block
          Connection: close
          Transfer-Encoding: chunked
          
          {"requestMethod":"GET", ...',
      'headers' =>    array (
        'content-security-policy' => 'default-src \'self\'; ...',
        'content-type' => 'application/json',
        'date' => 'Thu, 06 Feb 2025 13:17:44 GMT',
        'server' => 'Apache/2.4.62 (Debian)',
        'strict-transport-security' => 'max-age=63072000',
        'x-content-security-policy' => 'default-src \'self\'; ...',
        'x-frame-options' => 'allow-from \'none\'',
        'x-webkit-csp' => 'default-src \'self\'; ...',
        'x-xss-protection' => '1; mode=block',
        'connection' => 'close',
        'transfer-encoding' => 'chunked',
    ),
      'status_code' => 200,
      'protocol_version' => 1.1,
      'success' => true,
      'redirects' => 0,
      'url' => 'https://emvicy2x.ddev.site/api/',
      'history' =>    array (
    ),
      'cookies' =>    array (
        'cookies' =>        array (
        ),
    ),
))
~~~

_perform a remote GET request_
~~~php
/** @var \MVC\DataType\DTResponse $oDTResponse */
$oDTResponse = Request::out(
    DTRequestOut::create()
        ->set_eRequestMethod(EnumRequestMethod::GET)
        ->set_sUrl('https://api.ddev.site/table/address/2/')
        ->set_aHeader(array(
            'accept' => Type_Application_json::DESCRIPTION,
            'apikey' => getenv('apikey')
        ))
);
~~~