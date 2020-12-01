# 4. api structure

Date: 2020-08-28

## Status

In progress

## Context

Recon API structure proposal

## Decision

### General

## Pagination

Pagination is implemented using **limit/offset** query params (except pagination for feed).
All methods with pagination must support the default values for query parameters (if paramater is missing use 
the default value).

The values:
 * limit == 20
 * offset == 0
   
# Table of contents
1. [Profile](#profile)
2. [User](#user)
3. [Reviews](#reviews)
4. [Places](#places)
5. [Followers](#followers)
6. [Geocoding](#geocoding)
7. [Feed](#feed)

## <a name="profile"></a>
## Profile

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
* **PUT /profile/avatar** - upload avatar
* **PUT /profile/push_token** - set push token
* **PUT /profile/logout** - logout (clear push token)

* **GET /profile/my_reviews?offset=..&limit=..** - get all reviews created by user
```
Response similar to /places/:id/reviews
```

* **GET /profile/availability?nick=...*** - return 200 if nick avaible; return 409  if nickname not available

## <a name="user"></a>
## User

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

* **GET /user/:id/reviews?offset=..&limit=..** - get user reviews
```
Response similar to /places/:id/reviews
```

* **GET /user/search?name=..?limit=..** - search user by name
```
Response:
{
   "users": [
           {
               "id": 1,
               "name": "John Doe",
               "nick": "deer",
               "account_type": "public",
               "bio": "Nothing special",
               "avatar": ""
           }
       ]
}
```

## <a name="reviews"></a>
## Reviews

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
    "id": 1,
    "creator": 1,
    "created": "2020-11-18T10:49:52Z",
    "place": {
        "id": 1,
        "external_id": {
            "id": "50a2bd98e4b070229a45269d",
            "place_type": 3
        },
        "title": "Local Pho",
        "tags": [
            "Vietnamese Restaurant"
        ],
        "location": {
            "latitude": 47.6146226,
            "longitude": -122.3446252
        },
        "address": {
            "country": "United States",
            "region": "Washington",
            "county": "King",
            "city": "Seattle",
            "postal": "98121-2019",
            "address_line": "3rd Ave 2230"
        },
        "formatted_address": "3rd Ave 2230 Seattle 98121-2019",
        "phone_number": "",
        "web_site": "",
        "rating": 0,
        "reviews_count": 1,
        "distance": 0,
        "completed": false,
        "sources": [
            {
                "id": "50a2bd98e4b070229a45269d",
                "place_type": 3
            },
            {
                "id": "here:pds:place:840c22yz-cdd24b4782144d51b49444fe402d01d8",
                "place_type": 2
            }
        ]
    },
    "text": "Perfect place for lunch",
    "visibility": "public",
    "stars": 5,
    "images": [
        {
            "id": 1,
            "url": "https://storage.googleapis.com/image-store-local-razorback/34a68f45-9e3c-479c-828e-1b043dbb52a4"
        }
    ]
}
```

## <a name="places"></a>
## Places

* **GET /places/search/area?rect=..** - search in the rectange area or with point and radius. Returns only registered places.
Optional:
- ?rect=47.6146226,-122.3446252,47.614017,-122.343562
- ?at=47.6146226,-122.3446252&radius=20 

* **GET /places/:id** - get the registered place info.
```
Response:
    {
        "id": 1,
        "external_id": {
            "id": "50a2bd98e4b070229a45269d",
            "place_type": 3
        },
        "title": "Local Pho",
        "tags": [
            "Vietnamese Restaurant"
        ],
        "location": {
            "latitude": 47.6146226,
            "longitude": -122.3446252
        },
        "address": {
            "country": "United States",
            "region": "Washington",
            "county": "King",
            "city": "Seattle",
            "postal": "98121-2019",
            "address_line": "3rd Ave 2230"
        },
        "formatted_address": "3rd Ave 2230 Seattle 98121-2019",
        "phone_number": "",
        "web_site": "",
        "rating": 0,
        "reviews_count": 1,
        "distance": 0,
        "completed": false,
        "sources": [
            {
                "id": "50a2bd98e4b070229a45269d",
                "place_type": 3
            },
            {
                "id": "here:pds:place:840c22yz-cdd24b4782144d51b49444fe402d01d8",
                "place_type": 2
            }
        ]
    }
```

* **GET /places/search?q=..&rect=..** - text search in the rectange area or with point and radius or with location. q is mandatory.
Optional:
- &rect=47.6146226,-122.3446252,47.614017,-122.343562
- &at=47.6146226,-122.3446252&radius=20
- &loc=Los Angeles

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
    "places": [],
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

## <a name="followers"></a>
## Followers 

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

## <a name="geocoding"></a>
## Geocoding 

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

## <a name="feed"></a>
## Feed

* **GET /feed?from=..&to=..&limit=..** - get feed

### Notifications

In design

## Questions

* Design for the review edit
