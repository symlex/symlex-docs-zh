# Business Models

**Models** are logically located between **Controllers** - which render views and validate user input - and **Data Access Objects** (DAOs), that are low-level interfaces to a storage backend or Web service.

They are associated with their respective Dao using a protected class property:

```
// DAO class name without namespace or postfix
protected $_daoName = ''; 
```

## Example ##

```php
<?php

namespace App\Model;

use Doctrine\ActiveRecord\Model\EntityModel;

class User extends EntityModel
{
    protected $_daoName = 'User';

    public function delete() 
    {
        $dao = $this->getEntityDao();
        $dao->is_deleted = 1;
        $dao->update();
    }

    public function undelete() 
    {
        $dao = $this->getEntityDao();
        $dao->is_deleted = 0;
        $dao->update();
    }

    public function search(array $cond, array $options = array()) 
    {
        $cond['is_deleted'] = 0;
        return parent::search($cond, $options);
    }

    public function getValues()
    {
        $result = parent::getValues();
        unset($result['password']);
        return $result;
    }
}
```

## Validation ##

**How much validation should be implemented within a model?** Wherever invalid data can lead to security issues or major inconsistencies, some core validation rules must be implemented in the model layer. Model exception messages usually don’t require translation (in multilingual applications), since invalid values should be recognized beforehand by a form class. If you expect certain exceptions, you should catch and handle them in your controllers.

## API ##

Public interfaces of models are high-level and should reflect all use cases within their domain. There are a number of standard use-cases that are pre-implemented in the base class `Doctrine\ActiveRecord\Model\EntityModel`:

`createModel(string $name = '', Dao $dao = null)`: Create a new model instance

`find($id)`: Find a record by primary key

`reload()`: Reload values from database

`findAll(array $cond = array(), $wrapResult = true)`: Find multiple records; if `$wrapResult` is false, plain DAOs are returned instead of model instances

`search(array $cond, array $options = array())`: Returns a `SearchResult` object ($options can contain count, offset, sort order etc, see search() in the DAO documentation above)

`searchAll(array $cond = array(), $order = false)`: Simple version of search(), similar to findAll()

`searchOne(array $cond = array())`: Search a single record; throws an exception if 0 or more than one record are found

`searchIds(array $cond, array $options = array())`: Returns an array of matching primary keys for the given search condition

`getModelName()`: Returns the model name without prefix and postfix

`getId()`: Returns the ID of the currently loaded record (throws exception, if empty)

`hasId()`: Returns true, if the model instance has an ID assigned (primary key)

`getValues()`: Returns all model properties as associative array

`getEntityTitle()`: Returns the common name of this entity

`isDeletable()`: Returns true, if the model instance can be deleted with delete()

`isUpdatable()`: Returns true, if the model instance can be updated with update($values)

`isCreatable()`: Returns true, if new entities can be created in the database with create($values)

`batchEdit(array $ids, array $properties)`: Update data for multiple records

`getTableName()`: Returns the name of the associated main database table

`hasTimestampEnabled()`: Returns true, if timestamps are enabled for the associated DAO

`delete()`: Permanently delete the entity record from the database

`save(array $values)`: Create a new record using the values provided

`update(array $values)`: Update model instance database record; before assigning multiple values to a model instance, data should be validated using a form class
