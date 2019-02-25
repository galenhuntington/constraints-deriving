constraints-deriving
==================================
[![Build Status](https://secure.travis-ci.org/achirkin/constraints-deriving.svg)](http://travis-ci.org/achirkin/constraints-deriving)

This project is based on the [constraints](http://hackage.haskell.org/package/constraints) library.
Module `Data.Constraint.Deriving` provides a GHC Core compiler plugin that generates class instances.

The main goal of this project is to make possible a sort of ad-hoc polymorphism that I wanted to
implement in [easytensor](http://hackage.haskell.org/package/easytensor) for performance reasons:
an umbrella type unifies multiple specialized type family backend instances;
if the type instance is known, GHC picks a specialized (overlapping) class instance for a required function;
otherwise, GHC resorts to a unified (overlappable) instance that is defined for the whole type family.



### DeriveAll

DeriveAll plugin pass inspects a newtype declaration.
It looks through all possible type instances (in the presence of type families) of the base type,
and copies all class instances for the newtype wrapper.

...

### ToInstance

ToInstance plugin pass converts a top-level `Ctx => Dict (Class t1..tn)` value declaration into
an instance of the form `instance Ctx => Class t1..tn`.
Thus, one can write arbitrary Haskell code (returning a class dictionary) to be executed every time
an instance is looked up by the GHC machinery.

...

## Is it production ready?

If the module that uses the plugin compiles, and the required instances are there, all is good!
At this moment, the plugin pass itself may fail while inspecting some complicated types.
But the whole thing is executed at compile time, so there is no danger of failure at runtime.

...

## Further work

  * [x] Cover more compiler versions: GHC 8+ (tested on 8.0.2 - 8.6.3)
  * [x] Add travis
  * [ ] Add more tests (in progress)
  * [x] Refactor `Data.Constraint.Deriving` into submodules.
  * [ ] Finish the readme, show examples
  * [ ] Put the library on hackage
  * [x] Create new vars in ToInstance instead of replacing existing
  * [x] Keep generated files from Spec folder during tests
  * [x] Remove generated .hi and .o files from test/Spec
  * [ ] Add an option to blacklist some classes for DeriveAll
