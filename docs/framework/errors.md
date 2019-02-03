# 错误处理

应用程序会自动捕获异常，然后将其传递给 ErrorRouter ， ErrorRouter 会呈现 HTML 错误页面或将错误详细信息返回为 JSON （具体取决于请求标头）。 异常类名称映射到错误代码中 `app/config/exceptions.yml`:

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

Twig 错误模板的文件名是`app/templates/error/[code].twig`。 如果未找到模板，则为默认模板 (`default.twig`) 。
