
# Error

- [notice](#notice)
- [warning](#warning)
- [error](#error)
- [exception](#exception)
- [fatal](#fatal)
- [get](#get)

---

_TLDR;_  
~~~php
Error::notice('this is a notice');
Error::warning('this is a warning');
Error::error('this is an error');
Error::exception(new \Exception('this is an exception'));
Error::fatal();

$aError = Error::get();
~~~

<a id="notice"></a>
## `notice`

~~~
notice (string $sMessage = '', int $iCode = E_NOTICE, int $iSeverity = 0, string $sFilename = '', int $iLineNr = 0) : void
~~~

_Example_  
~~~php
Error::notice('this is a notice');
~~~

---

<a id="warning"></a>
## `warning`

~~~
warning (string $sMessage = '', int $iCode = E_WARNING, int $iSeverity = 0, string $sFilename = '', int $iLineNr = 0) : void
~~~

_Example_
~~~php
Error::warning('this is a warning');
~~~

---

<a id="error"></a>
## `error`

~~~
error (string $sMessage = '', int $iCode = E_ERROR, int $iSeverity = 0, string $sFilename = '', int $iLineNr = 0) : void
~~~

_Example_
~~~php
Error::error('this is an error');
~~~

---

<a id="exception"></a>
## `exception`

~~~
exception(\Exception $oErrorException) : void
~~~

_Example_
~~~php
Error::exception(new \Exception('this is an exception'));
~~~

---

<a id="fatal"></a>
## `fatal`

~~~
function fatal() : void
~~~

_Example_
~~~php
Error::fatal();
~~~

---

<a id="get"></a>
## `get`

~~~
get() : array
~~~

_Example_
~~~php
$aError = Error::get();
~~~

_Example Retrieving error data using [ArrDot](/1.x/arrdot)_  
~~~php
$oArrDot = new ArrDot(Error::get());

info(
    $oArrDot->get('0.aKeyValue')
);
display(
    $oArrDot->getIndexOnValue('this is an error')
);
~~~
