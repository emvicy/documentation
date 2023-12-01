
# Database ⛁

- [1. Credentials](#1)
- [2. Creation](#2)
    - [2.1. Create DB Config](#2-1)
    - [2.2. Creating a concrete Table Class](#2-2)
    - [2.3. Creating a DBInit class that is used for each DB access](#2-3)
    - [2.4. Let generate an openapi yaml schema file for data type classes](#2-4)
- [3. Usage](#3)
    - [3.1. create](#3-1)
    - [3.2. retrieve](#3-2)
    - [3.3. update](#3-3)
    - [3.4. delete](#3-4)
    - [3.5. count](#3-5)
    - [3.6. checksum](#3-6)
    - [3.7. getFieldInfo](#3-7)
    - [3.8. SQL](#3-8)
- [4. Events](#4)
    - [4.1. Logging SQL](#4-1)

---

<a id="1"></a>
## Credentials

Edit the `db.*` settings in your `/.env` file.

~~~bash
#-----------------------------------------------------
# My Application

# Environment
MVC_ENV=develop


#-----------------------------------------------------
# DB
db.type=mysql
db.host=127.0.0.1
db.port=3306
db.dbname=Emvicy1x
db.username=root
db.password=
db.charset=utf8
db.collation=utf8_unicode_ci
db.prefix=
~~~

<a id="2"></a>
## Creation

<a id="2-1"></a>
### 2.1. Create DB Config


In your main module's config environment folder edit your DB Config.
(@see [/1.x/configuration#Modules-environment-config-file](/1.x/configuration#Modules-environment-config-file))

The main config file resides in `_db.php`.

Overwrite the settings from `_db.php` in your concrete environment config file e.g. `develop.php` (depends on what your `MVC_ENV` is set to.)

For example you might want to turn on Logging for develop environment:

_Db Config example for `develop` environments_
~~~php

//----------------------------------------------------------------------------------------------------------------------
// DB
// watch database log; e.g.:      cd /tmp; tail -f Foo*.log

require realpath(__DIR__) . '/_db.php';

// consider a logrotate mechanism for this logfile as it may grow quickly
$aConfig['MODULE']['Foo']['DB']['logging']['general_log'] = 'ON'; // consider to set it to ON for develop or test environments only

~~~


<a id="2-2"></a> 
### 2.2. Creating a concrete Table Class

_PHP Class_
as a Representation of the DB Table


_file: `modules/Foo/Model/Table/User.php`_
~~~php
<?php

namespace Foo\Model\Table;

use MVC\DB\DataType\DB\Foreign;
use MVC\DB\Model\Db;

class User extends Db
{
    /**
     * @var array
     */
    protected $aField = array();

    /**
     * @param array $aDbConfig
     * @throws \ReflectionException
     */
    public function __construct(array $aDbConfig = array())
    {
        $this->aField = array(
            'email'     => 'varchar(255)    COLLATE utf8_general_ci NOT NULL UNIQUE',
            'active'    => "int(1)          DEFAULT '0'             NOT NULL",
            'uuid'      => "varchar(36)     COLLATE utf8_general_ci NOT NULL UNIQUE COMMENT 'uuid permanent'",
            'uuidtmp'   => "varchar(36)     COLLATE utf8_general_ci NOT NULL UNIQUE COMMENT 'uuid; changes on create|login'",
            'password'  => 'varchar(60)     COLLATE utf8_general_ci NOT NULL',
            'nickname'  => "varchar(10)     COLLATE utf8_general_ci NOT NULL",
            'forename'  => "varchar(25)     COLLATE utf8_general_ci NOT NULL",
            'lastname'  => "varchar(25)     COLLATE utf8_general_ci NOT NULL",
        );

        // basic creation of the table
        parent::__construct(
            $this->aField,
            $aDbConfig
        );
    }
}
~~~

- creates the Table `FooModelTableUser`
    - Table has several fields from `email` ... `lastname` as declared in property `$aField`
        - 🛈 The Table fields `id`, `stampChange` and `stampCreate` are added automatically
        - do not add these fields by manually
- generates a DataType Class `\Foo\DataType\DTFooModelTableUser` in `modules/Foo/DataType/`

---

**Creating a Table and adding a Foreign Key**


_file: `modules/Foo/Model/Table/User.php`_
~~~php
<?php

namespace Foo\Model\Table;

use MVC\DB\DataType\DB\Foreign;
use MVC\DB\Model\Db;

class User extends Db
{
    /**
     * @var array
     */
    protected $aField = array();

    /**
     * @param array $aDbConfig
     * @throws \ReflectionException
     */
    public function __construct(array $aDbConfig = array())
    {
        $this->aField = array(
            'email'     => 'varchar(255)    COLLATE utf8_general_ci NOT NULL UNIQUE',
            'active'    => "int(1)          DEFAULT '0'             NOT NULL",
            'uuid'      => "varchar(36)     COLLATE utf8_general_ci NOT NULL UNIQUE COMMENT 'uuid permanent'",
            'uuidtmp'   => "varchar(36)     COLLATE utf8_general_ci NOT NULL UNIQUE COMMENT 'uuid; changes on create|login'",
            'password'  => 'varchar(60)     COLLATE utf8_general_ci NOT NULL',
            'nickname'  => "varchar(10)     COLLATE utf8_general_ci NOT NULL",
            'forename'  => "varchar(25)     COLLATE utf8_general_ci NOT NULL",
            'lastname'  => "varchar(25)     COLLATE utf8_general_ci NOT NULL",
        );

        // basic creation of the table
        parent::__construct(
            $this->aField,
            $aDbConfig
        );
        $this->setForeignKey(
            Foreign::create()
                ->set_sForeignKey('id_TableGroup')
                ->set_sReferenceTable('FooModelTableGroup')
        );
    }
}
~~~

- creates the Table `FooModelTableUser`
    - Table has several fields from `email` ... `lastname` as declared in property `$aField`
        - 🛈 The Table fields `id`, `stampChange` and `stampCreate` are added automatically
        - do not add these fields by manually
- The foreign key `id_TableGroup` -pointing to table `FooModelTableGroup`- is added by method `set_sForeignKey()`
- generates a DataType Class `\Foo\DataType\DTFooModelTableUser` in `modules/Foo/DataType/`

---

<a id="2-3"></a> 
### 2.3. Creating a DBInit class that is used for each DB access

_example: `modules/Foo/Model/DB.php`_  
~~~php
<?php

/**
 * - register your db table classes as static properties.
 * - add a doctype to each static property
 * - these doctypes must contain the vartype information about the certain class
 * @example
 *      @var Foo\Model\TableUser
 *      public static $oFooModelTableUser;
 * ---
 * [!]  it is important to declare the vartype expanded with a full path
 *      avoid to make use of `use ...` support
 *      otherwise the classes could not be read correctly
 */

namespace Foo\Model;

use DB\Model\DbInit;
use DB\Trait\DbInitTrait;

class DB extends DbInit
{
    use DbInitTrait;
    
    /**
     * @var \Foo\Model\TableUser
     */
    public static $oFooModelTableUser;
}
~~~

---

<a id="2-4"></a>  
### 2.4. Let generate an openapi yaml schema file for data type classes

This builds an openapi.yaml `DTTables.yaml` in the primary module's DataType folder based on data type classes of the DB tables.

if not already exists, create a file `db.php` in the event folder of your Emvicy module and declare the bindings as follows.

_example: `/modules/Foo/etc/event/db.php`_
~~~php
<?php

\MVC\Event::processBindConfigStack([

    'mvc.db.model.db.construct.saveCache' => [
        /*
         * let create an openapi yaml file according to DB Table DataType Classes when the DataBase Tables setup changes
         */
        function() {
            // one-timer
            if (false === \MVC\Registry::isRegistered('DB::openapi'))
            {
                \MVC\Registry::set('DB::openapi', true);
                // generate /modules/{MODULE}/DataType/DTTables.yaml
                $sYamlFile = \MVC\DB\Model\Openapi::createDTYamlOnDTClasses(
                    // pass instance of your concrete DB Class
                    \Foo\Model\DB::init()
                );
            }
        },
    ],
]);
~~~

---

<a id="3"></a>
## 3. Usage

In your main **Controller** class just create a new Instanciation of your DBInit class.
A good place is the `__construct()` method.

~~~php
namespace Foo\Controller;

use Foo\Model\DB;

public function __construct ()
{
    DB::init();
}
~~~

after that you can access your TableClass from everywhere - even from frontend templates:


_Usage_
~~~php
DB::$oFooModelTableUser->...<method>...
~~~

<a id="3-1"></a>  
### 3.1. create

therefore an object of its related Datatype must be instanciated and given to the method `create`.
Here e.g. with Datatype "DTFooModelTableUser" to TableClass "modules/Foo/Model/Table/User":

~~~php
// create DataType object
$oDTFooModelTableUser = DTFooModelTableUser::create()
    ->set_id_TableGroup(1)
    ->set_email('foo@example.com')
    ->set_password('...password...')
    ->set_forename('foo')
    ->set_lastname('bar')
    ->set_nickname('foo')
    ->set_uuid(Strings::uuid4())
    ->set_uuidtmp(Strings::uuid4())
    ->set_active(1);
        
// put DataType object into Table and get updated object back        
$oDTFooModelTableUser = DB::$oFooModelTableUser->create(
    $oDTFooModelTableUser
);

// if successful, `$oDTFooModelTableUser` now has an id (auto increment of database table) set
$iId = $oDTFooModelTableUser->get_id();
~~~


<a id="3-2"></a>  
#### 3.2. retrieve

`retrieveTupel` asks for a specific Tupel and returns the DataType Object according to the requested Table.

_`retrieveTupel` - identified by `id`_
~~~php
/** @var \Foo\DataType\DTFooModelTableUser $oDTFooModelTableUser */
$oDTFooModelTableUser = DB::$oFooModelTableUser->retrieveTupel(
    DTFooModelTableUser::create()
        ->set_id(2)
)
~~~
- get User Object whose id=2


`retrieve` returns an array of DataType Objects according to the requested Table.

_`retrieve`: get all Datasets_
~~~php
/** @var \Foo\DataType\DTFooModelTableUser[] $aDTFooModelTableUser */
$aDTFooModelTableUser = DB::$oFooModelTableUser->retrieveTupel();
~~~

_`retrieve`: get specific Datasets_
~~~php
/** @var \Foo\DataType\DTFooModelTableUser[] $aDTFooModelTableUser */
$aDTFooModelTableUser = DB::$oFooModelTableUser->retrieve(
    DTArrayObject::create()
        ->add_aKeyValue(
            DTKeyValue::create()
                ->set_sKey('stampChange')
                ->set_mOptional1('LIKE')
                ->set_sValue('2021-06-19')
            );
);
~~~

_`retrieve`: get Datasets with sort order_
~~~php
/** @var \Foo\DataType\DTFooModelTableUser[] $aDTFooModelTableUser */
$aDTFooModelTableUser = DB::$oFooModelTableUser->retrieve(
    DTArrayObject::create()
        ->add_aKeyValue(
            DTKeyValue::create()
                ->set_sKey('email')
                ->set_mOptional1('LIKE')
                ->set_sValue('%@example.com%')
        ),
    DTArrayObject::create()
        ->add_aKeyValue(
            DTKeyValue::create()
                ->set_sValue('ORDER BY id ASC')
        )
);
~~~

_`retrieve`: get first 30 Datasets (LIMIT 0,30)_
~~~php
/** @var \Foo\DataType\DTFooModelTableUser[] $aDTFooModelTableUser */
$aDTFooModelTableUser = DB::$oFooModelTableUser->retrieve(
    null,
    DTArrayObject::create()
        ->add_aKeyValue(
            DTKeyValue::create()
                ->set_sValue('LIMIT 0,30')
        )
)
~~~


<a id="3-3"></a>  
#### 3.3. update


_`updateTupel`: update this specific Tupel - identified by `id`_
~~~php
/** @var boolean $bSuccess */
$bSuccess = DB::$oFooModelTableUser->updateTupel(
    DTFooModelTableUser::create()
        ->set_id(1)
        ->set_nickname('XYZ')
);
~~~
- the equivalent dataset tupel with object's `id` will be updated.

_`update`: update all Tupel which are affected by the where clause_
~~~php
/** @var boolean $bSuccess */
$bSuccess = DB::$oFooModelTableUser->update(
    DTFooModelTableUser::create()
        ->set_active('1'),
    // where
    DTArrayObject::create()
        ->add_aKeyValue(
            DTKeyValue::create()
                ->set_sKey('active')
                ->set_mOptional1('=')
                ->set_sValue('0')
        )
);
~~~


<a id="3-4"></a>  
#### 3.4. delete

_`deleteTupel`: delete this specific Tupel - identified by `id`_
~~~php
/** @var boolean $bSuccess */
$bSuccess = DB::$oFooModelTableUser->deleteTupel(
    DTFooModelTableUser::create()
        ->set_id(2)
)
~~~

_`delete`: delete all Tupel which are affected by the where clause_
~~~php
$bSuccess = DB::$oFooModelTableUser->delete(
    // where
    DTArrayObject::create()
        ->add_aKeyValue(
            DTKeyValue::create()
                ->set_sKey('stampCreate')
                ->set_mOptional1('<')
                ->set_sValue('2023-06-19 00:00:00')
        )
);
~~~

<a id="3-5"></a>  
### 3.5. count

~~~php
// Amount of all Datasets
$iAmount = DB::$oFooModelTableUser->count();

// Amount of specific Datasets
$iAmount = DB::$oFooModelTableUser->count(
    DTArrayObject::create()
        ->add_aKeyValue(
            DTKeyValue::create()
                ->set_sKey('stampChange')
                ->set_mOptional1('=')
                ->set_sValue('2021-06-19')
    )
);
~~~

<a id="3-6"></a>  
### 3.6. checksum

~~~php
// Returns a checksum of the table
$iChecksum = DB::$oFooModelTableUser->checksum();
~~~

<a id="3-7"></a>  
### 3.7. getFieldInfo

returns array with table fields info

~~~php
$aFieldInfo = DB::$oFooModelTableUser->getFieldInfo();
~~~

_example return_  
~~~
Array
(
    [id_TableGroup] => Array
        (
            [Field] => id_TableGroup
            [Type] => int(11)
            [Null] => YES
            [Key] => MUL
            [Default] => 
            [Extra] => 
            [php] => int
            [_php] => int
            [_type] => int
            [_typeValue] => 11
        )

    [email] => Array
        (
            [Field] => email
            [Type] => varchar(255)
            [Null] => NO
            [Key] => 
            [Default] => 
            [Extra] => 
            [php] => string
            [_php] => string
            [_type] => varchar
            [_typeValue] => 255
        )

    [active] => Array
        (
            [Field] => active
            [Type] => int(1)
            [Null] => NO
            [Key] => 
            [Default] => 0
            [Extra] => 
            [php] => int
            [_php] => int
            [_type] => int
            [_typeValue] => 1
        )

    [uuid] => Array
        (
            [Field] => uuid
            [Type] => varchar(36)
            [Null] => NO
            [Key] => 
            [Default] => 
            [Extra] => 
            [php] => string
            [_php] => string
            [_type] => varchar
            [_typeValue] => 36
        )

    [uuidtmp] => Array
        (
            [Field] => uuidtmp
            [Type] => varchar(36)
            [Null] => NO
            [Key] => 
            [Default] => 
            [Extra] => 
            [php] => string
            [_php] => string
            [_type] => varchar
            [_typeValue] => 36
        )

    [password] => Array
        (
            [Field] => password
            [Type] => varchar(60)
            [Null] => NO
            [Key] => 
            [Default] => 
            [Extra] => 
            [php] => string
            [_php] => string
            [_type] => varchar
            [_typeValue] => 60
        )

    [nickname] => Array
        (
            [Field] => nickname
            [Type] => varchar(10)
            [Null] => NO
            [Key] => 
            [Default] => 
            [Extra] => 
            [php] => string
            [_php] => string
            [_type] => varchar
            [_typeValue] => 10
        )

    [forename] => Array
        (
            [Field] => forename
            [Type] => varchar(25)
            [Null] => NO
            [Key] => 
            [Default] => 
            [Extra] => 
            [php] => string
            [_php] => string
            [_type] => varchar
            [_typeValue] => 25
        )

    [lastname] => Array
        (
            [Field] => lastname
            [Type] => varchar(25)
            [Null] => NO
            [Key] => 
            [Default] => 
            [Extra] => 
            [php] => string
            [_php] => string
            [_type] => varchar
            [_typeValue] => 25
        )
)
~~~

<a id="3-8"></a>  
#### 3.8. SQL

_`SQL` example `fetchRow`_
~~~php
public function getUserByNickname(string $sNickname = '')
{
    $sSql = "SELECT * FROM `FooModelTableUser` WHERE nickname = '" . $sNickname . "'";
    $aResult = DB::$oFooModelTableUser->fetchRow($sSql);

    /** @var DTFooModelTableUser[] $aDTFooModelTableUser */
    $aDTFooModelTableUser = array();

    foreach ($aResult as $aData)
    {
        $aDTFooModelTableUser[] = DTFooModelTableUser::create($aData);
    }
        
    return $aDTFooModelTableUser;
}
~~~
_`SQL` example `fetchAll`_
~~~php
public function getUserByNickname(string $sNickname = '')
{
    $sSql = "SELECT * FROM `FooModelTableUser` WHERE nickname = '" . $sNickname . "'";
    $aResult = DB::$oFooModelTableUser->fetchAll($sSql);

    /** @var DTFooModelTableUser[] $aDTFooModelTableUser */
    $aDTFooModelTableUser = array();

    foreach ($aResult as $aData)
    {
        $aDTFooModelTableUser[] = DTFooModelTableUser::create($aData);
    }
        
    return $aDTFooModelTableUser;
}
~~~

---

<a id="4"></a> 
## 4. Events

see [Database Events](/1.x/events#database_events)

<a id="4-1"></a>

### 4.1. Logging SQL

_enable sql logging in your config file_  
~~~php
// consider to set to true for develop environments only: logging request into MVC_LOG_FILE_SQL
$aConfig['MVC_LOG_SQL'] = true;            
~~~

if it not already exists, create a file `sql.php` in the event folder of your module
and declare the bindings as follows.

_`/modules/{MODULE}/etc/event/sql.php`_
~~~php
/*
 * log SQL Statements, if enabled via config
 */
if (true === \MVC\Config::get_MVC_LOG_SQL())
{
    \MVC\Event::processBindConfigStack([
        'mvc.db.model.db.create.sql' => array(function(\MVC\DataType\DTArrayObject $oDTArrayObject) {\MVC\Log::write($oDTArrayObject->getDTKeyValueByKey('sSql')->get_sValue(), \MVC\Config::get_MVC_LOG_FILE_SQL());}),
        'mvc.db.model.db.insert.sql' => array(function(\MVC\DataType\DTArrayObject $oDTArrayObject) {\MVC\Log::write($oDTArrayObject->getDTKeyValueByKey('sSql')->get_sValue(), \MVC\Config::get_MVC_LOG_FILE_SQL());}),
        'mvc.db.model.db.retrieve.sql' => array(function(\MVC\DataType\DTArrayObject $oDTArrayObject) {\MVC\Log::write($oDTArrayObject->getDTKeyValueByKey('sSql')->get_sValue(), \MVC\Config::get_MVC_LOG_FILE_SQL());}),
        'mvc.db.model.db.update.sql' => array(function(\MVC\DataType\DTArrayObject $oDTArrayObject) {\MVC\Log::write($oDTArrayObject->getDTKeyValueByKey('sSql')->get_sValue(), \MVC\Config::get_MVC_LOG_FILE_SQL());}),
        'mvc.db.model.db.delete.sql' => array(function(\MVC\DataType\DTArrayObject $oDTArrayObject) {\MVC\Log::write($oDTArrayObject->getDTKeyValueByKey('sSql')->get_sValue(), \MVC\Config::get_MVC_LOG_FILE_SQL());}),
        'mvc.db.model.db.createTable.sql' => array(function(\MVC\DataType\DTArrayObject $oDTArrayObject) {\MVC\Log::write($oDTArrayObject->getDTKeyValueByKey('sSql')->get_sValue(), \MVC\Config::get_MVC_LOG_FILE_SQL());}),
    ]);
}
~~~
