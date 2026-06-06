# ASP.NET Core

- Cross-platform framework to build optimized web apps and Web APIs, developed by Microsoft.
- `Program.cs` is the entry point: create the builder, register services, then define the middleware pipeline.
- Middleware are functions executed in the HTTP request/response pipeline used to filter or verify requests.
- ASP.NET Core has built-in dependency injection:
  - `AddTransient` — a new instance every time it's requested.
  - `AddScoped` — one instance per HTTP request.
  - `AddSingleton` — one instance for the application lifetime.
- `[AllowAnonymous]` marks endpoints that do not require authentication.
- Dependency injection: a class receives its dependencies via constructor injection instead of creating them internally. This makes classes loosely coupled and easier to test and swap implementations.
