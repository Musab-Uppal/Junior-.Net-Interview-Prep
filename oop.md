# OOP

## Encapsulation

### Protecting Invariants
Encapsulation is binding data members and functionalities into one unit. That's not enough — it also allows us to prevent an object from entering an inconsistent state. We define business rules and setters that ensure a product price can't be 0 or negative.

## Abstraction

### Dependency Inversion / Dependency Injection
Abstraction means programming to contracts, not implementations. We provide interfaces to controllers or services rather than concrete implementations. This allows us to swap implementations by registering a new implementation in `Program.cs` and makes testing easier.

Example: for payments, define an `IPayment` interface; the consumer doesn't need to know whether the implementation is `CreditCardPayment` or `JazzCashPayment`.

High-level modules should not depend on low-level implementations.

## Inheritance

### "Is-a" relationship
Inheritance means a derived class has the same properties and methods as the base class, plus its own. Use it only where there is an "is-a" relationship.

Example:
```csharp
public class ApiController : ControllerBase
{
}
```

## Polymorphism

### Open/Closed Principle
The same function can behave differently depending on the object. This supports the Open/Closed Principle: extend behavior without modifying existing code (for example, export formats: PDF, CSV, DOC).

- Use `virtual` to provide a default implementation.
- Use `override` to change behavior in derived classes.
- `abstract` members must be implemented by derived classes.
- An `abstract` class cannot be instantiated.

Interfaces contain signatures (implementations live in classes).

## SOLID

- **Single Responsibility:** a class should have only one reason to change.
- **Open/Closed:** open for extension, closed for modification.
- **Liskov Substitution:** derived classes must be substitutable for their base types.
- **Interface Segregation:** don't force classes to implement methods they won't use.
- **Dependency Inversion:** high-level modules should not depend on low-level modules; depend on abstractions.
