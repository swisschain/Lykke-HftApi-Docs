# API usage


## Allowed HTTP Verbs
- `PUT` : Updates a resource.
- `POST` : Creates a resource.
- `GET` : Gets a resource or a list of resources.
- `DELETE` : Deletes a resource.

## Description Of Usual HTTP Server Responses
- 200 `OK` : The request was successful.
- 401 `Unauthorized` : Authentication failed.
- 404 `Not Found` : Endpoint was not found.
- 500 `Internal Server Error` : Server error.

## Response structure

Every response contains two fields - `payload` and `error`. A successful response will contain the response data in the `payload` field and the *null* in the `error` field, and vise versa for the error response.

Here you have a list of errors you might encounter in the paragraph **Error codes** at the end of the document.

> Successful response

```json
{
    "payload": {
        "orderId": "a0e83da9-4a69-4ce4-9a42-6443ed1b45c0"
    },
    "error": null
}
```

> Error response

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

You can create your API keys on this page https://wallet.lykke.com/wallets/hft. You can also check our step by step guide [here](https://support.lykke.com/hc/en-us/articles/360000552605-How-do-I-create-an-API-Wallet-).

To use the API keys you should just add a header `Authorization: Bearer <your API Key>` with the bearer token on your request.

> Request Header

```json
  "Authorization": "Bearer **********************************"
```

## Decimal type
Here you can see: How to manage decimal types (Price, Volume, Amount, etc) in API contract.

### gRPC API
In the gRPC API contract, the decimal type is presented as a string type, with a textual representation of the number. This is done in order to avoid problems with the non-strict precision "double" type.

### Rest API
In the Rest API contact, the decimal type is presented as `number` with strict precision.

> Example in Rest API

```json
{
    "price": 222231.33420001911,
    "volume": 0.0000001
}
```

## Timestamp type
Here you can see: How to manage the `TimeStamp` type in the API contract.

<i>The timestamp is always used in the <b>time zone UTC+0</b></i>

### gRPC API
In the gRPC API contract, the `TimeStamp` type is presented as a `google.protobuf.Timestamp` type.

> Example in gRPC contract 

```json
import "google/protobuf/timestamp.proto";

google.protobuf.Timestamp time_name = 1;
```

### Rest API
In the Rest API contact, the `TimeStamp` type is presented as a `number` with "Milliseconds in Unix Epoch" format of date-time.

> Example in Rest API

```json
{
   "Timestamp": 1592903724406
}
```

## Order statuses

List of possible order states

Name | Meaning
---- | -------
Placed | Order in order book.
PartiallyMatched | Order in order book and partially filled.
Matched | Order is filled.
Pending | An order is pending a trigger to be placed in the order book.
Cancelled | Order is cancelled by user.
Replaced | Order is replaced (canceled) by the user.
Rejected | Order rejectd by the system.

