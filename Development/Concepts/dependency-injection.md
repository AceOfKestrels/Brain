# Dependency Injection
Dependency Injection is a programming technique and design pattern in which an object or function receives other objects or functions that it requires, as opposed to creating them internally. It aims to separate the concerns of constructing objects and using them, leading to loosely coupled applications. 

The Pattern ensures that an object or function which wants to use a given service should not have to know how to construct those services. Instead, the receiving "client" is provided with its dependencies by external code (an "injector"), which it is not aware of. Dependency Injections makes implicit dependencies explicit.

## Interfaces
Often it is beneficial for clients to have *interfaces* as dependencies, rather than concrete implementations of a given service. This makes [testing](./Testing/testing.md) (particularly [Unit Testing](./Testing/unit-testing.md)) significantly easier since dependencies can be *mocked* and their intended behavior simulated.

## Types of DI

### Constructor Injection
The most common form of Dependency Injection, supported by most injection frameworks, the client requests its dependencies through its constructor. Dependencies are injected when an object is created and references saved internally.

### Setter Injection
Using [setter methods](./getter-setter.md#setters) to accept dependencies it is possible to manipulate the dependencies at any time. While this offers some greater flexibility, it makes it difficult to ensure that all dependencies are set before the client tries to access them.

### Method Injection
Similarly to Constructor Injection, some injection frameworks allow dependencies to be injected when a method requiring them is called. Using Method Injection, not all dependencies need to be requested when an object is created, which can improve code readability.

## Read on
- [Dependency Injection (Wikipedia)](https://en.wikipedia.org/wiki/Dependency_injection)