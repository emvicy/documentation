
# Header

This class offers a range of ready-made headers.

- [Instantiation](#Instantiation)
- [`Content_Type`](#Content_Type)
- [`Content_Length`](#Content_Length)
- [`Content_Disposition_Attachment`](#Content_Disposition_Attachment)
- [`Content_Type_application_download`](#Content_Type_application_download)
- [`Content_Type_application_force_download`](#Content_Type_application_force_download)
- [`Content_Type_application_octet_stream`](#Content_Type_application_octet_stream)
- [`Content_Description_File_Transfer`](#Content_Description_File_Transfer)
- [`Access_Control_Allow_Origin`](#Access_Control_Allow_Origin)
- [`Cache_Control`](#Cache_Control)
- [`Location`](#Location)
- [`Expires`](#Expires)
- [`Etag`](#Etag)
- [`Last_Modified`](#Last_Modified)
- [`Set_Cookie`](#Set_Cookie)
- [`Refresh`](#Refresh)
- [`WWW_Authenticate`](#WWW_Authenticate)
- [`Retry_After`](#Retry_After)
- [`ContentSecurityPolicy`](#ContentSecurityPolicy)
- [`X_Accel_Buffering`](#X_Accel_Buffering)
- [Examples](#Examples)
  - [Provide a file for download](#download)
  - [HTTP Authentication example](#HTTP-Authentication-example)

------------------------------------------------------------------------------------------------------------------------

<a id="Instantiation"></a>
## Instantiation

~~~php
Header::init();
~~~

almost all methods are fluent, which allows you to combine the methods; see [Examples](#Examples)

------------------------------------------------------------------------------------------------------------------------

<a id="Content_Type"></a>
## `Content_Type`

~~~
public function Content_Type(string $sType = '') : Header
~~~

~~~php
Header::init()->Content_Type(\MVC\Media\Type_Application_json::DESCRIPTION);
~~~

alternatively, you can also send the Content_Type header via a media type class `\MVC\Media\Type*`

~~~php
\MVC\Media\Type_Application_json::header();
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Content_Length"></a>
## `Content_Length`

~~~
public function Content_Length(int $iFilesize = 0) : Header
~~~

~~~php
Header::init()->Content_Length(12345);
~~~


------------------------------------------------------------------------------------------------------------------------

<a id="Content_Disposition_Attachment"></a>
## `Content_Disposition_Attachment`

~~~
public function Content_Disposition_Attachment(string $sFilename = '') : Header
~~~

~~~php
Header::init()->Content_Disposition_Attachment('Document.md');
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Content_Type_application_download"></a>
## `Content_Type_application_download`

~~~
public function Content_Type_application_download() : Header
~~~

~~~php
Header::init()->Content_Type_application_download();
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Content_Type_application_force_download"></a>
## `Content_Type_application_force_download`

~~~
public function Content_Type_application_force_download() : Header
~~~

~~~php
Header::init()->Content_Type_application_force_download();
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Content_Type_application_octet_stream"></a>
## `Content_Type_application_octet_stream`

~~~
public function Content_Type_application_octet_stream() : Header
~~~

~~~php
Header::init()->Content_Type_application_octet_stream();
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Content_Description_File_Transfer"></a>
## `Content_Description_File_Transfer`

~~~
public function Content_Description_File_Transfer() : Header
~~~

~~~php
Header::init()->Content_Description_File_Transfer();
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Access_Control_Allow_Origin"></a>
## `Access_Control_Allow_Origin`

~~~
public function Access_Control_Allow_Origin(string $sOrigin = '*') : Header
~~~

~~~php
Header::init()->Access_Control_Allow_Origin(null);
~~~

see <a href="https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Origin" target="_blank">
    developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Access-Control-Allow-Origin
</a>

------------------------------------------------------------------------------------------------------------------------

<a id="Cache_Control"></a>
## `Cache_Control`

~~~
public function Cache_Control(EnumHttpHeaderCacheControl | array $mEnumCacheControl) : Header
~~~

~~~php
Header::init()->Cache_Control(\MVC\Enum\EnumCacheControl::NoCache);
~~~
~~~php
Header::init()->Cache_Control(array(
    \MVC\Enum\EnumCacheControl::NoCache, 
    \MVC\Enum\EnumCacheControl::MustRevalidate)
);
~~~


------------------------------------------------------------------------------------------------------------------------

<a id="Location"></a>
## `Location`

~~~
public function Location(string $sLocation = '', bool $bReplace = true, int $iResponseCode = 302, bool $bExit = true) : void
~~~

~~~php
Header::init()->Location('/404/');
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Expires"></a>
## `Expires`

~~~
public function Expires(int $iExpireSeconds = 0) : Header
~~~

~~~php
Header::init()->Expires(iExpireSeconds: (60 * 60 * 24)); # +1 day (future)
~~~
~~~php
Header::init()->Expires(iExpireSeconds: -(60 * 60 * 24)); # -1 day (past)
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Etag"></a>
## `Etag`

_Entity Tag_

~~~
public function Etag(string $sEtag = '""') : Header
~~~

~~~php
Header::init()->Etag('"xyzzy"');
~~~

see <a href="https://datatracker.ietf.org/doc/html/rfc7232#section-2.3" target="_blank">datatracker.ietf.org/doc/html/rfc7232#section-2.3</a>

------------------------------------------------------------------------------------------------------------------------

<a id="Last_Modified"></a>
## `Last_Modified`

~~~
public function Last_Modified(int $iUnixTimestamp = 0) : Header
~~~

~~~php
Header::init()->Last_Modified(
    new \DateTime("1 day ago")->getTimestamp()
);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Set_Cookie"></a>
## `Set_Cookie`

~~~
public function Set_Cookie(string $sName, string $sValue, int $iExpireUnixTimestamp = 0, string $sPath = '/', string $sDomain = '', string $sSameSite = '', bool $bSecure = false, bool $bHttpOnly = false) : Header
~~~

~~~php
Header::init()->Set_Cookie(
    sName: 'Name',
    sValue: 'value',
    iExpireUnixTimestamp: new \DateTime("+ 1 day")->getTimestamp(),
    sPath: '/',
    sSameSite: 'None; Partitioned',
    bSecure: true,
    bHttpOnly: true
);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Refresh"></a>
## `Refresh`

~~~
public function Refresh(int $iRefreshSeconds = 0, string $sUrl = '') : void
~~~

~~~php
Header::init()->Refresh(
    iRefreshSeconds: 3,
    sUrl: '/'
);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="WWW_Authenticate"></a>
## `WWW_Authenticate`

Only the "Basic" authentication method is supported. See the `header()` function for more information (see <a href="https://www.php.net/manual/de/function.header.php" target="_blank">www.php.net/manual/de/function.header.php</a>. 

`user` and `password` are stored in   
- `$_SERVER['PHP_AUTH_USER']`
- `$_SERVER['PHP_AUTH_PW']`.

Be aware this is state-less.

And you must manage any check of `PHP_AUTH_USER` and `PHP_AUTH_PW` yourself.  
see <a href="https://www.php.net/manual/en/features.http-auth.php" target="_blank">www.php.net/manual/en/features.http-auth.php</a>

~~~
public function WWW_Authenticate(string $sBasicRealm = 'Authentication')
~~~

~~~php
Header::init()->WWW_Authenticate(
    sBasicRealm: 'Authentication'
);
~~~
- [HTTP Authentication example](#HTTP-Authentication-example)

------------------------------------------------------------------------------------------------------------------------

<a id="Retry_After"></a>
## `Retry_After`

~~~
public function Retry_After(int $iValue = 0, EnumHttpHeaderRetryAfter $eEnumRetryAfter = EnumHttpHeaderRetryAfter::UnixTimestamp) : Header
~~~

~~~php
Header::init()->Retry_After(
    iValue: 300,
    eEnumRetryAfter: \MVC\Enum\EnumHttpHeaderRetryAfter::RetryAfterSeconds
);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="ContentSecurityPolicy"></a>
## `ContentSecurityPolicy`

sets CSP ("Content Security Policy") HTTP Header.

~~~
public function ContentSecurityPolicy(array $aCSP = array()) : Header
~~~

~~~php
Header::init()->ContentSecurityPolicy();
~~~
- if Argument `aCSP` is empty, the method tries to read the configuration array from `Config::MODULE()['CSP']`.

For more Information about Content Security Policy see <a href="https://content-security-policy.com/" target="_blank">content-security-policy.com</a>

------------------------------------------------------------------------------------------------------------------------

<a id="X_Accel_Buffering"></a>
## `X_Accel_Buffering`

~~~
public function X_Accel_Buffering(string $sStatus = 'no') : Header
~~~

~~~php
Header::init()->X_Accel_Buffering();
Header::init()->X_Accel_Buffering(sStatus: 'no');
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="Examples"></a>
## Examples

------------------------------------------------------------------------------------------------------------------------

<a id="download"></a>
### Provide a file for download

~~~php
Header::init()
    ->Content_Disposition_Attachment('robots.txt')
    ->Content_Type_application_force_download()
    ->Content_Type_application_octet_stream()
    ->Content_Type_application_download()
    ->Content_Description_File_Transfer()
    ->Content_Length(filesize('/var/www/html/public/robots.txt'))
;
echo file_get_contents('/var/www/html/public/robots.txt');
~~~

---

<a id="HTTP-Authentication-example"></a>
### HTTP Authentication example

in the controller method you want to protect, place the following code

~~~php
/**
 * @param \MVC\DataType\DTRequestIn $oDTRequestIn
 * @param \MVC\DataType\DTRoute     $oDTRoute
 * @return void
 * @throws \ReflectionException
 */
public function index(DTRequestIn $oDTRequestIn, DTRoute $oDTRoute)
{
    $sAuthUser = 'foo';
    $sPassword = 'bar';

    // as long as user/password is not correct, the auth prompt occurs 
    if (
        false === (true === isset($_SERVER['PHP_AUTH_USER']) && true === isset($_SERVER['PHP_AUTH_PW'])) ||
        false === (($_SERVER['PHP_AUTH_USER'] === $sAuthUser) && ($_SERVER['PHP_AUTH_PW'] === $sPassword))
    )
    {
        unset($_SERVER['PHP_AUTH_USER']);
        unset($_SERVER['PHP_AUTH_PW']);
        Header::init()->WWW_Authenticate();
    }

    view()->autoAssign();
}
~~~






































