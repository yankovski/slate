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
  "youtube_client_id": "9827364793138-rjckfasfsadcqj9kgv2b0jjuragoo.apps.googleusercontent.com",
  "soundcloud_client_id": "9akjsh3a6c1f7e901298347dd11f5341bf"
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

## Index

List the currently-connected providers for the logged in user (baed on token).

`GET /providers`

```shell
curl "/providers"
  -H "Authorization: mysupersecrettokenhere"
```

> The above command returns status 200 and structure like this if successful

```json
[
  {
    "provider": "soundcloud",
    "created_at": "2016-06-02T21:44:07.029Z",
    "updated_at": "2016-06-03T01:53:36.063Z"
  },
  {
    "provider": "spotify",
    "created_at": "2016-05-23T03:37:57.195Z",
    "updated_at": "2016-05-24T22:07:24.943Z"
  }
]
```

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

## SoundCloud

`POST /providers/soundcloud`

```shell
curl "/providers/soundcloud"
  -H "Authorization: mysupersecrettokenhere"
  -d code="b123llkj1h23ljgfdsd5ce4e80cd4906"
  -d callback_uri="https://www.example.com/soundcloud/callback"
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

### URL Parameters

Parameter | Description
--------- | -----------
code | Authorization code from soundcloud, to be exchanged for permanent access_token.
callback_uri | Original uri sent to soundcloud when requesting code.

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
`facebook`, `youtube`, `spotify`, `soundcloud`... See full list from `GET /providers` call.

# Lists

Playce supports many providers and many types of lists. Favorites, Playlists, Channels, Albums, etc...
In order to pull multiple lists from the API and display those in a unified list, we abstract everything into a single 'list' concept. Playce lists have a 'type' attribute, which can be used to identify the list type.

## Index

This returns all the lists for the current user.

`GET /lists`

### URL Parameters

Parameter | Description
--------- | -----------
type | Either `album` or `playlist`

```shell
curl "/lists?type=playlist"
  -H "Authorization: mysupersecrettokenhere"
```

> The above command returns status 200 and structure like this if successful

```json
{
  "items": [
    {
      "id": 29,
      "name": "Discover Weekly",
      "provider": "spotify",
      "external_id": "2e1FY03Oz92Tr3ll3uOFk5",
      "external_type": "playlist",
      "image_url": "https://u.scdn.co/images/pl/default/540efd75f5fc93c74787a375c39e4597c0975be8",
      "num_items": 30
    },
    {
      "id": 30,
      "name": "G-DJ(merengue)",
      "provider": "spotify",
      "external_id": "4kD5uFCn1x7ueKHtD5qQd3",
      "external_type": "playlist",
      "image_url": "https://i.scdn.co/image/f371f5617776d602b510c2ec614d1d1ee0bf16f1",
      "num_items": 3
    },
    {
      "id": 31,
      "name": "G-Junior(salsa)",
      "provider": "spotify",
      "external_id": "05FPlTIeC6njLa9lfH0tKr",
      "external_type": "playlist",
      "image_url": "https://mosaic.scdn.co/640/a1481a34d9a89fffc4f74ba2c10dd180349c6372e1186628b709496bf6c7933c459282c2c4151bd25a8ec98a8f7792eb15d30d3f5ff0155fa8d85ff7e83b19e81858bea51043db57420365ebe1a2cb53",
      "num_items": 101
    },
    {
      "id": 39,
      "name": "Liked from Radio",
      "provider": "spotify",
      "external_id": "3Osh7qElUCZmPQgRnIEFCo",
      "external_type": "playlist",
      "image_url": "https://mosaic.scdn.co/640/2c38c0544fd880c6c9622210d299a598c52f3bce38e99691ebdf3772177e3a5d3a4c51935cdeebb0895a3dc15ea1b8fb3143c080aea55c96e36fed323545ace7c23112471d43d2144d048e7028aded56",
      "num_items": 7
    },
    {
      "id": 40,
      "name": "Nothing But The Dance Party (feat Duke Dumont, Zedd, Swedish House Mafia, Avicii)",
      "provider": "spotify",
      "external_id": "6Uek2C5VrJ8abYXne8YrdI",
      "external_type": "playlist",
      "image_url": "https://u.scdn.co/images/pl/default/e17f4a128fca354bd6cbc33a661b54db4d4a0392",
      "num_items": 71
    }
  ]
}
```

## Tracks

This returns all the tracks for the list.

`GET /lists/1/tracks`

```shell
curl "/lists/1/tracks"
  -H "Authorization: mysupersecrettokenhere"
```

> The above command returns status 200 and structure like this if successful

```json
{
  "items": [
    {
      "id": 35,
      "external_id": "2c6LUS1MUVUSJwWToVGmG4",
      "name": "Idilio",
      "external_type": "track",
      "image_url": null,
      "duration_ms": 310226,
      "popularity": 4,
      "created_at": "2016-06-21T04:32:49.047Z",
      "updated_at": "2016-06-21T04:32:49.047Z"
    },
    {
      "id": 36,
      "external_id": "1LzycAdNif6ibMUZfG0431",
      "name": "Vivir lo nuestro",
      "external_type": "track",
      "image_url": null,
      "duration_ms": 367213,
      "popularity": 52,
      "created_at": "2016-06-21T04:32:49.050Z",
      "updated_at": "2016-06-21T04:32:49.050Z"
    }
  ]
}
```

# Artists

## Index

This returns all the artists for the current user.

`GET /artists`

```shell
curl "/artists"
  -H "Authorization: mysupersecrettokenhere"
```

> The above command returns status 200 and structure like this if successful

```json
{
  "items": [
    {
      "id": 2,
      "name": "Armin van Buuren",
      "provider": "spotify",
      "external_id": "0SfsnGyD8FpIN4U4WCkBZ5",
      "image_url": "https://i.scdn.co/image/6204e504f408d6cf9822a70661609e4fac4cbe81",
      "popularity": 72
    }
  ]
}
```

## Tracks

This returns all the tracks for the artist.

`GET /artists/1/tracks`

```shell
curl "/artists/1/tracks"
  -H "Authorization: mysupersecrettokenhere"
```

> The above command returns status 200 and structure like this if successful

```json
{
  "items": [
    {
      "id": 57945,
      "external_id": "3HXH8MQqmYyMXizFkBPJNi",
      "name": "A State Of Trance (ASOT 774) - Intro",
      "external_type": "track",
      "provider": "spotify",
      "image_url": null,
      "duration_ms": 83625,
      "popularity": null,
      "created_at": "2016-08-03T04:47:02.700Z",
      "updated_at": "2016-08-03T04:47:02.700Z"
    },
    {
      "id": 57947,
      "external_id": "3WWVRnjgQZPsM0t2b0sYLQ",
      "name": "A State Of Trance (ASOT 774) - Coming Up, Pt. 1",
      "external_type": "track",
      "provider": "spotify",
      "image_url": null,
      "duration_ms": 44665,
      "popularity": null,
      "created_at": "2016-08-03T04:47:32.655Z",
      "updated_at": "2016-08-03T04:47:32.655Z"
    },
    {
      "id": 57949,
      "external_id": "2qZObFY6e0pMbLDyYnGAtM",
      "name": "Heading Up High (ASOT 774) - Swanky Tunes Remix",
      "external_type": "track",
      "provider": "spotify",
      "image_url": null,
      "duration_ms": 207422,
      "popularity": null,
      "created_at": "2016-08-03T04:47:44.275Z",
      "updated_at": "2016-08-03T04:47:44.275Z"
    }
  ]
}
```