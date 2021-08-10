# Booking

## Overview

A **booking** is the physical booking slot of the venue. A booking is only created when a booking request is approved.

The booking is a confirmed slot of the venue that has been taken by a certain user.

## Booking Schema

| key        | type     | description                                                   | isUnique | defaultValue |
| ---------- | -------- | ------------------------------------------------------------- | -------- | ------------ |
| id         | ObjectId | AlphaNumeric ID given by mongodb                              | false    | Given        |
| email      | String   | Email of the booking request                                  | false    | None         |
| venue      | ObjectId | AlphaNumeric ID of the venue                                  | false    | None         |
| date       | Number   | Unix time stamp date that the use wants to book               | false    | None         |
| timingSlot | Number   | time slot user has booked                                     | false    | None         |
| notes      | String   | Details about the booking the user want to let the admin know | false    | None         |

## User API Resources

### Get Booking of Venue

**Description**  
Get the booked slots of the venue given a range of dates

**Request**

Method - `GET`

Route - `/api/v1/booking/get`

Example Request - `/api/v1/booking/get?venueId=6106db0948fdff1f8d065588&startDate=20210808&endDate=20211225`

**Query Parameters**

| param     | type     | description                                                |
| --------- | -------- | ---------------------------------------------------------- |
| venueId   | ObjectId | ID of the booking request                                  |
| startDate | String   | Start Date of the range of search in the format `YYYYMMDD` |
| endDate   | String   | End Date of the range of search in the format `YYYYMMDD`   |

**Response**

Description - Array of time slots that are booking tagged to the date

Code - `200 OK`

Example Response

```json
{
  "bookings": {
    "20210808": [1, 2]
  }
}
```

Details - The queried venue is booked on time slot 1 and 2 on 2021/08/08
