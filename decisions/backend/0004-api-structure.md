# 4. api structure

Date: 2020-08-28

## Status

In progress

## Context

Recon API structure proposal

## Decision

### General

#### Pagination

Pagination is implemented using **limit/offset** query params (expect pagination for feed).
All methods with pagination must support the default values for query parameters (if paramater is missing use 
the default value).

The values:
 * limit == 20
 * offset == 0

### Profile

* **GET /profile** - get current user profile
```
Response:
{
    "id": 1,
    "name": "John Doe",
    "age": 23,
    "email": "john.doe@hell.com",
    "avatar": "https://storage.googleapis.com/avatar-store-local-eugenf/30db7f00-7776-4259-a4f8-8aeb01622df6",
    "user_settings": {}
}
```
* **PUT /profile** - create/update profile
```
*Profile constraints:*
 - name: from 2 to 80 characters, can't be blank, leading and trailing spaces are trimmed, spaces inside allowed
 - nick: from 2 to 64 characters, can't be blank, leading and trailing spaces are trimmed, spaces inside are not allowed
 - account_type: `public` or `private` (empty value will be treated as `public`)
 - email: no restrictions because user can't edit email in application
 - bio: any text (can be blank)
```
* **PUT /profile/avatar** - upload avatar
* **PUT /profile/push_token** - set push token
* **PUT /profile/logout** - logout (clear push token)

* **GET /profile/my_reviews** - get all reviews created by user

* **GET /profile/availability?nick=...*** - ....

### User

* **GET /user/:id** - get any user by id
```
Response:
{
    "id": 1,
    "name": "John Doe",
    "nick": "deer",
    "account_type": "public",
    "bio": "",
    "avatar": ""
}
```

### Reviews

* **POST /reviews** - create a review
```
Request:
{
    "place": {
        "external_id": "ChIJy3tqPUwVkFQRuEM8cw5X8aY",
        "place_type": 1
    },
    "text": "Perfect place for lunch",
    "stars": 5
}
```

* **POST /reviews/:id/images/:number** - upload an image
- :id - review ID
- :number - image position

* **PUT /reviews/:id/complete** - complete a review

* **GET /reviews/:id** - get review by id
```
Response:
{
    "review": {
        "id": 1,
        "creator": 1,
        "created": "2020-10-18T14:24:10Z",
        "place": {
            "id": 1,
            "external_id": "ChIJy3tqPUwVkFQRuEM8cw5X8aY",
            "place_type": 1,
            "title": "Korean Bamboo",
            "tags": [
                "restaurant",
                "food",
                "point_of_interest",
                "establishment"
            ],
            "location": {
                "latitude": 47.6147389,
                "longitude": -122.3445459
            },
            "address": {
                "country": "United States",
                "region": "WA",
                "city": "King County",
                "postal": "98121",
                "address_line": "2236 3rd Ave"
            },
            "vicinity": "",
            "phone_number": "+1 206-443-9898",
            "rating": 0,
            "website": ""
        },
        "text": "Perfect place for lunch",
        "stars": 5,
        "images": [
            {
                "id": 1,
                "url": "https://storage.googleapis.com/image-store-local-eugenf/0835074b-fa1c-4cc8-b6ec-54a828b3ea9a"
            }
        ]
    }
}
```

### Places

* **GET /places/search/area?top_left_lat=..&top_left_long=..&bottom_right_lat=..&bottom_right_long=..** - search in the rectange area. Returns only registered places.

* **GET /places/:id** - get the registered place info.

* **GET /places/:id/reviews?offset=..&limit=..** - get all ready reviews for place.
```
Response:
{
    "reviews": [
        {
            "id": 1,
            "creator": 1,
            "created": "2020-10-18T14:24:10Z",
            "place": {
                "id": 1,
                .......
            },
            "text": "Perfect place for lunch",
            "stars": 5,
            "images": [
                {
                    "id": 1,
                    "url": "https://storage.googleapis.com/image-store-local-eugenf/0835074b-fa1c-4cc8-b6ec-54a828b3ea9a"
                }
            ]
        }
    ],
    "users": [
        {
            "id": 1,
            "name": "John Doe",
            "avatar": "https://storage.googleapis.com/image-store-local-eugenf/30db7f00-7776-4259-a4f8-8aeb01622df6",
        }
    ],
    "from": 0,
    "limit": 5
}
```

### Followers

* **GET /follow/info** - get all follow related information. Returns 4 arrays:
    * **followers** - my followers
    * **followed** - users I follow
    * **followers_requests** - pending follow requests from users wants to follow me
    * **followed_requests** - my pending follow requests
```
Response:
{
    "followed": [],
    "followed_requests": [],
    "followers": [],
    "followers_requests": []
}
```

* **PUT /follow/:user_id** - send follow request
* **PUT /follow/:user_id/unfollow** - unfollow user

* **PUT /follow/followers/pending/:id/approve** - approve the pending request
* **PUT /follow/followers/pending/:id/reject** - reject the pending request

* **PUT /follow/followed/pending/:id/cancel** - cancel my pending follow request

### Geocoding

* **GET /geo/rev?lat=..&long=..&radius=..** - search places for point and radius. Returns the merged result of places from google ,here and repo
```
Response:
{
    "aggregation_id": "ChIJy3tqPUwVkFQRuEM8cw5X8aY",
    "address": {
        "country": "United States",
        "region": "WA",
        "city": "King County",
        "postal": "98121",
        "address_line": "2236 3rd Ave"
    },
    "places": [
        {
            "place": {
                "id": 1,
                "external_id": "ChIJy3tqPUwVkFQRuEM8cw5X8aY",
                "place_type": 1,
                "title": "Korean Bamboo",
                "tags": [
                    "restaurant",
                    "food",
                    "point_of_interest",
                    "establishment"
                ],
                "location": {
                    "latitude": 47.6147389,
                    "longitude": -122.3445459
                },
                "address": {
                    "country": "United States",
                    "region": "WA",
                    "city": "King County",
                    "postal": "98121",
                    "address_line": "2236 3rd Ave"
                },
                "vicinity": "",
                "phone_number": "+1 206-443-9898",
                "rating": 0,
                "website": ""
            },
            "distance": 12
        },
    ]
}
```

* **POST /geo/rev/batch** - search places for the points sets. Creates points clusters with defined radius and searches places for the each cluster.
```
Request:
{
    "points":[
        {
            "id":"1",
            "point":{
                "latitude":47.6147389,
                "longitude":-122.3445459
            }
        }
    ],
    "radius":85
}

Response:
{
    "clusters": [
        {
            "point_ids": [
                "1"
            ],
            "center": {
                "latitude": 47.61473889999999,
                "longitude": -122.34454590000001
            },
            "places": [
                {
                    "place": {
                        "id": 1,
                        "external_id": "ChIJy3tqPUwVkFQRuEM8cw5X8aY",
                        "place_type": 1,
                        "title": "Korean Bamboo",
                        .........
                    },
                    "distance": 0
                },
            ],
            "with_error": false
        }
    ]
}
```

### Feed

* **GET /feed?from=..&to=..&limit=..** - get feed

### Notifications

In design

## Questions

* Design for the review edit

