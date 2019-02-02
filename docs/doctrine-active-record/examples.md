# Examples
 
## Basic Usage ##

```php
<?php

use Doctrine\ActiveRecord\Dao\Factory as DaoFactory;
use Doctrine\ActiveRecord\Model\Factory;

$daoFactory = new DaoFactory($db); 

$modelFactory = new Factory($daoFactory);
$modelFactory->setFactoryNamespace('App\Model');
$modelFactory->setFactoryPostfix('Model');

// Returns instance of App\Model\UserModel
$user = $modelFactory->create('User'); 

// Throws exception, if not found
$user->find(123); 

if ($user->email == '') {
    // Update email
    $user->update(array('email' => 'user@example.com')); 
}

// Returns instance of App\Model\GroupModel
$group = $user->createModel('Group'); 
```

## REST Controller Context ##

Doctrine ActiveRecord is perfectly suited for building high-performance REST services.

This example shows how to work with the EntityModel in a REST controller context. Note, how easy it is to avoid deeply 
nested structures. User model and form factory (provided by [InputValidation](../input-validation.md)) are injected as dependencies.

```php
<?php

namespace App\Controller\Rest;

use Symfony\Component\HttpFoundation\Request;
use App\Exception\FormInvalidException;
use App\Form\FormFactory;
use App\Model\User;

class UsersController
{
    protected $user;
    protected $formFactory;

    public function __construct(User $user, FormFactory $formFactory)
    {
        $this->user = $user;
        $this->formFactory = $formFactory;
    }
    
    public function cgetAction(Request $request)
    {
        $options = array(
            'count' => $request->query->get('count', 50),
            'offset' => $request->query->get('offset', 0)
        );
        
        return $this->user->search(array(), $options);
    }

    public function getAction($id)
    {
        return $this->user->find($id)->getValues();
    }

    public function deleteAction($id)
    {
        return $this->user->find($id)->delete();
    }

    public function putAction($id, Request $request)
    {
        $this->user->find($id);
        
        $form = $this->formFactory->create('User\Edit');
        $form
             ->setDefinedWritableValues($request->request->all())
             ->validate();

        if($form->hasErrors()) {
            throw new FormInvalidException($form->getFirstError());
        } 
        
        $this->user->update($form->getValues());

        return $this->user->getValues();
    }

    public function postAction(Request $request)
    {
        $form = $this->formFactory->create('User\Create');
        
        $form
            ->setDefinedWritableValues($request->request->all())
            ->validate();

        if($form->hasErrors()) {
            throw new FormInvalidException($form->getFirstError());
        }
        
        $this->user->save($form->getValues());

        return $this->user->getValues();
    }
}
```

See also [Easy & Secure Whitelist Validation for PHP](../input-validation.md).
