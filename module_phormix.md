# Phormix

a PHP HTML-Forms Checker and Validator module  
for Emvicy2 PHP Framework: https://github.com/emvicy/Emvicy/tree/2.x

## Overview

- [Installation](#Installation)
- [Usage](#Usage)
    - [1. declare Form Elements](#1)
        - [1.1 Examples](#1-1)
        - [1.2 Using Variables](#1-2)
    - [2. declare a `formular.yaml`](#2)
    - [3. run Phormix inside your Controller method](#3)
        - [3.1. `single-page` formular](#3-1)
        - [3.2. `multi-page` formular (chained forms)](#3-2)
- [Modify element config](#Modify)
- [Demo](#Demo)

---

## Installation <a id="Installation"></a>

_cd into the modules folder of your `Emvicy` copy; e.g.:_
~~~bash
cd /var/www/html/modules/;
~~~

_clone `Phormix`_
~~~bash
git clone --branch 1.x https://github.com/emvicy/Phormix.git Phormix;
~~~

---

## Usage <a id="Usage"></a>

### 1. declare Form Elements <a id="1"></a>

declare all distinct elements of your formular in a single `yaml` file.  
Put all your `yaml` files in a separate folder.

Each declaration...

should have:

- `label`: (string)
- `description`: (string)

must have:

- `tag`: `input`|`select`|`textarea`
- `attribute`: (array) HTML attributes

may have:

- `filter`:
    - `validate`: (array)


#### 1.1 Examples <a id="1-1"></a>

*`MAX_FILE_SIZE.yaml`*
~~~yaml
label: &element.MAX_FILE_SIZE.label MAX_FILE_SIZE
description: &element.MAX_FILE_SIZE.description 'You can upload files up to 10 MB in size'
tag: input
attribute:
  type: hidden
  name: MAX_FILE_SIZE
  value: &element.MAX_FILE_SIZE.value 10485760 # value in Bytes; equals to 10 MB
  required: true
~~~

_`Firstname.yaml`_
~~~yaml
label: &element.Firstname.label Firstname
description:
tag: input
attribute:
  type: text
  id: &element.Firstname.attribute.id Firstname
  name: *element.Firstname.attribute.id
  value: ''
  title: *element.Firstname.label
  placeholder: *element.Firstname.label
  minLength: &element.Firstname.attribute.minLength 3
  maxlength: &element.Firstname.attribute.maxlength 50
  autofocus: false
  autocomplete: false
  required: false
  disabled: false
filter:
  validate:
    minLength:
      value: *element.Firstname.attribute.minLength
      message:
        fail: Please enter at least %s characters.
        success: You have specified more than the required minimum amount of %s characters.
    empty:
      value: false
      message:
        fail: This field can not be empty. Please fill in.
    regex:
      value: "/^[\\p{L}\\p{Zs}\\p{M}\\p{Pd}\\p{Ps}\\p{Pe}\\p{Pc}.]+$/u"
      message:
        fail: Your entry includes not authorized characters
        success: Your entry includes only authorized characters.
~~~

see folder `Phormix/element` for all examples.

---

#### 1.2 Using Variables <a id="1-2"></a>

you can "save" values into a variable so that you can reuse that variable later again.

**Example**

_here the value `Firstname` is saved into the variable `element.Firstname.label` by adding `&` before_
~~~yaml
label: &element.Firstname.label Firstname
~~~

_you can then access the value of that variable by adding `*` before_
~~~yaml
title: *element.Firstname.label
~~~

---

### 2. declare a `formular.yaml` <a id="2"></a>

here you declare your formular and which elements it contains.

_`formular.yaml`_
~~~yaml
form:
  id: &form.id Profile
  name: *form.id
  action: ""
  method: post
element:
  # names have to match to existing equal named files (without suffix) in $_sElementDirectory (see /Phormix/element/)
  - Salutation
  - Firstname
  - Surname
  - Company
  - Street
  - Postcode
  - City
  - Telephone
  - Email
  - Message
  - Captcha
~~~
- names beneath `element` have to match to existing equal named files (without suffix) in the Element Directory

---

### 3. run Phormix inside your Controller method <a id="3"></a>

#### 3.1. single-page formular <a id="3-1"></a>

_Inside your Controller method_
~~~php
// start
$oPhormix = Phormix::init(
    DTPhormixSetup::create()
        ->set_sConfigYamlFile('/path/to/formular.yaml')
        ->set_sElementDirectory('/path/to/element/folder/')
        ->set_sValidateClass('\Phormix\Model\PhormixValidate')
); 
    
// run Phormix
$oPhormix->run();

// form was successfully sent + validated
if (true === $oPhormix->bSuccess)
{
    // get Data Array
    $aData = $oPhormix->getDataAccepted();
    
    // get uploaded Files
    $aFiles = $oPhormix->getFilesAccepted();
    
    // reset
    $oPhormix->reset(bForce: true);  
}

// assign to view
view()->assign('oPhormix', $oPhormix);
view()->assign('oDTRoute', $oDTRoute);
view()->assign('aData', ($aData ?? array()));
view()->assign('aFiles', ($aFiles ?? array()));
view()->autoAssign();
~~~

#### 3.2. multi-page formular (chained forms) <a id="3-2"></a>

_Inside your Controller method_
~~~php
// start
$oDTPhormixChain = DTPhormixChain::create()
    ->add_aDTPhormixSetup(
      DTPhormixSetup::create()
          ->set_sLabel('Name / Company')
          ->set_sConfigYamlFile('/path/to/formular_1.yaml')
          ->set_sElementDirectory('/path/to/element/folder/')
          ->set_sValidateClass('\Phormix\Model\PhormixValidate'))
    ->add_aDTPhormixSetup(
      DTPhormixSetup::create()
          ->set_sLabel('Address')
          ->set_sConfigYamlFile('/path/to/formular_2.yaml')
          ->set_sElementDirectory('/path/to/element/folder/')
          ->set_sValidateClass('\Phormix\Model\PhormixValidate'))
    ->add_aDTPhormixSetup(
      DTPhormixSetup::create()
          ->set_sLabel('Submit data')
          ->set_sConfigYamlFile('/path/to/formular_3.yaml')
          ->set_sElementDirectory('/path/to/element/folder/')
          ->set_sValidateClass('\Phormix\Model\PhormixValidate'));

$oPhormixChain = new PhormixChain($oDTRequestIn, $oDTPhormixChain);
$oPhormix = $oPhormixChain->getPhormix();
$oPhormix = $oPhormixChain->setActionOnRoutePath($oDTRoute, $oPhormix);
$oPhormix = $oPhormixChain->proceed($oPhormix);

if (true === $oPhormix->bSuccess)
{
    // get Data Array
    $aData = $oPhormixChain->getDataAccepted();

    // get uploaded Files
    $aFiles = $oPhormixChain->getFilesAccepted();

    // reset
    $oPhormixChain->reset($oPhormix);
}

view()->assign('oDTPhormixChain', $oDTPhormixChain);
view()->assign('oPhormix', $oPhormix);
view()->assign('oDTRoute', $oDTRoute);
view()->assign('aData', ($aData ?? array()));
view()->assign('aFiles', ($aFiles ?? array()));
view()->autoAssign();
~~~

---

### Templating

#### auto-creating a html formular

*`modules/Phormix/templates/phormix/phormix_formular.tpl`*
~~~html
<!--form-->
{if false === $oPhormix->bSuccess}
<form {$oPhormix->getMarkupFormAttributes()}>
  <fieldset>
    <legend>{$oPhormix->aConfig.form.name}</legend>

    {$oPhormix->getMarkupFormIdentifier()}
    {$oPhormix->getMarkupTicket()}

    {foreach item=element from=$oPhormix->aConfig.element}
    <div class="mb-3">
      {if 'input' === $element.tag}
      {if true === isset($element.attribute['data-element']) && 'input_captcha' === $element.attribute['data-element']}
      {include file="phormix/phormix_input_captcha.tpl"}
      {elseif 'checkbox' === $element.attribute.type}
      {include file="phormix/phormix_input_checkbox.tpl"}
      {elseif 'radio' === $element.attribute.type}
      {include file="phormix/phormix_input_radio.tpl"}
      {elseif 'file' === $element.attribute.type}
      {include file="phormix/phormix_input_file.tpl"}
      {elseif 'hidden' === $element.attribute.type}
      {include file="phormix/phormix_input_hidden.tpl"}
      {else}
      {include file="phormix/phormix_input_default.tpl"}
      {/if}
      {elseif 'select' === $element.tag}
      {include file="phormix/phormix_select.tpl"}
      {elseif 'textarea' === $element.tag}
      {include file="phormix/phormix_textarea.tpl"}
      {/if}
    </div>
    {/foreach}
    <button type="submit" class="btn btn-primary" style="width: 100%;">Submit</button>
  </fieldset>
</form>
{/if}
<!--/form-->
~~~

---

#### Messages

**Errors**

- when something went wrong

~~~html
{if false === empty($oPhormix->getErrorArray())}
<!--error-->
<ul class="list-unstyled">
  {foreach key=sKey item=sItem from=$oPhormix->getErrorArray()}
  {if !is_array($sItem)}
  <li class="alert alert-danger">
    <a href="#{$sKey}">{$sItem|escape}</a>
  </li>
  {/if}
  {/foreach}
</ul>
<!--/error-->
{/if}
~~~

**Missings**

- if e.g. an element is marked as `required` but was not sent

~~~html
{if false === empty($oPhormix->getMissingArray())}
<!--missing-->
<ul class="list-unstyled">
  {foreach key=sKey item=sItem from=$oPhormix->getMissingArray()}
  <li class="alert alert-warning">
    Missing: "{$sItem.label|escape}"
  </li>
  {/foreach}
</ul>
<!--/missing-->
{/if}
~~~

---

## Modify element config <a id="Modify"></a>

the cleanest way is to write a new element config file.

But you can also modify the element config to your needs on-the-fly. Make sure to
modify before calling `$oPhormix->run();`

**Examples**

_add filetype "text/plain" to the "Upload" element filter/validate (and leave the element config files unchanged)_
~~~php
[..]
    
$oPhormix->aConfig['element']['Upload']['filter']['validate']['filetype']['value'][] = array(
  'label' => 'Text', 
  'value' => 'text/plain'
);
            
// run Phormix
$oPhormix->run();

[..]
~~~

_modify max filesize to 2MB (and leave the element config files unchanged)_
~~~php
[..]

// modify "MAX_FILE_SIZE" element filesize and "description" text
$oPhormix->aConfig['element']['MAX_FILE_SIZE']['attribute']['value'] = 2097152; # 2MB
$oPhormix->aConfig['element']['MAX_FILE_SIZE']['description'] = 'You can upload files up to ' . $oPhormix->aConfig['element']['MAX_FILE_SIZE']['attribute']['value'] . ' Bytes in size';

// modify "Upload" element filesize and "description" text
$oPhormix->aConfig['element']['Upload']['attribute']['required'] = true;
$oPhormix->aConfig['element']['Upload']['description'] = $oPhormix->aConfig['element']['MAX_FILE_SIZE']['description'];
$oPhormix->aConfig['element']['Upload']['filter']['validate']['filemaxfilesize']['value'] = $oPhormix->aConfig['element']['MAX_FILE_SIZE']['attribute']['value'];
            
// run Phormix
$oPhormix->run();

[..]
~~~

---

## Demo <a id="Demo"></a>

for a running Demo in your App, add the phormix routing folder by adding    
the following lines to your primary module config, .e.g.: `modules/Foo/etc/config/_mvc.php`.

*`modules/Foo/etc/config/_mvc.php`*
~~~php
#-----------------------------------------------------------------------------------------------------------------------
# Phormix

// add Phormix routing dir
$aConfig['MVC_ROUTING_DIR'][] = realpath(__DIR__ . '/../../../') . '/Phormix/etc/routing';
~~~
- adjust the realpath if necessary

after that you can call the Route `/phormix/` in your Browser.

---

## License

**Font used for Captcha**

- "Educational Gothic V2" (EducationalGothic-Regular.otf)
    - Copyright © XYZ Co. Inc.
    - Version 1.2.3.4
    - License: GNU General Public License v3.0 (see `etc/config/Phormix/config/Educational_Gothic_V2/LICENSE.txt`)