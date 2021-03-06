# typeson

## 5.7.1

- Fix `.npmignore`

## 5.7.0

- Build: Add Yarn.lock
- npm: Update dev deps

## 5.6.0

- Build: Add minification for its own sake and also avoiding our
    Rollup process (Babel?) unminifying our source and creating inconsistency

## 5.5.0

- Build: Preminimize internally used constructors so basic functionality not
    fragile across multiple Typeson instances

## 5.4.0

- npm: Include typeson.js file with npm in case using as module
    and also include LICENSE (but ignore new test directory)
- npm: Remove build and prepublish scripts (runs on local installs in npm <= 4)
- Build: Switch from Webpack to Rollup
- npm/Testing: add ESLint script to test routines
- npm/Testing: Add browser test file and npm script
- Testing refactoring: Write tests in ES6
- Testing: Add messages for tests missing them
- gitignore: Ignore new auto-generated test file

## 5.3.0

- npm: Add `browser` to `package.json` for bundlers

## 5.2.0

- npm: Add `module` to `package.json` for downstream Rollup/Webpack

## 5.1.0

- For `encapsulateObserver`, report `typeDetected` as `true` when a
    Typeson-detected type is found but no relevant replacer is present
    to be performed.
- For `encapsulateObserver`, report `replacing` as `true` preceding a
    Typeson-detected type replacement. (Can be used to get at the original
    object value before encapsulation)
- npm/Testing: Update dev dependencies

## 5.0.3

- Add back prepublish script to ensure rebuilding upon publish

## 5.0.2

- Ensure build occurs on install

## 5.0.1

- Fix `rootTypeName` to work with promises and for replacements

## 5.0.0

- Breaking change (minor): Throw upon attempts to register objects with JSON type names
- Breaking change (minor): Include JSON type names on `type` property
    of object passed to `encapsulateObserver`
- Enhancement (Basic API): `specialTypeNames` instance method to return array
    of unique Typeson type names
- Enhancement (Basic API): `rootTypeName` instance method to return string of
    Typeson type name at root if present or the JSON type name otherwise
- Enhancement (Basic API): Add `Typeson.getJSONType` class method to return
    `null` and `array` where appropriate or `typeof` otherwise
- Enhancement (Basic API): Add `Typeson.JSON_TYPES` class property to list
    the available JSON types

## 4.0.0

- Breaking change: Use object-based structure on `types` (esp. important
    as more possible properties/methods are added, reliance on array
    ordering will become less manageable/intelligible for users).
- Breaking change: Changed default to optimize and avoid replacement
    checks on arrays unless there is at least one type whose spec has
    `testPlainObjects` set. Brings consistency given that non-array
    plain object types must also have the option set to be discoverable.

- Fix: Allow missing recursive array or object member possibilities
- Fix: Cross-frame/Cross-module plain-object
    (including `Object.create(null)`) and array detection
- Fix: Escape keypaths to avoid conflicts when property names
    include dots; fixes #5
- Fix: Nest inside `$` even if object has `$types` that is non-truthy
    or if it has `$types` but no actual types (with tests)
- Fix: Throw if user attempts to define a type with ID "#"
- Fix: Implementation of pojo registration. The sugar:
    `typeson.register({MyClass});` failed if parsing `undefined`
    or `null` since the tester did not check for that.

- Enhancement (Basic API): Avoid needing a replacer (so that it can add
    state config without modifying the object and e.g., set to iterate
    "in" properties; also can be used for validation only)
- Enhancement (Basic API): Allow `fallback` option to `register` to
    assign lower priority to a later registration addition
- Enhancement (Basic API): Support object-based API in addition to
    function/array (with properties, `test`, `replace`, `revive`);
    `this` context is supplied object
- Enhancement (Basic API): Allow per-method-invocation options on
    `encapsulate`/`stringify`/`revive`/`parse`
- Enhancement (Basic API): Support `encapsulateObserver` callback option
    (sending detected `type` when present), adding tests for sync and async

- Enhancement (Types): Allow `encapsulate` to represent primitives and `revive`
    to revive them as such (e.g., to encode a bare `undefined` in the
    same manner as `undefined` properties)
- Enhancement (Types): Allow revival to return instanceof `Typeson.Undefined`
    to indicate restoring to `undefined`
- Enhancement (Types): Add config `testPlainObjects` (which can
    also work with arrays) to allow (non-recursive) tests and replacements
    for plain objects (or arrays)

- Enhancement (async): `stringifyAsync`, `parseAsync`, `encapsulateAsync`,
    `reviveAsync` methods which return promises; have them use `replaceAsync`
    and `reviveAsync` on type specs where relevant
- Enhancement (async): Add `throwOnBadSyncType` option (defaults to `true`)
    which the `*Sync/Async` methods automatically apply
- Enhancement (async): Rather than relying on Promises proper (which could be
    desired as encapsulation results, at least as targets for throwing
    (e.g., Structured Cloning Throwing in `typeson-registry`)), require
    use of a `Typeson.Promise()` (which supports promise methods) to
    indicate a result is to be obtained asynchronously.

- Enhancement (utility): Expose `isObject`, `isPlainObject`,
    `isUserObject`, and `isThenable` as class methods of `Typeson`
- Enhancement (utility): Allow `Typeson.hasConstructorOf` to be tested against
    classes without constructors
- Enhancement (utility): Add `compareConstructors` method for detection of
    instances like `Intl.Collator` whose `toStringTag` is
    indistinguishable from other classes; document
- Enhancement (utility): Add `Typeson.toStringTag` class method given frequent
    need by types
- Enhancement (utility): Add `escapeKeyPathComponent`,
    `unescapeKeyPathComponent`, and `getByKeyPath` as class methods of
    `Typeson`

- Enhancement (key iteration): Allow setting of `iterateIn` state
    ('object' or 'array') by type methods to give purely modular control
    and ensure iteration on prototype only occurs when set by the type
    (e.g., so not iterating iterating all objects unless detected as such);
    `iterateIn` defaults to not being set.
- Enhancement (key iteration): Allow setting of `iterateUnsetNumeric`
    boolean state object property to give purely modular control
    and ensure iteration of unset (sparse) numeric array indexes only
    occurs when set by the type (e.g., so not iterating iterating all arrays
    unless detected as such); defaults to `false`

- Testing (fix): Prevent `ArrayBuffer` detection clashing with other types
- Testing bugs: Add test for current behavior of replacer
    (tester/encapsulator) ordering
- Testing bugs: Keypaths with property names including dots
- Testing bugs: Test for clashes with objects using own `$` and `$types`
    properties
- Testing bugs: Ensure preventing types with ID "#"
- Testing features: Add encapsulate observer tests
- Testing features: Add `toJSON` tests
- Testing features: Add `Typeson.Promise` `all`/`race` tests
- Testing features: Support promise-based tests

- Docs (README): Document new features
- Docs (README): Describe more of the resulting encapsulated structure
- Docs (README): Document recursive nature of replacements (and promises)
- Docs (README): Indicate sequence upon subsequent `register` calls
- Docs (README): Indicate precedence of testing/encapsulating when an
    array is supplied
- Docs (README): Indicate support for arrays and primitives at root by
    return of new object
- Docs (README): summarize properties and specify types consistently in headers
- Docs (README): Use backticks to set off code references
- Docs (README): Improve headings hierarchy, more descriptions and
    documentation of async/sync methods

- npm: Update dev dependencies

## 3.2.0

- Enhancement: Allow reviver to set the explicit value `undefined` (by
    returning an instance of `Typeson.Undefined`)
- Enhancement: Pass in state objects to tests and replacers so that
    they can vary their results depending on setting (currently only
    whether we are iterating an "own" key property or not)
- Refactoring: Remove redundant nullish-check
- Clean-up: Remove trailing WS, consistent semi-colon usage
- Docs: Consistent and best practices heading hierarchy in README
- Docs: Mention use of tester for throwing exceptions
- Docs: Add docs for `stateObj`
- Docs: Add docs for `undefined` behavior in encapsulators and revivers,
    the use of `Typeson.Undefined` to explicitly add `undefined`
