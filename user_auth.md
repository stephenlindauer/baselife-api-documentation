# User Authentication

- [Using tokens](#using-tokens)
- [Request an oauth token](#request-an-oauth-token)
- [Refresh a token](#refresh-a-token)
- [Create a new user](#create-a-new-user-profile)


##Using tokens##

After receiving a token from the [/oauth/token](#request-an-oauth-token) endpoint, set the `HTTP_Authorization` header to the received `access_token`. 

```
"Authorization" : "Bearer AIXCehuD1AqO5fLiwzlXKQ2hjm7xph"
```





##Request an oauth token##

POST /oauth/token/
```
{
  “username”: “_my_username_”,
  “password”: “_my_password_”,
  “grant_type”: “password”
  “client_id”: “_client_id_”
}
```

Response Body
```
status: 200 OK
{
  "access_token": "fgsDtRzNfKsPJtxRZImlBIxcmM9w10",
  "token_type": “Bearer",
  "expires_in": 1209600,
  "refresh_token": “AIXCehuD1AqO5fLiwzlXKQ2hjm7xph”,
  "scope": "read write groups"
}
```

Error Response Body
```
status: 401 Unauthorized
{
  "error_description": "Invalid credentials given.",
   "error": "invalid_grant"
}
```

_Notes: The token will be valid for 14 days. Tokens must be refreshed after they expire. See next section._


##Refresh a token##
POST /oauth/token/
```
{
  “refresh_token”: “AIXCehuD1AqO5fLiwzlXKQ2hjm7xph”,
  “grant_type”: “refresh_token”
  “client_id”: “_client_id_”
}
```

Response Body
```
status: 200 OK
{
  "access_token": "4WTb2KlzLgXfzkLBNM6HOxruDoPTjl",
  "token_type": "Bearer",
  "expires_in": 1209600,
  "refresh_token": "0GkPufhtC3Q6s7UA49ZXuUXOtnUEpA",
  "scope": "read write groups"
}
```

Error Response Body
```
status: 401 Unauthorized
{
  "error": "invalid_grant"
}
```

##Create a new user (profile)##

POST api/1.0/profiles
```
{
  "username": "bobsmith",
  "first_name": "Bob",
  "last_name": "Smith",
  "rank": "PVT",
  "email": "bobsmith@example.com",
  "password": "This 1s my p@ssw0rd"
}
```
_Note: Rank must be a 3 character long string._

Response Body
```
status: 200 OK
{
  "id": 123,
  "username": "bobsmith",
  "is_verified": false,
  "display_name": "Bob Smith",
  "image_url": "",
  "first_name": "Bob",
  "last_name": "Smith",
  "rank": "PVT"
}
```

Error Response Body
```
status: 409 Unauthorized
{
  "error_description": "Username is already taken.",
  "error": "username_taken"
}
```

_Notes: An additional request will need to be made in order to fetch an oauth token. See [Request an oauth token](#request-an-oauth-token)._



