
# Session

- [is](#is)
- [setNamespace](#setNamespace)
- [set](#set)
- [get](#get)
- [getAll](#getAll)
- [has](#has)

---

<a id="is"></a>
## `is`

~~~
is(string $sNamespace = '') : Session
~~~

_Example_  
~~~php
$oSession = Session::is();
~~~
~~~
// type: object
\MVC\Session::__set_state(array(
      '_aOption' =>    array (
        'cookie_httponly' => true,
        'auto_start' => 0,
        'save_path' => '/var/www/Emvicy/application/session',
        'cookie_secure' => false,
        'name' => 'Emvicy',
        'save_handler' => 'files',
        'cookie_lifetime' => 0,
        'gc_maxlifetime' => 65535,
        'gc_probability' => 1,
        'use_strict_mode' => 1,
        'use_cookies' => 1,
        'use_only_cookies' => 1,
        'upload_progress.enabled' => 1,
    ),
      '_sNamespace' => 'Emvicy',
      '_bSessionEnable' => true,
))
~~~

---

<a id="setNamespace"></a>
## `setNamespace`

~~~
Session::is('Foo')
~~~
~~~
Session::is()->setNamespace(string $sNamespace = '') : Session|null
~~~

_Example_  
~~~php
Session::is('Foo');
~~~
~~~
// type: object
\MVC\Session::__set_state(array(
      '_aOption' =>    array (
        'cookie_httponly' => true,
        'auto_start' => 0,
        'save_path' => '/var/www/Emvicy/application/session',
        'cookie_secure' => false,
        'name' => 'Emvicy',
        'save_handler' => 'files',
        'cookie_lifetime' => 0,
        'gc_maxlifetime' => 65535,
        'gc_probability' => 1,
        'use_strict_mode' => 1,
        'use_cookies' => 1,
        'use_only_cookies' => 1,
        'upload_progress.enabled' => 1,
    ),
      '_sNamespace' => 'Foo',
      '_bSessionEnable' => true,
))
~~~

<a id="set"></a>
## `set`

sets a value by its key.

~~~
Session::is()->set(string $sKey = '', mixed $mValue = null) : Session
~~~

_Example_  
~~~php
Session::is('Foo')
    ->set('foo', 'bar')
    ->set('milly', 'moo');
~~~

---

<a id="get"></a>
## `get`

gets a value by its key.

~~~
Session::is()->get(string $sKey = '') : mixed
~~~

_Example_
~~~php
Session::is('Foo')->get('foo');
~~~

_Result_ 
~~~
bar
~~~

---

<a id="getAll"></a>
## `getAll`

gets session key/values on the current namespace

~~~
Session::is()->getAll() : mixed
~~~

_Example_
~~~php
Session::is('Foo')->getAll();
~~~

_Result_
~~~
[
    'foo' => 'bar',
    'milly' => 'moo',
]
~~~

---

<a id="has"></a>
## `has`

checks whether a given key exists.

~~~
Session::is()->has(string $sKey = '') : bool
~~~

_Example_
~~~php
Session::is('Foo')->has('foo')
~~~

_Result_
~~~
true
~~~

---

