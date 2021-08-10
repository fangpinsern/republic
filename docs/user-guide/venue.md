---
sidebar_position: 2
---

# Venue

## Overview

A **venue** is a location that is available for booking or reservation. There is support for **sub-venues** under the main venue so you are able to split your venues into smaller sections to open for booking.

## Venue Schema

| key          | type       | description                                                                                                       | isUnique | defaultValue |
| ------------ | ---------- | ----------------------------------------------------------------------------------------------------------------- | -------- | ------------ |
| id           | ObjectId   | AlphaNueric ID given by mongodb                                                                                   | true     | Given        |
| name         | String     | Name of the venue                                                                                                 | true     | None         |
| capacity     | Integer    | Capacity of the venue                                                                                             | false    | None         |
| openingHours | String     | Opening hours for show. This opening hours has no relation to the booking slots available                         | false    | None         |
| description  | String     | Description of the venue                                                                                          | false    | None         |
| visible      | Boolean    | Venue is visible to the public if the parameter is set to `true`                                                  | false    | true         |
| image        | String     | Link to the image of the venue. (No support for image uploads yet)                                                | false    | None         |
| isChildVenue | Boolean    | Venue is a child venue of a larger parent venue if parameter set to `true`                                        | false    | false        |
| parentVenue  | ObjectId   | AlphaNumeric Object ID of the parent venue                                                                        | false    | None         |
| childVenues  | [ObjectId] | List AlphaNumeric Object IDs of the child venue of the parent. (only populated if `isChildVenue` is set to false) | false    | []           |

## User API Resources

### Get All Venues (visible)

**Description**  
This returns all venues that are set to visible. You can filter the visible venues using some available query parameters. By default, the API only returns all venues regardless of status

**Request**

Method - `GET`

Route - `/api/v1/venue/search`

Example Request - `localhost:8080/api/v1/venue/search?parentVenue=6100d84eb4051df2f0baef1f`

**Query Parameters**

| param        | type     | description                                                                                   |
| ------------ | -------- | --------------------------------------------------------------------------------------------- |
| parentVenue  | ObjectId | Enter a the id of the parent venue and the request will return the child venue of this parent |
| isChildVenue | Boolean  | This query paremeter is mainly to return only parent venues (when set to false)               |
| \_id         | ObjectId | Input the venue id to get specific venue information                                          |

**Response**

Description - Returns a list of venues under the `venues` key

Code - `200 OK`

Example Response

```json
{
  "venues": [
    {
      "id": "6106db0948fdff1f8d065588",
      "name": "Basketball Court A",
      "capacity": 2,
      "description": "Half nearer to the tennis court",
      "childVenues": [],
      "visible": true,
      "openingHours": "7am - 11pm"
    }
  ]
}
```

## Admin API Resources

All APIs used here has to have the authorization key set in your `env` file. Set the http request header with a authorization key. There will be an update to this soon.

### Get All Venues

**Description**  
This returns all venues regardless of visiblity. You can filter the venues using some available query parameters.

**Request**

Method - `GET`

Route - `/api/v1/venue/admin/search`

Example Request - `localhost:8080/api/v1/venue/admin/search?isChildVenue=false`

**Query Parameters**

| param        | type     | description                                                                                   |
| ------------ | -------- | --------------------------------------------------------------------------------------------- |
| parentVenue  | ObjectId | Enter a the id of the parent venue and the request will return the child venue of this parent |
| isChildVenue | Boolean  | This query paremeter is mainly to return only parent venues (when set to false)               |
| \_id         | ObjectId | Input the venue id to get specific venue information                                          |

**Response**

Description - Returns a list of venues under the `venues` key

Code - `200 OK`

Example Response

```json
{
  "venues": [
    {
      "id": "6100d84eb4051df2f0baef1f",
      "name": "Comms Hall",
      "capacity": 4,
      "image": "/img/band-room.jpeg",
      "description": "Just a place in KE will low ceilings.",
      "visible": true,
      "openingHours": "8am-11pm"
    },
    {
      "id": "61022836ef227f554ee007d1",
      "name": "Tennis Court",
      "capacity": 2,
      "image": "/img/tennis-court.jpeg",
      "description": "just a place to play tennis",
      "visible": true,
      "openingHours": "7am - 11pm"
    },
    {
      "id": "6102f712f8fa896f2f44f1e2",
      "name": "Band Room",
      "capacity": 1,
      "image": "/img/band-room.jpeg",
      "description": "Not testing anymoreeee",
      "visible": true,
      "openingHours": "TEST"
    },
    {
      "id": "6106da7e48fdff1f8d065587",
      "name": "BasketBall Court",
      "capacity": 10,
      "image": "/img/MPC.jpeg",
      "description": "Lights off whenever OHS wants. I have no control",
      "visible": true,
      "openingHours": "7am - 11pm"
    }
  ]
}
```

### Update Venue Visibility

**Description**  
Update the visibility of the venue. This API toggles between true and false

**Request**

Method - `PUT`

Route - `/api/v1/venue/visibility/:venueId`

Example Request - `localhost:8080/api/v1/venue/visibility/6100d84eb4051df2f0baef1f`

**Parameters**

| param   | type     | description                   |
| ------- | -------- | ----------------------------- |
| venueId | ObjectId | ID of the venue to be toggled |

**Response**

Description - Returns info of the venue being toggled

Code - `202 ACCEPTED`

Example Response

```json
{
  "venue": {
    "priorityEmails": [],
    "visible": true,
    "isChildVenue": false,
    "childVenues": [
      "6102602857bab95b394ae1ee",
      "61057f55199c320b953b16bb",
      "61066c18d5c2491eca67d0a2"
    ],
    "_id": "6100d84eb4051df2f0baef1f",
    "name": "Comms Hall",
    "capacity": 4,
    "openingHours": "8am-11pm",
    "description": "Just a place in KE will low ceilings.",
    "__v": 3,
    "createdAt": "2021-07-29T08:00:40.030Z",
    "updatedAt": "2021-08-10T07:57:16.648Z",
    "image": "/img/band-room.jpeg"
  }
}
```

### Create Venue

**Description**  
Create a new venue available for booking

**Request**

Method - `POST`

Route - `/api/v1/venue/`

Example Request - `localhost:8080/api/v1/venue/`

Example Body

```json
{
  "name": "Squash Court",
  "capacity": 2,
  "openingHours": "7am - 11pm",
  "description": "just a place to play tennis"
}
```

**Body Parameters**

| param        | type   | description               |
| ------------ | ------ | ------------------------- |
| name         | String | Name of the venue         |
| capacity     | Number | Capacity of the venue     |
| openingHours | String | Opening hours description |
| description  | String | Description of the venue  |

**Response**

Description - Returns info of the venue being toggled

Code - `202 ACCEPTED`

Example Response

```json
{
  "venue": {
    "priorityEmails": [],
    "visible": true,
    "isChildVenue": false,
    "childVenues": [],
    "_id": "611233ecc773690015aa79d0",
    "name": "Squash Court",
    "capacity": 2,
    "openingHours": "7am - 11pm",
    "description": "just a place to play tennis",
    "createdAt": "2021-08-10T08:08:12.826Z",
    "updatedAt": "2021-08-10T08:08:12.826Z",
    "__v": 0
  }
}
```

### Create Subvenue

**Description**  
Create a sub venue under a parent venue to split the venue into smaller venues. Every sub venue will take on the values of the parent venue unless stated otherwise

**Request**

Method - `POST`

Route - `/api/v1/venue/childVenue/:parentId`

Example Request - `localhost:8080/api/v1/venue/childVenue/6100d84eb4051df2f0baef1f`

Example Body

```json
{
  "name": "Squash Court",
  "capacity": 2,
  "openingHours": "7am - 11pm",
  "description": "just a place to play tennis"
}
```

**URL Parameter**

| param    | type     | description                  |
| -------- | -------- | ---------------------------- |
| parentId | ObjectId | ObjectId of the parent Venue |

**Body Parameters**

| param        | type   | description               |
| ------------ | ------ | ------------------------- |
| name         | String | Name of the venue         |
| capacity     | Number | Capacity of the venue     |
| openingHours | String | Opening hours description |
| description  | String | Description of the venue  |

**Response**

Description - Returns info of the venue being toggled

Code - `202 ACCEPTED`

Example Response

```json
{
  "venue": {
    "priorityEmails": [],
    "visible": true,
    "isChildVenue": true,
    "childVenues": [],
    "_id": "61123657c773690015aa79d9",
    "name": "Comm Hall D",
    "capacity": 1,
    "openingHours": "8am-11pm",
    "description": "stage part of comm hall",
    "image": "/img/band-room.jpeg",
    "parentVenue": "6100d84eb4051df2f0baef1f",
    "createdAt": "2021-08-10T08:18:31.344Z",
    "updatedAt": "2021-08-10T08:18:31.344Z",
    "__v": 0
  }
}
```

### Edit Venue

**Description**  
Edit details of a venue. Input in the request body information you want to update. In the example below, we are editing the name of the venue

**Request**

Method - `PATCH`

Route - `/api/v1/venue/:venueId`

Example Request - `/api/v1/venue/61123657c773690015aa79d9`

Example Body

```json
{
  "name": "Comms Hall"
}
```

**URL Parameter**

| param   | type     | description           |
| ------- | -------- | --------------------- |
| venueId | ObjectId | ObjectId of the Venue |

**Body Parameters**
These are some parameters available

| param        | type   | description               |
| ------------ | ------ | ------------------------- |
| name         | String | Name of the venue         |
| capacity     | Number | Capacity of the venue     |
| openingHours | String | Opening hours description |
| description  | String | Description of the venue  |

**Response**

Description - Returns info of the venue being toggled

Code - `202 ACCEPTED`

Example Response

```json
{
  "venue": {
    "priorityEmails": [],
    "visible": true,
    "isChildVenue": true,
    "childVenues": [],
    "_id": "61123657c773690015aa79d9",
    "name": "Comm Hall D",
    "capacity": 1,
    "openingHours": "8am-11pm",
    "description": "stage part of comm hall",
    "image": "/img/band-room.jpeg",
    "parentVenue": "6100d84eb4051df2f0baef1f",
    "createdAt": "2021-08-10T08:18:31.344Z",
    "updatedAt": "2021-08-10T08:18:31.344Z",
    "__v": 0
  }
}
```
