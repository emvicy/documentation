
# Closure

- [Check on Closure](#check-on-closure)
- [Closure to String](#closure-to-string)

---

<a id="check-on-closure"></a>
## Check on Closure

~~~
Closure::is(closure $oClosure);
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
## Closure to String

with the `Closure::toString` method you can convert a Closure into a String, which is useful if you want to log a closure into a logfile for example. 

~~~
Closure::toString(clsoure $oClosure);
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

