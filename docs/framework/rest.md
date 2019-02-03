# 建立 REST 服务

Symlex REST 控制器使用类似于 FOSRestBundle 的 *隐式资源名称定义* 的命名方案。 操作名称派生自请求方法和可选的子资源：

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

**REST 控制器动作** 可以返回 *array*, 它会自动转换为有效的JSON或一个 `Symfony\Component\HttpFoundation\Response` 对象。
删除操作也可以返回 *null*（"204 No Content"）。

!!! Links example
     - [UsersController](https://github.com/symlex/symlex/blob/master/src/Controller/Rest/V1/UsersController.php)
     - [SessionController](https://github.com/symlex/symlex/blob/master/src/Controller/Rest/V1/SessionController.php)
     - [RegistrationController](https://github.com/symlex/symlex/blob/master/src/Controller/Rest/V1/RegistrationController.php)

## 输入验证和数据库抽象 ##

以下示例显示了如何使用我们经过实战考验 [InputValidation](../input-validation.md) 和 [Doctrine ActiveRecord](../doctrine-active-record.md) REST控制器上下文中的库。 请注意，它是多么容易避免深层嵌套的结构。 用户模型和表单工厂被注入为依赖项。

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
