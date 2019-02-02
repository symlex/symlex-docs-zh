# Input Validation using Forms

Our [InputValidation](https://github.com/symlex/input-validation) package provides 
whitelist validation ("accept known good") that is perfectly suited for building secure REST services. It uses programming language independent validation rules (plain array) that can be reused for additional client-side validation (JavaScript) or passed to template rendering engines such as Twig. By design, it is compatible with any framework and input source (HTML, REST, RPC, ...).

A major advantage of this data source agnostic approach is that developers can do bottom-up development using unit tests to find bugs early and work on validation rules without an existing HTML frontend or storage backend. Use case specific input value validation is also more secure than general model validation, which often relies on a blacklist ("reject known bad").

Besides basic validation rules such as type or length, more advanced features are supported as well - for example dependent fields, internationalization and multi-page forms. Validated values can be fetched individually, as flat array, by tag or by page.

The usage is simple: Form classes can inherit their definitions from each other. If needed, validation behavior can be changed using standard object-oriented methodologies. You don't need to hold a PhD in design patterns to understand how it works.

!!! example
    ```php
    <?php
    
    namespace App\Form\User;
    
    use App\Form\FormAbstract;
    
    class ProfileForm extends FormAbstract
    {
        protected function init(array $params = array())
        {
            $definition = [
                'userFirstName' => [
                    'caption' => 'First Name',
                    'type' => 'string',
                    'min' => 2,
                    'max' => 64,
                    'required' => true,
                ],
                'userLastName' => [
                    'caption' => 'Last Name',
                    'type' => 'string',
                    'min' => 2,
                    'max' => 64,
                    'required' => true,
                ],
                'userEmail' => [
                    'caption' => 'E-mail',
                    'type' => 'email',
                    'max' => 127,
                    'required' => true,
                ],
                'userNewsletter' => [
                    'caption' => 'Receive newsletter and other occasional updates',
                    'type' => 'bool',
                    'required' => false,
                ]
           ];
    
            $this->setDefinition($definition);
        }
    }
    ```
    
## Properties ##

Name                   | Description
---------------------- | ---------------------------------------------------------------------------------------------------
caption                | Field title (used for form rendering and in validation messages)
type                   | Data type: int, numeric, scalar, list, bool, string, email, ip, url, date, datetime, time and switch
type_params            | Optional parameters for data type validation
options                | Array of possible values for the field (for select lists or radio button groups)
min                    | Minimum value for numbers/dates, length for strings or number of elements for lists
max                    | Maximum value for numbers/dates, length for strings or number of elements for lists
required               | Field cannot be empty (if false, setDefinedValues() and setDefinedWritableValues() still throw an exception, if it does not exist at all)
optional               | setDefinedValues() and setDefinedWritableValues() don't throw an exception, if the field is missing in the input values (usefull for checkboxes or certain JavaScript frameworks, that do not submit any data for empty form elements e.g. AngularJS)
readonly               | User is not allowed to change the field (not writable)
hidden                 | User can not see the field (no impact on the validation)
default                | Default value
regex                  | Regular expression to match against
matches                | Field value must match another form field (e.g. for password or email validation). Property can be prefixed with "!" to state that the fields must be different.
depends                | Field is required, if the given form field is not empty
depends_value          | Field is required, if the field defined in "depends" has this value
depends_value_empty    | Field is required, if the field defined in "depends" is empty
depends_first_option   | Field is required, if the field defined in "depends" has the first value (see "options")
depends_last_option    | Field is required, if the field defined in "depends" has the last value (see "options")
page                   | Page number for multi-page forms
tags                   | Optional list of tags (can be used to extract values by tag, see getValuesByTag())

## Client-side, Form and Model Validation ##

The following visualization highlights the differences between client-side, form (input value) and model validation. Model validation generally operates on trusted data (internal system state) and should be repeatable at any point in time while input validation explicitly operates once on data that comes from untrusted sources (depending on the use case and user privileges). This separation makes it possible to build reusable models, controllers and forms that can be coupled through dependency injection (see REST controller example).

Think of form validation as whitelist validation ("accept known good") and model validation as blacklist validation ("reject known bad"). Whitelist validation is more secure while blacklist validation prevents your model layer from being overly constrained to very specific use cases.

Invalid model data should always cause an exception to be thrown (otherwise the application can continue running without noticing the mistake) while invalid input values coming from external sources are not unexpected, but rather common (unless you got users that never make mistakes). Validation within a specific model may not be possible at all, if a set of input values must be validated together (because they depend on each other) but individual values are then stored in different models - at least it can create additional dependencies between models that would not be there otherwise up to the point that all models depend on each other. In short: The application may still work as expected, but the code is a mess.

From a theoretical standpoint, any complex system has more internal state than it exposes to the outside, thus it is never sufficient to use model validation only - except the model provides two sets of methods: some that are used internally and some that can be exposed to arbitrary input data from any source. Aside from side-effects such as limited user feedback (exception messages) and bloated model code, this approach may easily lead to serious security flaws. Malicious input data is a much higher threat to multi-user Web applications than to classical single-user desktop applications. Simple blacklist model validation may be fully sufficient for desktop applications, which are in full control of the user interface (view layer).

Client-side (JavaScript or HTML) form validation is always just a convenience feature and not reliable. However, with this library you can (at least partly) reuse existing server-side form validation rules to perform client-side validation, since they can be easily converted to JSON (for JavaScript) or be passed to template rendering engines such as Twig or Smarty (for HTML). Reusing model layer validation rules in a similar fashion is at least difficult, if not impossible.

See also [Where to include business rule validation (OWASP)](https://www.owasp.org/index.php/Data_Validation#Where_to_include_business_rule_validation).

![Differences between client-side, input value (form) and model validation](https://www.lucidchart.com/publicSegments/view/5461f867-ae1c-44a4-b565-6f780a00cf27/image.png)
