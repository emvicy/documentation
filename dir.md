
# Dir <small>[directory]</small>

- [exists](#exists)
- [isEmpty](#isEmpty)
- [make](#make)
- [recursiveCopy](#recursiveCopy)
- [remove](#remove)

---

<a id="exists"></a>
## `exists`

checks if a given directory exists.

~~~
Dir::exists(string $sDirectory = '') : bool
~~~

_Example_  
~~~php
$bExists = Dir::exists(
    '/tmp/foo'
);
~~~
~~~
// type: boolean
true
~~~

---

<a id="isEmpty"></a>
### `isEmpty`

checks if a directory is empty.

~~~
Dir::isEmpty(string $sDirectory = '') : bool
~~~

_Example_  
~~~php
$bEmpty = Dir::isEmpty(
    '/tmp/foo'
);
~~~
~~~
// type: boolean
false
~~~

---

<a id="make"></a>
### `make`

creates a directory.

~~~
Dir::make(string $sDirectory = '', int $iPermission = 0755, bool $bRecursive = true) : bool
~~~

_Example_
~~~php
$bEmpty = Dir::make(
    '/tmp/foo/bar'
);
~~~
~~~
// type: boolean
true
~~~

---

<a id="recursiveCopy"></a>
### `recursiveCopy`

copies a directory recursively.

~~~
Dir::recursiveCopy(string $sSource = '', string $sDestination = '') : void
~~~

_Example_
~~~php
Dir::recursiveCopy(
    '/tmp/foo/', 
    '/tmp/bar'
);
~~~

---

<a id="remove"></a>
### `remove`

deletes a directory.

⚠ be aware: `Dir::remove('/tmp/foo/')` would delete folder `foo` as well; as a trailing slash has no meaning.  
⚠ parameter `$bForce`: if set to true: ignores if directory is not empty; removes given directory no matter what (default=false).

~~~
Dir::remove(string $sDirectory = '', bool $bForce = false) : bool
~~~

_Example_
~~~php
$bRemove = Dir::remove(
    '/tmp/foo/bar'
);
~~~
~~~
// type: boolean
true
~~~