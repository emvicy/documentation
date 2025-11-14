
# Strings

- [~~createPassword~~](#createPassword)
- [cutOff](#cutOff)
- [getJson](#getJson)
- [highlight_html](#highlight_html)
- [isJson](#isJson)
- [isMarkup](#isMarkup)
- [isUtf8](#isUtf8)
- [isUuid4](#isUuid4)
- [~~markdown~~](#markdown)
- [parsedown](#parsedown)
- [random](#random)
- [removeDoubleDotSlashesFromString](#removeDoubleDotSlashesFromString)
- [replaceMultipleForwardSlashesByOneFromString](#replaceMultipleForwardSlashesByOneFromString)
- [seofy](#seofy)
- [tidy](#tidy)
- [ulli](#ulli)
- [uuid4](#uuid4)

---

## ~~`createPassword`~~ <a id="createPassword"></a>

@deprecated use instead: [Strings::random()](#random)

---

## `cutOff` <a id="cutOff"></a>

cuts off a string at given limit, appends a custom string if string to cut off is longer than limit, can purify broken markup string before return.

~~~
Strings::cutOff(string $sString = '', int $iLimit = 0, string $sAppendix = ' […]', bool $bPurify = true) : string
~~~

_Example: cutOff at char 50, No Appendix, with `bPurify: true` (repair broken html string)_
~~~php
$sString = Strings::cutOff(
    '<p><strong>Lorem ipsum dolor sit <i>amet</i>, consectetur adipiscing elit.</strong></p>',
    50,
    sAppendix: '',
    bPurify: true
);
~~~

_Result of `$sString`_
~~~html
// type: string
'<p><strong>Lorem ipsum dolor sit <i>amet</i>, cons</strong></p>'
~~~

---

## `getJson` <a id="getJson"></a>

parses JSON out of a mixed String and returns Array with detected JSON.

~~~
Strings::getJson(string $sString = '', bool $bReturnValidJsonOnly = true) : array
~~~

_Example_  
~~~php
$sString = 'foo bar {"john": "doe"} baz example {"jane":"baz"} whatever';
$aJson = Strings::getJson($sString);
~~~

_Result of `$aJson`_
~~~html
// type: array, items: 2
[
    0 => '{"john": "doe"}',
    1 => '{"jane":"baz"}',
]
~~~

---

## `highlight_html` <a id="highlight_html"></a>

returns `$sTag`-encapsulated, highlighted html markup.

~~~
Strings::highlight_html(string $sMarkup = '', string $sTag = 'code', bool $bPurify = false) : string
~~~

*Example: with `bPurify: true` (repair broken html string)*    
~~~php
$sString = Strings::highlight_html(
            '<p><strong>Lorem ipsum dolor sit <i>amet</i>, consectetur adipiscing elit.</strong>',
            bPurify: true
        );
~~~

_Result of `$sString`_  
~~~html
// type: string
'<code>&lt;<span style="color:#d02">p</span>&gt;&lt;<span style="color:#d02">strong</span>&gt;Lorem&nbsp;ipsum&nbsp;dolor&nbsp;sit&nbsp;&lt;
    <span style="color:#d02">i</span>&gt;amet&lt;/<span style="color:#d02">i</span>&gt;,&nbsp;consectetur&nbsp;adipiscing&nbsp;elit.&lt;/
    <span style="color:#d02">strong</span>&gt;&lt;/<span style="color:#d02">p</span>&gt;</code>'
~~~

---

## `isJson` <a id="isJson"></a>

checks whether a string is json.

~~~
Strings::isJson(string $sString = '') : bool
~~~

_Example_  
~~~php
// true
$bIsJson = Strings::isJson(
    '{"foo":"bar"}'
);
~~~

---

## `isMarkup` <a id="isMarkup"></a>

checks whether a string contains markup.

~~~
Strings::isMarkup(string $sString = '', bool $bPurify = true) : bool
~~~

_Example_  
~~~php
// true
$bIsMarkup = Strings::isMarkup(
    '<p><strong>Lorem ipsum dolor sit <i>amet</i>, consectetur adipiscing elit.</strong>',
);
~~~

---

## `isUtf8` <a id="isUtf8"></a>

checks whether a string is utf8.

~~~
Strings::isUtf8(string $sString = '') : bool
~~~

_Example_
~~~php
// true
$bIsUtf8 = Strings::isUtf8('Straßenfest in München !');
~~~

---

## `isUuid4` <a id="isUuid4"></a>

checks whether a param is a valid uuid Version4 string.

~~~
Strings::isUuid4(mixed $sUuid4): bool
~~~

_Example_  
~~~php
// true
$bIsUuid4 = Strings::isUuid4('6c07e320-b33d-4ce0-9fae-96c59861d51f');
~~~
   
---

## ~~`markdown`~~ <a id="markdown"></a>

@deprecated use instead: [Strings::parsedown()](#parsedown)

converts markdown syntax into markup.

---

## `parsedown` <a id="parsedown"></a>

converts "markdown" syntax into markup.

~~~
Strings::parsedown(string $sMarkdown) : string
~~~

_Example_  
~~~php
$sMarkup = Strings::parsedown("# Hello World\n- one\n- two\n- three");
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



---

## `random` <a id="random"></a>

creates a random string.

~~~
Strings::random(
        int $iLength = 16,
        string $sCharSpecial = '#*!$.',
        int $iMandatoryStringLower = 1,
        int $iMandatoryStringUpper = 1,
        int $iMandatoryInt = 1,
        int $iMandatorySpecial = 1,
        string $sCharLower = 'abcdefghijklmnopqrstuvwxyz',
        string $sCharUpper = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ',
        string $sCharInt = '123467890'
    ) : string
~~~

**Examples**

_create a 16 char random string;  
it contains at least (for minimum): 1 lower a-z char, 1 upper A-Z char, 1 int number, 1 special char_
~~~php
$sRandom = Strings::random();
~~~
~~~
// type: string
$cZY/iZncdgWwC83
~~~

_create a 16 char random string;  
it contains at least 7 int numbers for minimum_
~~~php
$sRandom = Strings::random(
    iMandatoryInt: 7
);
~~~
~~~
// type: string
1du72U*4c096hS43
~~~

_create a 5 char random string;  
but only lower a-z chars_
~~~php
$sRandom = Strings::random(
    iLength: 5,
    iMandatoryStringLower: 5,
    iMandatoryStringUpper: 0,
    iMandatoryInt: 0,
    iMandatorySpecial: 0,
);
~~~
~~~
// type: string
khvsf
~~~

_create a 5 char random string;  
but only upper A-Z chars_
~~~php
$sRandom = Strings::random(
    iLength: 5,
    iMandatoryStringLower: 0,
    iMandatoryStringUpper: 5,
    iMandatoryInt: 0,
    iMandatorySpecial: 0,
);
~~~
~~~
// type: string
TGMPD
~~~

---

## `removeDoubleDotSlashesFromString` <a id="removeDoubleDotSlashesFromString"></a>

removes all doubleDot+Slashes (`../`) from a string.

~~~
Strings::removeDoubleDotSlashesFromString(string $sString = '') : string
~~~

_Example_  
~~~php
// string containing multiple `../` 
$sString = '../../../path/to/foo/';

// remove
$sString = Strings::removeDoubleDotSlashesFromString($sString);

// Result: path/to/foo/
var_dump($sString);
~~~

---

## `replaceMultipleForwardSlashesByOneFromString` <a id="replaceMultipleForwardSlashesByOneFromString"></a>

replaces multiple forwardSlashes (e.g.: `//`, `///`, `////`, etc.) from string by a single forwardSlash.  

~~~
Strings::replaceMultipleForwardSlashesByOneFromString(string $sString = '', bool $bIgnoreProtocols = false) : string
~~~

_Example_
~~~php
// string containing multiple `../` 
$sString = '../../../path//to///foo////';

// remove
$sString = Strings::replaceMultipleForwardSlashesByOneFromString($sString);

// Result: '../../../path/to/foo/'
var_dump($sString);
~~~

---

## `seofy` <a id="seofy"></a>

replaces special chars, umlauts by `-` (or given char).

~~~
Strings::seofy(string $sString = '', string $sReplacement = '-', bool $bStrToLower = true) : string
~~~

_Example_  
~~~php
$sSeofied = Strings::seofy('Straßenfest in München !');
~~~

_Result_  
~~~
strassenfest-in-muenchen
~~~

---

## `tidy` <a id="tidy"></a>

cleans up a string by removing newlines, multiple whitespaces.

~~~
Strings::tidy(string $sString = '')
~~~

_Example_  
~~~php
$sTidy = Strings::tidy("<p><strong>
Lorem ipsum dolor sit <i>amet</i>,          
            consectetur adipiscing elit.
    
</strong></p>");
~~~

_Result_
~~~
<p><strong> Lorem ipsum dolor sit <i>amet</i>, consectetur adipiscing elit. </strong></p>
~~~

---

## `ulli` <a id="ulli"></a>

creates a markup html `<ul>/<li>` list on given data (string|array).

~~~
Strings::ulli(mixed $aData, array $aConfig = array('ul' => 'list-group', 'li' => 'text-break list-group-item bg-transparent', 'spanPrimary' => 'text-primary', 'spanInfo' => 'text-info', 'allocator' => '=>', 'arrayIdentifier' => 'array(...')) : string
~~~

_Example_  
~~~php
$sUlli = Strings::ulli(
    Config::MODULE()['DATATYPE']['class']['DTRoutingAdditional']
);
~~~
_Result of `$sUlli`_  
~~~html
<ul class="list-group">
    <li class="text-break list-group-item bg-transparent"><span class="text-primary">name</span> <span class="text-info">=></span> DTRoutingAdditional</li>
    <li class="text-break list-group-item bg-transparent"><span class="text-primary">file</span> <span class="text-info">=></span> DTRoutingAdditional.php</li>
    <li class="text-break list-group-item bg-transparent"><span class="text-primary">extends</span> <span class="text-info">=></span> \\MVC\\DataType\\DTRoutingAdditional</li>
    <li class="text-break list-group-item bg-transparent"><span class="text-primary">namespace</span> <span class="text-info">=></span> Foo\\DataType</li>
    <li class="text-break list-group-item bg-transparent"><span class="text-primary">createHelperMethods</span> <span class="text-info">=></span> 1</li>
    <li class="text-break list-group-item bg-transparent"><span class="text-primary">constant</span> <span class="text-info">=></span> </li>
    <li class="text-break list-group-item bg-transparent"><span class="text-primary">property</span> <span class="text-info">=></span> </li>
</ul>
~~~

---

## `uuid4` <a id="uuid4"></a>

returns a random uuid Version4 string (8-4-4-4-12).

~~~
Strings::uuid4() : string
~~~

_Example_  
~~~php
$sUuid = Strings::uuid4();
~~~

_Result_  
~~~
889abaf2-461d-42a1-86f4-07eb3e9876a5
~~~
