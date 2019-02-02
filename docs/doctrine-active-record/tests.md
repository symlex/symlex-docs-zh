# Unit Tests

This library comes with a `docker-compose.yml` file for MySQL and database fixtures to run unit tests (MySQL will bind to 127.0.0.1:3308):

```
localhost# docker-compose up -d
localhost# docker-compose exec mysql sh
docker# cd /share/src/Tests/_fixtures
docker# mysql -u root --password=doctrine doctrine-active-record < schema.sql
docker# exit
localhost# bin/phpunit 
PHPUnit 7.3.2 by Sebastian Bergmann and contributors.

................................................................. 65 / 91 ( 71%)
..........................                                        91 / 91 (100%)

Time: 251 ms, Memory: 8.00MB

OK (91 tests, 249 assertions)
localhost# docker-compose down

```