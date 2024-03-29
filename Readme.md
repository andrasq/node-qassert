qassert
=======

Nodejs `assert` with useful new features and fewer rough edges.

[![Build Status](https://app.travis-ci.com/andrasq/node-qassert.svg?branch=master)](https://app.travis-ci.com/github/andrasq/node-qassert)
[![Coverage Status](https://coveralls.io/repos/github/andrasq/node-qassert/badge.svg?branch=master)](https://coveralls.io/github/andrasq/node-qassert?branch=master)


Features
--------

`qassert` wrappers the built-in `assert` package to make it nicer to use.
It normalizes error messages and includes some new test methods.

- error diagnostics include both the inspected values and the user-supplied message
- `fail()` method to error out and not continue
- `within()` method to numeric range-test two values
- `inorder()` test that an array is sorted
- `contains()` test inclusion in strings, arrays, objects


`assert` methods
----------------

### qassert.ok( condition, [message] )

assert that the condition is truthy, else fail the test.

### qassert.equal( a, b, [message] )

coercive equality test, `a == b`.  Like in nodeunit and phpunit, this assertion
is also available by the alias `equals`.

### qassert.notEqual( a, b, [message] )

coercive inequality, `a != b`

### qassert.deepEqual( a, b, [message] )

recursive equality, objects and arrays have equal elements.  Element
comparisons are coercive `==`, thus `[1]` and `[true]` are deepEqual.

### qassert.notDeepEqual( a, b, [message] )

recursive inequality, objects and arrays differ.

### qassert.deepStrictEqual( a, b, [message] )

recursive strict equality, objects and arrays have srict equal elements.
Comparison is non-coercive `===`, thus `0` and `false` differ.

Under older versions of node that lack `assert.deepStrictEqual`, this
call uses the coercive `assert.deepEqual` instead.

### qassert.notDeepStrictEqual( a, b, [message] )

recursive inequality, objects and arrays are not strict equal.
Under older versions of node that lack `assert.notDeepStrictEqual`,
this call uses the coercive `notDeepEqual`.

Under older versions of node that lack `assert.deepStrictEqual`, this
call uses the coercive `assert.deepEqual` instead.

### qassert.strictEqual( a, b, [message] )

strict equality test, `a === b`

### qassert.notStrictEqual( a, b, [message] )

strict inequality test, `a !== b`

### qassert.throws( block, [error], [message] )

check that the code block throws the specified error.  Error can be an Error
object, a RegExp, or a validator function.

### qassert.doesNotThrow( block, [message] )

check that the code block does not throw an error.

### qassert.fail( )

fail the test.  This overrides `assert.fail`

### qassert.ifError( err )

fail the test if the error is set

`qassert` methods
----------------------

### qassert.assert( condition, [message] )

same as `qassert.ok`.

### qassert.equals( a, b, [message] )

same as `qassert.equal`.

### qassert.within( a, b, distance )

check that `a` is within `distance` units of `b`, ie that `abs(a - b) <= abs(distance)`.

### qassert.inorder( arr, [compar,] [message] )

check that the arguments in the array `arr` are in non-descending order, compared with ` > `.
If `compar` is specified, elements will be compaired pairwise with `compar(a, b)`.
`compar(a, b)` should return a value `> 0` if `a > b`.

### qassert.contains( a, b, [message] )

check that `a` contains `b`.

B is contained in a if object a has b as a property or has all the properties of
object b with the same values as in b, or if array a includes b, or if string a has
b as a substring.  Non-container types contain themselves, but the inclusion test
is coercive, so eg `true` contains `1`.

Specifically, when a is a
- `string` - check that b occurs as a substring of a.  If b is not already a string, it it coerced.
  If b is a RegExp, check that a matches b.
- `Buffer` - check that b occurs as a sequence of bytes in a.  If b is not a buffer, it is coerced.
  If b is a RegExp, check that a as a string matches b.
- `Array` - check that one of the elements of `a` is b, or if b is an object, contains b.
If b itself is an array, then check that all elements of `b` are contained in `a` (To test that the array b
occurs in a, check that a contains the array of [`b`].)
- `Object` - check that one of the fields of `a` is b.  If b is also an object,
check that all the key-value properties of b are contained in a.

### qassert.notContains( a, b, [message] )

like `contains` but with the sense reversed, the test fails if a contains b

### qassert.strictContains( a, b, [message] )

Under older versions of node that lack `assert.deepStrictEqual`, this
call does a coercive `qassert.deepEqual` instead.

### qassert.notStrictContains( a, b, [message] )

Under older versions of node that lack `assert.deepStrictEqual`, this
call does a coercive `qassert.deepEquls` instead.


Change Log
----------

- 1.6.8 - fix `includes` for objects that do not have a constructor (like node-v12 req._headers)
- 1.6.7 - make `throws` treat falsy errors like the running node version, inspect the expected vs actual values
- 1.6.6 - do not annotate non-AssertionErrors rethrown from `throws()`, which can be non-objects or null
- 1.6.5 - normalize assertion error messages
- 1.6.4 - fix AssertionError stack trace to omit qassert internal functions
- 1.6.3 - fix assert.fail deprecation notice, fix possible message annotation mis-positioning
- 1.6.2 - fix contains([1,2,3], {y:null}), work around Buffer deprecation warnings
- 1.6.1 - fix throws() to increment `assertionCount`, omit own sources from error stack trace,
  guard against assertion errors without a message
- 1.6.0 - support `contains()` test of string/Buffer and RegExp,
  fix `contains(array, non-hash-obj)` test to look for the object itself
- 1.5.0 - fix `assertionCount` incrementing for all tests, only increment it if set,
  change _wrap* to allow tests to be called as functions, fix tests to all have the
  same parent object, simplify within() implementation, fix occasional clipping of
  last char of error message, return the actual test failure error, fix error
  annotation, 100% test coverage
- 1.4.1 - fix unit tests under node that lack strictDeepEquals
- 1.4.0 - notContains, notStrictContains
- 1.3.0 - fix `strictContains` to be strict, new `inorder()`, 100% test coverage
- 1.2.0 - deepStrictEqual and notDeepStrictEqual, document fail
- 1.1.2 - fix `contains` refactor errors
- 1.1.1 - edits to readme and package.json
- 1.1.0 - `within`, `contains`
- 1.0.0 - mostly just `assert`
