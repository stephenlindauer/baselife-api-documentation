# Thread/Post Endpoints

- [Threads](#threads)
- [Thread Details](#thread-details)
- [Reply to Thread](#reply-to-thread)
- [Include Image(s) in Replies/Threads](#include-images-in-repliesthread)
- [Create new Thread](#create-new-thread)


##Threads##

GET /api/1.0/threads

Response Body
```
status: 200 OK
{
  "meta": {
    "count": 17,
    "page": 1
  },
  "threads": [
    {
      "id": 55,
      "posted_by_profile": 123,
      "posted_anonymously": false,
      "group": 2,
      "last_reply": {
        "id": 140,
        "created_date": "2016-07-20 01:04:31",
        "posted_by_profile": 123,
        "posted_anonymously": false,
        "thread": 55,
        "text": "Here's a picture of a smilie, just because.",
        "media_objects": [
          {
            "id": 26,
            "url": "https://s3.amazonaws.com/pnnapp/media/images/reg/2aea994df2e46eb74c66ed258b5d70f9.jpg",
            "media_type": "image"
          }
        ],
        "media": [26]
      },
      "first_post": {
        "id": 130,
        "created_date": "2016-07-16 21:29:24",
        "posted_by_profile": 123,
        "posted_anonymously": false,
        "thread": 55,
        "text": "Hello, this is the thread title.",
        "media_objects": [],
        "media": []
      },
      "number_of_replies": 6
    },
    ...
  ],
  "profiles": [
    {
      "id": 123,
      "username": "bobsmith",
      "is_verified": false,
      "display_name": "Bob Smith",
      "image_url": "https://s3.amazonaws.com/pnnapp/media/images/reg/afba579120c3ed942f55c8ca50fe39fc.jpg",
      "first_name": "Bob",
      "last_name": "Smith",
      "rank": "PVT"
    },
    ...
  ]
}
```

_Notes:_
- _Times are all stored in UTC._
- _The Thread object is really just a wrapper for a group of Posts. The `first_post.text` is where the Thread's title comes from. The optional `last_reply` is what shows underneath the title if someone has responded._
- _To make sure all required Profile information is available, a list of any used Profile objects are returned in the serialization._
- _`posted_anonymously` and `posted_by_profile`: In a future version, users will be able to post anonymously. At a later date, `posted_by_profile` may be `null` and `posted_anonymously` will be `true`. All versions of the app should be built to support this now._
- _Pagination: To fetch the next page of results, include a GET parameter `page=<page_index>`. Page indexs start at 1._
- _Filter by Group: Include a GET parameter `group=<group_id>` to view only threads for a particular group. Leaving off this parameter returns threads for all groups the user has joined._


##Thread Details##

GET /api/1.0/threads/\<thread_id\>

Response Body
```
status: 200 OK
{
  "thread": {
    "id": 55,
    "created_date": "2016-03-09 05:15:32",
    "posted_by_profile": 123,
    "group": 3,
    "posts": [
      {
        "id": 130,
        "created_date": "2016-03-09 05:15:55",
        "posted_by_profile": 123,
        "posted_anonymously": false,
        "thread": 55,
        "text": "Hello, this is the thread title.",
        "media_objects": [],
        "media": []
      },
      {
        "id": 131,
        "created_date": "2016-03-09 05:23:03",
        "posted_by_profile": 2,
        "posted_anonymously": false,
        "thread": 2,
        "text": "This is the first reply.",
        "media_objects": [],
        "media": []
      },
      {
        "id": 132,
        "created_date": "2016-03-12 03:23:19",
        "posted_by_profile": 3,
        "posted_anonymously": false,
        "thread": 2,
        "text": "This is the second reply.",
        "media_objects": [],
        "media": []
      },
      {
        "id": 133,
        "created_date": "2016-07-20 01:04:31",
        "posted_by_profile": 123,
        "posted_anonymously": false,
        "thread": 55,
        "text": "Here's a picture of a smilie, just because.",
        "media_objects": [
          {
            "id": 26,
            "url": "https://s3.amazonaws.com/pnnapp/media/images/reg/2aea994df2e46eb74c66ed258b5d70f9.jpg",
            "media_type": "image"
          }
        ],
        "media": [26]
      },
    ]
    "number_of_replies": 3
  },
  "profiles": [
    {
      "id": 123,
      "username": "bobsmith",
      "is_verified": false,
      "display_name": "Bob Smith",
      "image_url": "https://s3.amazonaws.com/pnnapp/media/images/reg/afba579120c3ed942f55c8ca50fe39fc.jpg",
      "first_name": "Bob",
      "last_name": "Smith",
      "rank": "PVT"
    },
    ...
  ]
}
```




##Reply to Thread##

POST /api/1.0/posts

```
{
  "posted_by_profile": 1,
  "posted_anonymously": false,
  "thread": 55,
  "text": "This is my reply.",
  "media":[]
}
```

Response Body
```
status: 201 Created
{
  "id": 150,
  "created_date": "2016-08-03 21:49:27",
  "posted_by_profile": 123,
  "posted_anonymously": false,
  "thread": 55,
  "text": "This is my reply.",
  "media_objects": [],
  "media": []
}
```



##Include Image(s) in Replies/Thread##

To reply to a thread with any number of images:

1. Upload each image with `POST /api/1.0/media` and hold onto the `id`.
2. Create reply as seen in [Reply to Thread](#reply-to-thread) and include the associated Media.ids in the `media` element.

POST /api/1.0/media

Set image on request using content type of `multipart/form-data`.

Response Body
```
status: 200 OK
{
  "id": 37,
  "url":"https://s3.amazonaws.com/pnnapp/media/images/reg/afba579120c3ed942f55c8ca50fe39fc.jpg",
  "media_type": "image"
}
```

_Note: `media_type` will always return `image` in the 1.0 API. This will change in the future._




##Create new Thread##

To create a thread:

1. Create the Thread object with the below endpoint (`POST /api/1.0/threads`) and hold onto the `id`.
2. Create a Post object with the [POST /api/1.0/posts](#reply-to-thread) endpoint and include the Thread.id.

POST /api/1.0/threads

```
{
  "posted_by_profile": 123,
  "posted_anonymously": false,
  "group":1
}
```

Response Body
```
status: 201 Created
{
  "id": 64,
  "posted_by_profile": 123,
  "posted_anonymously": false,
  "group": 1,
  "last_reply": null,
  "first_post": null,
  "number_of_replies": 0
}
```

