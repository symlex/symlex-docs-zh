# Error Handling

Exceptions are automatically catched by the application and then passed on to ErrorRouter, which either renders an HTML error page or returns the error details as JSON (depending on the request headers). Exception class names are mapped to error codes in `app/config/exceptions.yml`:

```yaml
parameters:
    exception.codes:
        InvalidArgumentException: 400
        App\Exception\UnauthorizedException: 401
        App\Exception\AccessDeniedException: 403
        App\Exception\FormInvalidException: 409
        Exception: 500

    exception.messages:
        400: 'Bad request'
        401: 'Unauthorized'
        403: 'Forbidden'
        404: 'Not Found'
        405: 'Method Not Allowed'
        500: 'Looks like something went wrong!'

services:
    router.error:
        class: Symlex\Router\Web\ErrorRouter
        public: true
        arguments:
            - "@app"
            - "@twig"
            - "%exception.codes%"
            - "%exception.messages%"
            - "%app.debug%"
```

The filename for Twig error templates is `app/templates/error/[code].twig`. If no template is found, the default template (`default.twig`) is used.
