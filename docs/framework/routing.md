# 路由和渲染 #

将控制器动作的匹配请求基于约定而不是广泛的配置来执行。

我们有 4 个示例路由器在我们的 [核心](../core/routers.md) 中。 他们的配置和使用与 [Symfony](https://symfony.com/doc/current/components/routing.html) 的路由器执行实际路由，因此可以期待相同的高性能。

在将请求路由到适当的控制器操作后，路由器随后呈现响应以简化控制器测试：

`Symlex\Router\Web\RestRouter` 处理 REST 请求（JSON）

`Symlex\Router\Web\ErrorRouter` 将异常呈现为错误消息（ HTML 或 JSON ）

`Symlex\Router\Web\TwigRouter` 通过 Twig 呈现常规网页 (HTML)

`Symlex\Router\Web\TwigDefaultRouter` 就像 TwigRouter ，但将所有请求发送到默认控制器操作（客户端路由需要，例如使用 Vue.js ）

控制器动作不应直接返回 JSON 或 HTML ，但它们可以返回 Symfony Request 对象，例如 返回二进制数据或实现特殊用例

## 默认路由 ##

下面，您将根据我们的默认配置找到一些示例 `App\Kernel\WebApp`:

```php
<?php

// 错误路由器捕获错误并将其显示为错误页面
$container->get('router.error')->route();

// REST API调用的路由
$container->get('router.rest')
    ->route($this->getUrlPrefix('/api/v1'), 'controller.rest.v1.');

// 所有其他请求都路由到默认控制器操作
$container->get('router.twig_default')
    ->route($this->getUrlPrefix(), 'controller.web.index', 'index');

// 取消注释以下行以启用服务器端路由
// $container->get('router.twig')
//    ->route($this->getUrlPrefix(), 'controller.web.');
```

所有请求（除了那些以 `/api/v1`）将被路由到 `controller.web.index` 服务的 `indexAction(Request $request)`

`GET /api/v1/users` 将被路由到 `controller.rest.v1.users` 服务的 `cgetAction(Request $request)`

`POST /api/v1/users` 将被路由到 `controller.rest.v1.users` 服务的 `postAction(Request $request)`

`OPTIONS /api/v1/users` 将被路由到 `controller.rest.v1.users` 服务的 `coptionsAction(Request $request)`

`GET /api/v1/users/123` 将被路由到 `controller.rest.v1.users` 服务的 `getAction($id, Request $request)`

`OPTIONS /api/v1/users/123` 将被路由到 `controller.rest.v1.users` 服务的 `optionsAction($id, Request $request)`

`GET /api/v1/users/123/comments` 将被路由到 `controller.rest.v1.users` 服务的 `cgetCommentsAction($id, Request $request)`

`GET /api/v1/users/123/comments/5` 将被路由到 `controller.rest.v1.users` 服务的 `getCommentsAction($id, $commendId, Request $request)`

`PUT /api/v1/users/123/comments/5` 将被路由到 `controller.rest.v1.users` 服务的 `putCommentsAction($id, $commendId, Request $request)`

如果你取消注释 `router.twig` 而替换 `router.twig_default`, 所有的请求 (除了那些以 `/api/v1` 开头的)
将被路由到匹配 Web 控制器操作 e.g.:

`GET /` 将被路由到 `controller.web.index` 服务的 `indexAction(Request $request)`

`GET /foo` 将被路由到 `controller.web.foo` 服务的 `indexAction(Request $request)`

`GET /foo/bar` 将被路由到 `controller.web.foo` 服务的 `barAction(Request $request)`

`POST /foo/bar` 将被路由到 `controller.web.foo` 服务的 `postBarAction(Request $request)`


## 开发 ##

根据我们的示例，可以轻松创建自己的自定义路由。

在这种情况下，您应该使用自定义内核作为应用程序的[ HTTP 内核](https://github.com/symlex/symlex/blob/master/src/Kernel/WebApp.php)
负责初始化路由器和设置可选的 URL /  service name prefixes。
