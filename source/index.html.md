---
title: API Reference

language_tabs: # must be one of https://git.io/vQNgJ
  - json
  - protobuf

toc_footers:

includes:
  - api_usage.md
  - change-log.md
  - public-api.md
  - private-api.md
  - errors  

search: true
---

# Lykke trading API

Lykke high-frequency trading API.

This API allows Lykke clients to carry out automated trading on special sub-accounts. Also, the API allows you to get public data about the Lykke market, such as current prices, order books, and other parameters for trading instruments.

[www.lykke.com](https://www.lykke.com/)

[Lykke Web Wallet](https://wallet.lykke.com/)

# Protocol description

Lykke HFT API allows users to use 2 kinds of protocol `gRPC API` and `Rest API`. 

Both kinds of APIs support the same methods with the same data format. But only gRPC API supports receiving streaming data from the platform.

## gRPC API

The gRCP API utilizes a new generation of RPC framework that includes working with `HTTP 2`, `ProtoBuf`, and `WebSocket` for faster and more efficient interaction with the platform. The `gRPC framework` uses the declarative description of the contract in the `.proto` files. On the basis of which you can easily generate a client library in many [programming languages](https://grpc.io/docs/languages/). Also, the `gRPC framework` supports working with `stream`, which makes it easy to receive events from the server.

**Lykke team strongly recommend using gRPC API protocol to communicate with the platform**

Read more about `gRPC framework`: [https://grpc.io/](https://grpc.io/)

A useful tool for manual gRPC API requests: [BloomRPC](https://github.com/uw-labs/bloomrpc)

**API Endpoint**: 

* https://hft-apiv2-grpc.lykke.com - url format
* `hft-apiv2-grpc.lykke.com:443`   - host format

**Proto files**

* [common.proto](https://github.com/swisschain/Lykke-HftApi-Docs/blob/master/grpc_proto_contracts/common.proto) :Common data models
* [isalive.proto](https://github.com/swisschain/Lykke-HftApi-Docs/blob/master/grpc_proto_contracts/isalive.proto) : Is Alive API service
* [publicService.proto](https://github.com/swisschain/Lykke-HftApi-Docs/blob/master/grpc_proto_contracts/publicService.proto) : Public API service
* [privateService.proto](https://github.com/swisschain/Lykke-HftApi-Docs/blob/master/grpc_proto_contracts/privateService.proto) : Private API service

## Rest API

The rest API uses the classic HTTP based framework that includes working with `HTTP 1.1`, and `JSON`. Rest API allows users to just call RPC methods without streaming data from the server.

A useful tool for manual rest API requests: [HFT API Swagger](https://hft-apiv2.lykke.com/swagger/ui/index.html)

**API Endpoint**: 

- https://hft-apiv2.lykke.com/api

