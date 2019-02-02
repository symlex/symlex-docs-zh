# Kernel

Our light-weight kernel can bootstrap almost any PHP application. It is based on our 
[DIMicroKernel](../di-microkernel.md) library. The kernel itself is just a few lines 
to set environment parameters, initialize the Symfony service container and then start the app by calling `run()`.

## Configuration ##

YAML files located in `config/` configure the application and all of it's dependencies as a service. The filename matches 
the application's environment name (e.g. `config/console.yml`). The configuration can additionally be modified 
for sub environments such as local or production by providing a matching config file like `config/console.local.yml`
(see `app.sub_environment` parameter). These files are in the same [well documented](https://symfony.com/doc/current/components/dependency_injection.html) 
format you might know from Symfony:

```yaml
parameters:
    app.name: 'My App'
    app.version: '1.0'

services:
    doctrine.migrations.migrate:
        class: Doctrine\DBAL\Migrations\Tools\Console\Command\MigrateCommand
        
    app:
        class: Symfony\Component\Console\Application
        arguments: [%app.name%, %app.version%]
        public: true
        calls:
            - [ add, [ "@doctrine.migrations.migrate" ] ]
```

This provides a uniform approach for bootstrapping Web applications such as `Symlex\Application\Web` or command-line 
applications like `Symfony\Component\Console\Application` (wrapped in `Symlex\Application\Console`) using the same kernel.
The result is much cleaner and leaner than the usual bootstrap and configuration madness you know from many frameworks.

## Disable Caching ##

If debug mode is turned off, the service container configuration is cached by the kernel in the directory set as cache path. 
You have to delete all cache files after updating the configuration. To disable caching completely, add 
`container.cache: false` to your config parameters:

```yaml
parameters:
    container.cache: false
```

## Run multiple kernels via `Symlex\Kernel\WebApps` ##

!!! info
    This is an experimental proof-of-concept. Feedback welcome.

As an alternative to Symfony bundles, `Symlex\Kernel\WebApps` is capable of running multiple apps based on `Symlex\Kernel\App` on the same Symlex installation:

```php
<?php
$app = new WebApps('web', __DIR__ . '/../app', false);
$app->run();
```

It's bootstrapped like a regular WebApp and subsequently bootstaps other Symlex apps according to the configuration in `app/config/web.guests.yml` (path, debug, prefix and domain are optional; bootstrap and config are required):

```yaml
example:
    prefix: /example
    domain: www.example.com
    bootstrap: \Symlex\Kernel\WebApp
    config: web.yml
    debug: true
    path: vendors/foo/bar/app

default:
    bootstrap: \Symlex\Kernel\WebApp
    config: web.default.yml
```

!!! note
    Assets in web/ like images, CSS or JavaScript in are not automatically shared in a way Assetic does this with Symfony bundles. If your apps not only provide Web services, you might have to create symbolic links or modify your HTML templates.

## Interceptors ##

HTTP interceptors can be used to perform HTTP authentication or other actions (e.g. blocking certain IP ranges) **before** routing a request:

```php
<?php

use Symlex\Kernel\App;

class WebApp extends App
{
    public function __construct($appPath, $debug = false)
    {
        parent::__construct('web', $appPath, $debug);
    }

    public function boot () {
        parent::boot();

        $container = $this->getContainer();

        /*
         * In app/config/web.yml:
         *
         * services:
         *     http.interceptor:
         *         class: Symlex\Router\HttpInterceptor
         */
        $interceptor = $container->get('http.interceptor');
        $interceptor->digestAuth(
            'Realm', 
            array('foouser' => 'somepassword')
        );

        $container
            ->get('router.error')
            ->route();

        $container
            ->get('router.rest')
            ->route('/api', 'controller.rest.');

        $container
            ->get('router.twig')
            ->route('', 'controller.web.');
    }
}
```