---
layout: page
title: Universal Links
permalink: /universal-links/
---
# Universal Links

Knoow’s Universal Links are enhanced deeplinks that work seamlessly across platforms with less overhead for developers. With universal links, developers needn't add client-side logic to determine whether the Knoow app is installed on Android or iOS—users without the native Knoow app will automatically be sent to the App Store or Google Play Store to download the Knoow app. Users with the Knoow app already installed will be deeplinked into it.

## Partner Tracking

Always include partner tracking on all deeplinks to the Knoow app by specifying your Client ID in the query string of any deeplink. If you do not have a Client ID, you can obtain one by reaching out to your Knoow Business Contact.

# Available Links
- [Post a Question](#post-a-question)


## Post a Question

`https://links.knoow.co/question`

This link will take the user directly to the question posting screen within the Knoow app.

There are four required parameters: `latitude`, `longitude`, `questionTypeId`, and `partnerId`.

Example: `https://links.knoow.co/question?latitude=40.748400&longitude=-73.985700&questionTypeId=1&partnerId=YOUR_CLIENT_ID`

Currently, Universal Links support the following question type ids:

| Question Type Id | Description                 |
| ---------------- | --------------------------- |
| `1`              | [Outside Ask](#outside-ask) |
| `2`              | [Inside Ask](#inside-ask)   |
| `3`              | [Wait for Me](#wait-for-me) |

You should set the question type using the `questionTypeId` parameter.

The `partner` parameter corresponds to your Client ID. If you do not have a Client ID, you can obtain one by reaching out to your Knoow Business Contact.

```
Note - It's important to ensure latitude and longitude are sent along with the parameters and that they are properly formatted decimal degrees.
Example: 40.748400, -73.985700 (latitude, longitude)
```


### Question Types

#### Outside Ask

Involves wait times, line times, court availability, and more


#### Inside Ask

Involves going into a location to put a name down, checking availability, speaking to an employee, and more.


#### Wait for Me

Have someone wait in line for you, for one hour.
