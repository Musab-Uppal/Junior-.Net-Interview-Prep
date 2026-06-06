# Authentication and Authorization

Request flow:

- Authentication middleware — reads token, identifies the user.
- Authorization middleware — checks if this user can access the route.
- Controller handles the request.

## Authentication — Who are you?
- Login → verify identity → issue token.

### JWT (JSON Web Token)
A JWT has three parts separated by dots: header, payload, signature.

Example token structure:
`eyJhbGciOiJIUzI1NiJ9.eyJzdWIiOiIxMjMifQ.SflKxwRJS`

Header:
```json
{ "alg": "HS256", "typ": "JWT" }
```

Payload (claims):
```json
{
  "sub": "user-guid-here",
  "email": "musab@gmail.com",
  "role": "Admin",
  "exp": 1718000000
}
```

Signature:
```
HMACSHA256(base64(header) + "." + base64(payload), secretKey)
```

Important: the JWT payload is base64 encoded, not encrypted. Never put sensitive data like passwords in it.

## Authorization — What can you do?
- **Role-based:** `[Authorize]`, `[Authorize(Roles = "Admin")]`.
- **Claims-based:** use claims inside tokens for more granular control.
- **Policy-based:** define policies combining multiple requirements.
