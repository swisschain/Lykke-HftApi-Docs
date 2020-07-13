# Group Public APIs

## Get all assets

Get a list of supported assets with parameters.

### HTTP Request

`GET /api/assets`

### Response

Array of assets:

Parameter | Default | Description
--------- | ------- | -----------
assetId | string | Asset unique identifier.
name | string | Asset name.
symbol | string | Asset symbol.
accuracy | uint | Maximum number of digits after the decimal point which are supported by the asset.

```json
GET /api/assets

> Response 200 (application/json) - success response

{
    "payload": [
        {
            "assetId": "AUD",
            "name": "AUD",
            "displayName": "AUD",
            "accuracy": 2
        },
        {
            "assetId": "BTC",
            "name": "BTC",
            "displayName": "BTC",
            "accuracy": 8
        }
    ]
}
```

```protobuf

package hft;
service PublicService {
  rpc GetAssets (google.protobuf.Empty) returns (AssetsResponse);
}

message AssetsResponse {
    repeated Asset payload = 1;
    hft.common.Error error = 2;
}
```

## Get Asset by ID

Get inforomation about specific asset.

### HTTP Request

`GET /api/assets/{assetId}`

### Query Parameters

Parameter | Type | Place | Description
--------- | ---- | ----- | -----------
assetId | string | path | Asset uniquie ID

### Response

Asset description:

Property | Type | Description
-------- | ---- | -----------
assetId | string | Asset unique identifier.
name | string | Asset name.
symbol | string | Asset symbol.
accuracy | uint | Maximum number of digits after the decimal point which are supported by the asset.

> Response 200 (application/json) - success response

```json
{
    "payload": {
        "assetId": "AUD",
        "blockchainId": "AUD",
        "name": "AUD",
        "displayName": "AUD",
        "accuracy": 2
    }
}
```

> Response 200 (application/json) - error code

```RestAPI
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

## Get all asset pairs

Get all supported asset pairs (symbols).

### HTTP Request

`GET /api/assetpairs`

### Response

Array of trading instruments.

Property | Type | Description
-------- | ---- | -----------
assetPairId | string | Symbol unique identifier.
baseAssetId | string | Base asset unique identifier.
quoteAssetId | string | Quote asset unique identifier.
name | string | Trading instrument name.
priceAccuracy | uint | Trading instrument price accuracy.
baseAssetAccuracy | uint | Base asset accuracy.
quoteAssetAccuracy | uint | Quote asset accuracy.
minVolume | [decimal](#decimal-type) | Minimum order volume in base currency.
minOppositeVolume | [decimal](#decimal-type) | Minimum order volume in quote currency.

> Response 200 (application/json) - success response

```json
{
  "payload": [
    {
      "assetPairId": "BTCUSD",
      "baseAssetId": "BTC",
      "quoteAssetId": "91960f78-7ea1-49f7-be27-34221a763b02",
      "name": "BTC/USD",
      "priceAccuracy": 3,
      "baseAssetAccuracy": 8,
      "quoteAssetAccuracy": 2,
      "minVolume": 0.0001,
      "minOppositeVolume": 1
    },
    {
      "assetPairId": "BTCLKK1Y",
      "baseAssetId": "BTC",
      "quoteAssetId": "LKK1Y",
      "name": "BTC/LKK1Y",
      "priceAccuracy": 2,
      "baseAssetAccuracy": 8,
      "quoteAssetAccuracy": 2,
      "minVolume": 0.0001,
      "minOppositeVolume": 4
    }
  ]
}
```

## Get a specific asset pair.

Get a specific asset pair(symbol).

### HTTP Request

`GET /api/assetpairs/{assetPairId}`

### Query Parameters

Parameter | Type | Place | Description
--------- | ---- | ----- | -----------
assetPairId | string | path | Symbol unique ID

### Responce

Trading instrument:

Property | Type | Description
-------- | ---- | -----------
assetPairId | string | Symbol unique identifier.
baseAssetId | string | Base asset unique identifier.
quoteAssetId | string | Quote asset unique identifier.
name | string | Trading instrument name.
priceAccuracy | uint | Trading instrument price accuracy.
baseAssetAccuracy | uint | Base asset accuracy.
quoteAssetAccuracy | uint | Quote asset accuracy.
minVolume | [decimal](#decimal-type) | Minimum order volume in base currency.
minOppositeVolume | [decimal](#decimal-type) | Minimum order volume in quote currency.

> Response 200 (application/json) - success response

```json
{
  "payload":
    {
      "assetPairId": "BTCUSD",
      "baseAssetId": "BTC",
      "quoteAssetId": "91960f78-7ea1-49f7-be27-34221a763b02",
      "name": "BTC/USD",
      "priceAccuracy": 3,
      "baseAssetAccuracy": 8,
      "quoteAssetAccuracy": 2,
      "minVolume": 0.0001,
      "minOppositeVolume": 1
    }
}
```

## Asset Pair Order Book Ticker

Get the order book by asset pair. The order books contain a list of Buy(Bid) and Sell(Ask) orders with their corresponding price and volume.

### HTTP Request

`GET /api/orderbooks`

`GET /api/orderbooks?assetPairId={assetPairId}&depth={4}`

### Query Parameters

Parameter | Type | Place | Description
--------- | ---- | ----- | -----------
assetPairId | string | query | *(Optional)* Identificator of specific symbol. By default return all sumbols.
depth | uint | query | *(Optional)* How many best levels need to include in order books. By default include all levels.

### Response

Array of order books by instruments:

Property | Type | Description
-------- | ---- | -----------
assetPairId | string | Symbol unique identifier.
timestamp | [TimeStamp](#timestamp-type) | Timestamp of last order book update.
bids | Array of PriceLevel | List of buy orders.
asks | Array of PriceLevel | List of sell orders.

**PriceLevel**:

Property | Type | Description
-------- | ---- | -----------
p | [decimal](#decimal-type) | Order price indicated in quoted asset per unit of base asset.
v | [decimal](#decimal-type) | Order volume indicated in base asset.

> Response 200 (application/json) - success response

```json
{
  "payload": [
    {
      "assetPairId": "BTCUSD",
      "timestamp": "2020-06-23T07:11:48.299Z",
      "bids": [
        {
          "v": 0.06771538,
          "p": 9599
        },
        {
          "v": 0.05210805,
          "p": 9599
        }
      ],
      "asks": [
        {
          "v": -1.67230494,
          "p": 9633.613
        },
        {
          "v": -0.1,
          "p": 9641.42
        }
      ]
    }
  ]
}
```

## 24hr Ticker Price Change Statistics

24 hour rolling-window price change statistics.

### HTTP Request

`GET /api/tickers`

`GET /api/tickers?assetPairIds=BTCUSD&assetPairIds=BTCEUR`

### Query Parameters


Parameter | Type | Place | Description
--------- | ---- | ----- | -----------
assetPairIds | array of strings | query | *(Optional)* Filter by symbols(returns all asset pair information by default).

### Response

Asset description:

Property | Type | Description
-------- | ---- | -----------
assetPairId | string | Symbol unique identifier.
volumeBase | [decimal](#decimal-type) | Trading volume for last 24h in base asset.
volumeQuote | [decimal](#decimal-type) | Trading volume for last 24h in quote asset.
priceChange | [decimal](#decimal-type) | Price changes(in %) in the last 24h.
lastPrice | [decimal](#decimal-type) | The last trade price.
high | [decimal](#decimal-type) | The maximum trade price from last 24h.
low | [decimal](#decimal-type) | The minimum trade price from last 24h.
timestamp | [TimeStamp](#timestamp-type) | Last update timestamp.

> Response 200 (application/json) - success response

```json
{
  "payload": [
    {
      "assetPairId": "BTCEUR",
      "volumeBase": 0.98657285,
      "volumeQuote": 8350.5868,
      "priceChange": 0.036057567781542336,
      "lastPrice": 8620,
      "high": 8620,
      "low": 8320,
      "timestamp": "2020-06-22T21:10:06.5623911Z"
    },
    {
      "assetPairId": "BTCUSD",
      "volumeBase": 2.15139075,
      "volumeQuote": 20649.0296,
      "priceChange": 0.023048622404312356,
      "lastPrice": 9637.117,
      "high": 9700,
      "low": 9397.999,
      "timestamp": "2020-06-23T07:08:07.3753293Z"
    }
  ]
}
```



