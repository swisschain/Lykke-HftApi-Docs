---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - json

toc_footers:

includes:
  - public-api.md
  - errors

search: true
---

# Lykke trading API

Lykke high-frequency trading API.

This API allows Lykke clients to carry out automated trading on special sub-accounts. Also, the API allows you to get public data about the Lykke market, such as current prices, books of orders, and other parameters of trading instruments.

[www.lykke.com](https://www.lykke.com/)

[Lykke Web Wallet](https://wallet.lykke.com/)

# Protocol description

Lykke HFT API allow user to use 2 kind of protocol `gRPC API` and `Rest API`. 

Both kind of API support same methos with same data format. But only gRPC API supported receive striming data from platform.

## gRPC API

A new generation RPC framework that includes working with `HTTP 2`, `ProtoBuf`, and `WebSocket` for faster and more efficient interaction with the platform. The `gRPC framework` uses the declarative description of the contract in `.proto` files. On the basis of which you can easily generate a client library in many [programming languages](https://grpc.io/docs/languages/). Also, the `gRPC framework` supports working with `stream`, which makes it easy to receive events from the server.

**Lykke team strongly recommend using gRPC API protocol to communicate with a platform**

Read more about `gRPC framework`: [https://grpc.io/](https://grpc.io/)

Usfull tool for manual call gRPC API: [BloomRPC](https://github.com/uw-labs/bloomrpc)

**API Endpoint**: 
* https://hft-apiv2-grpc.lykke.com - url format
* `hft-apiv2-grpc.lykke.com:443`   - host format

## Rest API

Classical HTTP based framework that includes working with `HTTP 1.1`, and `JSON`. Rest API allows users just call RPC methods without streaming data from the server.

Usfull tool for manual call gRPC API: [HFR API Swagger](https://hft-apiv2.lykke.com/swagger/ui/index.html)

**API Endpoint**: 
* https://hft-apiv2.lykke.com/api

# API usage


## Allowed HTTP Verbs
- `PUT` : Updates a resource 
- `POST` : Creates a resource
- `GET` : Gets a resource or list of resources
- `DELETE` : Deletes a resource

## Description Of Usual HTTP Server Responses
- 200 `OK` : the request was successful.
- 401 `Unauthorized` : authentication failed.
- 404 `Not Found` : endpoint was not found.
- 500 `Internal Server Error` : server error.

## Response structure

Every response contains two fields - `payload` and `error`. Successful response will contain response data in the `payload` field and *null* in `error` field and vise versa for the error response.

Fill list of error you can see in paragraf **Error codes** in end of document.

> Successful response

```json
{
    "payload": {
        "orderId": "a0e83da9-4a69-4ce4-9a42-6443ed1b45c0"
    },
    "error": null
}
```

> Eror response

```json
{
    "error": {
        "code": 1100,
        "message": "Asset not found",
        "fields": {
            "assetId": "Asset not found"
        }
    },
    "payload": null
}
```

## Authorization

You can create API keys on this page https://wallet.lykke.com/wallets/hft

To use API keys you should just add a header `Authorization: Bearer <your API Key>` with bearer token to your request.
```json
  "Authorization": "Bearer **********************************"
```

## Decimal type
Here you can see: How manage decimal type (Price, Volume, Amount, etc) in API contract.

### gRPC API
In the gRPC API contract decimal type present as `string` type, with a textual representation of the number. This is done in order to avoid problems with non-strict precision "double" type.

### Resp API
In the Rest API contact decemal type present as `number` with strict precision.
```json
{
    "price": 222231.33420001911,
    "volume": 0.0000001
}
```

## Timestamp type
Here you can see: How manage timestamp type in API contract.

*The timestamp is always used in the **time zone UTC+0***

### gRPC API
In the gRPC API contract timestamp type present as `google.protobuf.Timestamp` type.

```json
import "google/protobuf/timestamp.proto";

google.protobuf.Timestamp time_name = 1;
```

### Resp API
In the Rest API contact timestamp type present as `number` with "Milliseconds since Unix Epoch" format of date-time.


```json
{
   "Timestamp": 1592903724406
}
```