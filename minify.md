
# Minify

---


## `init`

minifies all `*.css` and `*.js` files found in the given folder and beneath - recursively. The results are being cached so 
it will only run once if there are changes made to css/js files.

~~~
init(array $aContentFilterMinify = array()) : bool
~~~
- if leaving `$aContentFilterMinify` empty, the `MVC_PUBLIC_PATH` (`/public/`) will be scanned recursively.

_Example: minify css/js in given folders_    
~~~php
\MVC\Minify::init(array(
    Config::get_MVC_PUBLIC_PATH . '/Emvicy/styles/',
    Config::get_MVC_PUBLIC_PATH . '/Emvicy/scripts/',
));
~~~

## Activating via event

If you created a module using `emvicy.php` (see [Creating a Module](/1.x/creating-a-module)) you will already find 
an Implementation / Activation in `/modules/{module}/etc/event/default.php`:

![Emvicy Minify](/doc/1.x/minify/emvicy_minify.png)

---

## `minifyCss`

creates a minified CSS file.

~~~
minifyCss(\SplFileInfo $oSplFileInfo) : bool
~~~

---

## `minifyJs`

creates a minified JS file.

~~~
minifyJs(\SplFileInfo $oSplFileInfo) : bool
~~~

