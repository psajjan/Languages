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

    // Solution-1: Explicitly specify the template parameter type
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

// Create reference wrapper
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

The only downside is that to access the members of the referred object of type `T`, we have to use the `std::reference_wrapper<T>::get()` method. 
And to assign to the referred object also we have to use `get()`.

```C++
std::string str{"Hello"};

// Create a reference wrapper
auto sref = std::ref(str);

// Use get() to access members
std::cout << sref.get().length() << "\n"; //5

// Use get() to re-assign
sref.get() = "World"; // str is changed to "World"

std::cout << str << "\n"; // World
```

## Common Use Cases

### With `std::make_pair` and `std::make_tuple`
We can pass reference wrapper to function templates to pass arguments by references without explicitly specifying the template parameter types (as we have seen in the problems section above).
This makes creating a pair or a tuple with references easy.

```C++
int x = 10;
int y = 20;
int z = 30;

// Explicitly need to specify the template parameter types as reference type
std::pair<int&, int> p1{x, y};
std::tuple<int&, int, int&> t1{x, y, z};

// Simply create reference wrapper and pass.
auto p2 = std::make_pair(std::ref(x), y);
auto t2 = std::make_tuple(std::ref(x), y, std::ref(z));
```

The `make_pair` and `make_tuple` are somewhat irrelevant since the C++17's class template argument deduction (CTAD). But the reasons still apply, now, with the constructors of pair and tuple.
However, `make_pair` and `make_tuple` decay a `reference_wrapper<T>` to a reference `T&`, whereas, that is not the case with CTAD construction of pair and tuple.

```C++
std::string m{"Hello"};

std::pair p(std::ref(m), n); 
std::tuple t(std::ref(m), n, std::ref(s));

std::cout << p.get().length() << std::endl;
```

Now that we know that a reference wrapper cannot be created with a temporary, following will result in compile error.

```C++
std::string getStr() { return "Hello"; }

auto p = std::make_pair(std::ref(getStr()), 10);    // Error -> good

std::pair<const str::string&, int> p2{getStr(), 10}; // No Error -> bad, can have undefined behavior
```

### Container of references

Unlike a reference, a `std::reference_wrapper` is an object and thus satisfies the STL container element requirements (Erasable, to be precise). Therefore, `std::reference_wrapper` can be used as a vector element type.

A `std::reference_wrapper<T>` could be a safe alternative to a pointer type (`T*`) for storing in a vector:

```C++
int a = 10;
std::vector<reference_wrapper<int>> v;
v.push_back(std::ref(a));
```

### `std::thread`: Passing arguments by reference
Lets say, we have a function named `startFunction` which takes some arguments. To run this function in a thread, we have to pass the function arguments when creating the object of std::thread in the constructor as `std::thread(startFunction, args)`. Those arguments are passed by value from the thread creator because the `std::thread` constructor copies or moves the creator's arguments before passing them to the start-function.

Any reference parameter of startFunction cannot bind to a creator's argument. It can only bind to a temporary (only if function accepts `const T&`) created by `std::thread`.

```C++
void start(int& i) { i += 1; }
void start_const(const int& i) { }

void createThread() {
    int x = 10;
    // x is copied below to a temporary
    std::thread(start, x).join();        // Error! can't bind temporary to int&.
    std::thread(start_const, x).join();  // OK. But sort of by-value 
}
```

If we want to pass an argument to a startFunction by reference, we can do that by passing a referrence wrapper through `std::ref`, as follows:
The `std::ref` generates a `reference_wrapper<int>` that eventually implicitly converts to an `int&`, thus binding the `start(int&)`'s reference parameter to the argument passed by the `createThread()`.

```C++
void createThread() {
    int x = 10;
    std::thread(start, std::ref(x)).join(); // OK. By-ref, x is 11 now

    std::thread(start_const, std::ref(x)).join();  // By-ref
    std::thread(start_const, std::cref(x)).join(); // By-ref 
}
```

### Reference data member in a class

Having a reference class member poses problems, as it makes the class non-assignable and practically immovable. The usual practice is to avoid references as class members and use pointers instead.

A `reference_wrapper` offers the best of both worlds.

```C++
struct A {
    A(int& i):iRef(i) {}
    std::reference_wrapper<int> iRef;
};

A a1(u); 
A a2(v);
a1 = w2; // OK
```

### Passing a function object by reference

An `std::reference_wrapper<T>` can be invoked like a function as long as the `T` is a callable. This feature is particularly useful with STL algorithms if we want to avoid copying a large or stateful function object. 
Besides, T can be any callable – a regular function, a lambda, or a function object.

```C++
struct MyFilter
{
    bool operator()(int i) const
    {
        // Filter and process
        return true;
    }
    // big data
};

void process()
{
    std::vector<int> in;
    std::vector<int> out;
    MyFilter filter;

    // in and filter are populated/processed....

    // Pass Large by-ref to avoid copy
    std::copy_if(in1.begin(), in1.end(), std::back_inserter(out), std::ref(filter)); 
}

```

### With bind expressions

### 




























