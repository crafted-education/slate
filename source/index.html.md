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

# Application Authentication

Application-only authentication is a form of authentication where an application makes API requests on its own behalf, without the user context. This method is useful for getting general information (as opposed to performing actions that would require a user).

To use this method, you need to use a bearer token. You can generate a bearer token by passing your consumer key and secret through the POST oauth2/token endpoint. 

Please contact InScribe (matt@inscribeapp.com) to receive a consumer key and secret. 

## Auth Flow
The application-only auth flow follows these steps:

* An application encodes its consumer key and secret into a specially encoded set of credentials.
* An application makes a request to the POST oauth2/token endpoint to exchange these credentials for a bearer token.
* When accessing the REST API, the application uses the bearer token to authenticate.


### Note: Tokens are passwords
Keep in mind that the consumer key & secret, bearer token credentials, and the bearer token itself grant access to make requests on behalf of an application. These values should be considered as sensitive as passwords, and must not be shared or distributed to untrusted parties.

### Note: SSL required
All requests (both to obtain and use the tokens) must use HTTPS endpoints. 


## Issuing application-only requests
### Step 1: Encode consumer key and secret

1. Concatenate the encoded consumer key, a colon character ”:”, and the encoded consumer secret into a single string.
2. Base64 encode the string from the previous step.

### Step 2: Obtain a bearer token

The value calculated in step 1 must be exchanged for a bearer token by issuing a request to POST oauth2/token:

* The request must be a HTTP POST request.
* The request must include an Authorization header with the value of Basic <base64 encoded value from step 1>.
* The request must include a Content-Type header with the value of application/x-www-form-urlencoded;charset=UTF-8.

### Step 3: Authenticate API requests with the bearer token
The bearer token may be used to issue requests to API endpoints which support application-only auth. To use the bearer token, construct a normal HTTPS request and include an Authorization header with the value of Bearer <base64 bearer token value from step 2>. 

### HTTP Request

To get an authentication bearer token:

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
        "createdDate": "2018-06-22:2T08:23:57-06:00",
        "lastModified": "2018-06-22T08:23:57-06:00"
    }
]
```
This endpoint retrieves a specific community.

### HTTP Request

`GET https://inscribe.education/api/v1/communities`

### URL Parameters

Parameter | Description
--------- | -----------
none

## Get a Specific Community


```shell
curl "https://inscribe.education/api/v1/communities/6538074649526272"
  -X GET
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

### HTTP Request

`GET https://inscribe.education/api/v1/communities/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
ID | The ID of the community to retrieve

