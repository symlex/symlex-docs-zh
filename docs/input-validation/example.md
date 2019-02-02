# REST Request Validation Example

This example shows how to validate user input in a REST controller context. 
Note, how easy it is to avoid the deeply nested structures you often find in validation code. User model and form are injected as dependencies. 

```php
<?php

class UserController
{
    protected $user;
    protected $form;

    public function __construct(UserModel $user, UserForm $form)
    {
        $this->user = $user;
        $this->form = $form;
    }
    
    // Update User
    public function putAction(int $id, Request $request): array 
    {
        // Find entity (throws exception, if not found)
        $this->user->find($id); 
        
        // Form initialization with current values
        $this->form->setDefinedValues($this->user->getValues()); 
        
        // Set input values
        $this->form->setDefinedWritableValues($request->request->all()); 
        
        // Validation
        $this->form->validate(); 

        if($this->form->hasErrors()) {
            throw new FormInvalidException($this->form->getFirstError());
        }
        
        // Update values in database
        $this->user->update($this->form->getValues()); 

        // Return updated values
        return $this->user->getValues(); 
    }
    
    // Return form fields incl current values for User
    public function optionsAction(int $id): array 
    {
        // Find entity (throws exception, if not found)
        $this->user->find($id); 
        
        // Form initialization with current values
        $this->form->setDefinedValues($this->user->getValues()); 
        
        // Returns form as JSON compatible array incl all values
        return $this->form->getAsArray(); 
    }
}
```

See also [Doctrine ActiveRecord - Object-oriented CRUD for Doctrine DBAL](../active-record.md)
