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
  "soundcloud_client_id": "9akjsh3a6c1f7e901298347dd11f5341bf",
  "deezer_client_id": 798471
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

## Deezer

`POST /providers/deezer`

```shell
curl "/providers/deezer"
  -H "Authorization: mysupersecrettokenhere"
  -d code="fkajshfdjkj2h3k1jaksjdn"
```

> The above command returns status 200 when connection is successful

```json
```

> The above command returns JSON structured like this when invalid:

```json
{
  "wrong code": null
}
```

### URL Parameters

Parameter | Description
--------- | -----------
code | Authorization code from soundcloud, to be exchanged for permanent access_token.

## Deezer via token

This lets the client obtain a token directly from Deezer and give it to the backend to be used. The access token is assumed to be never expire.

`POST /providers/deezer_token`

```shell
curl "/providers/deezer_token"
  -H "Authorization: mysupersecrettokenhere"
  -d access_token="fkajshfdjkj2h3k1jaksjdnasdhaksdhakjsdh312ajskd2"
```

> The above command returns status 200 when connection is successful

```json
```

### URL Parameters

Parameter | Description
--------- | -----------
access_token | A permanent access_token which was obtained from deezer.


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
curl "/lists?type=album"
  -H "Authorization: mysupersecrettokenhere"
```

> The above command returns status 200 and structure like this if successful

```json
{
  "items": [
    {
      "id": 14364,
      "name": "Let It Be (Remastered)",
      "provider": "spotify",
      "external_id": "0jTGHV5xqHPvEcwL8f6YU5",
      "external_type": "album",
      "image_url": "https://i.scdn.co/image/b3651a85f2bca826b38194c51d09cd7b068aa3ab",
      "num_items": null,
      "artists": [
        {
          "id": 1589,
          "name": "The Beatles",
          "provider": "spotify",
          "external_id": "3WrFJ7ztbogyGnTHbHJFl2",
          "image_url": "https://i.scdn.co/image/934c57df9fbdbbaa5e93b55994a4cb9571fd2085",
          "popularity": 84
        }
      ]
    },
    {
      "id": 14365,
      "name": "Cheap Thrills (Remixes)",
      "provider": "spotify",
      "external_id": "2NOa4do0Z6z3C6Zk6VniHb",
      "external_type": "album",
      "image_url": "https://i.scdn.co/image/f8f863775235350f2e1dae83025cc484d9eabcd5",
      "num_items": null,
      "artists": [
        {
          "id": 1590,
          "name": "Sia",
          "provider": "spotify",
          "external_id": "5WUlDfRSoLAfcVSX1WnrxN",
          "image_url": null,
          "popularity": null
        }
      ]
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

> The above command returns status 200 and a list of `Track` objects if successful


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

> The above command returns status 200 and a list of `Track` objects if successful


# Tracks

## Track Object

```json
{
  "items": [
    {
      "id": 75426,
      "external_id": "0eRyOunOVBChlXxIvqwOxH",
      "name": "Dig It - Remastered",
      "external_type": "track",
      "provider": "spotify",
      "image_url": "https://i.scdn.co/image/b3651a85f2bca826b38194c51d09cd7b068aa3ab",
      "duration_ms": 50466,
      "popularity": null,
      "playback_info": "spotify:track:0eRyOunOVBChlXxIvqwOxH",
      "preview_info": "https://p.scdn.co/mp3-preview/7fcb17fdd273bca3c37661e8f0d30446b9f87a31",
      "created_at": "2016-10-03T02:23:53.569Z",
      "updated_at": "2016-10-03T14:09:28.190Z",
      "artists": [
        {
          "id": 5918,
          "name": "The Beatles",
          "provider": "spotify",
          "external_id": "3WrFJ7ztbogyGnTHbHJFl2",
          "image_url": "https://i.scdn.co/image/934c57df9fbdbbaa5e93b55994a4cb9571fd2085",
          "popularity": null
        }
      ]
    },
    {
      "id": 75423,
      "external_id": "4OUmlC67FoPLvQNuE5C7kF",
      "name": "Dig A Pony - Remastered",
      "external_type": "track",
      "provider": "spotify",
      "image_url": "https://i.scdn.co/image/b3651a85f2bca826b38194c51d09cd7b068aa3ab",
      "duration_ms": 235000,
      "popularity": null,
      "playback_info": "spotify:track:4OUmlC67FoPLvQNuE5C7kF",
      "preview_info": "https://p.scdn.co/mp3-preview/033f1c9d697b69749a63621bdc8a93a897a1d394",
      "created_at": "2016-10-03T02:23:53.464Z",
      "updated_at": "2016-10-03T14:09:28.084Z",
      "artists": [
        {
          "id": 5918,
          "name": "The Beatles",
          "provider": "spotify",
          "external_id": "3WrFJ7ztbogyGnTHbHJFl2",
          "image_url": "https://i.scdn.co/image/934c57df9fbdbbaa5e93b55994a4cb9571fd2085",
          "popularity": null
        }
      ]
    },
    {
      "id": 75421,
      "external_id": "1A8DpCIimQB9bOunU43ezj",
      "name": "Cheap Thrills - John \"J-C\" Carr Remix",
      "external_type": "track",
      "provider": "spotify",
      "image_url": "https://i.scdn.co/image/f8f863775235350f2e1dae83025cc484d9eabcd5",
      "duration_ms": 316026,
      "popularity": null,
      "playback_info": "spotify:track:1A8DpCIimQB9bOunU43ezj",
      "preview_info": "https://p.scdn.co/mp3-preview/3f631c590c6ec79c6e2d7a17d5dba9e7fcd4fd0c",
      "created_at": "2016-10-03T02:23:53.365Z",
      "updated_at": "2016-10-03T14:09:14.674Z",
      "artists": [
        {
          "id": 5917,
          "name": "John J C Carr",
          "provider": "spotify",
          "external_id": "5gGoa8MFFvawLMeZU5GFB2",
          "image_url": null,
          "popularity": null
        },
        {
          "id": 5908,
          "name": "Sia",
          "provider": "spotify",
          "external_id": "5WUlDfRSoLAfcVSX1WnrxN",
          "image_url": null,
          "popularity": null
        }
      ]
    }
  ]
}
```

## Index

This returns all the tracks for the user.

`GET /tracks`

```shell
curl "/tracks"
  -H "Authorization: mysupersecrettokenhere"
```

> The above command returns status 200 and a list of `Track` objects if successful

# Search

Search is the same accross all providers

`POST /search`

```shell
curl "/search"
  -H "Authorization: mysupersecrettokenhere"
  -d provider="spotify"
  -d section="artist"
  -d query="beatles"
  -d limit="3"
```

> The above command returns status 200 and structure like this if successful

```json
{
  "items": {
    "total": 2397,
    "prev_page": 0,
    "next_page": 3,
    "items": [
      {
        "name": "Beatles",
        "external_id": "4Fjpu1v8bB3gBwutekOGRP",
        "num_items": 79,
        "external_type": "playlist",
        "image_url": "https://i.scdn.co/image/9ecfdf528562cae879748b73bd81b64dfa3d5704",
        "owner": "1218940119"
      },
      {
        "name": "beatles",
        "external_id": "2mxs6Dos203iTEa3pnoKTu",
        "num_items": 186,
        "external_type": "playlist",
        "image_url": "https://mosaic.scdn.co/640/2782d94528b449fb6910300cc8c8f93ab8cc7a8db3651a85f2bca826b38194c51d09cd7b068aa3abc429243cd056974175abe72a3142d3dccffc166a5efcba83e06ce03ca843b459a4189f861ddc5f23",
        "owner": "jack_hall_"
      },
      {
        "name": "Beatles Greatest Hits",
        "external_id": "1FbXE0DKfcNlIRexSEHcs8",
        "num_items": 50,
        "external_type": "playlist",
        "image_url": "https://mosaic.scdn.co/640/9cab76ad73ce2adbacbd118ebc632255ce7c1841809c6f28db643023d76b9cb650a8ea59689a3af20c268e33b8ea23b90b8debba71ecfe83195ed8659ecfdf528562cae879748b73bd81b64dfa3d5704",
        "owner": "beatlesplaylists"
      }
    ]
  }
}
```

### URL Parameters

Parameter | Description
--------- | -----------
provider | spotify/deezer/youtube/soundcloud
section | Possible sections vary by provider. See below.
        | spotify: track/album/artist/playlist
        | youtube: track/artist/playlist
        | soundcloud: track/artist/playlist
        | deezer: track/album/artist/playlist
query | What to search for? E.g. 'Beatles'
limit | results per page to return (best to keep low, but limits vary between providers
page | This is an optional page token (not page number). See `next_page`/`prev_page` in the results

# Search Details

Search drill down for retrieving tracks

`GET /search/details`

```shell
curl "/search/details?provider=youtube&section=artist&external_id=UC4dqLAF7yT-_DqeYisQ001w&limit=3"
  -H "Authorization: mysupersecrettokenhere"
```

> The above command returns status 200 and structure like this if successful

```json
{
  "items": {
    "total": 50,
    "prev_page": null,
    "next_page": "CAMQAA",
    "items": [
      {
        "name": "The Beatles - Eleanor Rigby (From \"Yellow Submarine\")",
        "external_id": "HuS5NuXRb5Y",
        "external_type": "video",
        "image_url": "https://i.ytimg.com/vi/HuS5NuXRb5Y/hqdefault.jpg",
        "artists": [
          {
            "external_id": "UC4dqLAF7yT-_DqeYisQ001w",
            "name": "The Beatles - Eleanor Rigby (From \"Yellow Submarine\")",
            "image_url": "https://i.ytimg.com/vi/HuS5NuXRb5Y/hqdefault.jpg"
          }
        ]
      },
      {
        "name": "The Beatles - We Can Work it Out",
        "external_id": "Qyclqo_AV2M",
        "external_type": "video",
        "image_url": "https://i.ytimg.com/vi/Qyclqo_AV2M/hqdefault.jpg",
        "artists": [
          {
            "external_id": "UC4dqLAF7yT-_DqeYisQ001w",
            "name": "The Beatles - We Can Work it Out",
            "image_url": "https://i.ytimg.com/vi/Qyclqo_AV2M/hqdefault.jpg"
          }
        ]
      },
      {
        "name": "The Beatles - Penny Lane",
        "external_id": "S-rB0pHI9fU",
        "external_type": "video",
        "image_url": "https://i.ytimg.com/vi/S-rB0pHI9fU/hqdefault.jpg",
        "artists": [
          {
            "external_id": "UC4dqLAF7yT-_DqeYisQ001w",
            "name": "The Beatles - Penny Lane",
            "image_url": "https://i.ytimg.com/vi/S-rB0pHI9fU/hqdefault.jpg"
          }
        ]
      }
    ]
  }
}
```

### URL Parameters

Parameter | Description
--------- | -----------
provider | spotify/deezer/youtube/soundcloud
section | Possible sections vary by provider. See below.
        | spotify: album/playlist*
        | youtube: artist/playlist
        | soundcloud: artist/playlist**
        | deezer: album/playlist
external_id | The id of the `section`
limit | results per page to return (best to keep low, but limits vary between providers
page | This is an optional page token (not page number). See `next_page`/`prev_page` in the results
*playlist_owner_id is only required when retrieving spotify playlist tracks.

**The SC API doesn't always return some or all playlist items. At times these are protected by the users.

# Artist Details (Unified Artists)

Get a lot of details about a specific artist

`POST /artist_details`

```shell
curl "/search"
  -H "Authorization: mysupersecrettokenhere"
  -d provider="spotify"
  -d artist_id="43ZHCT0cAZBISjO8DG9PnE"
```

> The above command returns status 200 and structure like this if successful

```json
{
  "items": {
    "albums": [
      {
        "name": "The Wonder of You: Elvis Presley with the Royal Philharmonic Orchestra",
        "external_id": "6oWz2hJ89n9mKarg3SO9ou",
        "external_type": "album",
        "image_url": "https://i.scdn.co/image/a07ca4453d11da5c73239a936aa154238106fd68"
      },
      {
        "name": "The Wonder of You: Elvis Presley with the Royal Philharmonic Orchestra",
        "external_id": "2kTL2bkqCBKBTLzXfZPGF7",
        "external_type": "album",
        "image_url": "https://i.scdn.co/image/eb8a80a5d7f3d9adab42010c1f8d33cab4a355d1"
      }
    ],
    "top_tracks": [
      {
        "name": "Can't Help Falling in Love",
        "external_id": "44AyOl4qVkzS48vBsbNXaC",
        "external_type": "track",
        "image_url": "https://i.scdn.co/image/4295b5ff74d4f944367144acbe616b6f62d20b17",
        "duration_ms": 179773,
        "popularity": 69,
        "playback_info": "spotify:track:44AyOl4qVkzS48vBsbNXaC",
        "preview_info": "https://p.scdn.co/mp3-preview/26e409b39a2da6dc18fab61020c90be2938dc0e9?cid=14d415257d794e76949f6e4f8b8fa34b"
      },
      {
        "name": "Jailhouse Rock",
        "external_id": "4gphxUgq0JSFv2BCLhNDiE",
        "external_type": "track",
        "image_url": "https://i.scdn.co/image/31301ae33f6ec1ca0f86edec54a9a7be14c78310",
        "duration_ms": 146480,
        "popularity": 63,
        "playback_info": "spotify:track:4gphxUgq0JSFv2BCLhNDiE",
        "preview_info": "https://p.scdn.co/mp3-preview/29990f669b5328b6c40320596a2b14d8660cdb54?cid=14d415257d794e76949f6e4f8b8fa34b"
      }
    ],
    "related_artists": [
      {
        "external_id": "0JDkhL4rjiPNEp92jAgJnS",
        "image_url": "https://i.scdn.co/image/684f6d45078b404e6b82e2c84f82859035dd3e3c",
        "name": "Roy Orbison",
        "popularity": 63
      },
      {
        "external_id": "3wYyutjgII8LJVVOLrGI0D",
        "image_url": "https://i.scdn.co/image/ff6c6ecd90618f5fb0f8fbf51bf8477586c8843b",
        "name": "Buddy Holly",
        "popularity": 57
      },
      {
        "external_id": "2zyz0VJqrDXeFDIyrfVXSo",
        "image_url": "https://i.scdn.co/image/e7505a9ef8849b2686ec759082b63c37d67c6d54",
        "name": "Jerry Lee Lewis",
        "popularity": 56
      }
    ]
  }
}
```

### Sections

Provider | Sections
-------- | --------
spotify | albums, top_tracks, related_artists
soundcloud | tracks, playlists, followers
deezer | top_tracks, albums, fans, related, playlists
youtube | likes, favorites, uploads


### URL Parameters

Parameter | Description
--------- | -----------
provider | spotify/deezer/youtube/soundcloud
artist_id | external id of the artist (Provider-specific)


