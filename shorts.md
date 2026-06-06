# Short Differences

## app.Use vs app.Run
- `app.Use(...)` registers middleware that can call the next middleware in the pipeline.
- `app.Run(...)` registers a terminal middleware that does not call the next delegate.

## string vs StringBuilder
- `string` is immutable; each modification creates a new object.
- `StringBuilder` is mutable and more efficient for many concatenations.

## var vs dynamic
- `var` is inferred at compile time and still strongly typed:
```csharp
var name = "abc";
// name = 123; // compiler error
```
- `dynamic` defers binding to runtime.

## ref vs out
- `ref` requires an initial value; the method may change it.
- `out` does not require an initial value; the method must assign it.
- Example: `int.TryParse("123", out int value);`

## Model Binding
- ASP.NET Core automatically maps incoming HTTP request data to your method parameters.

## Global Exception Handling
- Instead of try/catch in every controller, handle exceptions centrally in middleware.
