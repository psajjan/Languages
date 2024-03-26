# Future and Promise

## About
The `std::future` and `std::promise` were introduced in C++11's concurrency API as the two ends of a read-write channel.
The `std::promise` represents the producer/write-end and `std::future` represents the consumer/read-end.
Most common use case of these is when a thread wants to read a value computed by another thread. In other words, a thread wants to communicate a value to another thread.

There are three ways in which you can get a future.
1. From promise
2. From async
3. From packaged task

## `std::future` from `std::promise`
A `std::future` is always associated with a `std::promise`. A future is used to wait for some value, while the promise is used to supply that precise value.

First thing to do is to create a promise and get the corresponding future object from it using the `get_future()` member function.
```C++
#include <future>

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

## `std::future` from `std::async`
C++ `std::async` is a function template that takes functions or function objects (basically called callbacks) as input and run them asynchronously.
When you call `std::async`, it returns a `std::future` which is used to keep the result of the above callback.
In order to exact the value from the future, its member `get()` needs to be called.

### Launch Policies
In C++, async functions can we used in 2 ways, with or without specifying the policies in the function arguments.
When specifying the launch policy, the first argument is the policy itself which defines the asynchronous behavior of the function.

| S.NO | Policy | Behavior |  
|------|--------|----------|
| 1 | launch::async | This launch policy assures the async behavior of the function, which means that the callable function will be executed in a new thread immediately |  
| 2 | launch::deferred | In this launch policy, a callable function is not executed in the new thread; instead, it follows the non-async behavior. It follows the lazy evaluation policy in which the call to the function is deferred (postponed) till the previous thread calls the get on the future object, which makes the shared state again accessible |  
| 3 | launch::async\|launch::deferred | This is an automatic launch policy. In this policy, the behavior is not defined. The system can choose either asynchronous or deferred depending on the implementation according to the optimized availability of the system. Programmers have no control over it on anything. |  

### Without specifying policy

In the below syntax, the launch policy is not specified in the function arguments. Launch policy is automatically selected, which is launch:: async | launch:: deferred.

```C++
int f(int x)
{
    return x + 1;
}

// Note that a peculiarity about std::async is
// that this call will not necessarily result in
// f being run concurrently; it is based on the
// scheduler's decision (to avoid oversubscription)
auto result = std::async(f, 5);

std::cout << result.get() << std::endl;
```

### With specifying policy

```C++
// Example of checking the number is even or not using async
#include <iostream>       // library used for std::cout
#include <future>         // library used for std::async and std::futur

// Function to checking if the number is even or not.
bool check_even (int num)
{
    std::cout << "Hello I am inside the function!! \n";
    
    if (num % 2 == 0) {
        return true;
    }
    return false;
}

int main ()
{
    // Calling the above function check_even() asynchronously and storing the result in future object.
    std::future<bool> future = std::async(launch::deferred, check_even, 10);

    // retrieving the exact value from future object and waiting for check_even to return
    bool result = future.get();

    std::cout << result.get() << std::endl;

    return 0;
}
```

## Waiting for Futures
One thing to mention is that there exist two other ways of waiting for a future other than the plain `wait()`. These two are:

1. `std::future::wait_for`, which waits for a given (`std::chrono`) duration.
2. `std::future::wait_until`, which waits until a given (`std::chrono`) time-point.

Note that for (1) `std::chrono` literals are especially nice (future.wait_for(100ns)).

Both of these methods return one of three codes upon finishing:

1. `std::future_status::ready`: The future is ready.
2. `std::future_status::timeout`: The future was not yet ready after waiting.
3. `std::future_status::deferred`: The future is actually deferred and was not yet started.


## Complete Example

```C++
#include <future>
#include <iostream>
#include <thread>
 
int main()
{
    // future from a packaged_task
    std::packaged_task<int()> task([]{ return 7; }); // wrap the function
    std::future<int> f1 = task.get_future(); // get a future
    std::thread t(std::move(task)); // launch on a thread
 
    // future from an async()
    std::future<int> f2 = std::async(std::launch::async, []{ return 8; });
 
    // future from a promise
    std::promise<int> p;
    std::future<int> f3 = p.get_future();
    std::thread([&p]{ p.set_value_at_thread_exit(9); }).detach();
 
    std::cout << "Waiting..." << std::flush;
    f1.wait();
    f2.wait();
    f3.wait();
    std::cout << "Done!\nResults are: "
              << f1.get() << ' ' << f2.get() << ' ' << f3.get() << '\n';
    t.join();
}
```







