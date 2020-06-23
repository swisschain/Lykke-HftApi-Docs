# Group Private APIs

This method group requires API Key Authentication.

## Get current balance

Get current balance from current account.

### HTTP Request

`GET /api/balance`

### Responce

Array of balance by asset.

Property | Type | Description
-------- | ---- | -----------
assetId | string | Asset unique identifier.
available | decimal | Available amount
reserved | decimal | Amount reserved in active orders
timestamp | TimeStamp | Last update balance by current asset


> Response 200 (application/json) - success responce

```json
{
  "payload": [
    {
      "assetId": "BTC",
      "available": 2.433002,
      "reserved": 0.35,
      "timestamp": 1592928606187
    },
    {
      "assetId": "USD",
      "available": 0,
      "reserved": 0,
      "timestamp": 1592928506187
    },
  ]
}
```

## Get trade history

### HTTP Request

`GET /api/trades`

### Query Parameters

Parameter | Type | Place | Description
--------- | ---- | ----- | -----------
assetPairId | string | query | *(optional)* Symbol unique identifier.
side | string | query | *(optional)* Side of trade: `buy` or `sell`
offset | uint | query | *(optional)* Skip the specified number of elements
take | uint | query | *(optional)* Take the specified number of elements
from | TimeStamp | query | *(optional)* From timestamp
to | TimeStamp | query | *(optional)* To timestamp

### Responce

Arry of trades:

Property | Type | Description
-------- | ---- | -----------
id | string | Trade ID.
orderId | string | Order ID of this trade.
assetPairId | string | Trade asset pair ID (symbol).
index | number | Index of trade for this order.
timestamp | TimeStamp | Trade tamestamp.
role | string | Trade role. `Maker` or `Taker`
price | decimal | Trade price.
baseVolume | decimal | Trade volume in base asset.
quoteVolume | decimal | Trade volume in quote asset.
baseAssetId | string | Base asset ID.
quoteAssetId | string | Quote asset ID.
fee | TradeFee | *(optional)* Trade Fee description

**TradeFee**
Property | Type | Description
----|----|-----------
assetId|string|asset ID for the fee
size|decimal|fee size

```json
{
  "payload": [
    {
      "id": "b3a25228-5384-4b5f-95c3-3eb31f7e9aee",
      "timestamp": 1592938116360,
      "assetPairId": "BTCUSD",
      "orderId": "616374a2-02d0-4f01-8dce-6266fc30d4a1",
      "role": "Taker",
      "price": 9575.823,
      "baseVolume": 0.0001,
      "quoteVolume": 0.9576,
      "baseAssetId": "BTC",
      "quoteAssetId": "USD",
      "fee": null
    },
    {
      "id": "ebceb096-7766-437a-8e98-e1f6532f0268",
      "timestamp": 1592938016360,
      "assetPairId": "BTGUSD",
      "orderId": "fa19c315-b8b2-49bd-a426-45f9384fbad3",
      "role": "Taker",
      "price": 8.5,
      "baseVolume": 0.01,
      "quoteVolume": 0.085,
      "baseAssetId": "a4954205-48eb-4286-9c82-07792169f4db",
      "quoteAssetId": "USD",
      "fee": null
    }]
}
```

----------

## METHOD-NAME

### HTTP Request

`GET /api/assets/{assetId}`

### Query Parameters


Parameter | Type | Place | Description
--------- | ---- | ----- | -----------


### Responce

Asset description:

Property | Type | Description
-------- | ---- | -----------



## METHOD-NAME

### HTTP Request

`GET /api/assets/{assetId}`

### Query Parameters


Parameter | Type | Place | Description
--------- | ---- | ----- | -----------


### Responce

Asset description:

Property | Type | Description
-------- | ---- | -----------



## METHOD-NAME

### HTTP Request

`GET /api/assets/{assetId}`

### Query Parameters


Parameter | Type | Place | Description
--------- | ---- | ----- | -----------


### Responce

Asset description:

Property | Type | Description
-------- | ---- | -----------



## METHOD-NAME

### HTTP Request

`GET /api/assets/{assetId}`

### Query Parameters


Parameter | Type | Place | Description
--------- | ---- | ----- | -----------


### Responce

Asset description:

Property | Type | Description
-------- | ---- | -----------



## METHOD-NAME

### HTTP Request

`GET /api/assets/{assetId}`

### Query Parameters


Parameter | Type | Place | Description
--------- | ---- | ----- | -----------


### Responce

Asset description:

Property | Type | Description
-------- | ---- | -----------



