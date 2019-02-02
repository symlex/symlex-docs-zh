# Form Definition

A detailed overview of field properties can be found below. `$_('label')` is used for optional translation of field captions or other strings - a number of different translation file formats such as YAML are supported for that (see [Symfony Components](http://symfony.com/doc/current/components/translation.html) documentation)

```php
<?php

use InputValidation\Form;

class UserForm extends Form
{
    protected function init(array $params = array())
    {
        $definition = [
            'username' => [
                'type' => 'string',
                'caption' => $this->_('username'),
                'required' => true,
                'min' => 3,
                'max' => 15
            ],
            'email' => [
                'type' => 'email',
                'caption' => $this->_('email_address'),
                'required' => true
            ],
            'gender' => [
                'type' => 'string',
                'caption' => $this->_('gender'),
                'required' => false,
                'options' => [
                    'm' => 'Male',
                    'f' => 'Female',
                    'o' => 'Other'
                ],
                'optional' => true
            ],
            'birthday' => [
                'type' => 'date',
                'caption' => $this->_('birthday'),
                'required' => false
            ],
            'password' => [
                'type' => 'string',
                'caption' => $this->_('password'),
                'required' => true,
                'min' => 5,
                'max' => 30
            ],
            'password_again' => [
                'type' => 'string',
                'caption' => $this->_('password_again'),
                'required' => true,
                'matches' => 'password'
            ],
            'continent' => [
                'type' => 'string',
                'caption' => $this->_('region'),
                'required' => true,
                'options' => [
                    'north_america' => 'North America',
                    'south_america' => 'South Amertica',
                    'europe' => 'Europe,
                    'asia' => 'Asia',
                    'australia' => 'Australia'
                ]
            ]
        ];

        $this->setDefinition($definition);
    }
}
```

## Creating a Form Instance ##

You can create new form instances manually...

```php
<?php

use InputValidation\Form;
use InputValidation\Form\Validator;
use InputValidation\Form\Options\YamlOptions;
use Symfony\Component\Translation\Translator;
use Symfony\Component\Translation\MessageSelector;
use Symfony\Component\Translation\Loader\YamlFileLoader;
use Symfony\Component\Translation\Loader\ArrayLoader;

$translator = new Translator('en', new MessageSelector);
$translator->addLoader('yaml', new YamlFileLoader);
$translator->addLoader('array', new ArrayLoader);

$validator = new Validator();

$options = new YamlOptions($translator);

$form = new Form($translator, $validator, $options);
```

... or using our `Factory` class:

```php
<?php

use InputValidation\Form\Factory as FormFactory;

$formFactory = new FormFactory($translator, $validator, $options);
$formFactory->setFactoryNamespace('App\Form');
$formFactory->setFactoryPostfix('Form');

// Returns instance of App\Form\UserForm
$formFactory->create('User');
```
