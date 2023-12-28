
# Closure

- [is](#check-on-closure)
- [toString](#closure-to-string)

---

<a id="check-on-closure"></a>
## `is`

checks whether the unknown parameter is of type closure object.

~~~
Closure::is(mixed $mUnknown) : bool
~~~

_Example_  
~~~php
// a closure
$oClosure = function () {
    display('I am a Closure.');
};

// check
$bIsClosure = Closure::is($oClosure);

// bool(true)
var_dump($bIsClosure);
~~~

_Result_  
~~~
true
~~~

---

<a id="closure-to-string"></a>
## `toString`

with the `Closure::toString` method you can convert a Closure into a String, which is useful if you want to log a closure into a logfile for example. 

~~~
Closure::toString(\Closure $oClosure, bool $bShrink = true) : string
~~~

_Example_  
~~~php
// a closure
$oClosure = function () {
    display('I am a Closure.');
};

$sClosure = Closure::toString($oClosure);

// string(43) "function () { display('I am a Closure.'); }"
var_dump($sClosure);
~~~

_Result_
~~~
function () { display('I am a Closure.'); }
~~~

