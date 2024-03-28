# Reference Wrapper

## Problem with References

C++ references are favored over pointers wherever possible because they cannot be null, and they have better readability because of no dereferencing (*, ->) jumble.
However, a reference is a stubborn alias of an object that presents some challenges.

1. A reference must be initialized during its declaration/construction.
2. A reference cannot rebind, i.e. you cannot make it refer to another object.
3. Have a reference as a non-static member inside a class makes the class non-assignable (default copy-assignment operator is deleted). Move semantics doesn't make sense with a reference member.
4. A reference to reference, storing references in any container and pointer to a reference are forbidden in C++.

```C++
// PROBLEM 1
// Empty reference is not possible
int &ref;

// PROBLEM 2
int x =10, y = 20;
int &xref = x, &yref = y;
xref = yref; // xref still refers x, which is 20 now.

// PROBLEM 3
// Cannot store references in containers
std::vector<int&> v; // Error: as we cannot have empty references, assign/rebind references.


// PROBLEM 4
// T&& is not reference to reference, it is rvalue reference
int a = 10;
int& aref = a;
int&& arefref = aref; // not reference to reference, unlike pointer to pointer int**
```

Lets look at an example where you want pass a reference to a function template but the type deduced in not a reference.
Solution is that the function template parameter type has to be explicitly specified to pass by reference.

```C++
templace <typename T>
void increment(T n)
{
    n += 1;
}

int main()
{
    // x is of type int so the template type is deduced to int and x is passed by value
    int x = 10;
    increment(x); 

    // eventhough the type of xref is a reference int&, mere appearance of it is just treated as
    // the original non-reference type that it is referring to.
    int& xref = x;
    increment(xref);

    // Solution-1: Explicitly specify the type
    increment<int&>(x);

    // Solution-2: Use std:ref
    increment(std::ref(x));

    return 0;
}
```

## What is `std::reference_wrapper`

A reference wrapper `std::reference_wrapper<T>` is a copy-constructible and copy-assignable wrapper for an object of reference type `T&`.
It is defined in the header `<functional>`. It gives the non-nullable guarantee of a reference and the pointer-like flexibility to rebind to another object.
So you get an object that behaves like a reference but can be copied.

It imitates a reference. Contrary to its name, it does not wrap a reference. It works by encapsulating a pointer (T*) and by implicitly converting to a reference (T&).
It cannot be default constructed or initialized with a temporary; therefore, it cannot be null or invalid.

```C++
std::reference_wrapper<int> nr; //Error! must initialize
auto ref = std::ref(std::string("Hello")); // Error! no temporary (rvalue) allowed.
```

The usual way to create an `std::reference_wrapper<T>` is via `std::ref` (or `std::cref` for `std::reference_wrapper<const T>`).

```C++
std::string str1{"Hello"};
std::string str2{"World"};

std::reference_wrapper<std::string> r1 = std::ref(str1);
auto r2 = std::ref(str2);

// Assignment rebinds the reference_wrapper
// r2 also refers to str1 now 
r2 = r1;

// Implicit conversion to std::string&
std::string cstr = r2; // cstr is "Hello"

// Possible to create an array/vector of reference_wrapper
std::reference_wrapper<std::string> arr[] = {str1, str2};
std::vector<std::reference_wrapper<std::string>> vec = {r1, r2};
```
































