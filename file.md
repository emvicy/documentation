
# File

- [info](#info)
- [getMimeType](#getMimeType)
- [secureFilePath](#secureFilePath)

---

<a id="info"></a>
## `info`

get infos about a file via `stat`, `posix_getpwuid`, `pathinfo`.

~~~
info(string $sFilePathAbs = '') : DTFileinfo
~~~

_Example_  
~~~php
$oDTFileinfo = File::info(__FILE__);
~~~

_return Datatype object `$oDTFileinfo` (see [DTFileinfo](/1.x/datatype-classes#DTFileinfo))_  
~~~
// type: object
\MVC\DataType\DTFileinfo::__set_state(array(
      'dirname' => '/var/www/htdocs/Emvicy/modules/Foo/Controller',
      'basename' => 'Index.php',
      'path' => '/var/www/htdocs/Emvicy/modules/Foo/Controller/Index.php',
      'is_file' => true,
      'is_dir' => false,
      'extension' => 'php',
      'filename' => 'Index',
      'name' => 'admin1',
      'passwd' => 'x',
      'uid' => 1000,
      'gid' => 1000,
      'gecos' => 'admin1,,,',
      'dir' => '/var/www',
      'shell' => '/bin/bash',
      'filemtime' => 1703679863,
      'filectime' => 1703679863,
      'mimetype' => 'text/x-php',
))
~~~

_Example: get Extension_  
~~~php
$sExtension = File::info(__FILE__)->get_extension();
~~~

_Result_
~~~
php
~~~

---

<a id="getMimeType"></a>
## `getMimeType`

returns mimetype of a given file.

~~~
getMimeType(string $sFileAbsolute = '') : string
~~~

_Example_  
~~~php
$sMimeType = File::getMimeType(__FILE__);
~~~

_Result_
~~~
// type: string
'text/x-php'
~~~

---

<a id="secureFilePath"></a>
## `secureFilePath`

removes doubleDot+Slashes (../) from string, replaces multiple forwardSlashes (//) from string by a single forwardSlash.

~~~
secureFilePath(string $sAbsoluteFilePath = '', bool $bIgnoreProtocols = false) : string
~~~

_Example_  
~~~php
$sPath = File::secureFilePath('../../../../../../../../../../../var/www/htdocs/../../..////////Emvicy/modules/Foo/Controller/Index.php');
~~~

_Result_  
~~~
// type: string
'var/www/htdocs/Emvicy/modules/Foo/Controller/Index.php'
~~~

