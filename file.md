
# File

- [getMimeType](#getMimeType)
- [info](#info)
- [saveIntoTemp](#saveIntoTemp)
- [secureFilePath](#secureFilePath)
- [temp](#temp)

---

## `getMimeType` <a id="getMimeType"></a>

returns mimetype of a given file.

~~~
File::getMimeType(string $sFileAbsolute = '') : string
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

## `info` <a id="info"></a>

get infos about a file via `stat`, `posix_getpwuid`, `pathinfo`.

~~~
File::info(string $sFilePathAbs = '') : DTFileinfo
~~~

_Example_  
~~~php
$oDTFileinfo = File::info(__FILE__);
~~~

_return Datatype object `$oDTFileinfo` (see [DTFileinfo](/2.x/datatype-classes#DTFileinfo))_  
~~~bash
 MVC\DataType\DTFileinfo {#91 ▼
  #dirname: "/var/www/html/modules/Foo/Controller"
  #basename: "Index.php"
  #path: "/var/www/html/modules/Foo/Controller/Index.php"
  #is_file: true
  #is_dir: false
  #extension: "php"
  #filename: "Index"
  #name: "admin1"
  #passwd: "x"
  #uid: 1000
  #gid: 1000
  #filemtime: 1739435042
  #filectime: 1739435042
  #filesize: 7562
  #gecos: ""
  #dir: "/home/admin1"
  #shell: "/bin/bash"
  #mimetype: "text/x-php"
}
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

## `saveIntoTemp` <a id="saveIntoTemp"></a>

writes data into a -temporary- file; returns absolute path to that file.

~~~
File::saveIntoTemp(mixed $mData = null, string $sPrefix = '', string $sSuffix = '') : string
~~~

_Example_  
~~~php
$sFileAbs = File::saveIntoTemp(
    'some Foo Bar Data'
);

// "/tmp/temp.ff4kdvv5i07o0AaUnYi"
dump($sFileAbs);
~~~

---

## `secureFilePath` <a id="secureFilePath"></a>

removes doubleDot+Slashes (../) from string, replaces multiple forwardSlashes (//) from string by a single forwardSlash.

~~~
File::secureFilePath(string $sAbsoluteFilePath = '', bool $bIgnoreProtocols = false) : string
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

---

## `temp` <a id="temp"></a>

creates a -temporary- file; returns absolute path to that file.

~~~
File::temp(string $sPrefix = '', string $sSuffix = '') : string
~~~

_Example_
~~~php
$sFileAbs = File::temp();

// "/tmp/temp.ff4kdvv5i07o0AaUnYi"
dump($sFileAbs);
~~~
