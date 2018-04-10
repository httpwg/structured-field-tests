# Structured Header Tests

These are test cases for implementations of [Structured headers for HTTP](http://httpwg.org/http-extensions/draft-ietf-httpbis-header-structure.html).

## Test Format

Each test file is a JSON document that contains an array of test records. A test record is an
object with the following members:

- name: A string describing the test
- raw: An array of strings, each representing a header field value received
- header_type: One of "item", "list", "dictionary", "param-list"
- success: A boolean describing whether parsing should succeed; default is true
- expected: The expected data structure after parsing (if successful); default is null

The `expected` data structure maps the types in Structured Headers to [JSON](https://tools.ietf.org/html/rfc8259) as follows:

* Dictionary: JSON object
* List: JSON array
* Parameterised List: JSON array of JSON arrays whose first member is a string and second member is an object
* Integer: JSON numbers; e.g. 1
* Float: JSON numbers; e.g. 2.5
* String: JSON string; e.g., "foo"
* Identifier: JSON string; e.g., "bar"
* Binary Content: **base32**-encoded string; e.g., "ZXW6==="



## Writing Tests

All tests should have a descriptive comment. Tests should be as simple as possible - just what's
required to test a specific piece of behavior. If you want to test interacting behaviors, create
tests for each behavior as well as the interaction.

If an 'error' member is specified, the error text should describe the error the implementation
should raise - *not* what's being tested. Implementation error strings will vary, but the suggested
error should be easily matched to the implementation error string. Try to avoid creating error
tests that might pass because an incorrect error was reported.

Please feel free to contribute!
