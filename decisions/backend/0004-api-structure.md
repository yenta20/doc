# 4. api structure

Date: 2020-08-28

## Status

In progress

## Context

Recon API structure proposal

## Decision

### Profile

* **GET /profile** - get current user profile

* **PUT /profile** - update profile
* **PUT /profile/avatar** - upload avatar
* **PUT /profile/push_token** - set push token
* **PUT /profile/logout** - logout (clear push token)

* **GET /profile/my_reviews** - get all reviews created by user

### Reviews

* **POST /reviews** - create a review

**change to /reviews/:id/images... ?**

* **POST /reviews/:id/images/:number** - upload an image
- :id - review ID
- :number - image position

* **PUT /reviews/:id/complete** - complete a review

* **GET /reviews/:id** - get review by id

### Places

* **GET /places/search/area?top_left_lat=..&top_left_long=..&bottom_right_lat=..&bottom_right_long=..** - search in the rectange area. Returns only registered places.

* **GET /place/:id** - get the registered place info.

* **GET /place/:id/reviews** - get all ready reviews for place.

### Followers

* **GET /follow/followers** - get my followers
* **GET /follow/followed** - get users I follow

* **PUT /follow/:user_id** - send follow request
* **PUT /follow/:user_id/unfollow** - unfollow user

* **GET /follow/followers/request/pending** - get pending follow requests from users wants to follow me

* **PUT /follow/followers/request/pending/:id/approve** - approve the pending request
* **PUT /follow/followers/request/pending/:id/reject** - reject the pending request

* **GET /follow/followed/request/pending** - get my pending follow requests
* **PUT /follow/followed/request/pending/:id/cancel** - cancel my pending follow request

### Geocoding

* **POST /geo/rev/batch** - search places for the points set and clusters them . Returns the merged result of places from google ,here and repo
-example of body:    "points":[{
                         "id":"1",
                         "point":{
                             "latitude":50.424949,
                             "longitude":23.67908
                         }
                     },
                     {
                         "id":"2",
                         "point":{
                             "latitude":39.9860,
                             "longitude":-120.3304
                         }
                     }],
                     "radius": 85
                 }

* **GET /geo/rev?lat=..&long=..&radius=..** - search places for point and radius. Returns the merged result of places from google ,here and repo

### Feed

* **GET /feed?from=..&to=..&limit=..** - get feed

### Notifications

In design

## Questions

* Feed API
* Design for the review edit
* Get/Search place should return reviews or not?

