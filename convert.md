
# Convert

- [objectToArray](#objectToArray)
- [boolToString](#boolToString)
- [constValueToKey](#constValueToKey)

---

<a id="objectToArray"></a>
## Object to array

converts any object into array.

~~~
Convert::objectToArray(object $oObject);
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
## Bool to String

converts `true` into `'true'` and `false` into `'false'`. Well of course it does not really convert the original value, but gives you a string representation of that value.

~~~
Convert::boolToString(bool $bBool);
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
## Constant value to Key (Name)

returns constant name on its integer value - works for `php` constants.

~~~
Convert::constValueToKey(int $iInt);
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
