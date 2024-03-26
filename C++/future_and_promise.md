# Future and Promise

## About
The `std::future` and `std::promise` were introduced in C++11's concurrency API as the two ends of a read-write channel. The `std::promise` represents the producer/write-end and `std::future` represents the consumer/read-end. Most common use case of these is when a thread wants to read a value computed by another thread. In other words, a thread wants to communicate a value to another thread.
