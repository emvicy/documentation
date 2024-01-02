 
# functions

## `get()`

simplifies the use of variables. If a variable does not exist, null or a defined value is returned.

_usually_  
~~~
$mValue = (isset($aData['foo']['bar'])) ? $aData['foo']['bar'] : null;
// or
$mValue = ($aData['foo']['bar'] ?? null);
~~~

_way with get()_  
~~~php
$mValue = get($aData['foo']['bar']);
~~~

---

## `display()`

---

## `info()`

---


## `stop()`

---

## `whereis()`

---

## `pr()`

---

## `mvcStoreEnv()`

---

## `mvcConfigLoader()`
