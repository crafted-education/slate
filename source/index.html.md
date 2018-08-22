---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - shell

toc_footers:
  - <a href='mailto:matt@inscribeapp.com'>Sign Up for a Developer Credentials</a>

includes:
  - errors

search: true
---

# Introduction

Welcome to the InScribe API! You can use our API to access InScribe API endpoints. The API can be used to retrieve communities, questions, posts, and answers.

# Authentication

Please contact InScribe (matt@inscribeapp.com) to receive credentials needed to get an Authentication Token.

### HTTP Request

To get an authentication token:

`GET https://inscribe.education/oauth2/token`

### Header Parameters

Parameter | Default | Description
--------- | ------- | -----------
Authorization | none | A "Basic access authentication" authorization header. Simply a username:password encoded with Base64 and formatted within the Authorization header as "Basic BASE64ENCODEDCREDSHERE"


> To authorize, use this code:

```shell
# With shell, you can just pass the correct header with each request
curl "https://inscribe.education/oauth2/token"
  -H "Authorization: Basic BASE64TOKEN"
```


> Make sure to replace `BASE64TOKEN` with your API key.


> The above command returns json formatted as follows:

```json
{
"token_type": "bearer",
"access_token": "eyJhbGciOiJIUzI19St9zOJjDLH2p4oyGQahlkxb9ahlLu4kcXQXs..."
}
```

InScribe uses API credentials to get Authentication tokens which allow access to the API. You can request api credentials from InScribe at matt@inscribeapp.com

InScribe expects the API Token (from the authentication call) to be included in all API requests to the server in a header that looks like the following:

`Authorization: Bearer TOKENHERE`

<aside class="notice">
You must replace <code>TOKENHERE</code> with the token you received from the authentication call.
</aside>

# Communities

## Get All Communities

`GET https://inscribe.education/api/v1/communities`

### Header Parameters

Parameter | Default | Description
--------- | ------- | -----------
Authorization | none | Bearer TOKENHERE


```shell
curl "https://inscribe.education/api/v1/communities"
  -H "Authorization: Bearer TOKENHERE"
```

> The above command returns JSON structured like this:

```json
[
    {
        "id": 4795407775301632,
        "name": "American Government",
        "ownerType": "system",
        "ownerId": 1,
        "creatorId": 1,
        "organization": "crafted",
        "createdDate": "2018-06-22T08:23:57-06:00",
        "lastModified": "2018-06-22T08:23:57-06:00"
    },
    {
        "id": 6538074649526272,
        "name": "Math",
        "ownerType": "system",
        "ownerId": 1,
        "creatorId": 1,
        "organization": "crafted",
        "createdDate": "2018-06-22T08:23:57-06:00",
        "lastModified": "2018-06-22T08:23:57-06:00"
    }
]
```
This endpoint retrieves a specific community.

### HTTP Request

`GET https://inscribe.education/api/v1/communities/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the community to retrieve

## Delete a Specific Communities


```shell
curl "https://inscribe.education/api/v1/communities/6538074649526272"
  -X DELETE
  -H "Authorization: Bearer TOKENHERE"
```

> The above command returns JSON structured like this:

```json
{
        "id": 6538074649526272,
        "name": "Math",
        "ownerType": "system",
        "ownerId": 1,
        "creatorId": 1,
        "organization": "crafted",
        "createdDate": "2018-06-22T08:23:57-06:00",
        "lastModified": "2018-06-22T08:23:57-06:00"
    }
```
