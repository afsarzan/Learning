# Java and Spring Framework Notes

## IDE Shortcuts & Tips
* **Eclipse:** To create an interface from an existing class, go to **Refactor -> Extract -> Interface**.
* **IntelliJ IDEA:** Press `Alt + Enter` on a class that implements an interface to view and generate all suggested method implementations.

##  Object-Oriented Principles & Design
* **Interfaces:** Act as contracts. They contain method declarations outlining *what* needs to be done, while the services that implement them define *how* it is done.
* **Encapsulation:** Protects the internal state of a class from being directly accessed or altered by external actors.
* **Dependency Integrity:** Prevents other parts of the application from unexpectedly swapping or tampering with injected services during runtime.
* **Implementation Hiding:** Keeps the public API clean by hiding internal dependencies, reducing tight coupling across the application.
* **Immutability (Best Practice):** When paired with the `final` keyword, it guarantees that a service reference is assigned exactly once during construction and remains thread-safe.

## Dependency Injection
* **Setter Injection:** You can use setter methods to inject dependencies, but it is not commonly recommended as a primary approach. A user or developer might forget to set the dependency before calling a service method, leading to runtime errors.

##  Spring Framework Core
* **Inversion of Control (IoC):** Spring can create objects and inject them into our classes. At its core, the Spring IoC container manages the lifecycle and wiring of these objects.
* **Terminology Differences:**
    * **POJO (Plain Old Java Object):** A regular Java object with no restrictions.
    * **JavaBean:** A POJO that strictly follows specific rules (must have a no-arg constructor, private fields with getters/setters, and implement `Serializable`).
    * **Spring Bean:** Any object (whether it's a POJO, a JavaBean, or something else) whose lifecycle and dependencies are managed by the Spring IoC container.

## Spring Annotations
* `@Component`: A stereotype annotation that tells Spring to manage the creation of this object as a bean.
* `@Primary`: Added to one of the implementation classes to mark it as the default choice when multiple implementations of an interface exist.
* `@Qualifier`: Added at the injection point (e.g., the constructor) to specify exactly which bean name you want to use when there are multiple candidates.

* ApplicationContext is an Inversion of Control (IoC) container in Spring.
* Bean scopes defines life cycle of beans, there are different kinds of it like Singleton, Prototype, Request, Session scope
* PostConstruct -> runs after contructor call
* 

