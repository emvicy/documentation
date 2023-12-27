
# Registry

- [set](#set)
- [isRegistered](#isRegistered)
- [get](#get)
- [take](#take)
- [delete](#delete)

---

<a id="set"></a>
# `set`

saves a key/value pair to registry storage.

~~~
set(string $sIndex = '', mixed $mValue = null) : void
~~~

_Example_
~~~php
Registry::set('foo', 'bar');
~~~

---

<a id="isRegistered"></a>
# `isRegistered`

Returns: 
- `true` if `$sIndex` exists as key in the registry 
- `false` if `$sIndex` was not found in the registry

~~~
isRegistered(string $sIndex = '') : bool
~~~

_Example_
~~~php
$bIsRegistered = Registry::isRegistered('foo');
~~~
~~~
// type: boolean
true
~~~

---

<a id="get"></a>
# `get`

gets (reads) a value by its key.

~~~
get(string $sIndex = '') : mixed
~~~

_Example_
~~~php
$sResult = Registry::get('foo');
~~~
~~~
// type: string
bar
~~~

---

<a id="take"></a>
# `take`

gets a value by its key and deletes the entry afterwards.

~~~
take(string $sIndex = '') : mixed
~~~

_Example_
~~~php
$sResult = Registry::take('foo');
~~~
~~~
// type: string
bar
~~~

---

<a id="delete"></a>
# `delete`

deletes a value in registry.

~~~
delete(string $sIndex = '') : bool
~~~

_Example_
~~~php
$bDeleteSuccess = Registry::delete('foo');
~~~
~~~
// type: boolean
true
~~~
