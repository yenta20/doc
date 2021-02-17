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
    "nick": "john123",
    "name": "John Doe",
    "account_type": "public",
    "bio": "It is my bio",
    "email": "john.doe@hell.com",
    "phone": "+123456789",
    "avatar": "https://storage.googleapis.com/avatar-store-local-eugenf/30db7f00-7776-4259-a4f8-8aeb01622df6"
    "activation_code": ""
}
```
* **PUT /profile** - create/update profile
```
Request:
{
    "nick": "deer",
    "account_type":"public",
    "name": "John Doe",   
    "bio": "Nothing special",
    "email": "john.doe@hell.com"
}
```
```
Response:
{
    "id": 1,
    "nick": "deer",
    "account_type": "public",
    "name": "John Doe",
    "bio": "Nothing special",
    "avatar": "",
    "email": "john.doe@hell.com",
    "activation_code": ""
}
```
* **PUT /profile/avatar** - upload avatar
* **PUT /profile/push_token?token=..** - set push token
* **PUT /profile/logout** - logout (clear push token)
* **GET /profile/my_reviews/image_ids** - get all user's image ids
```
Response:
{
    "ids": [
        "id-1",
        "id-2"
    ]
}
```  
* **GET /profile/settings** - get current user settings
```
Response:
{
    "push_notifications": true,
    "follow_accepted_push": true
}
```
* **PUT /profile/settings** - update current user settings
```
Request:
{
    "push_notifications": true,
    "follow_accepted_push": true
}
```  
* **GET /profile/follow_suggestions** - get follow suggestions for current user
```
Response:
[
    {
        "id": 1,
        "name": "John Doe",
        "nick": "deer",
        "account_type": "public",
        "bio": "Nothing special",
        "avatar": ""
    }
]
```

* **GET /profile/my_reviews?offset=..&limit=..&type=..** - get all reviews created by user
```
Response:
{
    "places": [
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
                "latitude": 47.614623,
                "longitude": -122.344625
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
            "distance": 0,
            "sources": [
                {
                    "id": "50a2bd98e4b070229a45269d",
                    "place_type": 3
                },
                {
                    "id": "here:pds:place:840c22yz-cdd24b4782144d51b49444fe402d01d8",
                    "place_type": 2
                }
            ],
            "timezone": "",
            "hours": {},
            "public_rating": 2.5,
            "public_review_count": 1,
            "private_rating": 0,
            "private_review_count": 0,
            "now": {
                "is_available": false,
                "is_opened": false,
                "will_open_today": false,
                "till_close": "00:00:00",
                "till_open": "00:00:00"
            }
        }
    ],
    "reviews": [
        {
            "id": 1,
            "creator": 1,
            "created": "2021-02-17T10:17:45Z",
            "place": {
                "id": 1
            },
            "text": "Perfect place for lunch",
            "visibility": "public",
            "stars": 5,
            "is_kitchen": false,
            "images": [
                {
                    "id": 1,
                    "url": "https://storage.googleapis.com/image-store-local-bohdan/c0meqjc2qtansgmqect0"
                }
            ],
            "likes_counter": 0,
            "liked_by_me": false,
            "comments": {
                "object_id": 0,
                "parent_id": 0,
                "items": null,
                "users": null,
                "total_count": 0,
                "limit": 0,
                "offset": 0
            },
            "feed_position": 0
        }
    ],
    "limit": 20,
    "offset": 0
}
```
- ***type*** - "poi" or "kitchen"

* **GET /profile/availability?nick=...*** - return 200 if nick available; return 409  if nickname not available

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
Response similar to /profile/my_reviews
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
* **GET /user/:id/places** - get user reviewed places
- Optional:
- ?rect=47.6146226,-122.3446252,47.614017,-122.343562
```
Response
{
    "places": [
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
                "latitude": 47.614623,
                "longitude": -122.344625
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
            "distance": 0,
            "sources": [
                {
                    "id": "50a2bd98e4b070229a45269d",
                    "place_type": 3
                },
                {
                    "id": "here:pds:place:840c22yz-cdd24b4782144d51b49444fe402d01d8",
                    "place_type": 2
                }
            ],
            "timezone": "",
            "hours": {},
            "public_rating": 2.5,
            "public_review_count": 1,
            "private_rating": 0,
            "private_review_count": 0,
            "now": {
                "is_available": false,
                "is_opened": false,
                "will_open_today": false,
                "till_close": "00:00:00",
                "till_open": "00:00:00"
            }
        }
    ],
    "rect": {
        "top_left": {
            "latitude": 0,
            "longitude": 0
        },
        "bottom_right": {
            "latitude": 0,
            "longitude": 0
        }
    }
}
```

## <a name="reviews"></a>
## Reviews

* **POST /reviews** - create a review
- ***POI***
```
Request:
{
    "place": {
        "external_id": "ChIJy3tqPUwVkFQRuEM8cw5X8aY",
        "place_type": 1
    },
    "text": "Perfect place for lunch",
    "stars": 5,
    "image_ids": ["id-3", "id-4"]
}
```
- ***Kitchen***
```
Request:
{
    "text": "Perfect place for lunch",
    "stars": 5,
    "is_kitchen": true,
    "image_ids": ["id-3", "id-4"]
}

```

* **POST /reviews/:id/images/:number** - upload an image
- ***id*** - review ID
- ***number*** - image position

* **PUT /reviews/:id/complete** - complete a review

* **POST /reviews/:id/like** - add like

* **DELETE /reviews/:id/like** - remove like

* **GET /reviews/:id/likes** - return users who liked review
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
* **PUT /reviews/:id/preview/generate** - generate preview
```
Response:
{
    "url": "http://localhost:9124/preview/v1/reviews/c0mf4gc2qtansgmqectg"
}
```  
* **GET /reviews/by_preview/:id** - get review by preview ID
* **DELETE /reviews/:id** - delete review
* **POST /reviews/:id/comments** - add comment
```
Request:
{
    "text": "my first comment"
}
```  
* **PUT /reviews/:id/comments/:comment_id** update comment
* **DELETE /reviews/:id/comments/:comment_id** - delete comment
* **GET /reviews/:id/comments?offset=..&limit=..** - get comments
```
Response:
{
    "object_id": 1,
    "parent_id": 0,
    "items": [
        {
            "id": 1,
            "author_id": 1,
            "published_at": "2021-02-16T16:22:28Z",
            "parent_id": 0,
            "content": {
                "text": "my first comment"
            },
            "replies_count": 0,
            "replies": null
        }
    ],
    "users": [
        {
            "id": 1,
            "name": "John Doe",
            "nick": "deer",
            "account_type": "public",
            "bio": "Nothing special",
            "avatar": ""
        }
    ],
    "total_count": 1,
    "limit": 20,
    "offset": 0
}
```

- **object_id** - review id
- **parent_id** - not implemented yet

## <a name="places"></a>
## Places

* **GET /places/search/area?rect=..** - search in the rectangle area or with point and radius. Returns only registered places.
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
        "latitude": 47.614623,
        "longitude": -122.344625
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
    "phone_number": "(206) 441-5995",
    "web_site": "http://localpho-seattle.com",
    "distance": 0,
    "sources": [
        {
            "id": "50a2bd98e4b070229a45269d",
            "place_type": 3
        },
        {
            "id": "here:pds:place:840c22yz-cdd24b4782144d51b49444fe402d01d8",
            "place_type": 2
        }
    ],
    "timezone": "America/Los_Angeles",
    "hours": {
        "frames": [
            {
                "days": [
                    1,
                    2,
                    3,
                    4,
                    5,
                    6
                ],
                "open": [
                    {
                        "start": "1100",
                        "end": "2130"
                    }
                ]
            }
        ]
    },
    "public_rating": 2.5,
    "public_review_count": 1,
    "private_rating": 2.5,
    "private_review_count": 1,
    "last_reviewers": [
        {
            "id": 1,
            "name": "John Doe",
            "nick": "deer",
            "account_type": "public",
            "bio": "Nothing special",
            "avatar": ""
        }
    ],
    "now": {
        "is_available": true,
        "is_opened": false,
        "will_open_today": true,
        "till_close": "00:00:00",
        "till_open": "08:16:00"
    }
}
```

* **GET /places/search?q=..&rect=..** - text search in the rectangle area or with point and radius or with location. q is mandatory.
Optional:
- &rect=47.6146226,-122.3446252,47.614017,-122.343562
- &at=47.6146226,-122.3446252&radius=20
- &loc=Los Angeles

* **GET /places/:id/reviews?offset=..&limit=..** - get all ready reviews for place.
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
    ],
    "reviews": [
        {
            "id": 1,
            "creator": 1,
            "created": "2021-02-17T10:17:45Z",
            "place": {
                "id": 1
            },
            "text": "Perfect place for lunch",
            "visibility": "public",
            "stars": 5,
            "is_kitchen": false,
            "images": [
                {
                    "id": 1,
                    "url": "https://storage.googleapis.com/image-store-local-bohdan/c0meqjc2qtansgmqect0"
                }
            ],
            "likes_counter": 0,
            "liked_by_me": false,
            "comments": {
                "object_id": 0,
                "parent_id": 0,
                "items": null,
                "users": null,
                "total_count": 0,
                "limit": 0,
                "offset": 0
            },
            "feed_position": 0
        }
    ],
    "limit": 5,
    "offset": 0
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
* **PUT /follow/followers/:user_id/remove** - remove follower

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

## <a name="activation_codes"></a>
## Activation Codes

* **PUT /app/activate?code=..** - activate code

## <a name="complaint"></a>
## Complaint

* **PUT /complaint** - post complaint
```
Request:
{
    "complaint_type": 1,
    "object_id": 1,
    "reason": "spam content"
}
```
* ***complaint_type:***
 **1** - review;
 **2** - user
  
* ***object_id***: **user id** or **review id**

### Notifications

In design

## Questions

* Design for the review edit
