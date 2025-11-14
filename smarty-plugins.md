
# Smarty PlugIns

Emvicy comes with several plug-ins for Smarty.

- [modifier](#modifier)
  - [smarty_modifier_centToEuro](#smarty_modifier_centToEuro)
  - [smarty_modifier_dateformat](#smarty_modifier_dateformat)
  - [smarty_modifier_filesize](#smarty_modifier_filesize)
  - [smarty_modifier_gtext](#smarty_modifier_gtext)
  - [smarty_modifier_hasValue](#smarty_modifier_hasValue)
  - [smarty_modifier_highlight_html](#smarty_modifier_highlight_html)
  - [smarty_modifier_shrink](#smarty_modifier_shrink)
  - [smarty_modifier_parsedown](#smarty_modifier_parsedown)

---

## modifier <a id="modifier"></a>

- see also official Smarty Documentation on this topic: <a href="https://www.smarty.net/docs/en/plugins.modifiers.tpl" target="_blank">www.smarty.net/docs/en/plugins.modifiers.tpl</a>

### `smarty_modifier_centToEuro` <a id="smarty_modifier_centToEuro"></a>

converts eurocent into euro (1000 => 10,00 €)

~~~
smarty_modifier_centToEuro(mixed $iValue = 0, bool $bShowEuroSymbol = true) : string
~~~

**Examples**  

_Default_  
~~~html
{1000|centToEuro}
~~~
~~~
10,00 €
~~~

_Do not display the euro symbol_  
~~~html
{1000|centToEuro:false}
~~~
~~~
10,00
~~~

---

### `smarty_modifier_dateformat` <a id="smarty_modifier_dateformat"></a>

converts a timestamp into the requested format. Per default the format is ISO Date and time.

~~~
smarty_modifier_dateformat(int $iTimestamp, string $sFormat = 'Y-m-d H:i:s') : string
~~~

**Examples**

_default_  
~~~html
{1763119198|dateformat}
~~~
~~~
2025-11-14 12:19:58
~~~

_format into `Y-m-d`_  
~~~html
{1763119198|dateformat:"Y-m-d"}
~~~
~~~
2025-11-14
~~~

---

### `smarty_modifier_filesize` <a id="smarty_modifier_filesize"></a>

converts Byte into Kilobyte, Megabyte, Gigabyte, Terabyte, Petabyte.

~~~
smarty_modifier_filesize(string $sByte, int $iDecimals = 2) : string
~~~

**Examples**

~~~html
{1234|filesize}
~~~
~~~
1.21K
~~~

~~~html
{9876543210|filesize}
~~~
~~~
9.20G
~~~

---

### `smarty_modifier_gtext` <a id="smarty_modifier_gtext"></a>

This Smarty modifier helps instantly translating Strings into other Languages using the PHP Extension `gettext`.

~~~
smarty_modifier_gtext(string $sString = '', string $sDomain = 'term', string $sLang = '') : string
~~~

**Requirements**

- requires installation of php module: `php{Version}-intl` (and maybe `libicu52`)
- You need to name the Place of so called `Translationtables`. That means the place where your `Translationfolder` reside.

_Simply place this code snippet somewhere into your configs_  
~~~php
$aConfig['APP'] = array(
    // path
    'GETTEXT' => '/var/www/html/translation',
    // default language 
    'LANG' => 'de_DE'
);
~~~

The structure of the Translationtables must follow the official declaration, e.g.
~~~
/var/www/html/translation/
└── de_DE
    └── LC_MESSAGES
        ├── backend.mo
        └── backend.po
~~~

**Examples** 

In all Examples, the File `backend` (`backend.mo`) will be consulted for Translation.

_Simple Output of a String_    
~~~html
{'Frontend'|gtext:'backend'}
~~~
~~~html
{'Frontend'|gtext:'backend':'de_DE'}
~~~

_Output using sprintf to set values dynamically_  
~~~html
{'(changed Menutitel; Original is `%s`)'|gtext:'backend'|sprintf:'123'}
{'Page %s of %s Pages'|gtext:'backend'|sprintf:'7':'100'}
~~~

_Output with Translation into a certain Language_  
~~~html
{'Desktop'|gtext:'backend':'it_IT'}
~~~

---

### `smarty_modifier_hasValue` <a id="smarty_modifier_hasValue"></a>

checks whether there is value or not; Everything is a value, except `null` and empty string `''`.

~~~
smarty_modifier_hasValue($mValue) : bool
~~~

**Examples**

~~~html
{0|hasValue}
~~~
~~~
true
~~~

~~~html
{null|hasValue}
~~~
~~~
false
~~~

~~~html
{''|hasValue}
~~~
~~~
false
~~~

---

### `smarty_modifier_highlight_html` <a id="smarty_modifier_highlight_html"></a>

returns `<tag>`-encapsulated, highlighted html markup.

~~~
smarty_modifier_highlight_html(string $sMarkup = '', string $sTag = 'code', bool $bPurify = false) : string
~~~

**Examples**

_default_  
~~~html
{'<h1>Title</h1><p>Test</p>'|highlight_html:"span"}
~~~
~~~
<span>&lt;<span style="color:#d02">h1</span>&gt;Title&lt;/<span style="color:#d02">h1</span>&gt;&lt;<span style="color:#d02">p</span>&gt;Test&lt;/<span style="color:#d02">p</span>&gt;</span>
~~~
<span>&lt;<span style="color:#d02">h1</span>&gt;Title&lt;/<span style="color:#d02">h1</span>&gt;&lt;<span style="color:#d02">p</span>&gt;Test&lt;/<span style="color:#d02">p</span>&gt;</span>


_with auto-repair broken html; option `true`_  
~~~html
{'<h1>Title</h1><span>Test'|highlight_html:"span":true}
~~~
~~~
<span>&lt;<span style="color:#d02">h1</span>&gt;Title&lt;/<span style="color:#d02">h1</span>&gt;&lt;<span style="color:#d02">p</span>&gt;Test&lt;/<span style="color:#d02">p</span>&gt;</span>
~~~
<span>&lt;<span style="color:#d02">h1</span>&gt;Title&lt;/<span style="color:#d02">h1</span>&gt;&lt;<span style="color:#d02">p</span>&gt;Test&lt;/<span style="color:#d02">p</span>&gt;</span>

---

### `smarty_modifier_shrink` <a id="smarty_modifier_shrink"></a>

truncates a string evenly distributed at the front and back to the specified total length and inserts a string (filler) in between.

~~~
smarty_modifier_shrink(string $sString = '', int $iMaxChars = 255, string $sFiller = '…') : string
~~~

**Examples**

_default_  
~~~html
{'This is a very long, meaningless text used for demonstration purposes only'|shrinkLink:30}
~~~
~~~
This is a ver…purposes only
~~~

_using a custom filler string_  
~~~html
{'This is a very long, meaningless text used for demonstration purposes only'|shrinkLink:30:"_*_"}
~~~
~~~
This is a ver_*_purposes only
~~~

---

### `smarty_modifier_parsedown` <a id="smarty_modifier_parsedown"></a>

converts "markdown" syntax into markup.

~~~
smarty_modifier_parsedown(string $sMarkdown = '') : string
~~~

_Example_  
~~~html
{"# Hello World\n- one\n- two\n- three"|parsedown}
~~~

_Result_
~~~html
<h1>Hello World</h1>
<ul>
    <li>one</li>
    <li>two</li>
    <li>three</li>
</ul>
~~~




















