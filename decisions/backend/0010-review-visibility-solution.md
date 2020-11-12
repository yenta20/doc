# 10. Review visibility solution.

Date: 2020-11-12

## Status

In progress

## Context

Describes which reviews (`public`, `private`) should be returned to user, considering below cases:
* review readiness
* user is owner
* user is follower

### User reviews API [`/user/:id/reviews`]

Current implementation checks if caller is follower:
* `false` - return all `public-ready` reviews
* `true` - return  all `private-public-ready` reviews

What should be improved/refactored:
1. If caller is owner, `private-ready` reviews should be available.
2. Should we return not ready reviews for the owner?

Solution:
* Additional check on `manager` layer (for owner role) and query improvement on `repository` layer.

### Place reviews API [`/place/:id/reviews`]

Current implementation returns only `public-ready` reviews.

What should be improved/refactored:
1. If caller is review owner, `private-ready` reviews should be returned.
2. If caller is an approved follower, `private-ready` reviews should be returned.

Solution:
* Below `select` can be used to improve query in `repository`:
```postgresql
with followed AS (
    select user_id
    from user_follower
    where follower_id = <caller_id> and status = 'accepted'
)
select *
from review r
join user_profile up on r.user_id = up.id
where r.place_id = <place_id> and r.ready = true
      and ((r.visibility = 'public')
           or (r.visibility = 'private' and r.user_id = <caller_id>)
           or (r.visibility = 'private' and r.user_id in (select user_id from followed)));
```

### Feed reviews

Current implementation creates feed with:
* `public-private-ready` review for the approved followers.
* `public-ready` review for pending followers.
