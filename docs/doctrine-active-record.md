# Doctrine ActiveRecord: Object-oriented CRUD for Doctrine DBAL

[![Latest Stable Version](https://poser.pugx.org/symlex/doctrine-active-record/v/stable.svg)](https://packagist.org/packages/symlex/doctrine-active-record)
[![License](https://poser.pugx.org/symlex/doctrine-active-record/license.svg)](https://packagist.org/packages/symlex/doctrine-active-record)
[![Test Coverage](https://codecov.io/gh/symlex/doctrine-active-record/branch/master/graph/badge.svg)](https://codecov.io/gh/symlex/doctrine-active-record)
[![Build Status](https://travis-ci.org/symlex/doctrine-active-record.png?branch=master)](https://travis-ci.org/symlex/doctrine-active-record)

As a lightweight alternative to Doctrine ORM, this battle-tested library provides Business Model and Database Access Object (DAO) classes 
that encapsulate **Doctrine DBAL** to provide high-performance, object-oriented CRUD (create, read, update, delete) 
functionality for relational databases. It is a lot faster and less complex than Datamapper ORM implementations.

![Doctrine ActiveRecord](doctrine-active-record/img/workflow.svg)

## Composer ##

To use this library in your project, simply run `composer require symlex/doctrine-active-record` or
add "symlex/doctrine-active-record" to your [composer.json](https://getcomposer.org/doc/04-schema.md) file and run `composer update`:

```json
{
    "require": {
        "php": ">=7.1",
        "symlex/doctrine-active-record": "^4.0"
    }
}
```

!!! info
    This library is part of [Symlex](https://symlex.org/) and not an official Doctrine project.
