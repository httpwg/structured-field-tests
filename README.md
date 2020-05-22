# Structured Field Tests

These are test cases for implementations of [Structured Fields for HTTP](http://httpwg.org/http-extensions/draft-ietf-httpbis-header-structure.html). They currently test draft 17 of the specification.

## Test Format

Each test file is a JSON document that contains an array of test records. A test record is an
object with the following members:

- `name`: A string describing the test
- `raw`: An array of strings, each representing a header field value received
- `header_type`: One of "item", "list", "dictionary"
- `expected`: The expected data structure after parsing (if successful). Required, unless `must_fail` is `true`.
- `must_fail`: boolean indicating whether the test is required to fail. Defaults to `false`.
- `can_fail`: boolean indicating whether failing this test is acceptable; for SHOULDs. Defaults to `false`.
- `canonical`: An array of strings representing the canonical form of the header field value, if it is different from `raw`. Not applicable if `must_fail` is `true`.

The `expected` data structure maps the types in Structured Headers to [JSON](https://tools.ietf.org/html/rfc8259) as follows:

* Dictionary: JSON object which maps [_member-name_ (Key), _member-value_ (Item or Inner-List)] pairs
* List: JSON array, where each element is either an Item or Inner-List
* Inner-List: JSON array with two elements, the list (a JSON array of Items) and Parameters
* Item: JSON array with two elements, the Bare-Item and Parameters
* Bare-Item: one of:
   * Integer: JSON numbers; e.g. 1
   * Float: JSON numbers; e.g. 2.5
   * String: JSON string; e.g., "foo"
   * Token: `token` __type Object (see below)
   * Binary Content: `binary` __type Object (see below)
* Parameters: a JSON object which maps [_param-name_ (Key), _param-value_ (Bare-Item)] pairs

For any test that case that has a valid outcome (i.e. `must_fail` is not `true`) the `expected`
data structure can be serialised.  The expected result of this serialisation is the `canonical`
member if specified, or `raw` otherwise.  The canonical form of a List or Dictionary with no
members is an empty array, to represent the field being omitted.

Test cases in the `serialisation-tests` directory can be used to test serialisation of an invalid
or non-canonical value.  The `expected` structure (as defined above) should serialise to the
`canonical` form, unless `must_fail` is `true` -- in which case the value cannot be serialised.
These cases do not have a `raw` element.

### __type Objects

Because JSON doesn't natively accommodate some data types that Structured Headers does, the `expected` member uses an object with a `__type` member and a `value` member to represent these values. 

For example:

~~~
{
  "__type": "token",
  "value": "foo"
}
~~~

... carries a "foo" token. The following types are defined:

* `token`: carries a Token as a JSON string; e.g., "bar"
* `binary`: carries Binary Content as a **base32**-encoded JSON string; e.g., "ZXW6==="


## Writing Tests

All tests should have a descriptive name. Tests should be as simple as possible - just what's
required to test a specific piece of behavior. If you want to test interacting behaviors, create
tests for each behavior as well as the interaction.

Please feel free to contribute!
