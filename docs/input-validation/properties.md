# Field Properties

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
