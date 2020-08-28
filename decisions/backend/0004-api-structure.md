# 4. api structure

Date: 2020-08-28

## Status

Accepted

## Context

Recon API structure proposal

## Decision

### Profile

GET /profile - get current user profile
PUT /profile
PUT /profile/avatar
PUT /profile/push_token
PUT /profile/logout

### Reviews

POST /review - create a review

POST /review/:id/images/:number - upload an image
- :id - review ID
- :number - image position

## Consequences

What becomes easier or more difficult to do and any risks introduced by the change that will need to be mitigated.
