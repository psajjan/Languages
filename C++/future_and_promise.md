# Future and Promise

## About
The `std::future` and `std::promise` were introduced in C++11's concurrency API as the two ends of a read-write channel.
The `std::promise` represents the producer/write-end and `std::future` represents the consumer/read-end.
Most common use case of these is when a thread wants to read a value computed by another thread. In other words, a thread wants to communicate a value to another thread.

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
