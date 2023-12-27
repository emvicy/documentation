
# Lock

ensures that there can only be one call at runtime; prevents multiple calls at runtime.

As long as the runtime takes, Lock prevents multiple calls.

~~~
create(string $sKey = '', bool $bReturn = false)
~~~

## Usage

simply note the following command at the place you want to be protected from multiple calls.

In most cases this would be inside a Controller method.

_Example_  
~~~php
public function index()
{
    Lock::create();
    ...
}
~~~

This command creates a lock folder inside your cache folder during runtime.

When runtime is over, the lock folder will be removed automatically.

![Lock](/doc/1.x/lock/emvicy_lock.png)