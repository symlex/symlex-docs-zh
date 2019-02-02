# API Documentation

**__construct(Translator $translator, Form\Validator $validator, Form\OptionsInterface $options, array $params = array())**

The constructor requires instances of Symfony\Component\Translation\TranslatorInterface, InputValidation\Form\Validator, InputValidation\Form\OptionsInterface and an optional set of arbitrary parameters, which are passed to **init(array $params = array())** (protected method for initializing the form).

**setOptions(Form\OptionsInterface $options)**

Sets an optional class instance to automatically fill option lists (see "options" form field property). OptionsInterface only requires a method get($listName) that returns an array of options.

**getOptions(): Form\OptionsInterface**

Returns the options instance 

**options(string $listName): array**

Returns a list of options e.g. countries:

```php
'country' => array(
    'type' => 'string',
    'caption' => 'Country',
    'default' => 'DE',
    'options' => $this->form->options('countries')
)
```

**optionsWithDefault(string $listName, string $defaultLabel = ''): array**

Returns a list of options with default label for no selection

```php
'country' => array(
    'type' => 'string',
    'caption' => 'Country',
    'required' => true,
    'options' => $this->form->optionsWithDefault('countries')
)
```
     
**getTranslator(): Translator**

Returns the Translator instance (see __construct)

**setTranslator(Translator $translator)**

Sets the Translator instance (see __construct)

**getValidator(): Validator**

Returns the Validator instance (see __construct)

**setValidator(Validator $validator)**

Sets the Validator instance (see __construct)

**getLocale()**

Returns the current locale e.g. en, de or fr

**setLocale($locale)**

Sets the current locale e.g. en, de or fr

**setDefinition(array $definition)**

Sets the form field definition array (see example and form field properties)

**getDefinition($key = null, $propertyName = null): mixed**

Returns the form field definition(s). If $key is null, definitions for all fields are returned. If $propertyName is null and $key is not null, only the definition of the given field key are returned. If both arguments are not null, only the definition of the given form field property is returned (for example, getDefinition('firstname', 'type') might return 'string'). A FormException is thrown otherwise.

**getFieldDefinition(string $name): array**

Returns form field definition as array (wrapper for getDefinition)

**getFieldProperty(string $name, string $property): mixed**

Returns a field property from the form definition (wrapper for getDefinition)

**addDefinition($key, array $definition)**

Adds a single form field definition (see form field properties)

**changeDefinition($key, array $changes)**

Changes a single form field definition (see form field properties)

**setGroups(array $groups)**

Sets form field groups (optional feature, if you want to reuse your form definition to reder the form as HTML).

Example:
```php
$form->setGroups(
    array(
        'name' => array('firstname', 'lastname'),
        'address' => array('street', 'housenr', 'zip', 'city')
    )
);
```

**getFieldAsArray(string $key): array**

Returns field definition and value as JSON compatible array:

```php
array (
  'name' => 'country',
  'caption' => 'Country',
  'default' => 'DE',
  'type' => 'string',
  'options' => array (
    array (
      'option' => 'US',
      'label' => 'United States',
    ),
    array (
      'option' => 'GB',
      'label' => 'United Kingdom',
    ),
    array (
      'option' => 'DE',
      'label' => 'Germany',
    ),
  ),
  'value' => 'DE',
  'uid' => 'id58a401f5a54e6',
),
```

**getAsArray(): array**

Returns the complete form (definition and all values) as JSON compatible array, which can be used to render the form in templates:

```php
array (
  'company' => array (
    'name' => 'company',
    'caption' => 'Company',
    'type' => 'string',
    'value' => 'IBM',
    'uid' => 'id58a401f5a54d6',
  ),
  'country' => array (
    'name' => 'country',
    'caption' => 'Country',
    'default' => 'DE',
    'type' => 'string',
    'options' => array (
      array (
        'option' => 'US',
        'label' => 'United States',
      ),
      array (
        'option' => 'GB',
        'label' => 'United Kingdom',
      ),
      array (
        'option' => 'DE',
        'label' => 'Germany',
      ),
    ),
    'value' => 'DE',
    'uid' => 'id58a401f5a54e6',
  ),
),
```

**getAsGroupedArray(): array**

Returns grouped form field definitions and values (you must use setGroups() first):

```php
array(
  'person' => array (
    'group_name' => 'person',
    'group_caption' => 'Person',
    'fields' => array(
      'person' => array (
        'name' => 'firstname',
        'caption' => 'First Name',
        'type' => 'string',
        'readonly' => true,
        'value' => NULL,
        'uid' => 'id58a401f5a5267',
      ),
      'lastname' => array (
        'name' => 'lastname',
        'caption' => 'Last Name',
        'type' => 'string',
        'readonly' => false,
        'value' => 'Mander',
        'uid' => 'id58a401f5a5279',
      ),
    ),
  ),
  'location' => array (
    'group_name' => 'location',
    'group_caption' => 'Location',
    'fields' => array (
      'company' => array (
        'name' => 'company',
        'caption' => 'Company',
        'type' => 'string',
        'value' => 'IBM',
        'uid' => 'id58a401f5a54d6',
      ),
      'country' => array (
        'name' => 'country',
        'caption' => 'Country',
        'type' => 'string',
        'default' => 'DE',
        'options' => array (
          array (
            'option' => 'US',
            'label' => 'United States',
          ),
          array (
            'option' => 'GB',
            'label' => 'United Kingdom',
          ),
          array (
            'option' => 'DE',
            'label' => 'Germany',
          ),
        ),
        'value' => 'DE',
        'uid' => 'id58a401f5a54e6',
      ),
    ),
  ),
),
```

**setAllValues(array $values)**

Sets all form values (does not check, if the fields exist or if the fields are writable by the user). Throws an exception, if you try to set values for undefined fields.

**setDefinedValues(array $values)**

Iterates through the form definition and sets the values for fields, that are present in the form definition.

**setWritableValues(array $values)**

Iterates through the passed value array and sets the values for fields, that are writable by the user.

**setDefinedWritableValues(array $values)**

Sets the values for fields, that are present in the form definition and that are writable by the user (recommended method for most use cases).

**setWritableValuesOnPage(array $values, $page)**

Sets the values for fields on the given page, that are present in the form definition and that are writable by the user (recommended method for most use cases, if the form contains multiple pages).

**getValuesByPage()**

Returns the form values for all elements grouped by page.

**getValuesByTag($tag)**

Returns the form values for all elements by tag (see "tags" form field property)

**getValues()**

Returns all form field values

**getWritableValues()**

Returns all user writable form field values

**translate($token, array $params = array())**

Uses the Translator adapter to translate the given string/token (accepts optional parameters for the translation string).

**_($token, array $params = array())**

Alias for translate()

**addError($key, $token, array $params = array())**

Adds a validation error (uses translate() for the error message internally)

**validate()**

Validates all form field values. You can use getErrors(), getErrorsByPage(), isValid() and hasErrors() to get the validation results.

**hasErrors(): bool**

Returns true, if the form has errors

**isValid(): bool**

eturns true, if the form is valid (has no errors)

**getErrors(): array**

Returns all errors and throws an exception, if the validation was not performed yet (you must call validate() before calling getErrors()).

**getFirstError(): string**

Returns the first error as string

**getErrorsAsText(): string**

Returns all errors as indented text (for command line applications)

**getErrorsByPage(): array**

Returns all errors grouped by page and throws an exception, if the validation was not performed yet (you must call validate() before calling getErrorsByPage()).

**clearErrors()**

Resets the validation and clears all errors

**getHash(): string**

Returns hash that uniquely identifies the form (for caching comprehensive forms)
