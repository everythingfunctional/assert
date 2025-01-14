Assert
======

A simple assertion utility taking advantage of the Fortran 2018 standard's introduction of variable stop codes
and error termination inside pure procedures.

Motivations
-----------
1. To mitigate against a reason developers often cite for not writing `pure` procedures: their inability to produce output in normal execution.
2. To promote the enforcement of programming contracts.

Overview
--------
This assertion utility contains three public entities:

1. An `assert` subroutine,
2. A `characterizable_t` abstract type supporting `assert`, and
3. An `intrinsic_array_t` non-abstract type extending `characterizable_t`.

The `assert` subroutine

* Error-terminates with a variable stop code when a user-defined logical assertion fails,
* Includes user-supplied diagnostic data in the output if provided by the calling procedure,
* Is callable inside `pure` procedures, and
* Can be eliminated during an optimizing compiler's dead-code removal phase based on a preprocessor macro: `-DUSE_ASSERTIONS=.false.`.

The `characterizable_t` type defines an `as_character()` deferred binding that produces `character` strings for use as diagnostic output from a user-defined derived type that extends  `characterizable_t` and implements the deferred binding.

The `intrinsic_array_t` type that extends `characterizable_t` provides a convenient mechanism for producing diagnostic output from arrays of intrinsic type `complex`, `integer`, `logical`, or `real`.

Use Cases
---------
1. [Enforcing programming contracts] throughout a project via runtime checks.
2. Produce output in `pure` procedures for debugging purposes.

### Enforcing programming contracts
Programming can be thought of as requirements for correct execution of a procedure and assurances for the result of correct execution.
The requirements and assurances might be constraints of three kinds:

1. **Preconditions (requirements):** `logical` expressions that must evaluate to `.true.` when a procedure starts execution,
2. **Postconditions (assurances):** expressions that must evaluate to `.true.` when a procedure finishes execution, and
3. **Invariants:** universal pre- and postconditions that must always be true when all procedures in a class start or finish executing.

The [examples/README.md] file shows examples of writing constraints in notes on class diagrams using the formal syntax of the Object Constraint Language ([OCL]).

Downloading, Building, and Running Examples
-------------------------------------------

### Prerequisites
1. A Fortran 2018 compiler.
2. The [Fortran Package Manager].
3. _Optional_: [OpenCoarrays] for parallel execution with the GNU Fortran compiler.

Recent versions of the Cray, Intel, GNU, and NAG compilers suffice.  Assert was developed primarily with `gfortran` 11.1.0 and `nagfor` 7.0 Build 7044.

### Downloading, building, and testing

#### Downloading Assert
```
git clone git@github.com:sourceryinstitute/assert
cd assert
```

#### Building and testing: single-image (serial) execution
The following command builds Assert and runs the full test suite in a single image:
```
fpm test
```
where `fpm test` builds the Assert library and runs the test suite, including the tests.
To build with the Numerical Algorithms Group (NAG) Fortran compiler, use
```
fpm test --compiler nagfor --flag -fpp
```

#### Building and testing: multi-image (parallel) execution
With `gfortran` and OpenCoarrays installed,
```
fpm test --compiler caf --runner "cafrun -n 2" designed-to-terminate-normally
fpm test --compiler caf --runner "cafrun -n 2" designed-to-error-terminate
```

For documentation on using Assert with other compilers, please submit an issue or pull request.  

### Running the examples
See the [./example](./example) subdirectory.

Documentation
-------------
For further documentation, please see [example/README.md] and the [tests].  Also, the code in [src] has comments formatted for generating HTML documentation using [FORD].

[Hyperlinks]:#
[OpenCoarrays]: https://github.com/sourceryinstitute/opencoarrays
[Enforcing programming contracts]: #enforcing-programming-contracts
[Single-image execution]: #single-image-execution
[example/README.md]: ./example/README.md
[tests]: ./tests
[src]: ./src
[FORD]: https://github.com/Fortran-FOSS-Programmers/ford
[Fortran Package Manager]: https://github.com/fortran-lang/fpm
[OCL]: https://en.wikipedia.org/wiki/Object_Constraint_Language
