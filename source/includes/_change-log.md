# Change Log

## 2020-07-01 - HFT API V2 Release.

Support **gRPC** protocol.
Communication uses the HTTP 2.0 standard, WebSocket, ProtoBuf, which helps to reduce latency and more efficiently use CPU and network resources on the client-side. It also makes it easy to integrate APIs in many programming languages uses proto-files to generate a client library. [Proto files with contracts](https://github.com/swisschain/Lykke-HftApi-Docs/tree/master/grpc_proto_contracts).

A new response format from the server, abstracted from the protocol. Allows you to work in a custom manner with the API regardless of the protocol.

Contracts in the RestAPI protocol are updated. A set of functions and models is used similar to **gRPC** protocol. Remove deprecated methods and added missing methods [Swagger UI](https://hft-apiv2.lykke.com/swagger/ui/index.html)

Added the ability to receive a market data stream from the server using ** gRPC ** protocol:

* Order books - Getting a snapshot and changes in the order book
* Prices - Get a snapshot of the current prices and change in prices
* 24hr Ticker Statistics - Get a snapshot of current ticker statistics and a stream of data changes

Added the ability to receive data flow by account from the server using **gRPC** protocol:

* Balances - Get a snapshot of account balances and the flow of account balance changes
* Transactions - Flow of completed trades on an account
* Orders - Flow of changes in active orders on an account

Performance optimization - reducing data backlogs: prices, order books, balances, active orers.

API Keys management optimization, increased security

* receiving the secret key of HFT account only after creating or regenerating the key
* secret key of HFT account regenerating only when using 2FA
* funds transfer between HFT, Trading accounts only when using 2FA

Optimization of internal services, bug fixes.



