# Command-Line Interface

Running `app/console` lists all commands available. The following commands including **Doctrine Migrations** 
for creating and migrating database tables are supported out of the box:

Command                  | Description
-------------------------|----------------------------------------------------------------------------
migrations:execute       | Execute a single migration version up or down manually
migrations:generate      | Generate a blank migration class
migrations:migrate       | Execute a migration to a specified version or the latest available version
migrations:status        | View the status of a set of migrations
migrations:version       | Manually add and delete migration versions from the version table
database:create          | Create the database configured in app/config/parameters.yml
database:drop            | Drop the database configured in app/config/parameters.yml
database:insert-fixtures | Insert database fixtures for testing (see app/db/fixtures/)
user:create              | Create a new user
user:delete              | Delete a user
user:reset-password      | Send password reset email to a user

## Configuration ##

All commands are configured as service in `app/config/console.yml`:

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

## Development ##

To develop a custom command, simply create a new class in `src/Command/`, configure it as service and
add it to the console application:

!!! example
    ```yaml
    app:
        class: Symfony\Component\Console\Application        
        calls:
            - [ add, [ "@command.user.reset-password" ] ]    
    ```

[Symfony Console](https://symfony.com/doc/current/components/console.html) has a great documentaion,
in case you haven't used it yet. You can also study our examples to get a better understanding.

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