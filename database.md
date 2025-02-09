
# Database ⛁

- [1. Credentials](#1)
- [2. Creation](#2)
    - [2.1. Create DB Config](#2-1)
    - [2.2. Table Class](#2-2)
    - [2.3. Table Collection](#2-3)
    - [2.4. Let generate an openapi yaml schema file for data type classes](#2-4)
- [3. Usage](#3)
    - [3.1. create](#3-1)
    - [3.2. retrieve](#3-2)
    - [3.3. update](#3-3)
    - [3.4. delete](#3-4)
    - [3.5. count](#3-5)
    - [3.6. checksum](#3-6)
    - [3.7. getFieldInfo](#3-7)
    - [3.8. PDO](#3-8)
    - [3.9. SQL](#3-9)
    - [3.10. Comment](#3-10)
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
db.dbname=Emvicy2x
db.username=root
db.password=
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="2"></a>
## Creation

<a id="2-1"></a>
### 2.1. Create DB Config


In your main module's config environment folder edit your DB Config.
(@see [/2.x/configuration#Modules-environment-config-file](/2.x/configuration#Modules-environment-config-file))

The main config file resides in `_db.php`.

Overwrite the settings from `_db.php` in your concrete environment config file e.g. `develop.php` (depends on what your `MVC_ENV` is set to.)

For example you might want to **turn on Logging** for develop environment:

_Db Config example for `develop` environments_
~~~php

//----------------------------------------------------------------------------------------------------------------------
// DB
// watch database log; e.g.:      cd /tmp; tail -f Foo*.log

require realpath(__DIR__) . '/_db.php';

// consider a logrotate mechanism for this logfile as it may grow quickly
$aConfig['MODULE']['Foo']['DB']['logging']['general_log'] = 'ON'; // consider to set it to ON for develop or test environments only
~~~

*example: file `_db.php` - (`modules/Foo/etc/config/Foo/config/_db.php`)*    
~~~php
<?php

//######################################################################################################################
// Module DB

$aConfig['MODULE']['Foo']['DB'] = array(

    'db' => array(
        'type' => getenv('db.type'),
        'host' => getenv('db.host'),
        'port' => getenv('db.port'),
        'username' => getenv('db.username'),
        'password' => getenv('db.password'),
        'dbname' => getenv('db.dbname'),
        'charset' => 'utf8',
    ),
    'caching' => array(
        'enabled' => true,
        'lifetime' => '1', # minutes
    ),
    'logging' => array(
        'log_output' => 'FILE',

        // consider to turn it on for develop and test environments only
        'general_log' => 'OFF',

        // 1) make sure write access is given to the folder
        // as long as the db user is going to write and not the webserver user
        // 2) consider a logrotate mechanism for this logfile as it may grow quickly
        'general_log_file' => $aConfig['MVC_LOG_FILE_DB_DIR'] . getenv('db.dbname') . '_' . getenv('MVC_ENV') . '.log',
    ),
);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="2-2"></a> 
### 2.2. Table Class

_creates DB Table `Bar` in the given module `Foo`._  
~~~bash
php emvicy db:table Bar Foo
~~~
- creates the Table `Bar` in `modules/Foo/Model/DB/Table/Bar.php`
- creates the Trait `TraitBar` in `modules/Foo/Model/DB/Collection/TraitBar.php`
- 🛈 The Table fields `id`, `stampChange` and `stampCreate` are always added automatically

use this command to access the table directly:

~~~php
\Foo\Model\DB\Table\Bar::init()
~~~

_The created DB Table class file: `modules/Foo/Model/DB/Table/Bar.php`_
~~~php
<?php

namespace Foo\Model\DB\Table;

use MVC\DB\Model\Db;
use MVC\DataType\DTDBWhere;
use MVC\DataType\DTDBOption;
use MVC\DataType\DTDBWhereRelation;
use MVC\DB\Trait\TraitDbInit;

class Bar extends Db
{
    use TraitDbInit;

    /**
     * @var array
     */
    protected $aField = array(
        'uuid'          => "varchar(36)     NOT NULL DEFAULT uuid() COMMENT 'uuid'",
        "name"          => "varchar(255)    NOT NULL DEFAULT '' COMMENT 'Name'",
        "description"   => "text            NOT NULL DEFAULT '' COMMENT 'Description'",
    );

    /**
     * @param array $aDbConfig
     * @throws \ReflectionException
     */
    public function __construct(array $aDbConfig = array())
    {
        parent::__construct(
            $this->aField,
            $aDbConfig
        );
    }
}
~~~

------------------------------------------------------------------------------------------------------------------------

**adding a Foreign Key**

_file: `modules/Foo/Model/Table/Bar.php`_
~~~php
/**
 * @param array $aDbConfig
 * @throws \ReflectionException
 */
public function __construct(array $aDbConfig = array())
{
    parent::__construct(
        $this->aField,
        $aDbConfig
    );
    
    $this->setForeignKey(
        Foreign::create()
            ->set_sForeignKey('id_AppTableGroup')
            ->set_sReferenceTable('AppTableGroup')
            ->set_sOnDelete(Foreign::DELETE_CASCADE)
            ->set_sComment('Group')
    );      
}
~~~
- The foreign key `id_AppTableGroup` -pointing to table `AppTableGroup`- is added by method `set_sForeignKey()`

------------------------------------------------------------------------------------------------------------------------

<a id="2-3"></a>
### 2.3. Table Collection

Combining table classes in a table collection class.

**create DB table collection class**

_creates DB table collection class `DBAccount` in the given module `Foo` under `/Foo/Model/DB/Collection/`_  
~~~bash
php emvicy db:tableCollection Account Foo
~~~
- the prefix `DB` is added to the class name if missing
- in this example `Account` will become `DBAccount`

_created DB table collection class `/Foo/Model/DB/Collection/DBAccount.php`_   
~~~php
<?php

/**
 * - add your db table classes as public properties
 * - add a doctype to each property, containing the var type information about the certain class
 * @example
 *      @var Foo\Model\Table\Example
 *      public $oFooModelTableExample;
 * ---
 * [!]  it is important to declare the var type expanded with a full path
 *      avoid to make use of `use ...` support
 *      otherwise the classes could not be read correctly
 */

namespace Foo\Model\DB\Collection;

use MVC\DB\Model\DbCollection;
use MVC\DB\Trait\TraitDbInit;

class DBAccount extends DbInit
{
    use TraitDbInit;

    #-------------------------------------------------------------------------------------------------------------------
    # tables
    
}
~~~

**implement any Table Class**


implement any Table Class into your DB collection class by adding its corresponding Trait.

_Example: `modules/Foo/Model/DBAccount.php`_
~~~php
<?php

namespace Foo\Model;

use MVC\DB\Model\DbCollection;
use MVC\DB\Trait\TraitDbInit;

/**
 * DB
 */
class DBAccount extends DbInit
{
    use TraitDbInit;

    #-------------------------------------------------------------------------------------------------------------------
    # tables
     
    use TraitBar;
}
~~~

**Access Db table collection**

_access DB table collection class and its tables via `use` command_    
~~~php
DBAccount::use()
~~~

------------------------------------------------------------------------------------------------------------------------


<a id="2-4"></a>
### 2.4. Let generate an openapi yaml schema file for data type classes

This builds an openapi.yaml `DTTables.yaml` in the primary module's DataType folder based
on data type classes of the DB tables.

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
                    \Foo\Model\DB\Collection\DB::init()
                );
            }
        },
    ],
]);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="3"></a>
## 3. Usage

access your table classes and table collection classes from everywhere - even from frontend templates:

_Example: access a table class directly_  
~~~php
\App\Table\User::init()
~~~

_Example: access a table class via table collection_
~~~php
\Foo\Model\Table\DB::use()->oAppTableUser
~~~


<a id="3-1"></a>  
### 3.1. create

therefore an object of its related Datatype must be instanciated and given to the method `create`.
Here e.g. with Datatype "DTAppTableUser" to TableClass "modules/Foo/Model/Table/User":

_example `create`: put data into table_  
~~~php
// create DataType object
$oDTAppTableUser = \App\DataType\DTAppTableUser::create()
    ->set_id_AppTableGroup(1)
    ->set_email('foo@example.com')
    ->set_password('...password...')
    ->set_forename('foo')
    ->set_lastname('bar')
    ->set_nickname('foo')
    ->set_uuid(Strings::uuid4())
    ->set_uuidtmp(Strings::uuid4())
    ->set_active(1);
        
// put DataType object into Table and get updated object back
/** @var \App\DataType\DTAppTableUser $oDTAppTableUser */
$oDTAppTableUser = DB::use()->oAppTableUser->create(
    $oDTAppTableUser
);

// on success, `$oDTAppTableUser` now has an id (auto increment of database table) set
// on fail, id = 0
$iId = $oDTAppTableUser->get_id();
~~~

---

<a id="3-2"></a>  
### 3.2. retrieve

`getOnId`: returns tupel object or field of tupel by id

_example `getOnId`: get Object from table where `id=2`_  
~~~php
/** @var \App\DataType\DTAppTableUser $oDTAppTableUser */
$oDTAppTableUser = DB::use()->oAppTableUser->getOnId(1)
~~~

_example `getOnId`: get `email` from table where `id=2`_    
~~~php
/** @var string $sEmail */
$sEmail = DB::use()->oAppTableUser->getOnId(2, DTAppTableUser::getPropertyName_email()); # using name helper
~~~
~~~php
/** @var string $sEmail */
$sEmail = DB::use()->oAppTableUser->getOnId(2, 'email'); # plain text
~~~

---

`retrieveTupel` asks for a specific Tupel and returns the DataType Object according to the requested Table.

_example `retrieveTupel`: get Object from table where `id=2`_
~~~php
/** @var \App\DataType\DTAppTableUser $oDTAppTableUser */
$oDTAppTableUser = DB::use()->oAppTableUser->retrieveTupel(
    DTAppTableUser::create()->set_id(2)
);
~~~

---

`retrieve` returns an array of DataType Objects according to the requested Table.

_example `retrieve`: get all Datasets_
~~~php
/** @var \App\DataType\DTAppTableUser[] $aDTAppTableUser */
$aDTAppTableUser = DB::use()->oAppTableUser->retrieve();
~~~

_example `retrieve`: get specific Datasets_
~~~php
/** @var \App\DataType\DTAppTableUser[] $aDTAppTableUser */
$aDTAppTableUser = DB::use()->oAppTableUser->retrieve(
    [ // where using name helper
        DTDBWhere::create()->set_sKey( DTAppTableUser::getPropertyName_email() )->set_sRelation('LIKE')->set_sValue('%example%')
    ]
);
~~~
~~~php
/** @var \App\DataType\DTAppTableUser[] $aDTAppTableUser */
$aDTAppTableUser = DB::use()->oAppTableUser->retrieve(
    [ // where
        DTDBWhere::create()->set_sKey('email')->set_sRelation('LIKE')->set_sValue('%example%')
    ]
);
~~~

_example `retrieve`: get Datasets with options_
~~~php
/** @var \App\DataType\DTAppTableUser[] $aDTAppTableUser */
$aDTAppTableUser = DB::use()->oAppTableUser->retrieve(
    [ // where
        DTDBWhere::create()->set_sKey( DTAppTableUser::getPropertyName_email() )->set_sRelation('LIKE')->set_sValue('%example%')
    ],
    [ // option
        DTDBOption::create()->set_sValue('ORDER BY `email` ASC'),
        DTDBOption::create()->set_sValue('LIMIT 0,10'),
    ]
);
~~~

_example `retrieve`: get first 30 Datasets (LIMIT 0,30)_
~~~php
/** @var \App\DataType\DTAppTableUser[] $aDTAppTableUser */
$aDTAppTableUser = DB::use()->oAppTableUser->retrieve(aDTDBOption: [
    DTDBOption::create()->set_sValue('LIMIT 0, 30')
]);
~~~
- here the named Parameter `aDTDBOption` is used to address the right parameter correctly


<a id="3-3"></a>  
### 3.3. update

_example `updateTupel`: update Object in table where `id=2`_
~~~php
// retrieve User object with id=2
/** @var \App\DataType\DTAppTableUser $oDTAppTableUser */
$oDTAppTableUser = DB::use()->oAppTableUser->getOnId(2);

// modify User object
$oDTAppTableUser->set_nickname('ABC');

// update tupel with modified object
$bSuccess = DB::use()->oAppTableUser->updateTupel(
    $oDTAppTableUser
);
~~~

<!--
_shortened example `updateTupel`: update this specific Tupel - identified by `id`_
~~~php
/** @var boolean $bSuccess */
$bSuccess = DB::use()->oAppTableUser->updateTupel(
    DB::use()->oAppTableUser->retrieveTupel( DTAppTableUser::create()->set_id(2) )->set_nickname('ABC')
);
~~~
-->
  
_example `update`: update **all** Tupel with data defined in **set** (array) which are affected by the **where** clause (array)_
~~~php
/** @var boolean $bSuccess */
$bSuccess = DB::use()->oAppTableUser->update(
    [ // set
        DTDBSet::create()->set_sKey( DTAppTableUser::getPropertyName_active() )->set_sValue(0),
        DTDBSet::create()->set_sKey( DTAppTableUser::getPropertyName_email() )->set_sValue('bar@example.com')
    ],
    [ // where
        DTDBWhere::create()->set_sKey( DTAppTableUser::getPropertyName_email() )->set_sRelation('LIKE')->set_sValue('%example.com')
    ]
);
~~~

_update via SQL Statement_  
~~~php
DB::use()->oDbPDO->query("UPDATE `AppTableUser` SET `active` = '0' WHERE `email` = 'foo@example.com'");
~~~
- see also: [Database Events](/2.x/events#database_events), and [Database - 3.8. SQL](#3-8)

---

<a id="3-4"></a>  
### 3.4. delete

_`deleteTupel`: delete this **one** specific Tupel - identified **only** by **`id`** (id is required; other values do not have an effect)_
~~~php
// delete User object (you retrieved before)
$bSuccess = DB::use()->oAppTableUser->deleteTupel(
    $oDTAppTableUser
);

// example deleting by setting object explicitly
$bSuccess = DB::use()->oAppTableUser->deleteTupel(
    DTAppTableUser::create()->set_id(2)
);
~~~

_`delete`: delete **all** Tupel which are affected by the **where** clause (array)_
~~~php
// example setting key and value directly
$bSuccess = DB::use()->oAppTableUser->delete([ // array of where clauses
    DTDBWhere::create()->set_sKey('id')->set_sValue(2),
    DTDBWhere::create()->set_sKey('active')->set_sValue(0),
]);

// example setting the key using Property getter
$bSuccess = DB::use()->oAppTableUser->delete([ // array of where clauses
    DTDBWhere::create()->set_sKey(DTAppTableUser::getPropertyName_id())->set_sValue(2),
    DTDBWhere::create()->set_sKey(DTAppTableUser::getPropertyName_active())->set_sValue(0),
]);

// example setting the key using Property getter; take values from User object (you retrieved before)
$bSuccess = DB::use()->oAppTableUser->delete([ // array of where clauses
    DTDBWhere::create()->set_sKey(DTAppTableUser::getPropertyName_id())->set_sValue($oDTAppTableUser->get_id()),
    DTDBWhere::create()->set_sKey(DTAppTableUser::getPropertyName_active())->set_sValue($oDTAppTableUser->get_active()),
]);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="3-5"></a>  
### 3.5. count

~~~php
// Amount of all Datasets
$iAmount = DB::use()->oAppTableUser->count();

// Amount of specific Datasets
$iAmount = DB::use()->oAppTableUser->count([
    DTDBWhere::create()->set_sKey( DTAppTableUser::getPropertyName_email() )->set_sRelation('LIKE')->set_sValue('%example.com')
]);
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="3-6"></a>  
### 3.6. checksum

~~~php
// Returns a checksum of the table
$iChecksum = DB::use()->oAppTableUser->checksum();
~~~
~~~
// type: integer
1138916503
~~~


<a id="3-7"></a>  
### 3.7. getFieldInfo

returns array with table fields info

_get info of certain field `email`_  
~~~php
// get info of certain field `email`
$aFieldInfo = DB::use()->oAppTableUser->getFieldInfo('email');
~~~
_example return_  
~~~
// type: array, items: 12
[
    'Field' => 'email',
    'Type' => 'varchar(255)',
    'Collation' => 'utf8_general_ci',
    'Null' => 'NO',
    'Key' => 'UNI',
    'Default' => NULL,
    'Extra' => '',
    'Privileges' => 'select,insert,update,references',
    'Comment' => '',
    '_php' => 'string',
    '_type' => 'varchar',
    '_typeValue' => 255,
]
~~~

_get info of all fields_  
~~~php
// get info of all fields
$aFieldInfo = DB::use()->oAppTableUser->getFieldInfo();
~~~
_example return (shortened)_  
~~~
// type: array, items: 9
[
    'id_TableGroup' => [
        'Field' => 'id_TableGroup',
        'Type' => 'int(11)',
        'Collation' => NULL,
        'Null' => 'YES',
        'Key' => 'MUL',
        'Default' => NULL,
        'Extra' => '',
        'Privileges' => 'select,insert,update,references',
        'Comment' => '',
        '_php' => 'int',
        '_type' => 'int',
        '_typeValue' => 11,
    ],
    'email' => [
        'Field' => 'email',
        'Type' => 'varchar(255)',
        'Collation' => 'utf8_general_ci',
        'Null' => 'NO',
        'Key' => 'UNI',
        'Default' => NULL,
        'Extra' => '',
        'Privileges' => 'select,insert,update,references',
        'Comment' => '',
        '_php' => 'string',
        '_type' => 'varchar',
        '_typeValue' => 255,
    ],
    ...
]
~~~

------------------------------------------------------------------------------------------------------------------------


<a id="3-8"></a>
#### 3.8. PDO

_using a table collection class_  
~~~php
DB::use()->oDbPDO
~~~

_otherwise this always works; get PDO object from Framework's Db class_  
~~~php
\MVC\DB\Model\Db::getDbPdo()
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="3-9"></a>  
#### 3.9. SQL

**fetchRow**

_select a single tupel (a row)_  
~~~php
/** @var \App\DataType\DTAppTableUser $oDTAppTableUser */
$oDTAppTableUser = DTAppTableUser::create(
    DB::use()->oDbPDO->fetchRow("SELECT * FROM `AppTableUser` WHERE id = '2'")
);
~~~
- here we select the entry which id = 2
- we put in into the table's Datatype object of type `\App\DataType\DTAppTableUser`

---

**fetchAll**

_returns the result as regular array_
~~~php
DB::use()->oDbPDO->fetchAll("SELECT * FROM `AppTableUser`")
~~~

_returns the result as an array of DataType objects_
~~~php
DB::use()->oAppTableUser->fetchAll("SELECT * FROM `AppTableUser`", true)
~~~
- the result gets mapped into the table's Datatype object
- so here we get an array of type `DTAppTableUser[]`

**query**

_insert_  
~~~php
DB::use()->oDbPDO->query("INSERT INTO  `AppTableUser` 
    (`stampChange`,`stampCreate`,`id_TableGroup`,`email`,`active`,`uuid`,`uuidtmp`,`password`,`nickname`,`forename`,`lastname`) 
    VALUES ('2023-12-01 18:53:33',
            '2023-12-01 18:53:33',
            '1','foo2@example.com',
            '1',
            '7cb3c040-36f1-4aa0-ae3a-8ef19d0667aa',
            '236dde6b-f6a1-440f-afd2-393291331642',
            '" . password_hash('...password...', PASSWORD_DEFAULT) . "',
            'foo2',
            'foo2',
            'bar2');"
);
~~~

_update_
~~~php
DB::use()->oDbPDO->query("UPDATE `AppTableUser` SET `active` = '0' WHERE `email` = 'foo@example.com'");
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="3-10"></a>
#### 3.10. Comment

**read comment from a DB Table Field**

_from Table `AppTableUser`, read the comment of field `nickname`_  
~~~php
DB::use()->oAppTableUser->getComment('nickname')
~~~
~~~
// type: string
'Abbreviation'
~~~

**read any DocComment from Db table collection class**

_from Table `oAppTableUser`, read the value of DocComment `@var`_  
~~~php
DB::use()->getDocCommentValueOfProperty('oAppTableUser', '@var')
~~~
~~~
// type: string
'\\App\\Table\\User'
~~~

------------------------------------------------------------------------------------------------------------------------

<a id="4"></a> 
## 4. Events

see [Database Events](/2.x/events#database_events)

<a id="4-1"></a>

### 4.1. Logging SQL

**logging at application side** 

Consider to set to `true` for develop environments only: logging request into `MVC_LOG_FILE_SQL`.

_enable sql logging in your config file_    
~~~php
$aConfig['MVC_LOG_SQL'] = true;   
~~~

if it not already exists, create a file `db.php` in the event folder of your module
and declare the bindings as follows.

Emvicy logs each action into the logfile specified in the var `MVC_LOG_FILE_SQL`; which is per default: `application/log/sql.log`. 

_`/modules/{MODULE}/etc/event/db.php`_
~~~php
\MVC\Event::processBindConfigStack([

    'mvc.db.model.*.sql' => array(
        /*
         * log *all* SQL Statements, if enabled via config
         */
        function(string $sSql) {
            if (true === \MVC\Config::get_MVC_LOG_SQL())
            {
                \MVC\Log::write(
                    \MVC\Strings::tidy($sSql),
                    \MVC\Config::get_MVC_LOG_FILE_SQL()
                );
            }
        }
    ),
]);
~~~

**logging at database engine side**

if you set `general_log` to `ON` iny your [DB Config](/2.x/database#2-1), the database logs each action into the logfile 
specified in the var `MVC_LOG_FILE_DB_DIR`.

~~~php
$aConfig['MVC_LOG_FILE_DB_DIR'] = '/tmp/';
~~~
- make sure write access is given to the folder  as long as the db user is going to write and not the webserver user
- consider a logrotate mechanism for this logfile as it may grow quickly
