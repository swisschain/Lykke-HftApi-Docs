# Group Public APIs

## Get all assets

Get list of supporter asset with parameters.

### HTTP Request

`GET /api/assets`

### Responce

Array of assets:

Parameter | Default | Description
--------- | ------- | -----------
assetId | string | Asset unique identifier.
name | string | Asset name.
symbol | string | Asset symbol.
accuracy | number | Maximum number of digits after the decimal point which are supported by the asset.

> Response 200 (application/json)

```json
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

## Get an asset by ID

Get info about specific asset.

### HTTP Request

`GET /api/assets/{assetId}`

### Query Parameters

Parameter | Type | Place | Description
--------- | ---- | ----- | -----------
assetId | string | path | Asset uniquie ID

### Responce

Asset description:

Property | Type | Description
-------- | ---- | -----------
assetId | string | Asset unique identifier.
name | string | Asset name.
symbol | string | Asset symbol.
accuracy | number | Maximum number of digits after the decimal point which are supported by the asset.

> Response 200 (application/json) - success responce

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

## Get all symbols

Get all supported symbols (asset pairs).

### HTTP Request

`GET /api/assetpairs`

### Responce

Array of trading instruments.

Property | Type | Description
-------- | ---- | -----------
assetPairId | string | Symbol unique identifier.
baseAssetId | string | Unique identifier of base asset.
quoteAssetId | string | Unique identifier of quote asset.
name | string | Name of the trading instrument.
priceAccuracy | 0 | Trading instrument price accuracy.
baseAssetAccuracy | 0 | Base asset accuracy.
quoteAssetAccuracy | 0 | Quote asset accuracy.
minVolume | 0 | Minimum order volume in base currency
minOppositeVolume | 0 | Minimum order volume in quote currency

> Response 200 (application/json) - success responce

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

## Get a specific symbol

Get a specific symbol (asset pair).

### HTTP Request

`GET /api/assetpairs/{assetPairId}`

### Query Parameters

Parameter | Type | Place | Description
--------- | ---- | ----- | -----------
assetPairId | string | path | Sumbol uniquie ID

### Responce

Trading instrument:

Property | Type | Description
-------- | ---- | -----------
assetPairId | string | Symbol unique identifier.
baseAssetId | string | Unique identifier of base asset.
quoteAssetId | string | Unique identifier of quote asset.
name | string | Name of the trading instrument.
priceAccuracy | 0 | Trading instrument price accuracy.
baseAssetAccuracy | 0 | Base asset accuracy.
quoteAssetAccuracy | 0 | Quote asset accuracy.
minVolume | 0 | Minimum order volume in base currency
minOppositeVolume | 0 | Minimum order volume in quote currency

> Response 200 (application/json) - success responce

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

## Symbol Order Book Tiker

Get order books by symbols. Order books contain a list of offers on buying and selling with price and volume.

### HTTP Request

`GET /api/orderbooks`

`GET /api/orderbooks?assetPairId={assetPairId}&depth={4}`

### Query Parameters

Parameter | Type | Place | Description
--------- | ---- | ----- | -----------
assetPairId | string | query | *(Optional)* Identificator of specific symbol. By default return all sumbols.
depth | uint | query | *(Optional)* How many best levels need to include in order books. By default include all levels.

### Responce

Array of order books by instruments:

Property | Type | Description
-------- | ---- | -----------
assetPairId | string | Symbol unique identifier.
timestamp | uint64 | Timestamp of last order book update.
bids | Array of PriceLevel | List of byuing offers
asks | Array of PriceLevel | List of selling offers

**PriceLevel**:

Property | Type | Description
-------- | ---- | -----------
p | decimal | Order price, indicated in quoted asset per unit of base asset.
v | decimal | Order volume, indicated in base asset.

> Response 200 (application/json) - success responce

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

24 hour rolling window price change statistics.

### HTTP Request

`GET /api/tickers`

`GET /api/tickers?assetPairIds=BTCUSD&assetPairIds=BTCEUR`

### Query Parameters


Parameter | Type | Place | Description
--------- | ---- | ----- | -----------
assetPairIds | array of strings | query | *(Optional)* Filter by symbols. By default return information by all symbols.

### Responce

Asset description:

Property | Type | Description
-------- | ---- | -----------
assetPairId | string | Symbol unique identifier
volumeBase | decimal | Trading volume for last 24h in base asset
volumeQuote | decimal | Trading volume for last 24h in quote asset
priceChange | decimal | Price changes in percentage in the last 24h
lastPrice | decimal | The last trade price
high | decimal | The max trade price from last 24h
low | decimal | The min trade price from last 24h
timestamp | string | Last update timestamp

> Response 200 (application/json) - success responce

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



