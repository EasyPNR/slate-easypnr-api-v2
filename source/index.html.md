---
title: EasyPNR API Version 2.0

language_tabs:
  - curl

toc_footers:
  - <a href="https://easypnr.com/signup" target="_blank">Sign up to use this API</a>
  #- <a href='https://github.com/tripit/slate'>Documentation Powered by Slate</a>

includes:
#  - errors

search: true
---

# Introduction

> API Endpoint Version 2

```
http://api.easypnr.com/v2/
```

**New API Available! http://docs.easypnr.com/api/v3/**

Welcome to the **EasyPNR API** documentation! Following the further instructions you will be able to decode Amadeus, Sabre and Galileo PNRs directly on your application.

The **EasyPNR API** is a REST Server available at the endpoint [http://api.easypnr.com/v2](http://api.easypnr.com/v2/).

The API is pretty simple, anyway, in case of problems, contact us on [support@easypnr.com](mailto:support@easypnr.com) or using the form available on the [website](http://www.easypnr.com/contact]).


# Authentication

> Do a POST request passing your username and password, and save the value returned on the Header **Location**:

```curl
# Request
curl -i -X POST -d "username=myuser@company.com&password=secret" https://accounts.easypnr.com/v1/tickets

# Response
HTTP/1.1 201 Created
Location: https://accounts.easypnr.com/v1/tickets/TGT-16-JAS2AXFXPAKXPinSOmggVfRYUtEm4jJeZJWSzaTJcnBqfCQDUVbY-sso01.easypnr.com
```
To use the Web services, you must have a free or paid account and use your credentials for authentication and obtain a **Token**.

Authentication is done with a HTTP POST to the endpoint [https://accounts.easypnr.com/v1/tickets](https://accounts.easypnr.com/v1/tickets) with your username and password.

In the server response, locate the value returned on HTTP Header "Location" and do a new request to obtain a *Token*. On this new POST request, send as parameter the service *"https://api.easypnr.com"*.

> Do a new POST request using the previous **Location** value that you saved, and passing the **service** parameter:

```curl
# Request
curl -i -X POST -d "service=https://api.easypnr.com" https://accounts.easypnr.com/v1/tickets/TGT-16-JAS2AXFXPAKXPinSOmggVfRYUtEm4jJeZJWSzaTJcnBqfCQDUVbY-sso01.easypnr.com

# Response
HTTP/1.1 200 OK
...
ST-5-aex0tCChpfmUgDbeHQbTTOg-sso01.easypnr.com
```

> If the response status is 200, you got your **Token**.

If everything worked, you will got a **Token**. It usually expires in one day, when this occurs, you just need to repeat the steps to obtain it again.

# decode

```curl
# Request
curl -i -X POST -d "encodedPNR=1.JOHNSON/BRIAN MR  2.JOHNSON/BRENDA MRS \n 2 QR 905 E 06MAR 4 DOHMEL HK1 1  0005 0810+1 *1A/E*"  -H "Content-Type: application/json" -H "Token: ST-5-aex0tCChpfmUgDbeHQbTTOg-sso01.easypnr.com" https://api.easypnr.com/v2/decode
```

> The above command returns JSON structured like this:

```json
{
   "persons":[
      {
         "firstName":"BRIAN",
         "lastName":"JOHNSON",
         "title":"MR",
         "rawData":"JOHNSON/BRIAN MR"
      },
      {
         "firstName":"BRENDA",
         "lastName":"JOHNSON",
         "title":"MRS",
         "rawData":"JOHNSON/BRENDA MRS \\n"
      }
   ],
   "others":null,
   "flightInfo":{
      "flights":[
         {
            "company":{
               "iataCode":"QR",
               "description":"Qatar Airways"
            },
            "departureTime":"2016-03-06 00:05:00",
            "landingTime":"2016-03-07 08:10:00",
            "departureAirport":{
               "iataCode":"DOH",
               "description":"Hamad International Airport - Doha, Qatar"
            },
            "flight":"905",
            "landingAirport":{
               "iataCode":"MEL",
               "description":"Melbourne Airport - Melbourne, Victoria, Australia"
            }
         }
      ]
   },
   "hotelInfo":{
      "hotels":[

      ]
   },
   "trainInfo":{
      "trains":[

      ],
      "stationNames":{

      }
   }
}
```

Decode a PNR.

### HTTP Request

`POST https://api.easypnr.com/v2/decode`

### Query Parameters

Parameter  | Required |Default | Description
---------  | ---------|------- | -----------
encodedPNR | true     |false   | The PNR that will be decoded.


<!-- aside class="success">
Remember — a happy kitten is an authenticated kitten!
</aside-->
