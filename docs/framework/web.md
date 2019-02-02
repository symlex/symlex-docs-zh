# Controllers for Web Applications

Symlex controllers are plain PHP classes by default. They are configured as public services either in `app/config/web.yml` (HTML) or `app/config/rest.yml` (REST):

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
    In many other frameworks, controllers aren't services by default. Some developers are used to give 
    controllers direct access to the service container instead of using dependency injection, which makes testing more 
    difficult and leads to less portable code (framework lock-in).

The routers pass on the request instance to each matched controller action as last argument. It contains request parameters and headers as described on the [Symfony documentation](http://symfony.com/doc/current/book/http_fundamentals.html#requests-and-responses-in-symfony).

**Web controller actions** can either return 

 - **null**: Matching Twig template will be rendered 
 - an **array**: Twig template can access those values as variables
 - a **string**: User will be redirected to URL
 - or a `Symfony\Component\HttpFoundation\Response` object 
 
Twig's template base directory can be configured in `app/config/twig.yml` (`twig.path`). The template filename is matching the request route: `[twig.path]/[controller]/[action].twig`. 

If no controller or action name is given, `index` is the default e.g. `index/index.twig` will be used for rending `/`.

!!! example
    ```php
    <?php
    
    namespace App\Controller\Web;
    
    class IndexController
    {
        /**
         * Renders the template in app/templates/default/index.twig
         */
        public function indexAction()
        {
        }
    }
    ```