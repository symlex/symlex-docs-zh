# Data Access Objects

DAOs directly deal with the database. They are suited for implementing custom methods using raw SQL,
if needed.

## Configuration ##

DAO entities are configured using protected class properties:

```php
<?php

// Database table name
protected $_tableName = ''; 
// Name or array of primary key(s)
protected $_primaryKey = 'id'; 
// 'db_column' => 'object_property'
protected $_fieldMap = array(); 
// Fields that should be hidden for getValues(), e.g. 'password'
protected $_hiddenFields = array(); 
// 'db_column' => Format::TYPE
protected $_formatMap = array();
// 'object_property' => 'db_column'
protected $_valueMap = array(); 
// Automatically update timestamps?
protected $_timestampEnabled = false;
// Column name for timestamp
protected $_timestampCreatedCol = 'created';
// Column name for timestamp
protected $_timestampUpdatedCol = 'updated';
```

Possible values for $_formatMap are defined as constants in `Doctrine\ActiveRecord\Dao\Format`:

```php
<?php

const NONE = '';
const INT = 'int';
const FLOAT = 'float';
const STRING = 'string';
const ALPHANUMERIC = 'alphanumeric';
const SERIALIZED = 'serialized';
const JSON = 'json';
const CSV = 'csv';
const BOOL = 'bool';
const TIME = 'H:i:s';
// Support for microseconds (up to six digits)
const TIMEU = 'H:i:s.u'; 
// Support for timezone (e.g. "+0230")
const TIMETZ = 'H:i:sO'; 
// Support for microseconds & timezone
const TIMEUTZ = 'H:i:s.uO'; 
const DATE = 'Y-m-d';
const DATETIME = 'Y-m-d H:i:s';
// Support for microseconds (up to six digits)
const DATETIMEU = 'Y-m-d H:i:s.u'; 
// Support for timezone (e.g. "+0230")
const DATETIMETZ = 'Y-m-d H:i:sO'; 
// Support for microseconds & timezone
const DATETIMEUTZ = 'Y-m-d H:i:s.uO'; 
const TIMESTAMP = 'U';
```

!!! example
    ```php    
    <?php
    
    namespace App\Dao;
    
    use Doctrine\ActiveRecord\Dao\EntityDao;
    
    class UserDao extends EntityDao
    {
        protected $_tableName = 'users';
        protected $_primaryKey = 'user_id';
        protected $_timestampEnabled = true;
    }
    ```
    
## Dao API ##

All DAOs expose the following public methods by default:

`createDao(string $name)`: Returns a new DAO instance

`beginTransaction()`: Start a database transaction

`commit()`: Commit a database transaction

`rollBack()`: Roll back a database transaction

## EntityDao API ##

In addition, `Doctrine\ActiveRecord\Dao\EntityDao` offers many powerful methods to easily deal with database table rows:

`setData(array $data)`: Set raw data (changes can not be detected, e.g. when calling update())

`setValues(array $data)`: Set multiple values

`setDefinedValues(array $data)`: Set values that exist in the table schema only (slower than setValues())

`getValues()`: Returns all values as array

`find($id)`: Find a row by primary key

`reload()`: Reload row values from database

`getValues()`: Returns all values as associative array

`exists($id)`: Returns true, if a row with the given primary key exists

`save()`: Insert a new row

`update()`: Updates changed values in the database

`delete()`: Delete entity from database

`getId()`: Returns the ID of the currently loaded record (throws exception, if empty)

`hasId()`: Returns true, if the DAO instance has an ID assigned (primary key)

`setId($id)`: Set primary key

`findAll(array $cond = array(), $wrapResult = true)`: Returns all instances that match $cond (use search() or searchAll(), if you want to limit or sort the result set)

`search(array $params)`: Returns a `SearchResult` object (see below for supported parameters)

`wrapAll(array $rows)`: Create and return a new DAO for each array element

`updateRelationTable(string $relationTable, string $primaryKeyName, string $foreignKeyName, array $existing, array $updated)`: Helper function to update n-to-m relationship tables

`hasTimestampEnabled()`: Returns true, if this DAO automatically adds timestamps when creating and updating rows

`findList(string $colName, string $order = '', string $where = '', string $indexName = '')`: Returns a key/value array (list) of all matching rows

`getTableName()`: Returns the name of the underlying database table

`getPrimaryKeyName()`: Returns the name of the primary key column (throws an exception, if primary key is an array)
