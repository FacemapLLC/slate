---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ

  - shell

toc_footers:
  - <a href='#'>Sign Up for a Developer Key</a>
  - <a href='https://github.com/lord/slate'>Documentation Powered by Slate</a>

includes:
  - errors

search: true
---

# Introduction

## Facemap Chatacter Analysis API

Draft of the API design. Goal of the API to handle Picture / Movie uploads and deliver analysis results back to the client.

<aside class="warning">This draft and the API´s are under construction. This document is supposed to be a basis for a discussion and a plan for building API v2</aside>

# Endpoints
### Production
`GET http://api.facemap.com/v2`

### Sandbox
`GET http://api_sandbox.facemap.com/v2`

# Authentication

<aside class="notice">
No decision yet made for authentication / security. Up to now, JWT seems to be the best option. Help needed here.
</aside>

# Registration

Facemap needs to handle

- client
- app - app_id
- device - device_id

The registration / subscription via website is mandatory.
A client can create multiple apps.
For each app will exist a name, a token and a shared secret.

The token identifies the app.

#### HTTP Request

`GET https://api.facemap.com/v2/device/<device_id>/register`

```shell
curl "https://api.facemap.com/v2/device/<device_id>/register"
  -H "Authorization: Token <token>"
```
> Success HTTP Response Code 200:

```json
 {
   "device_id": "818184b8-670e-4444-b094-2317b7990b45"
 }
```

> Error HTTP Response Code 400:

```json
 {
   "message": "device already registered"
 }

```


# People

### GET me

"Me" is a Person Object with the "me" flag set to true. For one device there can only be marked one person as "me".

#### HTTP Request

`GET http://api.facemap.com/v2/me`

```shell
curl "https://api.facemap.com/v2/me"
  -H "Authorization: <token>"
```
> Success HTTP Response Code 200:

```json
{
  "id": "09457ac7-5c64-4d5d-b13d-ab6f23e9c915",
  "name": [
    "full_name": "Peter Salomon",
    "first_name": "Peter",
    "last_name": "Salomon"
  ],
  "avatar": "https://s3.amazonaws.com/facemap/data/views/001f2675-2a95-4add-8fe3-52a82be0826f/390ff1a1-9e3f-4f3f-950f-497a324db47b.jpg",
  "gender": "male"
  "age": 50,
  "created_at": "2017-02-01 22:09:55.946267",
  "updated_at": "2017-02-01 22:09:55.946267",
  "last_location": [
    "lat": "34.0328260325212",
    "lgt": "-118.8198261616",
    "address": {
      "id": "ec21781a-6204-407e-ae7c-bade6357c497",
      "country": "United States",
      "country_code": "US",
      "region_name": "California",
      "city": "Malibu",
      "zip": "90265",
      "street": "Sea Star Drive",
      "house_number": "6411"
    }
  ]
}

```

## GET all people

```shell
curl "https://api.facemap.com/v2/people"
  -H "Authorization: <token>"
```

> Success HTTP Response Code 200:

```json
{
  "people_count": 2,
  "people": [
    "id": "09457ac7-5c64-4d5d-b13d-ab6f23e9c915",
    "name": [
      "full_name": "Peter Salomon",
      "first_name": "Peter",
      "last_name": "Salomon"
    ],
    "avatar": "https://s3.amazonaws.com/facemap/data/views/001f2675-2a95-4add-8fe3-52a82be0826f/390ff1a1-9e3f-4f3f-950f-497a324db47b.jpg",
    "gender": "male"
    "age": 50,
    "created_at": "2017-02-01 22:09:55.946267",
    "updated_at": "2017-02-01 22:09:55.946267",
    "last_location": [
      "lat": "34.0328260325212",
      "lgt": "-118.8198261616",
      "address": {
        "id": "ec21781a-6204-407e-ae7c-bade6357c497",
        "country": "United States",
        "country_code": "US",
        "region_name": "California",
        "city": "Malibu",
        "zip": "90265",
        "street": "Sea Star Drive",
        "house_number": "6411"
      }
    ]
  ],
  [
    "id": "d54281a0-6538-4e13-a571-70add0001b8e",
    "name": [
      "full_name": "Carinda Bourgeois",
      "first_name": "Carinda",
      "last_name": "Bourgeois"
    ],
    "avatar": "https://s3.amazonaws.com/facemap/data/views/001f2675-2a95-4add-8fe3-52a82be0826f/390ff1a1-9e3f-4f3f-950f-497a324db47b.jpg",
    "gender": "female"
    "age": 43,
    "created_at": "2017-04-21 07:43:19.537821",
    "updated_at": "2017-04-21 07:43:19.537821",
    "last_location": [
      "lat": "34.0328260325212",
      "lgt": "-118.8198261616",
      "address": {
        "id": "ec21781a-6204-407e-ae7c-bade6357c497",
        "country": "United States",
        "country_code": "US",
        "region_name": "California",
        "city": "Malibu",
        "zip": "90265",
        "street": "Sea Star Drive",
        "house_number": "6411"
      }
    ]
  ]
}
```

This endpoint retrieves all people, captured with this device.
The maximum people amount is 200. For more use pagination.

#### HTTP Request

`GET http://api.facemap.com/v2/people`

`GET http://api.facemap.com/v2/people?page=2&per_page=10`

#### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
page | 1 | Pagination object, number of the page
per_page | 10 | Amount of items of a page


# Matches

### GET matches

A "match" is a person object, captured with other devices. How much another person is a match is determined by a match score, which is related to the latest result of "me" and the match category.

The maximum matches amount is 200. For more use pagination.


```shell
curl "https://api.facemap.com/v2/matches"
  -H "Authorization: <token>"
```

> Success HTTP Response Code 200:

```json
{
  "matches_count": 2,
  "per_page": 10,
  "page": 2,
  "matches": [
    "id": "09457ac7-5c64-4d5d-b13d-ab6f23e9c915",
    "name": [
      "full_name": "Joy Schneemann",
      "first_name": "Joy",
      "last_name": "Schneemann"
    ],
    "avatar": "https://s3.amazonaws.com/facemap/data/views/001f2675-2a95-4add-8fe3-52a82be0826f/390ff1a1-9e3f-4f3f-950f-497a324db47b.jpg",
    "gender": "female"
    "age": 45,
    "created_at": "2017-02-01 22:09:55.946267",
    "updated_at": "2017-02-01 22:09:55.946267",
    "last_location": [
      "lat": "34.0328260325212",
      "lgt": "-118.8198261616"
    ],
    "match_score": 33,
    "match_category": "personal"
  ,
    "id": "d54281a0-6538-4e13-a571-70add0001b8e",
    "name": [
      "full_name": "Carinda Bourgeois",
      "first_name": "Carinda",
      "last_name": "Bourgeois"
    ],
    "avatar": "https://s3.amazonaws.com/facemap/data/views/001f2675-2a95-4add-8fe3-52a82be0826f/390ff1a1-9e3f-4f3f-950f-497a324db47b.jpg",
    "gender": "female"
    "age": 43,
    "created_at": "2017-04-21 07:43:19.537821",
    "updated_at": "2017-04-21 07:43:19.537821",
    "last_location": [
      "lat": "34.0328260325212",
      "lgt": "-118.8198261616"
    ],
  "match_score": 76,
  "match_category": "personal"
  ]
}
```


#### HTTP Request

`GET http://api.facemap.com/v2/matches`

`GET http://api.facemap.com/v2/matches?page=2&per_page=10&near_me=10&matching=70&gender=female`

#### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
page | 1 | Pagination object, number of the page
per_page | 10 | Amount of items of a page
gender | - | "male" or "female"
near_me | 5 | matching people within a distance of x miles
matchscore | 30 | minimum percentage of match score
match_category | "personal" | The category, the match_score is related to
age | "same" | the age of the matching people. Options: "same", "about_same", "15-20", "20-25", "25-30", "30-35", "35-40", "45-50", "55-60", "65-70", "70+"



# Locations

### POST location


This endpoint receives the location of a device. The device should send the location on a regular basis.


```shell
curl -X POST "https://api.facemap.com/v2/<device_id>/location/" \
  -H "Authorization: <token>, Content-Type: application/json" \
  -d "{'lon': '-118.8198261616', 'lat': '34.0328260325212'}"
```

> Success HTTP Response Code 200

```json
  {}
```

### GET locations

The locations of a device (and therefore the owners person) in a list

`GET http://api.facemap.com/v2/<device_id>/locations`


```shell
curl "https://api.facemap.com/v2/<device_id>/locations&address=true"
  -H "Authorization: <token>"
```



```json
  {
    "locations_count": 525,
    "locations": [
      {
        "id": "6b18dac9-6882-4519-968a-f844b8528191",
        "scan_id": "1b2f36d1-84f9-4504-9126-7b99cc1886e8",
        "lat": "34.0328272059877" ,
        "lon": "-118.819855162984",
        "created_at": "2017-12-12 20:21:40",
        "distance_to_me": 1.45,
        "address": {
          "id": "ec21781a-6204-407e-ae7c-bade6357c497",
          "country": "United States",
          "country_code": "US",
          "region_name": "California",
          "city": "Malibu",
          "zip": "90265",
          "street": "Sea Star Drive",
          "house_number": "6411"
        }
      },
      {
        "id": "ea17d446-3d1d-4899-8f88-0c3f36aa8efc",
        "scan_id": null,
        "lat": "34.0328272059877" ,
        "lon": "-118.819855162984",
        "created_at": "2017-12-12 20:21:30",
        "distance_to_me": 1.44,
        "address": {
          "id": "4df96d0a-16a8-48b3-b624-5609270947a5"
          "country": "United States",
          "country_code": "US",
          "region_name": "California",
          "city": "Malibu",
          "zip": "90265",
          "street": "Sea Star Drive",
          "house_number": "6411"
        }
      },
      ...
    ]
  }


```

#### Query Parameters

Parameter | Default | Description
--------- | ------- | -----------
address | false | include the address of the geocoordinates
page | 1 | Pagination object, number of the page
per_page | 10 | Amount of items of a page



### GET address

receive the address of a specific location

`GET http://api.facemap.com/v2/locations/{location_id}/address`


```shell
curl "https://api.facemap.com/v2/<device_id>/locations&address=true"
  -H "Authorization: <token>"
```

> Success HTTP Response Code 200

```json
{
  "id": "4df96d0a-16a8-48b3-b624-5609270947a5"
  "country": "United States",
  "country_code": "US",
  "region_name": "California",
  "city": "Malibu",
  "zip": "90265",
  "street": "Sea Star Drive",
  "house_number": "6411"
}

```

# Scan

A scan is either a movie of a captured face or a gazet of fotos of a captureing session. Since the analysation of either a movie or a gazet of images takes some time, the server responds with a scan_id, which will be used to poll the results.

### POST upload

`POST https://api.facemap.com/v2/<device_id>/scans/upload`


```shell
curl -X POST "https://api.facemap.com/v2/<device_id>/scans/upload" \
  -H "Authorization: <token>, Content-Type: application/json" \
  -d "{'moveshot': '@video.mp4', 'front': '', left': '', 'right': '',\
   'top': '', 'bottom': '', lat': '34.0328260325212', \
   'lon': '-118.819855162984', 'owner': true}"


curl -X POST "https://api.facemap.com/v2/<device_id>/scans/upload" \
 -H "Authorization: <token>, Content-Type: application/json" \
 -d "{'moveshot': '', 'front': '@frontimage.jpg', left': '@leftimage.jpg', 'right': '@rightimage.jpg',\
  'top': '', 'bottom': '', lat': '34.0328260325212', \
  'lon': '-118.819855162984', 'owner': true}"

```
> Success HTTP Response Code 200

```json
{
  "scan_id": "9535adb0-69be-4b2b-9aef-3f147f5aed95"
}
```

### GET results

`GET https://api.facemap.com/v2/<device_id>/scans/<scan_id>`

```shell
curl "https://api.facemap.com/v2/<device_id>/scans/<scan_id>"
  -H "Authorization: <token>"
```

> Success HTTP Response Code 200; analysis in progress


```json
{
  "status": "in progress",
  "id": "a509a183-152d-4601-b51e-5bf313c998bc",
  "person_id": "09457ac7-5c64-4d5d-b13d-ab6f23e9c915"
  "created_at": "2017-04-27 00:08:12"
  "analysis": {}
}
```

> Success HTTP Response Code 200; analysis done


```json
{
  "status": "done",
  "id": "a509a183-152d-4601-b51e-5bf313c998bc",
  "person_id": "09457ac7-5c64-4d5d-b13d-ab6f23e9c915",
  "created_at": "2017-04-27 00:08:12",
  "analysis": {
    "archetype": "inventor",
    "categories": [
      {
        "title": "Personality",
        "strongest_traits": [
          {
            "title": "Skill in action",
            "handle": "skill-in-action",
            "score": 88.5,
            "order": 1,
            "description": "You need some time to decide on a course \
              of action. You often collect too much information and do \
              not set priorities. This hesitant behavior is a result of \
              your insecurity, and it will often cause difficulties \
              within your job. You simply are too afraid of doing the \
              wrong thing − both on a personal level and in the professional \
              sphere.",
            "tipp": "Take a pause more often and relax. All is good. You have \
              a good guttfeeling, you should trust it more."
          },
          {
            "title": "Emotional intelligence",
            "handle": "emotional-intelligence",
            "score": 79.6,
            "order": 2,
            "description": "You generally do not consciously perceive your \
              intuitive hunches, yet, nevertheless, you act on the “good” or \
              “strange” feelings that often come about in the course of your \
              thoughts or observations. You then often attempt to question or \
              justify these impulses as having arisen from rational processes.",
            "tipp": ""
          },
          {
            ...
          }
        ],
        "weakest_traits": [
          {
            "title": "Empathy",
            "handle": "empathy",
            "score": 43.5,
            "order": 1,
            "description": "Your joy in talking things out is healthy. When necessary \
              – for example, if you have to defend values or important projects – you \
              eagerly engage in verbal conflict. At the same time, you always remain \
              constructive and listen carefully to your colleagues’ and employees’ arguments. \
              Your balanced empathy is revealed in situations involving people you know \
              well or are particularily fond of. This emotional level is in a healthy relationship \
              with your obective considerations, and, therefore, you can usually show a very appropriate response.",
            "tipp": "And here some really good tipp for people without empathy."
          },
          {
            "title": "Endurance",
            "handle": "empathy",
            "score": 25,
            "order": 1,
            "description": "Your physical and psychological endurance is quite good. For this \
              reason you can concentrate well, though you do have to take regular breaks to \
              avoid overstrain.\r\n\r\nIn general, you actively pursue your goals and complete \
              your tasks with determination. You are nevertheless always able to react flexibly \
              to the working methods of your environment.",
            "tipp": ""
          },
          {
            ...
          }
        ]
      },
      {
        "title": "Personality",
        "strongest_traits": [...],
        "weakest_traits": [...]
      },
      {
        "title": "...",
        "strongest_traits": [...],
        "weakest_traits": [...]
      }
    ]

  }
}


```

#### Query Parameters

Files | Description
--------- | -------
moveshot  | a movie containing stills
front | face in neutral position | lang | the system language of the device (en, de)
left | face´s left side view
right | face´s right side view
top | face´s top view
bottom | face´s bottom view

Parameter | Description
----------- | ------------
owner | Boolean (false, true); is this a scan of the owner of this device
lat | latitude
lon | logitude
