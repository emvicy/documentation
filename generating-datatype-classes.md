
# Generating DataType Classes

Emvicy includes a Generator that you can use to generate DataType Classes. Consider those Classes more than a Storage than a Logic Element.  
Define what name the class and which namespace it should have, which properties or constants it should provide.  
Then just run the Generator and it will create the Class for you.

- [Configuration](#Configuration)
  - [Array Notation](#array_config)
  - [Object Notation](#object_config)
- [Creation](#Creation)
- [Note regarding php data types](#Hint)

------------------------------------------------------------------------------------------------------------------------

## Configuration <a id="Configuration"></a>

Write your own Configurations.

_Place for DataType Generating Configurations; (assuming module `Foo`)_  
~~~
modules/Foo/etc/config/Foo/config/_datatype.php
~~~

### Configuration as Array <a id="array_config"></a>

_Example file `modules/Foo/etc/config/DataType/datatype.php`_    
~~~php
<?php

/**
 * @usage php emvicy datatype:all
 *        php emvicy datatype:module Foo 
 *        Classes created by this script are placed into folder: `/modules/{module}/DataType/`
 */

#---------------------------------------------------------------
#  Defining DataType Classes

$sThisModuleDir = realpath(__DIR__ . '/../../../../');
$sThisModuleName = basename($sThisModuleDir);
$sThisModuleDataTypeDir = $sThisModuleDir . '/DataType';
$sThisModuleNamespace = str_replace('/', '\\', substr($sThisModuleDataTypeDir, strlen($aConfig['MVC_MODULES_DIR'] . '/')));

// base setup
$aDataType = array(

    // directory
    'dir' => $sThisModuleDataTypeDir,

    // remove complete dir before new creation
    'unlinkDir' => false,

    // enable creation of events in datatype methods
    'createEvents' => true,

    'class' => array(),
);

// classes
$aDataType['class']['DTFoo'] = array(    
    'name' => 'DTFoo',      # mandatory
    'file' => 'DTFoo.php',  # mandatory

    // optional; no need to even note the key here if not used
    'extends' => '', # e.g. '\MVC\DataType\DTRoutingAdditional'

    // optional; no need to even note the key here if not used
    'namespace' => $sThisModuleNamespace,

    // optional; add some useful Helper Methods like '__toString()` method (default: true)
    'createHelperMethods' => true,

    // optional; no need to even note the key here if not used
    'constant' => array(
        array(
            'key' => 'FOO',
            'value' => 'BAR',
            'visibility' => 'public'
        )
    ),

    'property' => array(
        array(
            'key' => 'sFoo',   # mandatory
            'var' => 'string', # mandatory  

            // optional property settings
            'nullable' => true,
            'value' => 'bar',
            'visibility' => 'protected',                    
            'static' => false,
            'setter' => true,
            'getter' => true,
            'explicitMethodForValue' => false,
            'listProperty' => true,
            'createStaticPropertyGetter' => true,
            'setValueInConstructor' => true,
            'forceCasting' => true,
            'required' => true,
            'addMyMVCEvents' => true,
        ),
        array('key' => 'sKey'               , 'var' => 'string'),
        array('key' => 'iDeliverable'       , 'var' => 'int'),
        array('key' => 'aJsonContext'       , 'var' => 'array'),
        array('key' => 'bSuccess'           , 'var' => 'bool'),
    )
);

#---------------------------------------------------------------
# copy settings to module's config
# in your code you can access this datatype config by: \MVC\Config::MODULE()['DATATYPE'];

$aConfig['MODULE'][$sThisModuleName]['DATATYPE'] = $aDataType;
~~~

------------------------------------------------------------------------------------------------------------------------

### Configuration as Object <a id="object_config"></a>

You can also create a DataType class the object way. 

_Example_
~~~php
// config
$oDTConfig = \MVC\DataType\DTConfig::create()
    ->set_dir(\MVC\Config::get_MVC_MODULES_DIR() . '/' . \MVC\Config::get_MVC_MODULE_PRIMARY_NAME() . '/DataType/')
    ->set_unlinkDir(false)
    ->set_createEvents(true)
    ->add_class(
        \MVC\DataType\DTClass::create()
            ->set_name('DTFoo')
            ->set_file('DTFoo.php')
            ->set_extends('\MVC\DataType\DTRoutingAdditional')
            ->set_namespace(\MVC\Config::get_MVC_MODULE_PRIMARY_NAME() . '\DataType')
            ->set_createHelperMethods(true)
            ->add_DTConstant(
                \MVC\DataType\DTConstant::create()
                    ->set_key('FOO')
                    ->set_value('"BAR"')
                    ->set_visibility('public')
            )
            ->add_DTProperty(
                \MVC\DataType\DTProperty::create()
                    ->set_key('bSuccess')
                    ->set_var('bool')

                    // optional property settings
                    ->set_nullable(true)
                    ->set_value(true)
                    ->set_visibility('protected')
                    ->set_static(false)
                    ->set_setter(true)
                    ->set_getter(true)
                    ->set_explicitMethodForValue(false)
                    ->set_listProperty(true)
                    ->set_createStaticPropertyGetter(true)
                    ->set_setValueInConstructor(true)
                    ->set_forceCasting(true)
                    ->set_required(true)
                    ->set_addMyMVCEvents(true)
            )
    )
;
// generate
$oDTGenerator = \MVC\Generator\DataType::create()->initConfigObject($oDTConfig);
~~~

------------------------------------------------------------------------------------------------------------------------

## Creation <a id="Creation"></a>

create DataType class files for your module by executing this command:

_creates datatype classes for module `Foo`_
~~~bash
php emvicy datatype:module Foo
~~~

create DataType class files for all modules by executing this command:

_creates datatype classes for all modules_
~~~bash
php emvicy datatype:all
~~~

------------------------------------------------------------------------------------------------------------------------

## Note regarding php data types <a id="Hint"></a>

- use `bool`, not ~~`boolean`~~
- use `int`, not ~~`integer`~~
