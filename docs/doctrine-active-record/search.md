# Performing Search Queries

## Parameters ##

**EntityDao::search() accepts the following optional parameters to limit, filter and sort search results:**

`table`: Table name

`table_alias`: Alias name for "table" (table reference for join and join_left)

`cond`: Search conditions as array (key/value or just values for raw SQL)

`count`: Maximum number of results (integer)

`offset`: Result offset (integer)

`join`: List of joined tables incl join condition e.g. `array(array('u', 'phonenumbers', 'p', 'u.id = p.user_id'))`, see Doctrine DBAL manual

`left_join`: See join

`columns`: List of columns (array)

`order`: Sort order (if not false)

`group`: Group by (if not false)

`wrap`: If false, raw arrays are returned instead of DAO instances

`ids_only`: Return primary key values only

`sql_filter`: Raw SQL filter (WHERE)

`id_filter`: If not empty, limit result to this list of primary key IDs

## Result Object ##

When calling `search()` on a `EntityDao` or `EntityModel`, you'll get a `SearchResult` instance as return value.
It implements `ArrayAccess`, `Serializable`, `IteratorAggregate` and `Countable` and can be used either as array
or object with the following methods:

`getAsArray()`: Returns search result as array

`getSortOrder()`: Returns sort order as string

`getSearchCount()`: Returns search count (limit) as integer

`getSearchOffset()`:  Returns search offset as integer

`getResultCount()`: Returns the number of actual query results (<= limit)

`getTotalCount()`: Returns total result count (in the database)

`getAllResults()`: Returns all results as array of `EntityDao` or `EntityModel` instances

`getAllResultsAsArray()`: Returns all results as nested array (e.g. to serialize it as JSON)

`getFirstResult()`: Returns first result `EntityDao` or `EntityModel` instance or throws an exception
