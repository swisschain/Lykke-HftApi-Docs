# Group Public APIs

## Get all assets

### HTTP Request

`GET /api/assets`

### Responce

Asset description:

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

### Query Parameters

Parameter | Type | Description
--------- | ---- | -----------
assetId | string | Asset qniquie ID

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
