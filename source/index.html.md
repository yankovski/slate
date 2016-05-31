---
title: Playce API Reference

language_tabs:
  - shell

toc_footers:
  - <a href='https://github.com/gigster-eng/playce_api'>Playce API Github</a>
  - Any questions? Slack @my

includes:
  - errors

search: true
---

# Introduction

Welcome to the Playce API! API is meant to be used by the Playce iOS and Android apps.

To start only Shell examples are provided, but my plan is to extend to Ruby as well! You can view code examples in the dark area to the right.



# Authentication

> To authorize, use this code:

```shell
# With all endpoints, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: mysupersecrettokenhere"
```

Playce uses Authorization header to control access to the different routes.

Auth token (obtained by logging in) is expected to be included in most API requests to the server in a header that looks like the following:

`Authorization: mysupersecrettokenhere`

<aside class="notice">
You must replace <code>mysupersecrettokenhere</code> with your personal API key.
</aside>

# Sessions

## Environment

This endpoint returns environment-specific information that the clients may need. This is better than storing such information on the client permanently.

`GET /sessions/environment`

```shell
curl "/sessions/environment"
```

> The above command returns JSON structured like this:

```json
{
  "facebook_app_id": "1398162837618273",
  "spotify_client_id": "14d415257d794e76949f6e4f8b8fa34b",
  "youtube_client_id": "9827364793138-rjckfasfsadcqj9kgv2b0jjuragoo.apps.googleusercontent.com"
}
```

## Authenticating

`POST /sessions`

```shell
curl "/sessions"
  -d session[email]="user@example.com"
  -d session[password]="letmein"
```

> The above command returns JSON structured like this:

```json
{
  "id": 3,
  "email": "user@example.com",
  "auth_token": "s8UpRifGis6Lwq22gxnL",
  "created_at": "2016-05-19T03:06:33.688Z",
  "updated_at": "2016-05-19T17:12:36.817Z"
}
```

> The above command returns JSON structured like this when invalid:

```json
{
  "errors": "Invalid email or password"
}
```


## Signing out

`DELELE /sessions/<id>`


```shell
curl "/sessions/<id>"
  -H "Authorization: mysupersecrettokenhere"
  -X DELETE
```

> The above command returns status 204 when the user is found and signed out

## Facebook

This endpoint signs in an existing user with Facebook or creates a user and then signs them in.

`POST /sessions/facebook`

```shell
curl "/sessions/facebook"
  -d facebook_token="annoyinglylongfacebooktokenhere"
```

> The above command returns JSON structured like this:

```json
{
  "id": 13,
  "email": "wldjqss_wongman_1351268732@tfbnw.net",
  "last_sign_in_at": "2016-05-20T03:27:55.106Z",
  "auth_token": "cJjkwDaDF6LYsudBybzz",
  "created_at": "2016-05-20T03:14:39.894Z",
  "updated_at": "2016-05-20T03:27:55.133Z"
}
```

> The above command returns JSON structured like this when invalid:

```json
{
  "errors": "Invalid Facebook token"
}
```

> Or like this when email is not shared:

```json
{
  "error": "Validation failed: Email can't be blank"
}
```

# Users

## Index
This is the root api welcome message

`GET /users`

```shell
curl "/users"
  -H "Authorization: mysupersecrettokenhere"
```

> The above command returns JSON structured like this when correct:

```json
{
  "message": "Hello"
}
```

## Show

`GET /users/<id>`

```shell
curl "/users/<id>"
  -H "Authorization: mysupersecrettokenhere"
```

> The above command returns JSON structured like this when correct:

```json
{
  "id": 1,
  "email": "user1@example.com",
  "last_sign_in_at": "2016-05-19T21:40:22.532Z",
  "auth_token": "mysupersecrettokenhere",
  "created_at": "2016-05-19T03:17:23.514Z",
  "updated_at": "2016-05-19T21:40:22.573Z"
}
```

## Create
Creating a user

`POST /users`

```shell
curl "/users"
  -d user[email]="user555@example.com"
  -d user[password]="password"
  -d user[password_confirmation]="password"
```

> The above command returns JSON structured like this when correct:

```json
{
  "id": 11,
  "email": "user10@example.com",
  "last_sign_in_at": null,
  "auth_token": "a5LsvBLgPe4RufgXyHcm",
  "created_at": "2016-05-19T21:43:48.622Z",
  "updated_at": "2016-05-19T21:43:48.622Z"
}
```

## Update
Updating a user

`PATCH /users/<id>`

```shell
curl "/users/<id>"
  -H "Authorization: somerandomtoken"
  -d user[email]="newemail@example.com"
  --request PATCH
```

> The above command returns JSON structured like this when correct:

```json
{
  "id": 11,
  "email": "newemail@example.com",
  "last_sign_in_at": null,
  "auth_token": "a5LsvBLgPe4RufgXyHcm",
  "created_at": "2016-05-19T21:43:48.622Z",
  "updated_at": "2016-05-19T21:50:47.472Z"
}
```

## Destroy

`DELELE /users/<id>`


```shell
curl "/users/<id>"
  -H "Authorization: mysupersecrettokenhere"
  -X DELETE
```

> The above command returns status 204 when the user is found and destroyed

# Providers

This is how the API supports connecting to different external providers.

## Spotify

`POST /providers/spotify`

```shell
curl "/providers/spotify"
  -H "Authorization: mysupersecrettokenhere"
  -d code="AQDUvJsv0CBnDKz3HBfKkXablig"
  -d callback_uri="https://www.example.com/spotify/callback"
```

> The above command returns status 200 when connection is successful

```json
```

> The above command returns JSON structured like this when invalid:

```json
{
  "error": "invalid_grant",
  "error_description": "Authorization code expired"
}
```

```json
{
  "error": "Failed to open TCP connection to accounts.spotify.com:443 (getaddrinfo: nodename nor servname provided, or not known)"
}
```

### URL Parameters

Parameter | Description
--------- | -----------
code | Authorization code from spotify, to be exchanged for permanent access_token.
callback_uri | Original uri sent to spotify when requesting code.


## YouTube

`POST /providers/youtube`

```shell
curl "/providers/youtube"
  -H "Authorization: mysupersecrettokenhere"
  -d code="lkfjpqlDAcCKddAmcmmskAkjahdsqwwqe"
  -d callback_uri="https://www.example.com/youtube/callback"
```

> The above command returns status 200 when connection is successful

```json
```

> The above command returns JSON structured like this when invalid:

```json
{
  "error": "invalid_grant",
  "error_description": "Code was already redeemed."
}
```

### URL Parameters

Parameter | Description
--------- | -----------
code | Authorization code from youtube, to be exchanged for permanent access_token.
callback_uri | Original uri sent to youtube when requesting code.

## Destroy

`DELETE /providers/<provider>`

```shell
curl "/providers/<provider>"
  -H "Authorization: mysupersecrettokenhere"
  -X DELETE
```

> The above command returns status 200 and structure like this if successful

```json
{
  "result": true
}
```

> The above command returns status 401 and structure like this if unsuccessful

```json
{
  "result": false
}
```

### Supported providers
`facebook`, `youtube`, `spotify`
