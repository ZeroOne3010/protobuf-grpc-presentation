# Protocol Buffers and gRPC

Ville Saalo

2024-09-20


# Background

I've created tons of JSON APIs and clients for JSON APIs.

**Problem**

- There's no good way to define a schema for a JSON API -> no automatic code generation
  - OpenAPI: auto-generation rarely succeeds; disconnected from the actual workings
  - JSON Schema: nobody uses it; also disconnected


# Solution

Ditch JSON, use **Protocol Buffers** instead (just _Protobuf_ with friends)!


# What Protobuf?

- Free and open binary data serialization format
- By Google, internally 2001, made public in 2008
- Definition and documentation is the *same thing*


# Example definition

`ironbank.proto`:

```protobuf
syntax = "proto3";
import "google/protobuf/timestamp.proto";

enum ItemCategory {
  UNDEFINED = 0;
  COMPUTER = 1;
  DISPLAY = 2;
  PHONE = 3;
}

message Item {
  ItemCategory category = 1;
  string description = 2;
  google.protobuf.Timestamp purchase_date = 3;
  optional string comment = 4;
  uint32 price_in_cents = 5;
}
```


# Example message

```json
{
"category":"COMPUTER",
"description":"Macbook Pro",
"purchase_date":{"epoch_seconds":1726829939},
"comment":"Core tool",
"price_in_cents":312000
}
```
In base64 encoded Protobuf: `CAESC01hY2Jvb2sgUHJvGgAiCUNvcmUgdG9vbCjAhRM=` or in binary (`hexdump`):

```
00000000  08 01 12 0b 4d 61 63 62  6f 6f 6b 20 50 72 6f 1a  |....Macbook Pro.|
00000010  00 22 09 43 6f 72 65 20  74 6f 6f 6c 28 c0 85 13  |.".Core tool(...|
```


# gRPC

- Cross-platform high-performance remote procedure call (RPC) framework
- Published in 2016
- Best way to use Protobuf in an API
- API defined using Protobuf too 🤯
- Officially support for a ~dozen programming languages
- The "g" originally (in v1.0) stood for "gRPC Remote Procedure Calls", nowadays it's [different for each version](https://github.com/grpc/grpc/blob/master/doc/g_stands_for.md)


# gRPC definition example

```protobuf
service IronBank {
  rpc AddItem (Item) returns (AddItemResponse) {}
}

message AddItemResponse {
    Item item = 1;
    // other fields
}
```


# Tooling

- Great tooling support
  - For example, IntelliJ Idea and Postman already support gRPC/Protobuf
  - Code generators available for Java, Kotlin, Node, ...


# Where to go from here?

**Protobuf**
- Language Guide (proto 3): https://protobuf.dev/programming-guides/proto3/
- Proto Best Practices: https://protobuf.dev/programming-guides/dos-donts/
- API Best Practices: https://protobuf.dev/programming-guides/api/

**gRPC**
- gRPC: https://grpc.io/


# Thank you!