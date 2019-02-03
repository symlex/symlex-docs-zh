# 快速开始

## 命令行应用 ##

确保你已安装 PHP 7.1+ 和 [Composer](https://getcomposer.org/) 在你的系统上。

**第一步:** 运行 `composer` 来创建一个新的项目用于我们的示例:

    composer create-project symlex/stream-sampler myapp

Composer 会询问一些配置的变量来为你生成 `app/config/parameters.yml` 。

**第二步:** 使用 `app/console` 来执行命令:

    cd myapp
    app/console sample -i internal -s 10

 根据参数和服务配置应用程序，YAML 会在 `app/config` 目录下生成。
主配置文件是 `app/config/console.yml`。

Repository: https://github.com/symlex/stream-sampler

## Web 应用 ##

在开始之前，请确定你已拥有 PHP 7.1+, [Composer](https://getcomposer.org/) 和 [Docker](https://www.docker.com/) 在你的系统里。
([如何安装](https://docs.symlex.org/zh/latest/osx/) 适用于 Mac OS X)。
如果你选择使用 Docker ，您可以根据现有的运行环境来设置自己的运行环境。
[Dockerfiles](https://github.com/symlex/symlex/tree/master/app/docker).
我们推荐使用 [Nginx](https://www.nginx.com/) 配合 [PHP-FPM](http://php.net/manual/en/install.fpm.php)
和 URL [重写规则](https://github.com/symlex/symlex/blob/master/app/docker/nginx/site.conf) 和 Symfony 十分相似。
你还需要一个 [数据库](https://dev.mysql.com/downloads/mysql/) 和
[nodejs](https://nodejs.org/en/)， [npm](https://www.npmjs.com/) ， [yarn](https://yarnpkg.com/) 来建立前端

### 简易 REST API ###

**第一步:** 使用 `composer` 来建立一个新项目:

```
composer create-project symlex/rest-api myapp
```

Composer 会询问一些配置的变量来为你生成 `app/config/parameters.yml` 。

确保 `storage/cache` 目录是可写的，以便用于生成缓存。

**第二步:** 用这个命令来启动 Nginx 和 PHP `docker-compose`:

```
cd myapp
docker-compose up
```

**第三步:** 在浏览器中访问 http://localhost:8088/example/123 ([源代码](https://github.com/symlex/rest-api/blob/master/src/Controller/ExampleController.php))。

打开终端器，运行 `docker-compose exec php sh`。

位于 app/config 中的 YAML 文件根据参数和服务配置应用程序。 主配置文件是 app/config/rest.yml。

!!! note
     如果你将 `localhost-debug` 添加到` /etc/hosts` 并使用它访问该站点，它将在 debug 模式中加载（您将在错误页面上看到堆栈跟踪和其他调试信息）。

Repository: https://github.com/symlex/rest-api

### 单页应用程序 ###

**第一步:** 使用 `composer` 来建立一个新项目:

```
composer create-project symlex/symlex myapp
```

Composer 会询问一些配置的变量来为你生成 `app/config/parameters.yml` 。

确保 `storage/cache` 目录是可写的，以便用于生成缓存。

**第二步:** Start nginx, PHP and MySQL using `docker-compose`:

```
cd myapp
docker-compose up
```

!!! info
    此 docker-compose 配置仅用于测试和开发目的。
    出于安全原因，如果您使用其他用户运行 Docker ，则可能需要对其进行调整。
    在OS X上，Docker的当前版本会 [非常慢](https://twitter.com/lastzero/status/829191426391027712)
    从主机的文件系统执行 PHP 。


**第三步:** 让 [Phing](https://www.phing.info/) 初始化数据库并为您构建前端组件：

```
docker-compose exec php sh
bin/phing dev
```

!!! tip
    您也可以使用此方法稍后执行其他命令(参见 `build.xml`)。 或者，您可以安装 [npm](https://www.npmjs.com/) 和 [yarn](https://yarnpkg.com/) 在 `/etc/hosts` 可以直接在本地链接 "db" 至 127.0.0.1。


Repository: https://github.com/symlex/symlex

#### Web UI ####

成功安装后，浏览器访问 http://localhost:8081/ 以用户名 `admin@example.com` 和密码 `passwd`。登入

!!! note
     如果你将 `localhost-debug` 添加到` /etc/hosts` 并使用它访问该站点，它将在 debug 模式中加载（您将在错误页面上看到堆栈跟踪和其他调试信息）。

![Screenshot](img/login.jpg)

#### MailHog ####

[Mailhog](https://github.com/ian-kent/MailHog) 界面可以在 http://localhost:8082/ 找到。 它可以使用
接收和查看系统自动发送的邮件，例如 当创建新用户。

![Screenshot](img/mailhog.jpg)
