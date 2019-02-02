# Easy & Secure Whitelist Validation for PHP

[![Latest Stable Version](https://poser.pugx.org/symlex/input-validation/v/stable.svg)](https://packagist.org/packages/symlex/input-validation)
[![License](https://poser.pugx.org/symlex/input-validation/license.svg)](https://packagist.org/packages/symlex/input-validation)
[![Test Coverage](https://codecov.io/gh/symlex/input-validation/branch/master/graph/badge.svg)](https://codecov.io/gh/symlex/input-validation)
[![Build Status](https://travis-ci.org/symlex/input-validation.png?branch=master)](https://travis-ci.org/symlex/input-validation)

**This library provides whitelist validation ("accept known good") that is perfectly suited for building secure REST services.** It uses programming language independent validation rules (plain array) that can be reused for additional client-side validation (JavaScript) or passed to template rendering engines such as Twig. By design, it is compatible with any framework and input source (HTML, REST, RPC, ...).

A major advantage of this data source agnostic approach is that developers can do bottom-up development using unit tests to find bugs early and work on validation rules without an existing HTML frontend or storage backend. Use case specific input value validation is also more secure than general model validation, which often relies on a blacklist ("reject known bad").

Besides basic validation rules such as type or length, more advanced features are supported as well - for example dependent fields, internationalization and multi-page forms. Validated values can be fetched individually, as flat array, by tag or by page.

The usage is simple: Form classes can inherit their definitions from each other. If needed, validation behavior can be changed using standard object-oriented methodologies. You don't need to hold a PhD in design patterns to understand how it works.

## Client-side, Form and Model Validation ##

The following visualization highlights the differences between client-side, form (input value) and model validation. Model validation generally operates on trusted data (internal system state) and should be repeatable at any point in time while input validation explicitly operates once on data that comes from untrusted sources (depending on the use case and user privileges). This separation makes it possible to build reusable models, controllers and forms that can be coupled through dependency injection (see REST controller example).

Think of form validation as whitelist validation ("accept known good") and model validation as blacklist validation ("reject known bad"). Whitelist validation is more secure while blacklist validation prevents your model layer from being overly constrained to very specific use cases.

Invalid model data should always cause an exception to be thrown (otherwise the application can continue running without noticing the mistake) while invalid input values coming from external sources are not unexpected, but rather common (unless you got users that never make mistakes). Validation within a specific model may not be possible at all, if a set of input values must be validated together (because they depend on each other) but individual values are then stored in different models - at least it can create additional dependencies between models that would not be there otherwise up to the point that all models depend on each other. In short: The application may still work as expected, but the code is a mess.

From a theoretical standpoint, any complex system has more internal state than it exposes to the outside, thus it is never sufficient to use model validation only - except the model provides two sets of methods: some that are used internally and some that can be exposed to arbitrary input data from any source. Aside from side-effects such as limited user feedback (exception messages) and bloated model code, this approach may easily lead to serious security flaws. Malicious input data is a much higher threat to multi-user Web applications than to classical single-user desktop applications. Simple blacklist model validation may be fully sufficient for desktop applications, which are in full control of the user interface (view layer).

Client-side (JavaScript or HTML) form validation is always just a convenience feature and not reliable. However, with this library you can (at least partly) reuse existing server-side form validation rules to perform client-side validation, since they can be easily converted to JSON (for JavaScript) or be passed to template rendering engines such as Twig or Smarty (for HTML). Reusing model layer validation rules in a similar fashion is at least difficult, if not impossible.

See also [Where to include business rule validation (OWASP)](https://www.owasp.org/index.php/Data_Validation#Where_to_include_business_rule_validation).

![Differences between client-side, input value (form) and model validation](https://www.lucidchart.com/publicSegments/view/5461f867-ae1c-44a4-b565-6f780a00cf27/image.png)

## Composer ##

To use this library in your project, simply run `composer require symlex/input-validation` or
add "symlex/input-validation" to your [composer.json](https://getcomposer.org/doc/04-schema.md) file and run `composer update`:

```json
{
    "require": {
        "php": ">=7.1",
        "symlex/input-validation": "^4.0"
    }
}
```