# Database

## First Normal Form (1NF)
- No repeating groups. For example, phone numbers should be modeled in a separate table rather than as multi-valued columns.
- Multiple values in a single column cause redundancy and difficulties with operations like `SELECT` or `JOIN`.
- Solution: create another table (for example, `OrderItems`) and use a foreign key (`OrderId`).

## Second Normal Form (2NF)
- No partial dependency: every non-key column must depend on the whole primary key.
- Example: if `OrderItems` contains `ProductName` and `ProductId`, move `ProductName` to the `Products` table.

## Third Normal Form (3NF)
- No transitive dependency: non-key columns should not depend on other non-key columns.
- Remove transitive dependencies by creating a separate table and referencing it with a foreign key.

## Indexing
- Data structures that speed up queries.
- Index foreign keys and columns commonly used in `WHERE` clauses.
- Index columns with high selectivity (for example, email, user id).

## LINQ and EF Core

LINQ (Language-Integrated Query) performs filtering and other operations on in-memory collections. EF Core is an ORM that communicates with the database, translating LINQ expressions into SQL and performing read/write operations for us.

EF Core provides features that LINQ alone does not, such as `Include` / `ThenInclude` (for eager loading), `AsNoTracking` (for read-only queries), and raw SQL via `FromSqlRaw`. When using EF Core in async contexts prefer `ToListAsync()`/`ToArrayAsync()`.

- LINQ works on any collection; EF Core uses LINQ over `IQueryable<T>` to translate expressions to SQL.
- `IQueryable` defers execution and runs on the database; `IEnumerable` runs in memory. Using the wrong one on large tables is a common performance bug.

Queries are not executed until a terminal operator is called (for example `ToListAsync()`, `ToArrayAsync()`, or `ToList()`).

### Types of LINQ Queries

#### 1. Method syntax (Lambda)
```csharp
var products = await _context.Products
	.Where(p => p.Price > 5000)
	.OrderBy(p => p.Name)
	.Select(p => p.Name)
	.ToListAsync();
```

#### 2. Query syntax (SQL-like)
```csharp
var products = (from p in _context.Products
				where p.Price > 5000
				orderby p.Name
				select p.Name)
			   .ToList();
```

## Dapper

Dapper is a micro-ORM. You write raw SQL yourself; Dapper maps the results to your C# objects. Use Dapper for complex, heavy queries where tuned SQL is required. EF Core is better for standard CRUD and change-tracking — the two can be used together.

### Where to use Dapper
- Not a replacement for EF Core — use both together. EF Core for standard CRUD, Dapper for complex, heavy queries.

## DB First and Code First approaches

### Code First
- Use Code First when the database does not exist yet (greenfield projects).
- Easy to track in source; good for small teams and simple projects.
- Complex databases can be harder to model in code.
- Create model classes, then run migrations:

```bash
dotnet ef migrations add InitialCreate
dotnet ef database update
```

### DB First

When the database already exists, scaffold entity classes and the `DbContext`:

```bash
dotnet ef dbcontext scaffold "ConnectionString"
```

EF will generate model classes and the `DbContext`.
