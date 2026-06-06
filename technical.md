# Technical Topics (moved)

This file has been split into topic-focused files. See:

- **OOP:** [oop.md](oop.md)
- **Database (Normalization & Indexing):** [database.md](database.md)
- **ASP.NET Core:** [aspnet-core.md](aspnet-core.md)
- **APIs:** [api.md](api.md)
- **Architectures:** [architectures.md](architectures.md)
- **Authentication & Authorization:** [authentication-and-authorization.md](authentication-and-authorization.md)
- **Short Differences:** [short-differences.md](short-differences.md)









































# OOP

## Encapsulation

### Protecting Invariants
Encapsulation is binding data members and fucntionalities into one unit but that not enough it also allow us to prevent the object going into inconsistent state we define business rules and setter that product price cant be 0 or -ve etc

## Abstraction

### Dependency Inversion/Injection
Abstraction means programming to contracts not implementations. We provide interfaces to controllers or services rather than implementations. It not allow us to easily swap impelementations by registering new implementation in program.cs but also makes testing easier.
Example payments we make payment interface dont know or care about if its credit card or jazzcash
high level modules never depend on low level implementations 

## Inheritance

### is a relationship

Means object is having same properties and methods as base class plus its own but is must be used where there is an 'is a' relationship 
Example APIController:ControllerBase
NotificationBase
then email notification, payment etc

## Polymorphism

### open/close principle
Same function behaving differently depending on obect 
applies open close principle we define types of export pdf,csv,doc etc
game character draw object
used by override keyword 
abtract classes have functions that define basic behaviuor and we can override them in child classes

virtual keyword gives default behaviour 
abstract class and abstract function says i must be implemented in child class
override keyword must be used when implementing virtual funciton if we dont write it we are hiding functionality and compiler gives warning 

abstract class ka object ni bny ga isme pure virual b ho skta or implementations b ho skti

interface me only sigratures no implementations

### SOLID

#### S 
Single responsibility
#### O
open close
#### L
liskov substitution principle base class pointer can point derive class object (dependency injection)
#### I
inversion seggregation dont force classes to implement funciton that they will not use
#### D
dependency inversion high level modules should not depend on low level implementations use interfaces


## Database (moved)

Database topics have been moved to [database.md](database.md).

# Asp .Net Core
- cross platform to create optimized web apps or web apis developed by microsoft
- program.cs is the entry point we create builder register services then define middleware pipeline 
- middle are the functions that executes in the https request response pipeline used to filter out or verify requests
- Asp.Net Core built in dependency injection
- AddTransient when new instance for every new request
- AddScoped one instance for one HTTP request
- AddSingleton one insance for the whole app
- AllowAnonymous for the end points that completely public not required authentication like home page
- Dependency injection is when a class recieves its dependencies via constructor instead of creating inside it itself, it makes classes losely coupled, make independent testing possible and easy to swap whenever needed

# What is API?

- "API stands for Application Programming Interface. It's a contract between two systems that defines how they communicate. In web development, a REST API exposes endpoints over HTTP — the client sends a request to a URL, the server processes it and returns JSON."

## Why APIs Exist — 3 Points
###  Separation 
- frontend and backend are independent, different teams can work on each
###  Reusability 
- same API serves web app, mobile app, and third party integrations
### Security 
- database is never exposed directly, API controls what goes in and out

# Architectures

## MVC
- Stands for model view controller full stack architecture the controllers map the url and takes request process then return .cshtml files as response rather than json 
- tightly couples used for monolithic apps and small apps because as app grows code become complex

## Monolith
- Everything in a single repo the frontend,backend and database tightly couples
## N Tier
- Mono repo but we have layers like presentation, database, services etc
## Clean Architecture
WebAPI → Application ← Infrastructure
              ↓
           Domain
- Dependency goes inward only the business layer is not dependent on any other layer but still a monolithic architecture
### Layers
Domain and application does not cummunicate with external libs like EF Core Infrastructure does that
#### Domain Layer
- Define entities and business rules here 
- Business rules inside entities

#### Application Layer
- Define Services,interfaces and DTO's here but not implementations

#### Infrastructure Layer
- Implement the interfaces here, write sql server queries in repositories here

#### Presentation Layer
- Write controllers here which exposes end point to clients
### CQRS (Command Query Responsibility Segregation)
- Often used alongside Clean Architecture. Separates read operations from write operations.
- Write id treated as command it changes state add or modify some data return nothing or an ID does not require to be fast
- Read is treated as a query and it should be fast and it never changes state
- In .NET this is usually implemented with MediatR library — handlers for each command/query. Just know the concept, mention MediatR if asked.
## Microservice
- Each service has its own responsibility and own database and each service is deployed separately 
### Cummunication

#### via HTTP
- This is called synchrnous cummunication each service waits untill the called service reponds

#### via Message Bus
- This is asynchrnous cummunication each service does its part of the job and put the request in the message bus other service consume it from the bus
- One service fail does not crash the whole app
- Complex required mature devOps engineers


# Authentication and Authorization
Request comes in
      ↓
Authentication middleware — reads token, identifies user
      ↓
Authorization middleware — checks if this user can access this route
      ↓
Controller
- They play their roles in the middleware
## Authentication Who are you?
- Login -> verify identity -> issue token
- It asks the user who are you do you belong here or not

### JWT (Json Web Token)

- A JWT is 3 parts separated by dots: 
- eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxMjMifQ.SflKxwRJS
-       Header                  Payload          Signature

// Header — algorithm used
{ "alg": "HS256", "typ": "JWT" }

// Payload — claims (user data)
{
  "sub": "user-guid-here",
  "email": "musab@gmail.com",
  "role": "Admin",
  "exp": 1718000000   // expiry timestamp
}

// Signature — Header + Payload signed with secret key
// If anyone tampers with payload, signature won't match → rejected
HMACSHA256(base64(header) + "." + base64(payload), secretKey)

- Important: JWT payload is base64 encoded, not encrypted. Anyone can decode and read it. Never put sensitive data like passwords in it.

## AUTHORIZATION What can you do?
- 3 types in ASP.NET Core — Role, Claims, Policy.
### Type 1 — Role Based (Most Common)
[Authorize]                       // any authenticated user
[Authorize(Roles = "Admin")]      // only Admin
[Authorize(Roles = "Admin,Manager")] // Admin OR Manager

### Type 2 — Claims Based
- Claims are key-value pairs inside the token. More granular than roles.

### Type 3 — Policy Based (Most Flexible)
- Policies let you combine multiple rules into one named requirement.


# Short Differences

## App.use vs App.run
- App.use() to call a middleware 
- App.run() final call to start the app no middleware after that

## String vs String builder
- String is immutable every operation creates a new object
- StringBuilder is mutable

## Var vs Dynamic
- var is inferred at compile time but its still strongly typed 
- var name = "abc"
- name = 123 compiler error compiler knows its a string

- dynamic is completely executed at runtime

## ref vs out
- ref = already has a value, method may change it.
-  out = method is responsible for giving it a value.
- Common real use: int.TryParse("123", out int value).

## Model Binding
- ASP.NET Core automatically maps incoming HTTP request data to your method parameters.

## Global Exception Handling
- Instead of try/catch in every controller, handle all exceptions in one place.