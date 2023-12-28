
# Convert

- [objectToArray](#objectToArray)
- [boolToString](#boolToString)
- [constValueToKey](#constValueToKey)

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
