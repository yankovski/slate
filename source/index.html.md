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

## Authenticating

`POST /api/sessions`

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


### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the user

