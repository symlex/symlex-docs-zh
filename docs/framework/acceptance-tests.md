# 验收测试

即用型 [ Codeception ](https://codeception.com/) 验收测试套件位于 `app/codeception/`:

    /var/www/html# bin/codecept run
    Codeception PHP Testing Framework v2.4.5
    Powered by PHPUnit 6.5.12 by Sebastian Bergmann and contributors.

    Acceptance Tests (8) --------------------------------------------
    ✔ HomeCest: Open homepage (2.35s)
    ✔ HomeCest: Login (7.66s)
    ✔ HomeCest: View register form (2.85s)
    ✔ HomeCest: Check links on welcome page (14.70s)
    ✔ UserCest: Logout (7.67s)
    ✔ UserCest: View users page and forms (12.46s)
    ✔ UserCest: View edit profile page (7.26s)
    ✔ UserCest: View change password page (6.45s)
    -----------------------------------------------------------------

    Time: 1.1 minutes, Memory: 12.00MB

    OK (8 tests, 25 assertions)

Codeception 的主配置文件是 `codeception.yml` 在项目目录中。
