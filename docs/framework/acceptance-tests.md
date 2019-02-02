# Acceptance Tests

A ready-to-use [Codeception](https://codeception.com/) test suite for acceptance tests is located in `app/codeception/`:

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

Codeception's main config file is `codeception.yml` in the project directory.