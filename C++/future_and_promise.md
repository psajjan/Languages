# Future and Promise

## About
The `std::future` and `std::promise` were introduced in C++11's concurrency API as the two ends of a read-write channel.
The `std::promise` represents the producer/write-end and `std::future` represents the consumer/read-end.
Most common use case of these is when a thread wants to read a value computed by another thread. In other words, a thread wants to communicate a value to another thread.

### `std::future` with `std::promise`
A `std::future` is always associated with a `std::promise`. A future is used to wait for some value, while the promise is used to supply that precise value.

First thing to do is to create a promise and get the corresponding future object from it using the `get_future()` member function.
```C++
// This will be the write end.
std::promise<int> promise;

// This will be the read end.
auto future = promise.get_future();
```

Then create a thread which will be computating a value and use the promise to set that value, so that in the main thread using future, this value can be accessed.
```C++
std::thread T1([&promise]() {
  int value = 0;
  // do some computation
  promise.set_value(value);
  // continues to do other processing
});

// In main thread, wait and read that value.
int value = future.get();

// Join the thread
T1.join();
```

As you can see, you use `std::promise::set_value()` from the callee-thread to set the value to be communicated. `std::future` object have a method `get()` which is used to return this value.
Actually, `get()` calls `std::future::wait()` until the return value is available, so you can also use `wait()` if you don't want to explicitly retrieve the value as soon as it is ready.

### `std::future` with `std::async`
C++ `std::async` is a function template that takes functions or function objects (basically called callbacks) as input and run them asynchronously.
When you call `std::async`, it returns a `std::future` which is used to keep the result of the above callback.
In order to exact the value from the future, its member `get()` needs to be called.

#### Launch Policies
In C++, async functions can we used in 2 ways, with or without specifying the policies in the function arguments.
When specifying the launch policy, the first argument is the policy itself which defines the asynchronous behavior of the function.

| S.NO | Policy | Behavior !
| 1 | launch::async ||
| 2 | launch::deferred ||
| 3 | launch::async launch::deferred ||











