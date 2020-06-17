---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - csharp

toc_footers:

includes:
  - errors

search: true
---

# Lykke trading API

Lykke trading api.

# API usage

## Allowed HTTP Verbs
- `PUT` : Updates a resource 
- `POST` : Creates a resource
- `GET` : Gets a resource or list of resources
- `DELETE` : Deletes a resource

## Description Of Usual Server Responses
- 200 `OK` : the request was successful.
- 401 `Unauthorized` : authentication failed.
- 404 `Not Found` : endpoint was not found.
- 500 `Internal Server Error` : server error.

## Response structure

Every response contains two fields - `payload` and `error`. Successful response will contain response data in the `payload` field and *null* in `error` field and vise versa for the error response.

### Successful response

```json
{
    "payload": {
        "orderId": "a0e83da9-4a69-4ce4-9a42-6443ed1b45c0"
    },
    "error": null
}
```

### Eror response

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

## Data structures

### PriceVolume
Price and volume for the level in the orderbook
+ `p` - price
+ `v` - volume

### Side
Order side
+ buy
+ sell

### Fee
+ assetId (string) - asset ID for the fee
+ size (number) - fee size

### Trade
+ id (string) - Trade ID.
+ orderId (string) - Order ID of this trade.
+ assetPairId (string) - Trade asset pair ID.
+ index (number) - Index of trade for this order.
+ timestamp (string) - Date time of the trade.
+ role (string) - Trade role. `Maker` or `Taker`
+ price (number) - Trade price.
+ baseVolume (number) - Trade volume in base asset.
+ quoteVolume (number) - Trade volume in quote asset.
+ baseAssetId (string) - Base asset ID.
+ quoteAssetId (string) - Quote asset ID.
+ fee (Fee) - trade Fee

# Group Public APIs

## Get all assets

Asset description:

Parameter | Default | Description
--------- | ------- | -----------
assetId | string | Asset unique identifier.
name | string | Asset name.
symbol | string | Asset symbol.
accuracy | number | Maximum number of digits after the decimal point which are supported by the asset.

### HTTP Request

`GET /api/assets`

> Response 200 (application/json)

```json
{
    "payload": [
        {
            "assetId": "AUD",
            "blockchainId": "AUD",
            "name": "AUD",
            "displayName": "AUD",
            "accuracy": 2
        },
        {
            "assetId": "BTC",
            "blockchainId": "BTC",
            "name": "BTC",
            "displayName": "BTC",
            "accuracy": 8
        }
    ],
    "error": null
}
```

## Get a asset by ID

### HTTP Request

`GET /api/assets/{assetId}`

Asset description:

property | type | description
---------- | -------
assetId | string | Asset unique identifier.
name | string | Asset name.
symbol | string | Asset symbol.
accuracy | number | Maximum number of digits after the decimal point which are supported by the asset.

### Query Parameters

Parameter | Type | Description
--------- | ------- | -----------
assetId | string | Asset qniquie ID

> Response 200 (application/json) - success responce

```json
{
    "payload": {
        "assetId": "AUD",
        "blockchainId": "AUD",
        "name": "AUD",
        "displayName": "AUD",
        "accuracy": 2
    },
    "error": null
}
```

> Response 200 (application/json) - error code
```json
{
  "Error": {
    "Code": 1100,
    "Message": "Asset not found",
    "Fields": {
      "assetId": "Asset not found"
    }
  }
}
```


