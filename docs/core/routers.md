# Routers

There are 4 example routers included in this library. They configure the [Symfony](https://symfony.com/doc/current/components/routing.html) router to perform the actual routing, so you can expect the same high performance.
After routing a request to the appropriate controller action, the router subsequently renders the response to ease controller testing (actions never directly return JSON or HTML):

`Symlex\Router\Web\RestRouter` handles REST requests (JSON)

`Symlex\Router\Web\ErrorRouter` renders exceptions as error messages (HTML or JSON)

`Symlex\Router\Web\TwigRouter` renders regular Web pages via Twig (HTML)

`Symlex\Router\Web\TwigDefaultRouter` is like TwigRouter but sends all requests to a default controller action (required for client-side routing e.g. with Vue.js)

It's easy to create your own custom routing/rendering based on the existing examples.

## Configuration ##

The application's HTTP kernel class initializes the routers that were configured in the service container:
```php
<?php

namespace Symlex\Kernel;

class WebApp extends App
{
    protected $urlPrefix = '';

    public function __construct($appPath, $debug = false)
    {
        parent::__construct('web', $appPath, $debug);
    }

    public function init()
    {
        if ($this->debug) {
            ini_set('display_errors', 1);
        }
    }

    public function getUrlPrefix($urlPrefixPostfix = ''): string
    {
        return $this->urlPrefix . $urlPrefixPostfix;
    }

    public function setUrlPrefix(string $urlPrefix)
    {
        $this->urlPrefix = $urlPrefix;
    }

    protected function setUp()
    {
        $container = $this->getContainer();

        // The error router catches errors and displays them
        $container
            ->get('router.error')
            ->route();

        // Routing for REST API calls
        $container
            ->get('router.rest')
            ->route($this->getUrlPrefix('/api'), 'controller.rest.');

        // All other requests are routed to matching actions
        $container
            ->get('router.twig')
            ->route($this->getUrlPrefix(), 'controller.web.');
    }
}
```

The REST and Twig routers accept optional URL (e.g. `/api`) and service name prefixes (e.g. `controller.rest.`).

## Examples ##

Routing examples for the default HTTP kernel (`Symlex\Kernel\WebApp`):

`GET /` will be routed to `controller.web.index` service's `indexAction(Request $request)`

`POST /session/login` will be routed to `controller.web.session` service's `postLoginAction(Request $request)`

`GET /api/users` will be routed to `controller.rest.users` service's `cgetAction(Request $request)`

`POST /api/users` will be routed to `controller.rest.users` service's `postAction(Request $request)`

`OPTIONS /api/users` will be routed to `controller.rest.users` service's `coptionsAction(Request $request)`

`GET /api/users/123` will be routed to `controller.rest.users` service's `getAction($id, Request $request)`

`OPTIONS /api/users/123` will be routed to `controller.rest.users` service's `optionsAction($id, Request $request)`

`GET /api/users/123/comments` will be routed to `controller.rest.users` service's `cgetCommentsAction($id, Request $request)`

`GET /api/users/123/comments/5` will be routed to `controller.rest.users` service's `getCommentsAction($id, $commendId, Request $request)`

`PUT /api/users/123/comments/5` will be routed to `controller.rest.users` service's `putCommentsAction($id, $commendId, Request $request)`

The routers pass on the request instance to each matched controller action as last argument. It contains request parameters and headers as described in the [Symfony documentation](http://symfony.com/doc/current/book/http_fundamentals.html#requests-and-responses-in-symfony).

Controller actions invoked by **TwigRouter** can either return nothing (the matching Twig template will be rendered), an array (the Twig template can access the values as variables) or a string (redirect URL). 

REST controller actions (invoked by **RestRouter**) always return arrays, which are automatically converted to valid JSON. Delete actions can return null ("204 No Content").

For more examples see [framework documentation](../framework/routing.md).