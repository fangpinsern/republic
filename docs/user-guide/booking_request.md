---
sidebar_position: 3
---

# Booking Request

## Overview

A **booking request** is a request made for booking a venue. This gives the administrator of the venue control over who the venue is allocated to.

Booking Requests can be made if there are no bookings in those time slots. Multiple booking requests can be made for the same time slots. It is up to the administrator to approve which booking requests they want.

## Booking Request Schema

| key         | type       | description                                                                       | isUnique | defaultValue |
| ----------- | ---------- | --------------------------------------------------------------------------------- | -------- | ------------ |
| id          | ObjectId   | AlphaNumeric ID given by mongodb                                                  | false    | Given        |
| email       | String     | Email of the booking request                                                      | false    | None         |
| venue       | ObjectId   | AlphaNumeric ID of the venue                                                      | false    | None         |
| date        | Number     | Unix time stamp date that the use wants to book                                   | false    | None         |
| timingSlots | [Number]   | Array of time slots user wants to book                                            | false    | None         |
| isApproved  | Boolean    | Request is approved if set to `true`                                              | false    | false        |
| isRejected  | Boolean    | Request is rejected if set to `false`                                             | false    | false        |
| bookingIds  | [ObjectId] | Array of booking ids that are associated with this request if booking is approved | false    | []           |
| notes       | String     | Details about the booking the user want to let the admin know                     | false    | None         |

## User API Resources

### Create Booking Request

**Description**  
Create a new booking request. An email will be sent to the email address provided once the booking request is recieved.

**Request**

Method - `POST`

Route - `/api/v1/bookingreq/`

Example Request - `localhost:8080/api/v1/bookingreq/`

Example Body

```json
{
  "email": "test@gmail.com",
  "venueId": "6106db0948fdff1f8d065588",
  "date": "20210808",
  "timingSlots": [1, 2],
  "notes": "Birthday Party SUPER SUPER DUPER FOR REAL PLS TEST"
}
```

**Body Parameters**

| param       | type     | description                   |
| ----------- | -------- | ----------------------------- |
| email       | String   | Email of the user             |
| venueId     | Number   | ID of the venue               |
| date        | String   | Date in the format `YYYYMMDD` |
| timingSlots | [Number] | Time slots user wants to book |
| notes       | String   | Notes about the booking       |

**Response**

Description - Returns booking reqeustId

Code - `202 ACCEPTED`

Example Response

```json
{
  "bookingRequestId": "611242378ce22500150be182"
}
```

### Get Booking Request Info

**Description**  
Get info about the booking request by ID

**Request**

Method - `GET`

Route - `/api/v1/bookingreq/`

Example Request - `/api/v1/bookingreq/?bookingRequestId=610d6f6cf210a28425b80dea`

**Query Parameters**

| param            | type     | description               |
| ---------------- | -------- | ------------------------- |
| bookingRequestId | ObjectId | ID of the booking request |

**Response**

Description - Returns details of the booking request under the `bookingRequest` key

Code - `200 OK`

Example Response

```json
{
  "bookingRequest": {
    "timingSlots": [1, 2],
    "isApproved": false,
    "isRejected": false,
    "bookingIds": [],
    "cca": "Block Committee E",
    "_id": "611242378ce22500150be182",
    "email": "keviiweb2@gmail.com",
    "venue": {
      "priorityEmails": [],
      "visible": true,
      "isChildVenue": true,
      "childVenues": [],
      "_id": "6106db0948fdff1f8d065588",
      "name": "Basketball Court A",
      "capacity": 2,
      "openingHours": "7am - 11pm",
      "description": "Half nearer to the tennis court",
      "parentVenue": "6106da7e48fdff1f8d065587",
      "createdAt": "2021-08-01T17:34:01.526Z",
      "updatedAt": "2021-08-08T15:13:39.585Z",
      "__v": 0
    },
    "date": 1628380800000,
    "notes": "Birthday Party SUPER SUPER DUPER FOR REAL PLS TEST",
    "createdAt": "2021-08-10T09:09:11.998Z",
    "updatedAt": "2021-08-10T09:09:11.998Z",
    "__v": 0
  }
}
```

## Admin API Resources

### Approve Booking Request

**Description**  
Approve a booking request. An approved email will be sent to the client with details of the approval. An automatic rejection will be sent to all booking requests that are made for the same time slot as well.

**Request**

Method - `POST`

Route - `/api/v1/bookingreq/approve`

Example Request - `localhost:8080/api/v1/bookingreq/approve`

Example Body

```json
{
  "bookingRequestId": "610826b9ac92f42f9e5af978"
}
```

**Body Parameters**

| param            | type     | description               |
| ---------------- | -------- | ------------------------- |
| bookingRequestId | ObjectId | ID of the booking request |

**Response**

Description - Returns booking reqeust Id, bookingIds assigned to the approved request and rejected bookingids on approval of that request

Code - `202 ACCEPTED`

Example Response

```json
{
  "bookingRequestId": "611242378ce22500150be182",
  "bookingIds": ["6112448a8ce22500150be18d", "6112448a8ce22500150be18f"],
  "rejectedBookingRequestsIds": [
    "610d815d42ce5e87a9c940a9",
    "610e9d04038d890015943067",
    "610e9d5b038d890015943080",
    "610e9daa6b2eb600155f6d1e",
    "610e9eaa6b2eb600155f6d33",
    "610e9ed2af50d40015d12cc3",
    "610ea578af50d40015d12e6a",
    "610ea60b02d35d00153c3f03",
    "610ea6ee78533500155869a9",
    "610ea81495c4150015ca0554",
    "6112410a5527080015415d61",
    "6112410a5527080015415d64",
    "611241708ce22500150be17a"
  ]
}
```

### Approve Booking Request Intent

**Description**  
Get infromation about the conflicts between booking requests if you intend to approve a certain booking request

**Request**

Method - `GET`

Route - `/api/v1/bookingreq/intent`

Example Request - `/api/v1/bookingreq/intent?bookingRequestId=6108b789ef225d3be9e40e09`

**Query Parameters**

| param            | type     | description               |
| ---------------- | -------- | ------------------------- |
| bookingRequestId | ObjectId | ID of the booking request |

**Response**

Description - Returns details of the booking request under the `bookingRequest` key and a list of conflicts under `conflicts` key

Code - `200 OK`

Example Response

```json
{
  "bookingRequest": {
    "id": "6112493be638900015152e32",
    "email": "keviiweb2@gmail.com",
    "date": "2021/08/08",
    "timingSlots": ["8am - 8.30am", "8.30am - 9am"],
    "notes": "Birthday Party SUPER SUPER DUPER FOR REAL PLS TEST",
    "venue": {
      "priorityEmails": [],
      "visible": true,
      "isChildVenue": true,
      "childVenues": [],
      "_id": "6106db0948fdff1f8d065588",
      "name": "Basketball Court A",
      "capacity": 2,
      "openingHours": "7am - 11pm",
      "description": "Half nearer to the tennis court",
      "parentVenue": "6106da7e48fdff1f8d065587",
      "createdAt": "2021-08-01T17:34:01.526Z",
      "updatedAt": "2021-08-08T15:13:39.585Z",
      "__v": 0
    }
  },
  "conflicts": [
    {
      "id": "61124923e638900015152e22",
      "email": "keviiweb2@gmail.com",
      "date": "2021/08/08",
      "timingSlots": ["8am - 8.30am", "8.30am - 9am"],
      "notes": "Birthday Party SUPER SUPER DUPER FOR REAL PLS TEST",
      "venue": "6106db0948fdff1f8d065588",
      "cca": "Block Committee E"
    },
    {
      "id": "61124936e638900015152e2a",
      "email": "keviiweb2@gmail.com",
      "date": "2021/08/08",
      "timingSlots": ["8am - 8.30am", "8.30am - 9am"],
      "notes": "Birthday Party SUPER SUPER DUPER FOR REAL PLS TEST",
      "venue": "6106db0948fdff1f8d065588"
    }
  ]
}
```
