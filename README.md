# problem-specifications

Shared metadata for Exercism exercises.

## Contributing Guide

Please see the [contributing guide](https://github.com/exercism/problem-specifications/blob/master/CONTRIBUTING.md)

## Problem metadata

Each problem's data lives in a directory under `exercises/`

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

There are three metadata files:

* `description.md` - the basic problem description
* `metadata.yml` - additional information about the problem, such as where it came from
* `canonical-data.json` - standardized test inputs and outputs that can be used to implement the problem

## Test Data Format (canonical-data.json)

This data can be incorporated into test programs manually or extracted by a
program.  The file format is described in [canonical-schema.json](https://github.com/exercism/problem-specifications/blob/master/canonical-schema.json), but it
is easier to understand with an example:

```json
{ "exercise": "foobar"
, "version" : "1.2.0"
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
          [ " A test case must have a 'description' and a 'property'.  "
          , " Anything else is optional.                               "
          , "                                                          "
          , " The 'property' is a string in lowerCamelCase identifying "
          , " the type of test, but most of the times it is just the   "
          , " name of a function being tested.                         "
          , "                                                          "
          , " Test cases can have any number of additional keys, and   "
          , " most of them also have an 'expected' one, defining the   "
          , " value a test should return.                              "
          ]
      , "description": "Foo'ing a word returns it reversed"
      , "property"   : "foo"
      , "input"      : {
          "word"       : "lion"
        }
      , "expected"   : "noil"
      }
    , { "description": "Bar'ing a name returns its parts combined"
      , "property"   : "bar"
      , "input"      : {
          "firstName"  : "Alan",
          "lastName"   : "Smithee"
        }
      , "expected"   : "ASlmainthee"
      }
    , { "description": "Baz'ing a name returns its parts combined to a multi-line string"
      , "property"   : "baz"
      , "multi-line-string-array": [ "expected" ]
      , "input"      : {
          "firstName"  : "Alan",
          "lastName"   : "Smithee"
        }  
      , "expected"   : [ "Their name is: "
                       , "Alan"
                       , "Smithee."
                       ]
      }
    , { "comments":
          [ " Test cases can be arbitrarily grouped with a description "
          , " to make organization easier.                             "
          ]
      , "description": "Abnormal inputs: numbers"
      , "cases":
          [ { "description": "Foo'ing a number returns nothing"
            , "property"   : "foo"
            , "input"      : {
                "word"       : "42"
              }
            , "expected"   : null
            }
          , { "description": "Foo'ing a very big number returns nothing"
            , "optional"   : "big-ints"
            , "comments"   : [ "Making this test case pass requires using BigInts." ]
            , "property"   : "foo"
            , "input"      : {
                "word"       : "28948022309329048855892746252171976962977213799489202546401021394546514198529"
              }
            , "expected"   : null
            }
          , { "description": "Bar'ing a name with numbers gives an error"
            , "property"   : "bar"
            , "input"      : {
                "firstName"  : "HAL",
                "lastName"   : "9000"
              }
            , "expected"   : { "error": "You should never bar a number" }
            }
          ]
      }
  ]
}

```

Keep in mind that the description should not simply explain **what** each case
is (that is redundant information) but also **why** each case is there. For
example, what kinds of implementation mistakes might this case help us find?

There are also some conventions that must be followed:

  - All keys should follow the [lowerCamelCase](http://wiki.c2.com/?LowerCamelCase) convention.
  - If the input is valid but there is no result for the input, the value at `"expected"` should be `null`.
  - If an error is expected (because the input is invalid, or any other reason), the value at `"expected"` should be an object containing exactly one property, `"error"`, whose value is a string.
    - The string should explain why the error would occur.
    - A particular track's implementation of the exercise **need not** necessarily check that the error includes that exact string as the cause, depending on what is idiomatic in the language (it may not be idiomatic to check strings for error messages).
  - Test cases that only some tracks should implement, for example because it would unnecessarily increase the complexity of the exercise in some but not all languages, mark it with an `optional`-key. Multiple cases related to the same reason for optionality should have the same key. The decision that a test case is optional will often be made in the PR discussion, so don't worry about it too much while creating a PR.

The `canonical.json` file can be validated against its schema prior to commiting using https://www.jsonschemavalidator.net/ with...
```
{
	"$schema": "https://github.com/exercism/problem-specifications/blob/master/canonical-schema.json"
}
```


### Test Data Versioning

Test data should be versioned according to [Semantic Version 2.0](http://semver.org/), which is defined as follows:

> Given a version number MAJOR.MINOR.PATCH, increment the:
>
> MAJOR version when you make incompatible API changes,
> MINOR version when you add functionality in a backwards-compatible manner, and
> PATCH version when you make backwards-compatible bug fixes.
> Additional labels for pre-release and build metadata are available as extensions to the MAJOR.MINOR.PATCH format.

#### MAJOR version changes

The MAJOR version should be changed when the test suite is modified in a
fundamentally incompatible way.

There are examples of changes requiring a MAJOR version change:

- A new "property" (test type).
- Renaming a "property".
- Insertion, deletion or renaming of keys in the test data object.
- Changing the type of one of the test data keys.

MAJOR changes should be expected to break even well-behaved test generators.

#### MINOR version changes

The MINOR version should change when you add functionality in a backwards-compatible manner, make
non-breaking changes that alter the meaning of the test suite, make previously
passing solutions possibly fail, or failing solutions fail at a different spot.

There are examples of changes requiring a MINOR version change:

- Adding or deleting test cases.
- Changing the test cases inputs and/or outputs.
- Changing the test cases ordering.

MINOR changes would never break well-designed test generators, because the test-generation logic remains exactly the same.

#### PATCH version changes

The PATCH version should change when you make backwards-compatible bug fixes or
 whenever the meaning of the tests does not change.

There are examples of changes requiring a PATCH version change:

- Regrouping/"Renesting" test cases without changing test case ordering.
- Changing descriptions or comments.
- Changing keys' ordering or formatting (would result in an equivalent JSON file).

PATCH changes would never break well-designed test generators, because the test data remains exactly the same.

#### Automatic verification of versioning changes

Travis CI will automatically verify that all changed JSON files have their version changed, via [bin/check_versions](https://github.com/exercism/problem-specifications/blob/master/bin/check_versions).

This helps avoid the mistake of forgetting to change the version when a change is made to the tests.

Although this is desired most of the time, there are instances (e.g. formatting change) where no version change is desired.
In such instances, see [bin/check_versions](https://github.com/exercism/problem-specifications/blob/master/bin/check_versions) for the override string.
Place the string inside a commit message to skip the version check.
Note that as the override applies to the entire PR, it's highly advised that changes that do not require a version change should not be in the same PR as changes that do require a version change.

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
