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
* ConfigurableApplicationContext will give us option to close context. Its an interface
* Bean scopes defines life cycle of beans, there are different kinds of it like Singleton, Prototype, Request, Session scope
* `PostConstruct` -> runs after contructor call
*  PreDestroy - runs before you close the context
*  **Record** 
   *  `hashCode()` - to allow objects to be stored and retrieved quickly in hash-based collections like HashMap and HashSet.
   *  The strict limitation is that you cannot declare any instance variables (non-static fields) inside the body of a record.
*  The canonical constructor is simply the "main" constructor that takes every field defined in the record header. It is generated automatically, but you can override it (ideally using the compact syntax) if you need to validate or adjust the incoming data before the record is created.

`
public record EmployeeRecord(String name, int employeeNumber) {
    // Compact canonical constructor
    public EmployeeRecord {
        if (employeeNumber <= 0) {
            throw new IllegalArgumentException("Employee number must be positive");
        }
        // If the name is null, default it to "Unknown"
        if (name == null) {
            name = "Unknown"; 
        }
        // Java automatically assigns this.name = name and this.employeeNumber = employeeNumber at the end
    }
}
`
* JPA: Jakarta Persistance API is an interface implemented by hybernate, openJPA
*  Spring Data JPA ( Repositories) <- JPA/Hibernate(Object oriendted persistence) <- JDBC(low-level access) <- Database
*  Flyway is db migration tool
*  @Table(name="tableName") mapping to table in db in Entity
*  Enable Annotation Processors to enable lombok in IntelliJ
*  Java records are an excellent native feature for handling simple, immutable data carriers, they are not a complete replacement for Project Lombok. You still need Lombok because records are strictly immutable, final, and cannot participate in class inheritance, making them incompatible with many enterprise framework patterns like JPA/Hibernate
*   Lombok’s @Builder annotation instantly creates a clean, readable builder API


