---
title: Survey Widget API

search: true
---

# Introduction

Welcome to the Kittn API! You can use our API to access Kittn API endpoints, which can get information on various cats, kittens, and breeds in our database.

We have language bindings in Shell, Ruby, and Python! You can view code examples in the dark area to the right, and you can switch the programming language of the examples with the tabs in the top right.

This example API documentation page was created with [Slate](https://github.com/tripit/slate). Feel free to edit it and use it as a base for your own API's documentation.

# Authentication

> To authorize, use this code:

```ruby
require 'kittn'

api = Kittn::APIClient.authorize!('meowmeowmeow')
```

```python
import kittn

api = kittn.authorize('meowmeowmeow')
```

```shell
# With shell, you can just pass the correct header with each request
curl "api_endpoint_here"
  -H "Authorization: meowmeowmeow"
```

```javascript
const kittn = require('kittn');

let api = kittn.authorize('meowmeowmeow');
```

> Make sure to replace `meowmeowmeow` with your API key.

Kittn uses API keys to allow access to the API. You can register a new Kittn API key at our [developer portal](http://example.com/developers).

Kittn expects for the API key to be included in all API requests to the server in a header that looks like the following:

`Authorization: meowmeowmeow`

<aside class="notice">
You must replace <code>meowmeowmeow</code> with your personal API key.
</aside>

#Pagination

Use Hapi pagination.


# Surveys

Methods | Path
--------- | -------
POST | /surveys
PUT | /surveys/{id}

## Generate a survey

> Example request: 

```json
{
  "parameters": {
  "length": 5,
  "bucket": "Main"
  },
  "target": {
  "id": "plan.tpondemand.com-1",
  "tpUserId": "1",
  "userCreated": "2012-04-23T18:25:43.511Z",
  "isAdministrator": true,
  "role": "Developer",
  "host": "plan.tpondemand.com",
  "hostCreated": "2012-04-23T18:25:43.511Z",
  "isPaid": true,
  "licenses": 100
  }
}
```

> Example response: 

```json
[
  {
  "id": 1,
  "questions": [
    {
    "id": 1
    },
    {
    "id": 2
    }
  ]
  }
]
```

`POST https://survey-widget.com/api/surveys`


Parameter | Default | Description
--------- | ------- | -----------
length | 5 | The number of questions in the survey
bucket | Main | The bucket from which to pick questions

## Submit answers to a survey

> Example request: 

```json
[
  {
    "id": 1,
    "wasShown": true,
    "wasAnswered": true,
    "answer": 6
  },
  {
    "id": 2,
    "wasShown": true,
    "wasAnswered": false
  }
]
```

> Example response: 

```json
{
  "wasAccepted": true
}
```

> or

```json
{
  "wasAccepted": false,
  "error": "There's no survey with this id"
}
```

`PUT https://survey-widget.com/api/surveys/{id}`

# Questions

## Available paths

Methods | Path
--------- | -------
GET, PUT | /questions
GET, POST, DELETE | /questions/{id}

## Get list of questions

>Example response

```json
[
  {
    "id": 1,
    "type": "rating",
    "text": "How likely would you be to recommend Targetprocess to a friend or colleague?",
    "from": 0,
    "to": 10,
    "isActive": true,
    "addedDate": "2012-04-23T18:25:43.511Z",
    "deletedDate": null,
    "answersLimit": 1000,
    "buckets": ["Main", "Automatic"]
  },
  {
    "id": 2,
    "type": "single choice",
    "text": "What is your primary messaging tool?",
    "options": ["Slack", "Hiphcat", "Skype", "Custom-built", "Email", "Other"],
    "isActive": true,
    "addedDate": "2012-04-23T18:25:43.511Z",
    "deletedDate": null,
    "answersLimit": null,
    "buckets": ["Main"]
  },
  {
    "id": 3,
    "type": "text",
    "text": "Tell us what you love and hate in Targetprocess",
    "isActive": true,
    "addedDate": "2012-04-23T18:25:43.511Z",
    "deletedDate": null,
    "answersLimit": null,
    "buckets": ["Main"]
  }
]
```

`GET https://survey-widget.com/api/questions`

### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
page_size | 100 | The number of questions in one page


## Get a specific question

> Example response:

```json
{
  "id": 1,
  "type": "rating",
  "text": "How likely would you be to recommend Targetprocess to a friend or colleague?",
  "from": 0,
  "to": 10,
  "isActive": true,
  "addedDate": "2012-04-23T18:25:43.511Z",
  "deletedDate": null,
  "answersLimit": 1000,
  "buckets": ["Main", "Automatic"]
}
```

`GET https://survey-widget.com/api/questions/{id}`

## Add a question

> Example request:

```json
{
  "type": "rating",
  "text": "How likely would you be to recommend Targetprocess to a friend or colleague?",
  "from": 0,
  "to": 10,
  "isActive": true,
  "answersLimit": 1000,
  "buckets": ["Main", "Automatic"]
}
```

> Example response:

```json
{
  "wasAdded": true,
  "id": 1
}
```

> or

```json
{
  "wasAdded": false,
  "error": "Wrong format"
}
```

`PUT https://survey-widget.com/api/questions`

## Edit a question

> Example request:

```json
{
  "text": "How do you rate Targetprocess?"
}
```

> Example response:

```json
{
  "wasEdited": true
}
```

> or

```json
{
  "wasEdited": false,
  "error": "There's no question with this id"
}
```

`POST https://survey-widget.com/api/questions/{id}`

## Delete a question

> Example response:

```json
{
  "wasDeleted": true
}
```

> or

```json
{
  "wasDeleted": false,
  "error": "There's no question with this id"
}
```

`POST https://survey-widget.com/api/questions/{id}`

# Buckets

Methods | Path
--------- | -------
GET, PUT | /buckets
GET, POST, DELETE | /buckets/{id}

## Get a list of buckets

> Example response:

```json
[
  {
    "id": 1,
    "name": "Main",
    "addedDate": "2012-04-23T18:25:43.511Z",
    "deletedDate": null,
    "firstQuestionId": 1,
    "lastQuestionId": 3
  },
  {
    "id": 2,
    "name": "Automatic",
    "addedDate": "2012-04-23T18:25:43.511Z",
    "deletedDate": "2015-04-23T18:25:43.511Z",
    "firstQuestionId": 3,
    "lastQuestionId": null
  }
]
```

`GET https://survey-widget.com/api/buckets`

## Get a specific bucket

> Example response:

```json
{
  "id": 1,
  "name": "Main",
  "addedDate": "2012-04-23T18:25:43.511Z",
  "deletedDate": null,
  "firstQuestionId": 1,
  "lastQuestionId": 3,
  "questions": [1, 2, 5]
}
```

`GET https://survey-widget.com/api/buckets/{id}`

## Add a bucket

> Example request:

```json
{
  "name": "Main",
  "firstQuestionId": 1,
  "lastQuestionId": 3
}
```

> Example response:

```json
{
  "wasAdded": true,
  "id": 1
}
```

> or

```json
{
  "wasAdded": false,
  "error": "Wrong format"
}
```

`PUT https://survey-widget.com/api/buckets`

## Edit a bucket

> Example request:

```json
{
  "name": "Main"
}
```

> Example response:

```json
{
  "wasEdited": true
}
```

> or

```json
{
  "wasEdited": false,
  "error": "There's no bucket with this id"
}
```

`POST https://survey-widget.com/api/buckets/{id}`

## Delete a bucket

> Example request:

```json
{
  "id": 2
}
```

> Example response:

```json
{
  "wasDeleted": true
}
```

> or

```json
{
  "wasDeleted": false,
  "error": "There's no bucket with this id"
}
```

`DELETE https://survey-widget.com/api/buckets/{id}`

# Users

Methods | Path
--------- | -------
GET | /users/{id}

## Get a specific user

> Example response:

```json
{
  "id": "plan.tpondemand.com-1",
  "userId": "1",
  "userCreated": "2012-04-23T18:25:43.511Z",
  "isAdministrator": true,
  "role": "Developer",
  "host": "plan.tpondemand.com",
  "hostCreated": "2012-04-23T18:25:43.511Z",
  "isPaid": true,
  "licenses": 100
}
```

`GET https://survey-widget.com/api/users/{id}`