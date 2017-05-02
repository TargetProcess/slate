---
title: Survey Widget API

search: true
---

# Authentication

Use HTTP-basic.

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
    "bucket": "Manual"
  },
  "target": {
    "id": "plan.tpondemand.com-1",
    "ip": "212.134.105.210",
    "user": {
      "userId": 1,
      "createdDate": "2012-04-23T18:25:43.511Z",
      "isAdministrator": true,
      "role": "Developer"
    },
    "account": {
      "host": "plan.tpondemand.com",
      "createdDate": "2012-04-23T18:25:43.511Z",
      "isPaid": true,
      "licenses": 100
    }
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
    "buckets": ["Manual", "Automatic"]
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
    "buckets": ["Manual"]
  },
  {
    "id": 3,
    "type": "text",
    "text": "Tell us what you love and hate in Targetprocess",
    "isActive": true,
    "addedDate": "2012-04-23T18:25:43.511Z",
    "deletedDate": null,
    "answersLimit": null,
    "buckets": ["Manual"]
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
  "buckets": ["Manual", "Automatic"]
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
  "buckets": ["Manual", "Automatic"]
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
    "name": "Manual",
    "isDefault": false,
    "isAutomatic": false,
    "addedDate": "2012-04-23T18:25:43.511Z",
    "deletedDate": null,
    "defaultSurveyLength": 5,
    "firstQuestionId": 3,
    "lastQuestionId": null,
    "autoDaysToAdapt": null,
    "autoFrequencyDays": null
  },
  {
    "id": 2,
    "name": "Automatic",
    "isDefault": true,
    "isAutomatic": true,
    "addedDate": "2012-04-23T18:25:43.511Z",
    "deletedDate": null,
    "defaultSurveyLength": 5,
    "firstQuestionId": 1,
    "lastQuestionId": 3,
    "autoDaysToAdapt": 90,
    "autoFrequencyDays": 180
  }
]
```

`GET https://survey-widget.com/api/buckets`

## Get a specific bucket

> Example response:

```json
{
  "id": 1,
  "name": "Manual",
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
  "name": "Manual",
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
  "name": "Manual"
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

# Automatic trigger

Methods | Path
--------- | -------
GET | /auto/{user.id}

## Generate the next automatic trigger date

```json
{
  "bucket": "Automatic",
  "nextDate": "2017-04-23T18:25:43.511Z"
}
```

We give users `autoDaysToAdapt` (default=90) days to adapt to the system.

Then we generate the next trigger date by adding a random number of days between 0 and `autoFrequencyDays` (default=180).

