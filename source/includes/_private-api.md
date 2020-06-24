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
available | [decimal](#decimal-type) | Available amount
reserved | [decimal](#decimal-type) | Amount reserved in active orders
timestamp | [TimeStamp](#timestamp-type) | Last update balance by current asset


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

Return a history of trades by an account or by a single order.

### HTTP Request

`GET /api/trades`

`GET /api/trades/order/{orderId}`

### Query Parameters for get trades by account

Parameter | Type | Place | Description
--------- | ---- | ----- | -----------
assetPairId | string | query | *(optional)* Symbol unique identifier.
side | string | query | *(optional)* Side of trade: `buy` or `sell`
offset | uint | query | *(optional)* Skip the specified number of elements
take | uint | query | *(optional)* Take the specified number of elements
from | [TimeStamp](#timestamp-type) | query | *(optional)* From timestamp
to | [TimeStamp](#timestamp-type) | query | *(optional)* To timestamp

### Query Parameters for get trades by single order

Parameter | Type | Place | Description
--------- | ---- | ----- | -----------
orderId | string | path | Unique Order ID

### Responce

Arry of trades:

Property | Type | Description
-------- | ---- | -----------
id | string | Trade ID.
orderId | string | Order ID of this trade.
assetPairId | string | Trade asset pair ID (symbol).
index | number | Index of trade for this order.
timestamp | [TimeStamp](#timestamp-type) | Trade tamestamp.
role | string | Trade role. `Maker` or `Taker`
price | [decimal](#decimal-type) | Trade price.
baseVolume | [decimal](#decimal-type) | Trade volume in base asset.
quoteVolume | [decimal](#decimal-type) | Trade volume in quote asset.
baseAssetId | string | Base asset ID.
quoteAssetId | string | Quote asset ID.
fee | TradeFee | *(optional)* Trade Fee description

**TradeFee**
Property | Type | Description
-------- | ---- | -----------
assetId | string | Asset ID for the fee
size | [decimal](#decimal-type) | Fee size

> Response 200 (application/json) - success responce

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

## Get active or closed orders

Return active orders or return closed orders fromm history.

### HTTP Request

`GET /api/orders/active`

`GET /api/orders/closed`

### Query Parameters


Parameter | Type | Place | Description
--------- | ---- | ----- | -----------
assetPairId | string | query | *(optional)* Symbol unique identifier.
offset | uint | query | *(optional)* Skip the specified number of elements
take | uint | query | *(optional)* Take the specified number of elements


### Responce

Asset description:

Property | Type | Description
-------- | ---- | -----------
id | strin | Unique Order ID
timestamp |  [TimeStamp](#timestamp-type) | Timestamp for creating an order
lastTradeTimestamp | [TimeStamp](#timestamp-type) | Timestamp for last trade by order
status | string | Order status. List of statuses: [here](#order-statuses).
assetPairId | string | Symbol unique identifier.
type | string | Order type: `Market` or `Limit`
side | string | Order side: `Sell` or `Buy`
price | [decimal](#decimal-type) | Order price (in quote asset for one unit of base asset)
volume | [decimal](#decimal-type) | Order volume (in base asset)
filledVolume | [decimal](#decimal-type) | Order filled volume (in base asset)
remainingVolume | [decimal](#decimal-type) | Order remaining volume (in base asset)

> Response 200 (application/json) - success responce

```json
{
  "payload": [
    {
      "id": "0c336213-0a64-44a8-9599-e88bf6aa1b69",
      "timestamp": 1592928606187,
      "lastTradeTimestamp": null,
      "status": "Placed",
      "assetPairId": "BTCUSD",
      "type": "Limit",
      "side": "Buy",
      "price": 4000,
      "volume": 0.0001,
      "filledVolume": 0,
      "remainingVolume": 0.0001
    }
  ]
}
```

## Make a limit order

Make a new limit order.

### HTTP Request

`POST /api/orders/limit`

### Request

Parameter | Type | Place | Description
--------- | ---- | ----- | -----------
assetPairId | string | body | Symbol unique identifier.
side | string | body | Order side: `Sell` or `Buy`
volume | [decimal](#decimal-type) | body | Order volume (in base asset)
price | [decimal](#decimal-type) | body | Order price, in quote asset for one unit of base asset)
  
> Request to create limit order

```json
{
  "assetPairId": "BTCUSD",
  "side": "buy",
  "volume": 0.0001,
  "price": 4000
}
```

### Responce

Operation result description:

Property | Type | Description
-------- | ---- | -----------
orderId | string | Unique order ID

> Response 200 (application/json) - success responce

```json
{
  "payload": {
    "orderId": "0c336213-0a64-44a8-9599-e88bf6aa1b69"
  },
  "error": null
}
```

## Make a market order 

Request to create Fill-Or-Kill market order.

### HTTP Request

`POST /api/orders/market`

### Request

Parameter | Type | Place | Description
--------- | ---- | ----- | -----------
assetPairId | string | body | Symbol unique identifier.
side | string | body | Order side: `Sell` or `Buy`
volume | [decimal](#decimal-type) | body | Order volume (in base asset)

> Request to create market order

```json
{
  "assetPairId": "BTCUSD",
  "side": "Buy",
  "volume": 1.554
}
```

### Responce

Operation result description:

Property | Type | Description
-------- | ---- | -----------
orderId | string | Unique order ID
price | [decimal](#decimal-type) | Market order result price

> Response 200 (application/json) - success responce

```json
{
  "payload": {
    "orderId": "string",
    "price": 6445.222311
  }
}
```




## Mass cancel orders

Cancel all active order or filter order to cancel by AssetPair or Side.

### HTTP Request

`DELETE /api/orders`

### Query Parameters

Parameter | Type | Place | Description
--------- | ---- | ----- | -----------
assetPairId | string | query | *(Optional)* Symbol unique identifier. By defaul all asset pairs.
side | string | query | *(Optional)* Order side `Buy` or `Sell`. By default both side.


### Responce

No content

> Response 200 (application/json) - success responce

```json
{
  "payload": null
}
```

## Cancel orders by ID

Cancel a specific order by order ID

### HTTP Request

`DELETE /api/orders/{orderId}`

### Query Parameters


Parameter | Type | Place | Description
--------- | ---- | ----- | -----------
orderId | string | path | Unique Order ID

### Responce

No content

> Response 200 (application/json) - success responce

```json
{
  "payload": null
}
```





