
# Asset

With Assets you define a set of settings and conditions that you can use anywhere in your code.

- [Declare a `_asset.yaml` file](#yaml)
  - [Reuse values by reference](#Reuse)
- [Instantiation](#Instantiation)
- [Accessing](#Accessing)

---

## Declare a `_asset.yaml` file <a id="yaml"></a>

save the file `_asset.yaml` into your primary module's config folder, e.g.: `modules/Foo/etc/config/Foo/config/_asset.yaml`.

🛈 In the YAML file, other values within the file can be used by reference.

*Example: `modules/Foo/etc/config/Foo/config/_asset.yaml`*    
~~~bash
User:
  email:
    set: &User.email.set
      name: &User.email.name "email"
      min: &User.email.min 5 # a@b.c
      max: &User.email.max 255
      minlength: &User.email.minlength *User.email.min # takes the value from `User.email.min`, which is 5
      maxlength: &User.email.maxlength *User.email.max # takes the value from `User.email.max`, which is 255
      filter_var: 274     # FILTER_VALIDATE_EMAIL # @see https://www.php.net/manual/en/filter.filters.validate.php
      sanitize_var: 517   # FILTER_SANITIZE_EMAIL # @see https://www.php.net/manual/en/filter.filters.sanitize.php
      #preg_replace: "/[^\\p{L}\\p{M}\\p{Z}\\p{S}\\p{N}\\p{P}]+/u" # @see https://www.regular-expressions.info/unicode.html

  password:
    set: &User.password.set
      name: &User.password.name "password"
      min: &User.password.min 10
      max: &User.password.max 60
      minlength: &User.password.minlength *User.password.min # takes the value from `User.password.min`, which is 10
      maxlength: &User.password.maxlength *User.password.max # takes the value from `User.password.max`, which is 60
      filter_var:
      sanitize_var:

#-----------------------------------------------------------------------------------------------------------------------
# define filter objects
# using assets defined above
# @see etc/event/filter.php

Filter:
  Request:
    input:
      email: *User.email.set
~~~

### Reuse values by reference <a id="Reuse"></a>

As you can see in the yaml file above, the key `User.email.set.minlength` does not have a concrete value, but a reference to the key `User.email.set.min`.  
This way it takes the value from that key.

To take a value from another key, just address that key by setting an `*` before the key:

_Example: take value from key `User.email.set.min`_  
~~~bash
*User.email.set.min
~~~

---

## Instantiation <a id="Instantiation"></a>

To access an asset, it must be initialized once.

~~~php
Asset::init(
    'modules/Foo/etc/config/Foo/config/_asset.yaml'
);

// or

Asset::init(
    Config::get_MVC_MODULE_PRIMARY_STAGING_CONFIG_DIR() . '/_asset.yaml'
);
~~~

An already defined Auto-Instantiation you find in a listener on event `mvc.event.init.after` in your `modules/Foo/etc/event/request.php`.

Modify it to your needs.

~~~php
    [...]

    'mvc.event.init.after' => [
        /**
         * at this early stage
         * create "Assets" object with given config
         */
        function(){
            \MVC\Asset::init(\MVC\Config::get_MVC_MODULE_PRIMARY_STAGING_CONFIG_DIR() . '/_asset.yaml');
        },
    ],

    [...]
~~~

---

## Accessing <a id="Accessing"></a>

_Example_
~~~php
Asset::init()->get('User.email.set.minlength');
~~~
~~~
5
~~~

_Example_  
~~~php
Asset::init()->get('Filter.Request.input')
~~~
~~~
// type: array, items: 1
[
    'email' => [
        'name' => 'email',
        'min' => 5,
        'max' => 255,
        'minlength' => 5,
        'maxlength' => 255,
        'filter_var' => 274,
        'sanitize_var' => 517,
    ],
]
~~~
---






























