---
layout: page
title: Question & Answer API Integration Guide
permalink: /satisfilabs/api-integration
---
# Table of Contents
- [Table of Contents](#table-of-contents)
- [Question & Answer API](#question--answer-api)
- [Obtaining Partner Id & Signing Secret](#obtaining-partner-id--signing-secret)
- [Available Endpoints](#available-endpoints)
  - [Post a Question](#post-a-question)
  - [Fetch an Answer](#fetch-an-answer)
  - [Response Statuses](#response-statuses)


# Question & Answer API

Knoow’s Question & Answer API allows our partners to seamlessly plugin to Knoow's location and community based Q&A ecosystem, providing the most up to date information about a given location. 

# Obtaining Partner Id & Signing Secret

In order to authenticate wth our Question & Answer API, you must provide a valid partner id as a `x-partner-id` header and a SHA256 signature of the payload body signed by a valid secret key as a `x-signature` header.

Partner Id and Signing secrets will be sent directly to your engineering team. 

# Available Endpoints
  - [Post a Question](#post-a-question)
  - [Fetch an Answer](#fetch-an-answer)


## Post a Question

To query our Knoower community, you must post a post a new question to our API by specifying the location the question should be posted at by latitude and longitude coordinates and the query text. Once the question is posted, our API will either send back a quick reply answer based on previous answers to similar questions that we've deemed to still be relevant, or push the question out to the Knoow community for someone to accept and answer.


`POST https://<API_URL>/api/v2/questions`

**Example Request**

```
curl --location --request POST 'http://test.api.knoow.co/api/v2/questions' \
--header 'x-signature: <SHA256_SIGNATURE>' \
--header 'x-partner-id: <PARTNER_ID>' \
--header 'Content-Type: application/json' \
--data-raw '{
    "latitude": "40.8038129",
    "longitude": "-73.9534146",
    "questionText": "How long is the wait to get in?"
}'
```


**Example Responses**
The responses format below is specific to the SatisfiLabs integration

```
{
    "status": "WAITING",
    "status_message": "",
    "content": {
        "content_id": 5346,
        "content_api_name": "How_long_is_the_wait_to_get_in?_knoow",
        "text": "Looking for someone to answer your question..."
    }
}
```

```
{
    "status": "COMPLETED",
    "status_message": "",
    "content": {
        "content_id": 5346,
        "content_api_name": "How_long_is_the_wait_to_get_in?_knoow",
        "text": "The estimated wait time is 15 minutes",
        "type": "video/mp4",
        "video_url": "https://d13f5ypbmv4idw.cloudfront.net/users/ec830c54-b98b-4c72-93a9-0ad4e46e98fa/uploads/3bff744b-b0a8-4721-b342-6c1c2f903faa/38C896BB-A1C3-4190-89E7-65EC7CB52CF1.mov",
        "image_url": "https://d13f5ypbmv4idw.cloudfront.net/users/ec830c54-b98b-4c72-93a9-0ad4e46e98fa/uploads/3bff744b-b0a8-4721-b342-6c1c2f903faa/preview.jpg"
    }
}
```

Response `status` meanings [Response Statuses](#response-statuses)


## Fetch an Answer

If our API provided a status of `WAITING`, or `IN_PROGRESS`, this means the question has been pushed out to the Knoow community for someone to answer and does not yet have an answer available to provide. In order to get the answer content, our this endpoint must be polled until we resolve a status of `COMPLETED` or `INCOMPLETE`, which means we have either successfully provided an answer or failed to provide an answer. The typical response time is ~<5 minutes from the time of posting the question, so the recommended polling interval is 10 seconds.

The `questionId` route parameter is the `content_id` of the [Post A Question](#post-a-question) response.


`GET https://<API_URL>/api/v1/questions/:questionId`

**Example Request**

```
curl --location --request GET 'http://test.api.knoow.co/api/v1/questions/5342' \
--header 'x-signature: <SHA256_SIGNATURE>' \
--header 'x-partner-id: <PARTNER_ID>' \
--header 'Content-Type: application/json' \
--data-raw ''
```


**Example Responses**
The responses format below is specific to the SatisfiLabs integration

```
{
    "status": "WAITING",
    "status_message": "",
    "content": {
        "content_id": 5346,
        "content_api_name": "How_long_is_the_wait_to_get_in?_knoow",
        "text": "Looking for someone to answer your question..."
    }
}
```

```
{
    "status": "COMPLETED",
    "status_message": "",
    "content": {
        "content_id": 5346,
        "content_api_name": "How_long_is_the_wait_to_get_in?_knoow",
        "text": "The estimated wait time is 15 minutes",
        "type": "video/mp4",
        "video_url": "https://d13f5ypbmv4idw.cloudfront.net/users/ec830c54-b98b-4c72-93a9-0ad4e46e98fa/uploads/3bff744b-b0a8-4721-b342-6c1c2f903faa/38C896BB-A1C3-4190-89E7-65EC7CB52CF1.mov",
        "image_url": "https://d13f5ypbmv4idw.cloudfront.net/users/ec830c54-b98b-4c72-93a9-0ad4e46e98fa/uploads/3bff744b-b0a8-4721-b342-6c1c2f903faa/preview.jpg"
    }
}
```

Response `status` meanings [Response Statuses](#response-statuses)


## Response Statuses

| Status        | Description                                                                                                       |
| ------------- | ----------------------------------------------------------------------------------------------------------------- |
| `WAITING`     | The question has been successfully posted to the Knoow community and is waiting for someone to accept and answer. |
| `IN_PROGRESS` | Someone in the Knoow community has accepted the question and is actively posting an answer.                       |
| `COMPLETED`   | The question has been answered                                                                                    |
| `INCOMPLETE`  | The question could not be answered                                                                                |




