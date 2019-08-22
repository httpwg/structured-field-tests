# Structured Header Tests

These are test cases for implementations of [Structured headers for HTTP](http://httpwg.org/http-extensions/draft-ietf-httpbis-header-structure.html).

## Test Format

Each test file is a JSON document that contains an array of test records. A test record is an
object with the following members:

- `name`: A string describing the test
- `raw`: An array of strings, each representing a header field value received
- `header_type`: One of "item", "list", "dictionary"
- `expected`: The expected data structure after parsing (if successful). Required, unless `must_fail` is `true`.
- `must_fail`: boolean indicating whether the test is required to fail. Defaults to `false`.
- `can_fail`: boolean indicating whether failing this test is acceptable; for SHOULDs. Defaults to `false`.

The `expected` data structure maps the types in Structured Headers to [JSON](https://tools.ietf.org/html/rfc8259) as follows:

* Dictionary: JSON object whose values are [member, parameters], where parameters is an object
* List: JSON array of [primary_item, parameteters], where parameters is an object
* List of Lists: JSON array of JSON arrays
* Integer: JSON numbers; e.g. 1
* Float: JSON numbers; e.g. 2.5
* String: JSON string; e.g., "foo"
* Token: JSON string; e.g., "bar"
* Binary Content: **base32**-encoded string; e.g., "ZXW6==="

`primary_item` and `member` can be either a simple item, or a list.

## Writing Tests

All tests should have a descriptive name. Tests should be as simple as possible - just what's
required to test a specific piece of behavior. If you want to test interacting behaviors, create
tests for each behavior as well as the interaction.

Please feel free to contribute!
