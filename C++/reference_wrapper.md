# Reference Wrapper

## Problem with References

C++ references are favored over pointers wherever possible because they cannot be null, and they have better readability because of no dereferencing (*, ->) jumble.
However, a reference is a stubborn alias of an object that presents some challenges.

1. A reference must be initialized during its declaration/construction.
2. A reference cannot rebind, i.e. you cannot make it refer to another object.
3. Have a reference as a non-static member inside a class makes the class non-assignable. (default copy-assignment operator is deleted).
4. A reference to reference, storing references in any container and pointer to a reference are forbidden in C++.

## What is `std::reference_wrapper`

A reference wrapper `std::reference_wrapper<T>` is a copy-constructible and copy-assignable wrapper for an object of reference type `T&`.
It is defined in the header `<functional>`. It gives the non-nullable guarantee of a reference and the pointer-like flexibility to rebind to another object.
So you get an object that behaves like a reference but can be copied.

The usual way to create an `std::reference_wrapper<T>` is via `std::ref` (or `std::cref` for `std::reference_wrapper<const T>`).


