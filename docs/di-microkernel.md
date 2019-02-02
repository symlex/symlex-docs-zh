# Micro-Kernel for PHP Applications

[![Latest Stable Version](https://poser.pugx.org/symlex/di-microkernel/v/stable.svg)](https://packagist.org/packages/symlex/di-microkernel)
[![License](https://poser.pugx.org/symlex/di-microkernel/license.svg)](https://packagist.org/packages/symlex/di-microkernel)
[![Test Coverage](https://codecov.io/gh/symlex/di-microkernel/branch/master/graph/badge.svg)](https://codecov.io/gh/symlex/di-microkernel)
[![Build Status](https://travis-ci.org/symlex/di-microkernel.png?branch=master)](https://travis-ci.org/symlex/di-microkernel)

This library contains a micro-kernel for bootstrapping almost any PHP application, including [Silex](https://silex.symfony.com/),
[Symlex](https://github.com/symlex/symlex) (a framework stack for agile Web development based on Symfony) 
and [Symfony Console](https://symfony.com/doc/current/components/console.html). 
The kernel itself is just a few lines to set a bunch of environment parameters and create a service container 
instance with that.

![Micro-Kernel Architecture](img/architecture.svg)

## Run an App ##

Creating a kernel instance and calling `run()` is enough to start an application:

```php
#!/usr/bin/env php
<?php

// Composer autoloader
require_once 'vendor/autoload.php'; 

$app = new \DIMicroKernel\Kernel('console');

// Run the 'app' service defined in config/console.yml
$app->run(); 
```

## Composer ##

To use this library in your project, simply run `composer require symlex/di-microkernel` or
add "symlex/di-microkernel" to your [composer.json](https://getcomposer.org/doc/04-schema.md) file and run `composer update`:

```json
{
    "require": {
        "php": ">=7.1",
        "symlex/di-microkernel": "^2.0"
    }
}
```