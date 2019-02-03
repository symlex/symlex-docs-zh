# 启动框架

## 微内核 ##

启动框架用来展示一个 [微内核](https://github.com/symlex/di-microkernel)。 只需几行即可设置环境参数，初始化服务容器并运行应用程序:

```php
<?php

namespace DIMicroKernel;

class Kernel
{
    protected $environment;
    protected $debug;
    protected $appPath;

    public function __construct(
        $environment = 'app', $appPath = '', $debug = false)
    {
        $this->environment = $environment;
        $this->debug = $debug;
        $this->appPath = $appPath;

        $this->boot();
    }

    ...

    public function getApplication()
    {
        return $this->getContainer()->get('app');
    }

    public function run()
    {
        return $this->getApplication()->run();
    }
}
```

## 自定义 ##

可以扩展内核基类以针对特定目的（例如，命令行应用程序）对其进行自定义:

```php
<?php
namespace App\Kernel;

use DIMicroKernel\Kernel;

class ConsoleApp extends Kernel
{
    public function __construct($appPath, $debug = false)
    {
        parent::__construct('console', $appPath, $debug);
    }

    public function setUp()
    {
        chdir($this->getAppPath());
        set_time_limit(0);
        ini_set('memory_limit', '-1');
    }
}
```

## 启动应用 ##

创建一个内核实例并调用 `run()` 足以启动任何应用程序（参见`app/console` 和 `web/app.php`）：

```php
#!/usr/bin/env php
<?php

require_once __DIR__ . '/../vendor/autoload.php';

use App\Kernel\ConsoleApp;
$app = new ConsoleApp (__DIR__);
$app->run();
```
