# 模型和数据库抽象

Symlex 不适用于任何特定的数据库抽象层或模型库。
我们的例子基于 MySQL 和[ Doctrine ActiveRecord ](../doctrine-active-record.md).

作为 Doctrine ORM 的轻量级替代，该库提供了业务模型和数据库访问对象（ DAO ）类，它们封装了 Doctrine DBAL ，为关系数据库提供了高性能，面向对象的 CRUD （create, read, update, delete）功能。 它比 Datamapper ORM 实现快得多且复杂得多。

## 商业模式

模型逻辑上位于控制器（用于呈现视图和验证用户输入）和数据访问对象（ DAO ）之间，后者是存储后端或 Web 服务的低级接口。

模型的公共接口是高级的，应该反映其域中的所有用例：

!!! example
    ```php
    <?php

    namespace App\Model;

    use App\Exception\InvalidArgumentException;
    use Doctrine\DBAL\Exception\UniqueConstraintViolationException;

    class User extends ModelAbstract
    {
        protected $_daoName = 'User';

        public function updatePassword($password)
        {
            if (strlen($password) < 8) {
                throw new InvalidArgumentException('Password is too short');
            }

            $hash = password_hash($password, PASSWORD_DEFAULT);

            $this->getDao()->userPassword = $hash;
            $this->getDao()->userPasswordResetToken = null;
            $this->getDao()->userVerificationToken = null;
            $this->getDao()->update();
        }
    }
    ```

## 数据访问对象

如果需要，DAO直接处理数据库表和原始SQL。 `Doctrine\ActiveRecord\Dao\Dao` 适合使用原始SQL实现自定义方法
而`Doctrine\ActiveRecord\Dao\EntityDao` 提供了许多强大的方法来轻松处理数据库表行：

!!! example
    ```php
    <?php

    namespace App\Dao;

    class UserDao extends DaoAbstract
    {
        protected $_tableName = 'users';
        protected $_primaryKey = 'userId';
        protected $_timestampEnabled = true;

        protected $_formatMap = [
            'userId' => Format::INT,
            'userRole' => Format::STRING,
            'userNewsletter' => Format::BOOL,
        ];

        protected $_hiddenFields = [
            'userPassword',
            'userPasswordResetToken',
            'userVerificationToken',
        ];
    }
    ```

## 工作流程

此图说明了 Controller，Model ， DAO 和数据库如何相互交互：

![Doctrine ActiveRecord](../doctrine-active-record/img/workflow.svg)
