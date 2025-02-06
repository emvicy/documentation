
# Creating a Module

- [Creating a `primary` Module](#creating-a-primary-module)
- [Creating a `secondary` Module](#creating-a-secondary-module)
- [How it works](#how-it-works)
  - [`primary` module](#primary-module)
      - [dotfile `.primary`](#dotfile-primary)
  - [`secondary` module](#secondary-module)

---

<a id="creating-a-primary-module"></a>
## Creating a `primary` Module

_use the following command to create the primary module `Foo`_  
~~~bash
php emvicy module:create Foo primary
~~~

---

<a id="creating-a-secondary-module"></a>
## Creating a `secondary` Module

_use the following command to create the secondary module `Bar`_
~~~bash
php emvicy module:create Bar secondary
~~~

<!--
---

_After creating a primary module, you could start Emvicy's local development server to test the module frontend_
~~~
php emvicy serve
~~~

Call http://127.0.0.1:1969/ and you will see your created module frontend

![Emvicy Creating a Module](/doc/2.x/getting-started/emvicy-creating-a-module.png)
-->

---

<a id="how-it-works"></a>
## How it works

There is a distinction between `primary` and `secondary` modules. You create a module by using `emvicy` on CLI. 
The creation of the necessary environment config file (e.g. 'develop.php') during this module creation process depends on which `MVC_ENV` is set in `/.env` (see [/2.x/configuration#Environment](/2.x/configuration#Environment)).

---

<a id="primary-module"></a>
### The `primary` module
You develop your application in a `primary` module. 
That `primary` module is also stored by name in the globally available config `$aConfig['MVC_MODULE_PRIMARY_NAME']` and is also available via `Config::get_MVC_MODULE_PRIMARY_NAME()`. 
The `primary` module is the leading, tone-setting module. You don't need to write secondary modules if you don't need one. Develop your application in a `primary` module.

<a id="dotfile-primary"></a>

**dotfile `.primary`**

A primary module needs to have the dotfile `.primary` in its root folder. There can only be one module having a `.primary` dotfile at once. 
If you add a module by following [Creating a `primary` Module](#creating-a-primary-module), the `.primary` dotfile will be automatically created. 
If you want so make a secondary module as the primary one, just move the `.primary` dotfile to the secondary module.

~~~
.primary
~~~

<a id="secondary-module"></a>
### A `secondary` module

Sometimes there are parts of code you want to reuse in other software projects. 
For such a case `secondary` modules are suitable. Code written here should be reused in other Emvicy software projects if necessary. 
For this version of Emvicy there are already some secondary, public Emvicy modules out there for reuse. 
See "Modules" Section in the Menu.

---

Conclusion

- You need to install at least **one Module as primary** to be able to work.
- You can only have **one primary module at a time**.
- You can have **unlimited secondary** modules.