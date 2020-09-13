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

* **POST /reviews/review/:id/images/:number** - upload an image
- :id - review ID
- :number - image position

* **PUT /reviews/review/:id/complete** - complete a review

* **GET /reviews/review/:id** - get review by id

### Places

* **POST places/search/points?type=(all|registered)** - search places for the points set. 
Returns only registered places (type==registered) or both registered + places found via Google Places API (type==all). 
Now supported only *all* mode.

* **GET places/search/area?top_left_lat=..&top_left_long=..&bottom_right_lat=..&bottom_right_long=..** - search in the rectange area. Returns only registered places.

* **GET place/:id** - get the registered place info.


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


### Feed

In design

### Notifications

In design

## Questions

* Feed API
* Design for the review edit
* Get/Search place should return reviews or not?

