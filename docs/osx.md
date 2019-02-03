# 在 Mac OS X 上使用 PHP, Composer 和 Docker

Mac OS X 附带过时的 PHP 版本。 您可以通过[ Homebrew ](https://brew.sh/)安装更新版本
或从[ Liip ](https://php-osx.liip.ch/)下载最新版本。

安装自定义 PHP 版本后，您应该将其路径添加到您的 `PATH` 位于你电脑上的 `~/.bash_profile`:

```
export PATH="/usr/local/bin:/usr/local/php5/bin:$PATH"
```

Composer 可从以下网址下载 https://getcomposer.org/download/ （按照说明）。 请把 `/usr/local/bin` 添加到以下文件中 `~/.bash_profile` 并将 composer 移动到那里，而不是将 composer.phar 保存在本地项目目录中：

```
sudo mv composer.phar /usr/local/bin/composer
```

Docker 是一款提供易于使用的容器虚拟化的免费工具，可从以下网址下载 https://download.docker.com/mac/stable/Docker.dmg

要正确使用 JavaScript ，您还应该安装[ NodeJS ](https://nodejs.org/en/download/) (包括 npm ) 和 [Yarn](https://yarnpkg.com/en/):

```
sudo npm install -g yarn
```
