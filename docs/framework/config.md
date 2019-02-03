# 配置框架

位于`app/config`的 YAML 文件根据参数和服务配置整个系统。  
文件名与应用程序的环境名称匹配（请参阅[Kernel](kernel.md)构造函数）：

 - `app/config/web.yml` 用于配置 Web (HTTP) 应用程序启动框架位于 `web/app.php`
 - `app/config/console.yml` 用于配置命令行应用程序启动框架位于 `app/console`

这些文件的格式与 Symfony 中的格式相同。 除常规服务外，它们还包含实际应用程序作为服务：

```yaml
services:
  app:
    class: Symlex\Application\Web
    public: true
```

这为使用相同内核引导Web（`Symlex\Application\Web`）和命令行（`Symfony\Component\Console\Application`）应用程序提供了统一的方法。

!!! info
    如果关闭调试模式，则服务容器将缓存在`storage/cache/`中。更新配置后，您必须运行`app/clearcache`。 要完全禁用缓存，请将`container.cache:false`添加到`app/config/parameters.yml` *
