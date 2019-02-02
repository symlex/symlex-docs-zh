# Configuration

YAML files located in `app/config` configure the entire system based on parameters and services. 
The filename matches the application's environment name (see [Kernel](kernel.md) constructor):
 
 - `app/config/web.yml` configures Web (HTTP) applications bootstrapped in `web/app.php`
 - `app/config/console.yml` configures command-line applications bootstrapped in `app/console`

These files are in the same format you know from Symfony. In addition to the regular services, they also contain the actual application as a service:

```yaml
services:
  app:
    class: Symlex\Application\Web
    public: true
```

This provides a uniform approach for bootstrapping Web (`Symlex\Application\Web`) and command-line (`Symfony\Component\Console\Application`) applications with the same kernel.

!!! info
    If debug mode is turned off, the service container is cached in `storage/cache/`. You have to run `app/clearcache` after updating the configuration. To disable caching completely, add `container.cache: false` to  `app/config/parameters.yml`*
