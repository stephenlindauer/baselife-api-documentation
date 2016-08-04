# Profile Endpoints

- [My Profile](#my-profile)
- [Update my profile](#update-my-profile)
- [Set profile image](#set-profile-image)


##My Profile##

GET /api/1.0/profiles/me

Response Body
```
status: 200 OK
{
  "id": 123,
  "username": "bobsmith",
  "email": "bobsmith@example.com",
  "is_verified": false,
  "display_name": "Bob Smith",
  "image_url": "https://s3.amazonaws.com/pnnapp/media/images/reg/afba579120c3ed942f55c8ca50fe39fc.jpg",
  "first_name": "Bob",
  "last_name": "Smith",
  "rank": "PVT",
  "base": {
    "id": 1,
    "name": "Joint Base Lewis-McChord",
    "city": "Tacoma",
    "state": "WA",
    "background_photo_url": "https://s3.amazonaws.com/pnnapp/media/bases/jblm.jpeg",
    "geo_latitude": 47.062775,
    "geo_longitude": -122.572613,
    "created_user": 1,
    "group": 1
  },
  "can_post_anonymously": false,
  "content_filter": false,
  "groups": [
    {
      "id": 1,
      "name": "Joint Base Lewis-McChord",
      "parent_group": null,
      "is_subscribed_to": false
    },
    {
      "id": 2,
      "name": "101st Airborne",
      "parent_group": 1,
      "is_subscribed_to": true
    },
    {
      "id": 18,
      "name": "Global PNN",
      "parent_group": null,
      "is_subscribed_to": false
    }
  ]
}
```

##Update my profile##
This endpoint supports partial updates, so only the fields changed need to be passed. Any of the below keys can be passed alone. _Note: If adding or removing groups, pass the entire list of Group.ids._

PATCH /api/1.0/profiles/me
```
{
  "username": "bobsmith",
  "email": "bobsmith@example.com",
  "first_name": "Bob",
  "last_name": "Smith",
  "rank": "PVT",
  "base": 1,
  “groups”: [1, 2, 18],
  "password": "mY n3w P@$$word"
}
```



Response Body
```
status: 200 OK
{
  "id": 123,
  "username": "bobsmith",
  "email": "bobsmith@example.com",
  "is_verified": false,
  "display_name": "Bob Smith",
  "image_url": "https://s3.amazonaws.com/pnnapp/media/images/reg/afba579120c3ed942f55c8ca50fe39fc.jpg",
  "first_name": "Bob",
  "last_name": "Smith",
  "rank": "PVT",
  "base": {
    "id": 1,
    "name": "Joint Base Lewis-McChord",
    "city": "Tacoma",
    "state": "WA",
    "background_photo_url": "https://s3.amazonaws.com/pnnapp/media/bases/jblm.jpeg",
    "geo_latitude": 47.062775,
    "geo_longitude": -122.572613,
    "created_user": 1,
    "group": 1
  },
  "can_post_anonymously": false,
  "content_filter": false,
  "groups": [
    {
      "id": 1,
      "name": "Joint Base Lewis-McChord",
      "parent_group": null,
      "is_subscribed_to": false
    },
    {
      "id": 2,
      "name": "101st Airborne",
      "parent_group": 1,
      "is_subscribed_to": true
    },
    {
      "id": 18,
      "name": "Global PNN",
      "parent_group": null,
      "is_subscribed_to": false
    }
  ]
}
```


##Set profile image##

POST /api/1.0/profiles/me/images

Set image on request using content type of `multipart/form-data`.

Response Body
```
status: 200 OK
{
  "url":"https://s3.amazonaws.com/pnnapp/media/images/reg/afba579120c3ed942f55c8ca50fe39fc.jpg"
}
```










