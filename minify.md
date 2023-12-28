
# Minify

- [init](#init)
- [Activating via Event Listener](#Activating)
- [minifyCss](#minifyCss)
- [minifyJs](#minifyJs)

---

<a id="init"></a>
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

---

<a id="Activating"></a>
## Activating via Event Listener

~~~php
\MVC\Event::processBindConfigStack([

    'mvc.reflex.reflect.targetObject.before' => [
        // minify css/js files
        function(\MVC\DataType\DTArrayObject $oDTArrayObject) {
            \MVC\Minify::init();
        },
    ],
]);
~~~

If you created a module using `emvicy.php` (see [Creating a Module](/1.x/creating-a-module)) you will already find an Activation via Event Listener in `/modules/{module}/etc/event/default.php`:

![Emvicy Minify](/doc/1.x/minify/emvicy_minify.png)

---

<a id="minifyCss"></a>
## `minifyCss`

creates a minified CSS file.

~~~
minifyCss(\SplFileInfo $oSplFileInfo) : bool
~~~

---

<a id="minifyJs"></a>
## `minifyJs`

creates a minified JS file.

~~~
minifyJs(\SplFileInfo $oSplFileInfo) : bool
~~~

