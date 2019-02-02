# Configuration

## YAML Files ##

YAML files located in `config/` configure the application and all of it's dependencies as a service. The filename matches 
the application's environment name (e.g. `config/console.yml`). The configuration can additionally be modified 
for sub environments such as local or production by providing a matching config file like `config/console.local.yml`
(see `app.sub_environment` parameter). These files are in the same [well documented](https://symfony.com/doc/current/components/dependency_injection.html) format you might know from Symfony:

```yaml
parameters:
    app.name: 'My App'
    app.version: '1.0'

services:
    doctrine.migrations.migrate:
        class: Doctrine\DBAL\Migrations\Tools\Console\Command\MigrateCommand
        
    app:
        class: Symfony\Component\Console\Application
        public: true
        arguments: [%app.name%, %app.version%]
        calls:
            - [ add, [ "@doctrine.migrations.migrate" ] ]
```

This provides a uniform approach for bootstrapping Web applications like `Silex\Application`,
`Symlex\Application\Web` or command-line applications like `Symfony\Component\Console\Application` using the same kernel. 
The result is much cleaner and leaner than the usual bootstrap and configuration madness you know from many frameworks.

## Parameters ##

The kernel sets a number of default parameters that can be used for configuring services. The default values can be 
changed via setter methods of the kernel or overwritten/extended by container config files 
and environment variables (e.g. `url: '%env(DATABASE_URL)%'`).

Parameter           | Getter method         | Setter method         | Default value            
--------------------|-----------------------|-----------------------|------------------
app.name            | getName()             | setName()             | 'Kernel'
app.version         | getVersion()          | setVersion()          | '1.0'
app.environment     | getEnvironment()      | setEnvironment()      | 'app'
app.sub_environment | getSubEnvironment()   | setSubEnvironment()   | 'local'
app.debug           | isDebug()             | setDebug()            | false
app.charset         | getCharset()          | setCharset()          | 'UTF-8'
app.path            | getAppPath()          | setAppPath()          | './'
app.config_path     | getConfigPath()       | setConfigPath()       | './config'
app.base_path       | getBasePath()         | setBasePath()         | '../'
app.storage_path    | getStoragePath()      | setStoragePath()      | '../storage'
app.log_path        | getLogPath()          | setLogPath()          | '../storage/log'
app.cache_path      | getCachePath()        | setCachePath()        | '../storage/cache'
app.src_path        | getSrcPath()          | setSrcPath()          | '../src'

## Customization ##

The kernel base class can be extended to customize it for a specific purpose such as long running console applications:

```php
<?php

use DIMicroKernel\Kernel;

class ConsoleApp extends Kernel
{
    public function __construct($appPath, $debug = false)
    {
        parent::__construct('console', $appPath, $debug);
    }

    public function setUp()
    {
        set_time_limit(0);
        ini_set('memory_limit', '-1');
    }
}
```

## Caching ##

If debug mode is turned off, the service container configuration is cached by the kernel in the directory set as cache path. You have to delete all cache files after updating the configuration. To disable caching completely, add `container.cache: false` to your configuration parameters: 

```yaml
parameters:
    container.cache: false
```
