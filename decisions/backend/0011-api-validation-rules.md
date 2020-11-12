# 11. API validation rules.

Date: 2020-11-12

## Status

In progress

## Context

Design solution for API validation rules.

### Design

#### Create/Update profile API [`PUT:/profile`]

API accepts DTO:
```json
{
    "name": "John Doe",   
    "nick": "deer",
    "account_type":"public",
    "bio": "Nothing special",
    "email": "john.doe@hell.com"
}
```

Current validation rules:
* name: trim spaces, must start with `[0-9A-Za-z]` up to 80 any characters.
* nick: trim spaces, must start with `[0-9A-Za-z]` up to 80 any characters.
* account_type: must be empty, `public` or `private` case-sensitive string (empty will be replaced with `public`).
* bio: any text.
* email: no validation, but DB accepts up to 50 characters.
