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

Validation constraints:
 - name: from 2 to 80 characters, can't be blank, leading and trailing spaces trimmed, spaces inside allowed
 - nick: from 2 to 80 characters, can't be blank, leading and trailing spaces trimmed, alphabetic characters lower-cased, and only alpha-numeric and underscore characters allowed (without spaces inside)
 - account_type: `public` or `private` (empty value will be treated as `public`)
 - email: no restrictions because user can't edit email in application
 - bio: any text (can be blank)
