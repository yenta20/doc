# 4. api structure

Date: 2020-08-28

## Status

Accepted

## Context

Recon API structure proposal

## Decision

### Profile

GET /profile - get current user profile
PUT /profile - update profile
PUT /profile/avatar - upload avatar
PUT /profile/push_token - set push token
PUT /profile/logout - logout (clear push token)


### Reviews

POST /review - create a review

POST /review/:id/images/:number - upload an image
- :id - review ID
- :number - image position

PUT /review/:id/complete - complete a review


### Places

POST place/search/points?type=(all|registered) - search places for the points set. 
Returns only registered places (type==registered) or both registered + places found via Google Places API (type==all)

POST place/search/area?with_reviews=(0|1) - search in the rectange area. Returns only registered places.
If with_reviews == 0, returns only places.
If with_reviews == 1, returns places with visible reviews and "user applicable" ratings

GET place/:id - get the registered place info (inclides visible reviews and "user applicable" rating)


### Followers

GET /follow/followers - get my followers
GET /follow/followed - get users I follow

PUT /follow/:user_id - send follow request
PUT /follow/:user_id/unfollow - unfollow user

GET /follow/followers/request/pending - get pending follow requests from users wants to follow me

PUT /follow/followers/request/pending/:id/approve - approve the pending request
PUT /follow/followers/request/pending/:id/reject - reject the pending request

GET /follow/followed/request/pending - get my pending follow requests
PUT /follow/followed/request/pending/:id/cancel - cancel my pending follow request


### Feed

GET /feed?offset=0&limit=10 - get user feed (visible reviews sorted by created date)


### Notifications

GET /notifications?since=time_token - returns all updates related to the user happened since `time_token`.
Returns new reviews, follow requests, new ratings for my places, etc but only 100 (?) latest. 


## Consequences

What becomes easier or more difficult to do and any risks introduced by the change that will need to be mitigated.
