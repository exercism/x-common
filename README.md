# problem-specifications

Shared metadata for Exercism exercises.

## Contributing Guide

Please see the [contributing guide](./CONTRIBUTING.md).

## Exercise Metadata

Each exercise's data lives in a directory under `exercises/` .

```text
exercises/
├── accumulate
│   ├── description.md
│   └── metadata.yml
├── ...
├── minesweeper
│   ├── canonical-data.json
│   ├── description.md
│   └── metadata.yml
├── ...
└── zipper
    ├── description.md
    └── metadata.yml
```

There are three metadata files per exercise:

- `description.md` - the basic problem description
- `metadata.yml` - additional information about the exercise, such as where it came from
- `canonical-data.json` (optional) - standardized test inputs and outputs that can be used to implement the exercise

## Test Data (canonical-data.json)

Test data can be incorporated into a track's test suites manually or extracted by a program (a.k.a. a _test generator_).

- Exercises _must_ contain tests that cover the public interface of the exercise (also thought of as "application tests").
- Exercises _may_ contain tests that cover the private or lower-level interface of the exercise (sometimes refered to as "library tests").

- Test cases are immutable, which means that once a test case has been added, it never changes. There are two exceptions:
  - The `comments` field _can_ be mutated and thus does not require adding a new test case when changing its value.
  - The `scenarios` field _can_ be mutated additively, by adding new scenarios. Existing scenarios must not be changed or removed. Adding new scenarios thus does not require adding a new test case.
- Test cases _must_ all be considered optional, insomuch that a track should determine per test case whether to implement it or not.
- Each test case has a [UUID (v4)](https://en.wikipedia.org/wiki/Universally_unique_identifier) to uniquely identify it.
- If tracks automatically generate test suites from test data, they _must_ do that based on an explicit list of test cases to include/exclude. Test cases must be identified by their UUID, and we'll provide tooling to help keep track of which test cases to include/exclude.

## Test Data Format

The file format is described in [canonical-schema.json](./canonical-schema.json), but it is easier to understand with an example:

```json
{ "exercise": "foobar"
, "comments":
    [ " Comments are always optional and can be used almost anywhere.      "
    , "                                                                    "
    , " They usually document how the exercise's readme ('description.md') "
    , " is generally interpreted in test programs across different         "
    , " languages.                                                         "
    , "                                                                    "
    , " In addition to a mainstream implementation path, this information  "
    , " can also document significant variations.                          "
    ]
, "cases":
    [ { "comments":
          [ " A test case must have 'uuid', 'description', 'property', "
          , " 'input' and 'expected' properties. The rest is optional. "
          , "                                                          "
          , " The 'property' is a string in lowerCamelCase identifying "
          , " the type of test, but most of the times it is just the   "
          , " name of a function being tested.                         "
          , "                                                          "
          , " Test cases can have any number of additional keys, and   "
          , " most of them also have an 'expected' one, defining the   "
          , " value a test should return.                              "
          ]
      , "uuid"       : "31e9db74-86b9-4b14-a320-9ea910337289"
      , "description": "Foo'ing a word returns it reversed"
      , "property"   : "foo"
      , "input"      : {
          "word"       : "lion"
        }
      , "expected"   : "noil"
      }
    , { "uuid"       : "09113ce5-b008-45d0-98af-c0378b64966b"
      , "description": "Bar'ing a name returns its parts combined"
      , "property"   : "bar"
      , "input"      : {
          "firstName"  : "Alan",
          "lastName"   : "Smithee"
        }
      ]
    }
  ]
}
```

## Scenarios

- The `scenarios` field can use one or more of a predefined set of values, which are defined in a [`SCENARIOS.txt`](./SCENARIOS.txt) file.
- The `scenarios` field can be mutated additively, by adding new scenarios. Existing scenarios must not be changed or removed. Adding new scenarios does therefore does not mean adding a new test case.
- Library tests will have a `library-test` scenario added to allow for easy including/excluding of library tests. Application tests won't have their own scenario, as they must be included and should not be filtered on.

## Conventions

There are also some conventions that must be followed:

- Descriptions should not simply explain **what** each case is (that is redundant information) but also **why** each case is there. For example, what kinds of implementation mistakes might this case help us find?
- All keys should follow the [lowerCamelCase](http://wiki.c2.com/?LowerCamelCase) convention.
- If the input is valid but there is no result for the input, the value at `"expected"` should be `null`.
- If an error is expected (because the input is invalid, or any other reason), the value at `"expected"` should be an object containing exactly one property, `"error"`, whose value is a string.
  - The string should explain why the error would occur.
  - A particular track's implementation of the exercise **need not** necessarily check that the error includes that exact string as the cause, depending on what is idiomatic in the language (it may not be idiomatic to check strings for error messages).

## Validation

`canonical.json` files can be validated against its schema using https://www.jsonschemavalidator.net/ with...

```
{
	"$schema": "https://github.com/exercism/problem-specifications/blob/master/canonical-schema.json"
}
```

## New Exercises Require a Glyph

When creating a new exercise the design team needs to be informed so that a new glyph can be created.

- An issue should be opened in [exercism/website-icons](https://github.com/exercism/website-icons/issues) after a PR has been opened in problem-specifications.
- This issue should reference the PR in problem-specifications.

## Automated Tests

`canonical-data.json` for each exercise is checked for compliance against the [canonical-schema.json](canonical-schema.json).
In order to run these tests, you will need to have `yarn` installed on your system.
Install them from [here](https://yarnpkg.com/en/docs/install).

Install the required packages:

```shell
yarn install
```

Run for all exercises:

```shell
yarn test
```

Run for single exercise:

```shell
yarn run test-one exercises/<exercise>/canonical-data.json
```

Replace `<exercise>` by the name of exercise which you want to check.
