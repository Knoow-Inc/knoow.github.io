---
layout: page
title: Question & Answer API Integration Guide
permalink: /api-integration
---
# Table of Contents
- [Table of Contents](#table-of-contents)
- [Question & Answer API](#question--answer-api)
- [Obtaining Partner Id & Signing Secret](#obtaining-partner-id--signing-secret)
- [Available Endpoints](#available-endpoints)
  - [Post a Question](#post-a-question)
  - [Fetch an Answer](#fetch-an-answer)


# Question & Answer API

Knoow’s Question & Answer API allows our partners to seamlessly plugin to Knoow's location and community based Q&A ecosystem, providing the most up to date information about a given location. 

# Obtaining Partner Id & Signing Secret

In order to authenticate wth our Question & Answer API, you must provide a valid partner id as a `x-partner-id` header and a SHA256 signature of the payload body signed by a valid secret key as a `x-signature` header.

Partner Id and Signing secrets will be sent directly to your engineering team. 

**Example SHA256 Signature**
```
const secret = 'D9WTHq9YfvfiJLZsfgOCARt'
const requestData = {
      latitude: '40.8038129',
      longitude: '-73.9534146',
      questionText: 'How long is the wait to get in?',
    };
    const dataSignature = JSON.stringify(requestData).trim();
    const signature = crypto.createHmac('sha256', secret).update(data).digest('hex');
    console.log(signature)
    // Should produce d77ad11e143759e35d82639a322d84b941c1b52b9a9eda5bfa24214dd398a1f4
```

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

```
{
    "question": {
        "userId": 2,
        "question": "How long is the wait to get in?",
        "latitude": "40.746380",
        "longitude": "-73.990883",
        "startDate": 1655517612,
        "expireDate": 1655521212,
        "questionType": {
            "example": [],
            "id": 1,
            "isDeleted": false,
            "createdAt": "0",
            "updatedAt": "1631735523",
            "type": 1,
            "price": 2,
            "title": "Outside Ask",
            "description": "Involves wait times, line times, court availability, and more",
            "currency": "USD",
            "takeRate": 5,
            "serviceFee": 0.3,
            "serviceCategory": "question",
            "moreInfo": ""
        },
        "paymentId": "bq_1655141052395wefnn39ue3",
        "amountPayable": 5,
        "location": {
            "types": [
                "nightclub"
            ],
            "activeDays": [
                "Friday",
                "Saturday",
                "Sunday"
            ],
            "activeTimes": [
                "10:00PM-11:59PM",
                "12:00AM-1:30AM;10:00PM-11:59PM",
                "12:00AM-1:30AM"
            ],
            "linkActions": null,
            "availableAt": 1655517612,
            "id": 4,
            "isDeleted": false,
            "createdAt": "1644547086",
            "updatedAt": "1648824494",
            "isActive": false,
            "name": "Fleur Room",
            "street": "105 W 28th St",
            "streetSecondary": null,
            "city": "New York",
            "state": "NY",
            "zipCode": "10001",
            "country": "US",
            "latitude": "40.746380",
            "longitude": "-73.990883",
            "weight": "0.00",
            "image": "https://static-files.knoow.co/locations/images/fleur-room-nightclub-nyc/fleur-room-nightclub.jpg",
            "detailsImage": null
        },
        "questionOption": {
            "activeDays": [
                "Friday",
                "Saturday",
                "Sunday"
            ],
            "activeTimes": [
                "10:00PM-11:59PM",
                "12:00AM-1:30AM;10:00PM-11:59PM",
                "12:00AM-1:30AM"
            ],
            "willBeScheduled": true,
            "id": 2,
            "isDeleted": false,
            "createdAt": "1644547088",
            "updatedAt": "1644547088",
            "questionTypeId": 1,
            "locationId": 4,
            "value": "How long is the wait to get in?",
            "amountPayable": 5,
            "isEditable": false,
            "isActive": true,
            "defaultDuration": null,
            "weight": "1.00",
            "location": {
                "types": [
                    "nightclub"
                ],
                "activeDays": [
                    "Friday",
                    "Saturday",
                    "Sunday"
                ],
                "activeTimes": [
                    "10:00PM-11:59PM",
                    "12:00AM-1:30AM;10:00PM-11:59PM",
                    "12:00AM-1:30AM"
                ],
                "linkActions": null,
                "availableAt": 1655517612,
                "id": 4,
                "isDeleted": false,
                "createdAt": "1644547086",
                "updatedAt": "1648824494",
                "isActive": false,
                "name": "Fleur Room",
                "street": "105 W 28th St",
                "streetSecondary": null,
                "city": "New York",
                "state": "NY",
                "zipCode": "10001",
                "country": "US",
                "latitude": "40.746380",
                "longitude": "-73.990883",
                "weight": "0.00",
                "image": "https://static-files.knoow.co/locations/images/fleur-room-nightclub-nyc/fleur-room-nightclub.jpg",
                "detailsImage": null
            }
        },
        "createdAt": 1655141052,
        "updatedAt": 1655141052,
        "questionTypeId": 1,
        "locationId": 4,
        "questionOptionId": 2,
        "answerId": null,
        "acceptedByUserId": null,
        "id": 18388,
        "isDeleted": false,
        "isAnswered": false,
        "address": {
            "types": [
                "nightclub"
            ],
            "zipCode": "10001",
            "city": "New York",
            "country": "US",
            "state": "NY",
            "street": "105 W 28th St",
            "name": "Fleur Room",
            "questionId": 18388,
            "createdAt": 1655141052,
            "updatedAt": 1655141052,
            "id": 17848,
            "isDeleted": false
        },
        "currency": "USD"
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
    "questionData": {
        "id": 4998,
        "isDeleted": false,
        "createdAt": "1640986588",
        "updatedAt": "1648824183",
        "question": "How long is the wait to get a test?",
        "questionTypeId": 1,
        "answerId": 0,
        "userId": 483,
        "acceptedByUserId": null,
        "startDate": "1640988133",
        "expireDate": "1640995453",
        "isAnswered": false,
        "latitude": "40.7249765",
        "longitude": "-74.0018494",
        "paymentId": "bq_1640986587494xirvc62owyn",
        "amountPayable": 1.9,
        "questionOptionId": null,
        "locationId": 954,
        "address": {
            "types": [
                "premise",
                "Covid Testing"
            ],
            "id": 4456,
            "isDeleted": false,
            "createdAt": "1640986588",
            "updatedAt": "1640995436",
            "questionId": 4998,
            "zipCode": "10012",
            "city": "New York",
            "country": "US",
            "state": "New York",
            "street": "414 West Broadway",
            "name": "Sameday Health"
        }
    }
}
```



