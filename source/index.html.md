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

`GET https://inscribe.education/api/<organization>/v1/communities`

### Header Parameters

Parameter | Default | Description
--------- | ------- | -----------
Authorization | none | Bearer TOKENHERE

### URL Parameters

Parameter | Description
--------- | -----------
organization | Your organization name key


### Example

```shell
curl "https://inscribe.education/api/<organization>/v1/communities"
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


## Get a Specific Community

This endpoint retrieves a specific community.

### HTTP Request

`GET https://inscribe.education/api/<organization>/v1/communities/<ID>`

### URL Parameters

Parameter | Description
--------- | -----------
organization | Your organization name key
ID | The ID of the community to retrieve



### Example

```shell
curl "https://inscribe.education/api/<organization>/v1/communities/6538074649526272"
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
# User

Creates a user and adds them to the Organization and Community in the request. Additionally, will perform any default system enrollments for the user if needed. This endpoint can be safely called multiple times and the user will not be duplicated or have duplicate enrollments.

## Create and Enroll a User in a Organization and Community

`GET https://inscribe.education/api/<organization>/v1/communities/<communityid>/users`

### Header Parameters

Parameter | Default | Description
--------- | ------- | -----------
Authorization | none | Bearer TOKENHERE
Range | none | entities=&lt;range-start&gt;-&lt;range-end&gt;

### URL Parameters

Parameter | Description
--------- | -----------
organization | The short name (key) for your organization
communityid | The identifier for the community in which you are getting conversationpreviews

### POST Parameters
```
{
  "externalId": "54464651615",
  "externalIdType": "external-system-name",
  "email": "matt@inscribeapp.com",
  "screenName": "Matt",
  "realName": "Matt Self",
  "avatarUrl": "https://lh4.googleusercontent.com/-gw3Csqy36fI/AAAAAAAAAAI/AAAAAAAAABM/widlaNMKU74/photo.jpg"
}
```

### Example

```shell
curl  -d '{ "externalId": "54464651615", "externalIdType": "external-system-name", "email": "matt@inscribeapp.com", "screenName": "Matt", "realName": "Matt Self", "avatarUrl": "https://test123123.com/photo.jpg" }' -X POST "https://inscribe.education/api/crafted/v1/communities/12341234/users" -H "Content-Type: application/json" -H "Authorization: Bearer TOKENHERE"

```

> The above command returns a location header containing the url for the newly created user (with a userid):

```
Location: https://inscribe.education/api/crafted/v1/communities/12341234/users/3434542345
```


# User's Conversation Previews

Conversations contain two types: 'question' and 'sharepost'. Both are returned in generalized json structure, but the types are returned (in "conversationType") in case there is a need to treat them differently.

## Get All ConversationPreviews in a Community for a User

`GET https://inscribe.education/api/<organization>/v1/communities/<communityid>/users/<userid>/conversationpreviews`

### Header Parameters

Parameter | Default | Description
--------- | ------- | -----------
Authorization | none | Bearer TOKENHERE
Range | none | entities=&lt;range-start&gt;-&lt;range-end&gt;

### URL Parameters

Parameter | Description
--------- | -----------
organization | The short name (key) for your organization
communityid | The identifier for the community in which you are getting conversationpreviews
userid | The user identifier context in which the conversation previews are relevant
topicSlug | [optional] Querystring param. The topic(s) in which results should be filtered. If not provided, results will not be filtered by topic.
channelSlugName | [optional] Querystring param. Using the slug name of the channel the results can be filtered. If not provided, results will be limitted to channels in which the user has subscribed.

### Example

```shell
curl "https://inscribe.education/api/crafted/v1/communities/12341234/users/1324134/conversationpreviews"
  -H "Authorization: Bearer TOKENHERE"
```

> The above command returns JSON structured like this:

```json
[
    {
        "title": "How do I make changes to my FAFSA form?",
        "bodyText": "How do I make changes to my FAFSA form? I made a mistake when I submitted the FAFSA form. How do I make changes?",
        "authorName": "MS",
        "authorAvatarUrl": "https://inscribe.education/avatar/1234.jpg",
        "conversationType": "question",
        "upVotesCount": 1,
        "viewsCount": 19,
        "responsesCount" : 2,
        "datePosted": "2018-01-31T14:53:02Z",
        "dateEdited": "0001-01-01T00:00:00Z"
    },
    {
        "title": "My techniques for success in online classes",
        "bodyText": "I've found that being organized and prepared has really helped.",
        "authorName": "JG",
        "authorAvatarUrl": "https://inscribe.education/avatar/1234.jpg",
        "conversationType": "sharepost",
        "upVotesCount": 0,
        "viewsCount": 16,
        "responsesCount" : 2,
        "datePosted": "2018-01-31T15:09:34Z",
        "dateEdited": "0001-01-01T00:00:00Z"
    }
]

```



# Conversation Previews

This is a non-user specific view of conversations. Conversations contain two types: 'question' and 'sharepost'. Both are returned in generalized json structure, but the types are returned (in "conversationType") in case there is a need to treat them differently.

## Get All ConversationPreviews in a Community

`GET https://inscribe.education/api/<organization>/v1/communities/<communityid>/conversationpreviews?channelSlugName=<channelSlugName>`

### Header Parameters

Parameter | Default | Description
--------- | ------- | -----------
Authorization | none | Bearer TOKENHERE
Range | none | entities=&lt;range-start&gt;-&lt;range-end&gt;

### URL Parameters

Parameter | Description
--------- | -----------
organization | The short name (key) for your organization
communityid | The identifier for the community in which you are getting conversationpreviews
topicSlug | [optional] Querystring param. The topic(s) in which results should be filtered
channelSlugName | [optional] Querystring param. Using the slug name of the channel the results can be filtered


### Example

```shell
curl "https://inscribe.education/api/crafted/v1/communities/12341234/conversationpreviews"
  -H "Authorization: Bearer TOKENHERE"
```

> The above command returns JSON structured like this:

```json
[
    {
        "title": "How do I make changes to my FAFSA form?",
        "bodyText": "How do I make changes to my FAFSA form? I made a mistake when I submitted the FAFSA form. How do I make changes?",
        "authorName": "MS",
        "conversationType": "question",
        "upVotesCount": 1,
        "viewsCount": 19,
        "datePosted": "2018-01-31T14:53:02Z",
        "dateEdited": "0001-01-01T00:00:00Z"
    },
    {
        "title": "My techniques for success in online classes",
        "bodyText": "I've found that being organized and prepared has really helped.",
        "authorName": "JG",
        "conversationType": "sharepost",
        "upVotesCount": 0,
        "viewsCount": 16,
        "datePosted": "2018-01-31T15:09:34Z",
        "dateEdited": "0001-01-01T00:00:00Z"
    }
]

```

# Resource Previews
## Get all ResourcePreviews

This endpoint retrieves resourcepreviews in a community

### HTTP Request

`GET https://inscribe.education/api/<organization>/v1/communities/<communityid>/conversationpreviews?channelSlugName=<channelSlugName>`

### Header Parameters

Parameter | Default | Description
--------- | ------- | -----------
Authorization | none | Bearer TOKENHERE
Range | none | entities=&lt;range-start&gt;-&lt;range-end&gt;

### URL Parameters

Parameter | Description
--------- | -----------
organization | Your organization name key
ID | The ID of the community to retrieve resourcepreviews from
channelSlugName | Querystring param. Using the slug name of the channel the results can be filtered



### Example

```shell
curl "https://inscribe.education/api/crafted/v1/communities/6538074649526272/resourcepreviews"
  -X GET
  -H "Authorization: Bearer TOKENHERE"
```

> The above command returns JSON structured like this:

```json
[
    {
        "title": "Welcome to your Q&A Community",
        "previewImageURL": "",
        "authorName": "MS",
        "upVotesCount": 0,
        "viewsCount": 9,
        "datePosted": "2018-01-30T22:29:58Z",
        "dateEdited": "2018-09-05T21:00:11Z"
    },
    {
        "title": "FAFSA Overview",
        "previewImageURL": "",
        "authorName": "MS",
        "upVotesCount": 0,
        "viewsCount": 8,
        "datePosted": "2018-01-31T14:48:45Z",
        "dateEdited": "2018-09-07T19:25:08Z"
    }
]
```



# Conversation Details

Conversation Detail contain two types: 'question' and 'sharepost'. Both are returned in generalized json structure, but the types are returned (in "conversationType") in case there is a need to treat them differently.

## Get a single ConversationDetail in a Community by ID

`GET https://inscribe.education/api/<organization>/v1/communities/<communityid>/users/<userid>/conversationdetails/<conversationtype>/<conversationid>`

### Header Parameters

Parameter | Default | Description
--------- | ------- | -----------
Authorization | none | Bearer TOKENHERE
Range | none | entities=&lt;range-start&gt;-&lt;range-end&gt;

### URL Parameters

Parameter | Description
--------- | -----------
organization | The short name (key) for your organization
communityid | The identifier for the community in which you are getting conversationpreviews
userid | The user identifier context in which the conversation previews are relevant
conversationType | The type of conversation: Either question or sharepost.
conversationId | The id of the conversation to get details of.

### Example

```shell
curl "https://inscribe.education/api/pearson/v1/communities/1234/users/12351234/conversationdetails/question/"
  -H "Authorization: Bearer TOKENHERE"
```

> The above command returns JSON structured like this:

```json
{
    "title": "testing embedded content",
    "bodyHtml": "<article> <h1>testing</h1> <div> <p>here is an image:</p><p>a bullet list:</p><ul><li>list item</li><li>list item</li><li>list item</li></ul> </div> </article>",
    "authorName": "Matt S",
    "authorAvatarUrl": "https://lh6.googleusercontent.com/-v4EGvrKXWIc/AAAAAAAAAAI/AAAAAAAAKM0/8_3b6gSrbag/photo.jpg",
    "conversationType": "question",
    "conversationId": 12341234,
    "conversationViewUrl": "https://inscribe.education/main/crafted/4352/questions/2345",
    "upVotesCount": 0,
    "viewsCount": 1,
    "responsesCount": 1,
    "datePosted": "2019-06-27T18:02:13Z",
    "dateEdited": "0001-01-01T00:00:00Z",
    "conversationResponses": [
        {
            "bodyHtml": "<p>A response</p>",
            "authorName": "Matt S",
            "authorAvatarUrl": "https://lh4.googleusercontent.com/-adf/AAAAAAAAAAI/AAAAAAAAABM/asdf/photo.jpg",
            "conversationType": "answer",
            "conversationId": 7890789025342453,
            "upVotesCount": 0,
            "datePosted": "2019-06-28T20:43:12Z",
            "dateEdited": "2019-06-28T20:43:12Z"
        }
    ]
}

```

