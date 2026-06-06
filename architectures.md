# Architectures

## MVC
- Model-View-Controller: controllers map URLs and return views (`.cshtml`). Often used in full-stack monolithic apps.

## Monolith
- Everything in a single repository; frontend, backend, and database tightly coupled.

## N-Tier
- Separate layers within the same repository (presentation, services, data access).

## Clean Architecture
- Dependencies point inward; business logic does not depend on external frameworks. EF Core and other infrastructure concerns live in the Infrastructure layer.

### Layers
- **Domain:** entities and business rules.
- **Application:** services, interfaces, and DTOs (no implementations).
- **Infrastructure:** implementations of interfaces and data access.
- **Presentation:** controllers and endpoints.

### CQRS
- Command Query Responsibility Segregation: separate reads from writes.
- Commands change state; queries read state.
- Often implemented with MediatR in .NET (handlers for each command/query).

## Microservices
- Each service owns its responsibility and its own database and is deployed separately.

### Communication
- **HTTP:** synchronous communication; caller waits for the response.
- **Message bus:** asynchronous communication; services publish/consume messages (more resilient, more complex).
