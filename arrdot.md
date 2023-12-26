 
# ArrDot - <small>[array dot notation]</small>

with ArrDot you can access arrays via dot notation.

- [Create](#Create)
- [Set](#Set)
- [Get](#Get)

---

<a id="Create"></a>
## Create

~~~
$oArrDot = new ArrDot(array $aArray);
~~~

_create a new instance of `ArrDot`_   
~~~php
$oArrDot = new ArrDot(
    array( 
        'user' => [
            'foo' => 'bar',
            'john' => 'doe'
        ],
        'etc' => [
            'OS' => 'Linux'
        ]       
    )
);
~~~

---

<a id="Set"></a>
## Set

~~~
$oArrDot->set(string $sKey, mixed);
~~~

_add a key/value by `set` method_  
~~~php
$oArrDot->set('user.mary', 'moo');
~~~

---

<a id="Get"></a>
## Get

~~~
$oArrDot->get();
$oArrDot->get(string $sKey);
~~~

_get complete array_  
~~~php
$aMyArray = $oArrDot->get();
~~~
~~~
// type: array, items: 2
[
    'user' => [
        'foo' => 'bar',
        'john' => 'doe',
        'mary' => 'moo',
    ],
    'etc' => [
        'OS' => 'Linux',
    ],
]
~~~

_get value on an existing **`key`**_
~~~php
$oArrDot->get('user.john')
~~~
~~~
// type: string
'doe'
~~~

_get key on existing **`value`**_  
~~~php
$oArrDot->getIndexOnValue('Linux')
~~~
~~~
// type: string
'etc.OS'
~~~