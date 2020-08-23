# 2. HTTP status codes

Date: 2020-08-23

## Status

Accepted

## Context

HTTP status codes are using on the project

## Decision

### Success
**201** - created, uses as POST operation result, means the resource is successfully created.
**200** - uses for indicating success in all other cases.

### Errors

#### Application generated
**400** - bad request. Means the request couldn't be parsed by service (invalid JSON, unknown query params).
**422** - validation error. The request is well-formed but has some logic errors (empty name, an invalid date, etc).
**409** - uses for the general logic error.

For **409 & 422** error codes service return JSON with more detail error description:
```
{
    "message": "validation error, BirthDate"
}
```

#### General

Other HTTP codes van be returned in other different situations
For example:
404 - url not found 
or
500 - unexpected server error or server is down 

## Consequences

Service should not use any other error codes but described above.
