# 命令行界面

`app/console` 可以列出所有可用的命令。以下命令包括 **Doctrine Migrations** 或者支持创建和迁移数据库表:

命令                      | 描述
-------------------------|----------------------------------------------------------------------------
migrations:execute       | 手动上下执行单个迁移版本
migrations:generate      | 生成空白迁移类
migrations:migrate       | 执行到指定版本或最新可用版本的迁移
migrations:status        | 查看一组迁移的状态
migrations:version       | 从版本表中手动添加和删除迁移版本
database:create          | 创建配置的数据库 app/config/parameters.yml
database:drop            | 删除配置的数据库 app/config/parameters.yml
database:insert-fixtures | 插入数据库夹具进行测试 (参见 app/db/fixtures/)
user:create              | 创建一个新用户
user:delete              | 删除用户
user:reset-password      | 将密码重置电子邮件发送给用户

## 配置 ##

所有命令都配置为服务 `app/config/console.yml`:

!!! example
    ```yaml
    command.user.reset-password:
        class: App\Command\UserResetPasswordCommand
        arguments:
            - 'user:reset-password'
            - "@service.mail"
            - "@model.user"
            - "@twig"
    ```

## 开发 ##

要开发自定义命令，只需在`src/Command/`中创建一个新类，将其配置为服务和将其添加到控制台应用程序：

!!! example
    ```yaml
    app:
        class: Symfony\Component\Console\Application        
        calls:
            - [ add, [ "@command.user.reset-password" ] ]    
    ```

[Symfony Console](https://symfony.com/doc/current/components/console.html) 有一个很好的文档，如果你还没有使用它。 您还可以研究我们的示例以更好地理解。

!!! example
    ```php
    <?php

    namespace App\Command;

    use App\Service\Mail;
    use App\Model\User;
    use Symfony\Component\Console\Input\InputArgument;
    use Symfony\Component\Console\Input\InputInterface;
    use Symfony\Component\Console\Output\OutputInterface;

    class UserResetPasswordCommand extends CommandAbstract
    {
        protected $mail;
        protected $user;

        public function __construct($name, Mail $mail, User $user)
        {
            $this->mail = $mail;
            $this->user = $user;

            parent::__construct($name);
        }

        protected function configure()
        {
            $this->setDescription('Send password reset email to a user');

            $this->addArgument('email', InputArgument::REQUIRED, 'E-Mail');

            parent::configure();
        }

        protected function execute(InputInterface $input, OutputInterface $output)
        {
            $email = $input->getArgument('email');

            $user = $this->user->findByEmail($email);

            $this->mail->passwordReset($user);
        }
    }
    ```
