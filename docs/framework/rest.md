# Building REST Services

Symlex REST controllers use a naming scheme similar to FOSRestBundle's *implicit resource name definition*. The action name is derived from the request method and optional sub resources:

!!! example
    ```php
    <?php
    
    class UsersController
    {
        ..
    
        public function cgetAction(Request $request)
        {} // [GET] /users
    
        public function coptionsAction(Request $request)
        {} // [OPTIONS] /users
        
        public function postAction(Request $request)
        {} // [POST] /users
    
        public function getAction($id, Request $request)
        {} // [GET] /users/{id}
        
        public function optionsAction($id, Request $request)
        {} // [OPTIONS] /users/{id}
    
        ..
        public function cgetCommentsAction($id, Request $request)
        {} // [GET] /users/{id}/comments
        
        public function getCommentsAction($id, $commentId, Request $request)
        {} // [GET] /users/{id}/comments/{commentId}
    
        ..
    }
    ```

**REST controller actions** can either return an *array*, which is automatically converted to valid JSON, or a `Symfony\Component\HttpFoundation\Response` object.
Delete actions can also return *null* ("204 No Content").

!!! Links example
     - [UsersController](https://github.com/symlex/symlex/blob/master/src/Controller/Rest/V1/UsersController.php)
     - [SessionController](https://github.com/symlex/symlex/blob/master/src/Controller/Rest/V1/SessionController.php)
     - [RegistrationController](https://github.com/symlex/symlex/blob/master/src/Controller/Rest/V1/RegistrationController.php)

## Input Validation and Database Abstraction ##

The following example shows how to work with our battle-tested [InputValidation](../input-validation.md)
and [Doctrine ActiveRecord](../doctrine-active-record.md)  libraries in a REST controller context. Note, how easy it is 
to avoid deeply nested structures. User model and form factory are injected as dependencies.

!!! example
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
            $form->setDefinedWritableValues($request->request->all())->validate();
    
            if($form->hasErrors()) {
                throw new FormInvalidException($form->getFirstError());
            } 
            
            $this->user->update($form->getValues());
    
            return $this->user->getValues();
        }
    
        public function postAction(Request $request)
        {
            $form = $this->formFactory->create('User\Create');
            $form->setDefinedWritableValues($request->request->all())->validate();
    
            if($form->hasErrors()) {
                throw new FormInvalidException($form->getFirstError());
            }
            
            $this->user->save($form->getValues());
    
            return $this->user->getValues();
        }
    }
    ```