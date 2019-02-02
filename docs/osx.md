# PHP, Composer and Docker on Mac OS X

Mac OS X is shipped with outdated PHP versions. You can install a more current version via [Homebrew](https://brew.sh/)
or download the latest version from [Liip](https://php-osx.liip.ch/).

After installing a custom PHP version, you should add its path to your `PATH` in `~/.bash_profile`:

```
export PATH="/usr/local/bin:/usr/local/php5/bin:$PATH"
```

Composer is available for download at https://getcomposer.org/download/ (follow the instructions). Please add `/usr/local/bin` to your path in `~/.bash_profile` and move composer there instead of keeping composer.phar in your local project directory:

```
sudo mv composer.phar /usr/local/bin/composer
```

Docker - a free tool that provides easy-to-use container virtualization - is available for download at https://download.docker.com/mac/stable/Docker.dmg

To properly work with JavaScript, you should also install [NodeJS](https://nodejs.org/en/download/) (includes npm) and [Yarn](https://yarnpkg.com/en/):

```
sudo npm install -g yarn
```
