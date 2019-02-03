# Web应用程序的控制器

默认情况下， Symlex 控制器是普通的 PHP 类。 它们在 `app/config/web.yml`（HTML）或 `app/config/rest.yml`（REST）中配置为公共服务:

```yaml
controller.web.index:
    public: true
    class: App\Controller\Web\IndexController

controller.rest.v1.users:
    public: true
    class: App\Controller\Rest\V1\UsersController
    arguments: [ "@service.session", "@model.factory", "@form.factory" ]
    calls:
        - [ setMailService, [ "@service.mail" ]]
```

!!! note
    在许多其他框架中，默认情况下控制器不是服务。 一些开发人员习惯于让控制器直接访问服务容器，而不是使用依赖注入，这使得测试更加困难并导致更少的可移植代码（框架锁定）。

路由器将请求实例作为最后一个参数传递给每个匹配的控制器操作。 它包含请求参数和标题，如上所述 [Symfony 的文档](http://symfony.com/doc/current/book/http_fundamentals.html#requests-and-responses-in-symfony).

**Web 控制器操作** 可以返回不同的

 - **null**: 将呈现匹配的 Twig 模板
 - 一个 **array**: Twig 模板可以将这些值作为变量访问
 - 一个 **string**: 用户将被重定向到 URL
 - 或者 `Symfony\Component\HttpFoundation\Response` 的对象

Twig 的模板基目录可以在`app/config/twig.yml`（`twig.path`）中配置。 模板文件名与请求路径匹配：`[twig.path]/[controller]/[action].twig`。

如果没有给出控制器或动作名称，`index` 是默认值，例如 `index/index.twig`将用于渲染 `/`。

!!! example
    ```php
    <?php

    namespace App\Controller\Web;

    class IndexController
    {
        /**
         * 在 app/templates/default/index.twig 中呈现模板
         */
        public function indexAction()
        {
        }
    }
    ```
