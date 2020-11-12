# 10. Review visibility solution.

Date: 2020-11-12

## Status

In progress

## Context

Describes which reviews (`public`, `private`) should be returned to user, considering below cases:
* review readiness
* user is owner
* user is follower

### Design

Visibility constraints implemented on DB layer.

### User reviews API [`/user/:id/reviews`]

API manager checks if caller is follower:
* `false` - return all `public-ready` reviews
* `true` - return  all `private-public-ready` reviews

### Place reviews API [`/place/:id/reviews`]

Returns `private-public-ready` reviews if caller is:
* review owner.
* an approved follower.

Returns only `public-ready` reviews in other cases.

### Feed reviews

Current implementation creates feed with:
* `public-private-ready` review for the approved followers.
* `public-ready` review for pending followers.
