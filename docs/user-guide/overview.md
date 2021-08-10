# Overview

The venue booking system is split into 4 key parts:

1. Venues
2. Booking Requests
3. Bookings
4. Recurring Bookings

## Venues

A **venue** is a location that is available for booking or reservation. There is support for sub-venues under the main venue so you are able to split your venues into smaller sections to open for booking.

## Booking Requests

A **booking request** is a request made for booking a venue. This gives the administrator of the venue control over who the venue is allocated to.

## Bookings

A **booking** is the physical booking slot of the venue. A booking is only created when a booking request is approved.

## Recurring Bookings

A **recurring booking** is a booking that happens consistently. Currently we only support weekly recurrences.
