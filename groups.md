#Groups Endpoints#

- [Groups](#groups)
- [My Groups](#my-groups)
- [Subscribe to Group](#subscribe-to-group)
- [Unsubscribe from Group](#unsubscribe-from-group)
- [Create new Group](#create-new-group)


##Groups##

GET /api/1.0/groups

Response Body
```
[
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
  },
  ...
]
```

**Available Filters:**

* Base: `?base=<base_id>` Returns all groups that fall under a base.
* Search: `?search=<search_string>` Returns all groups where Group.name.contains(`search_string`).



##My Groups##

GET /api/1.0/groups/me

Response Body
```
[
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
  },
  ...
]
```


##Subscribe to Group##

POST /api/1.0/groups/\<group_id\>/subscribe

Response Body
```
{
  "id": 18,
  "name": "Global PNN",
  "parent_group": null,
  "is_subscribed_to": 1
}
```

##Unsubscribe from Group##

DELETE /api/1.0/groups/\<group_id\>/subscribe

Response Body
```
{
  "id": 18,
  "name": "Global PNN",
  "parent_group": null,
  "is_subscribed_to": 0
}
```


##Create new Group##

POST /api/1.0/groups

```
{
  "name": "My New Group",
  "parent_group": 1
}
```

Response Body
```
{
  "id": 539,
  "name": "Test Group",
  "parent_group": 1,
  "is_subscribed_to": false
}
```

