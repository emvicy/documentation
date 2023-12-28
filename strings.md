
# Strings

- [cutOff](#cutOff)
- [highlight_html](#highlight_html)
- [isJson](#isJson)
- [isMarkup](#isMarkup)
- [isUtf8](#isUtf8)
- [removeDoubleDotSlashesFromString](#removeDoubleDotSlashesFromString)
- [replaceMultipleForwardSlashesByOneFromString](#replaceMultipleForwardSlashesByOneFromString)
- [seofy](#seofy)
- [tidy](#tidy)
- [ulli](#ulli)
- [uuid4](#uuid4)

---

<a id="cutOff"></a>
## `cutOff`

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

<a id="highlight_html"></a>
## `highlight_html`

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

<a id="isJson"></a>
## `isJson`

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

<a id="isMarkup"></a>
## `isMarkup`

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

<a id="isUtf8"></a>
## `isUtf8`

checks whether a string is utf8.

~~~
Strings::isUtf8(string $sString = '') : bool
~~~

---

<a id="removeDoubleDotSlashesFromString"></a>
## `removeDoubleDotSlashesFromString`

~~~
Strings::removeDoubleDotSlashesFromString(string $sString = '') : string
~~~

---

<a id="replaceMultipleForwardSlashesByOneFromString"></a>
## `replaceMultipleForwardSlashesByOneFromString`

~~~
Strings::replaceMultipleForwardSlashesByOneFromString(string $sString = '', bool $bIgnoreProtocols = false) : string
~~~

---

<a id="seofy"></a>
## `seofy`

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

<a id="tidy"></a>
## `tidy`

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

<a id="ulli"></a>
## `ulli`

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

<a id="uuid4"></a>
## `uuid4`

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
