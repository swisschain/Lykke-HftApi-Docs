---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - csharp

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

## Rest API

Classical HTTP based framework that includes working with `HTTP 1.1`, and `JSON`. Rest API allows users just call RPC methods without streaming data from the server.

Usfull tool for manual call gRPC API: [HFR API Swagger](https://hft-apiv2.lykke.com/swagger/ui/index.html)


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

# Data structures

## Decimal type
Here ypu can see: How manage decimal type (Price, Volume, Amount, etc) in API contract.

### gRPC API
In the gRPC API contract decemal type present as custom type `DecimalValue`. The type contains a single property - "value" with a textual representation of the number. This is done in order to avoid problems with non-strict precision "double" type.
In several languages after auto generate client code base on proto files, you can extend `DecimalValue` type to implict convert to decimal (or other analog) type.

### Resp API
In the Rest API contact decemal type present as `number`.
```json
{
    "price": 222231.33420001911,
    "volume": 0.0000001
}
```
## PriceVolume
Price and volume for the level in the orderbook

name | type | description
---- | ---- | -----------
`p` | decimal | price
`v` | decimal | volume

```json
{
    "p": 1.3342,
    "v": 40000.2213341
}
```

## Side
Order side enum
+ `buy` (0)
+ `sell` (1)

```json
{
    "side": "buy"
}
{
    "side": "sell"
}
```

## Trade
Structure to describe a Trade in history.

name|type|description
----|----|-----------
`id`|string|Trade ID.
`orderId`|string|Order ID of this trade.
`assetPairId`|string|Trade asset pair ID.
`index`|number|Index of trade for this order.
`timestamp`|string|Date time of the trade.
`role`|string|Trade role. `Maker` or `Taker`
`price`|decimal|Trade price.
`baseVolume`|decimal|Trade volume in base asset.
`quoteVolume`|decimal|Trade volume in quote asset.
`baseAssetId`|string|Base asset ID.
`quoteAssetId`|string|Quote asset ID.
`fee`|TradeFee|trade Fee

### TradeFee

Structure to describe fee for the trade.

name|type|description
----|----|-----------
`assetId`|string|asset ID for the fee
`size`|decimal|fee size

```json
{
    "id": "111a111aaaa1a",
    "timestamp": "2020-06-22T13:04:27.325Z",
    "assetPairId": "BTCUSD",
    "orderId": "112aabaaa",
    "role": "Taker",
    "price": 9443.234,
    "baseVolume": 1,
    "quoteVolume": 9443.234,
    "baseAssetId": "BTC",
    "quoteAssetId": "USD",
    "fee": {
        "size": 0.01,
        "assetId": "USD"
    }
}
```
