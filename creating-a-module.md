
# Creating a Module

- [How it works](#how-it-works)
  - [`primary` module](#primary-module)
      - [dotfile `.primary`](#dotfile-primary)
  - [`secondary` module](#secondary-module)
- [Creating a `primary` Module](#creating-a-primary-module)
- [Creating a `secondary` Module](#creating-a-secondary-module)

---

<a id="how-it-works"></a>
## How it works

There is a distinction between `primary` and `secondary` modules.

You create a module by using `emvicy.php` on CLI. The creation of the necessary environment config file (e.g. 'develop.php') during this module creation process depends on which `MVC_ENV` is set in `/.env` (see [/1.x/configuration#Environment](/1.x/configuration#Environment)).

---

<a id="primary-module"></a>
### `primary` module    
You develop your application in a `primary` module.
That `primary` module is also stored by name in the globally available config `$aConfig['MVC_MODULE_PRIMARY_NAME']` and is also available via `Config::get_MVC_MODULE_PRIMARY_NAME()`.

The `primary` module is the leading, tone-setting module.
You don't need to write secondary modules if you don't need one. Develop your application in a `primary` module.

<a id="dotfile-primary"></a>

**dotfile `.primary`**

A primary module needs to have the dotfile `.primary` in its root folder. There can only be one module having a `.primary` dotfile at once.

If you add a module by following [Creating a `primary` Module](#creating-a-primary-module), the `.primary` dotfile will be automatically created.

If you want so make a secondary module as the primary one, just move the `.primary` dotfile to the secondary module.

~~~
.primary
~~~

<a id="secondary-module"></a>
### `secondary` module

But sometimes there are parts of code you want to reuse in other software projects.
For such a case `secondary` modules are suitable. Code written here should be reused in other Emvicy software projects if necessary.

<!--
For this version of Emvicy there are already some secondary, public Emvicy modules out there for reuse.
Public available modules:  
- <a href="https://github.com/gueff/Emvicy_module_DB" target="_blank">`https://github.com/gueff/Emvicy_module_DB`</a>
- <a href="https://github.com/gueff/Emvicy_module_OpenApi" target="_blank">`https://github.com/gueff/Emvicy_module_OpenApi`</a>
- <a href="https://github.com/gueff/Emvicy_module_Idolon" target="_blank">`https://github.com/gueff/Emvicy_module_Idolon`</a>
- <a href="https://github.com/gueff/Emvicy_module_Email" target="_blank">`https://github.com/gueff/Emvicy_module_Email`</a>
- <a href="https://github.com/gueff/Emvicy_module_Captcha" target="_blank">`https://github.com/gueff/Emvicy_module_Captcha`</a>
-->

---

Conclusion

- You need to install at least **one Module as primary** to be able to work.
- You can only have **one primary module at a time**.
- You can have **unlimited secondary** modules.

---

<a id="creating-a-primary-module"></a>
## Creating a `primary` Module

_use one of the following commands to create a primary module_  
~~~bash
php emvicy.php c module:foo                         # creates primary module "Foo"; asks if modulename is correct
~~~
~~~bash
php emvicy.php c module:foo force:no                # same
php emvicy.php c module:foo force:yes               # no asking - instantly creating of primary module "Foo"
php emvicy.php c module:foo primary:yes             # creates primary module "Foo"; asks if modulename is correct
php emvicy.php c module:foo primary:yes force:no    # same
php emvicy.php c module:foo primary:yes force:yes   # no asking - instantly creating of primary module "Foo"
~~~

_After creating a primary module, you could start Emvicy's local development server to test the module frontend_
~~~bash
php emvicy.php s
~~~

Call http://127.0.0.1:1969/ and you will see your created module frontend

![Emvicy Creating a Module](/doc/1.x/getting-started/emvicy-creating-a-module.png)

---

<a id="creating-a-secondary-module"></a>
## Creating a `secondary` Module

_use one of the following commands to create a secondary module_
~~~bash
php emvicy.php c module:bar primary:no              # creates module "Bar" as a secondary module; asks if modulename is correct
php emvicy.php c module:bar primary:no force:yes    # no asking - instantly creating of secondary module "Bar"
~~~
