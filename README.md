cloudrpc
========

RPC over message queues.

Protocol
---

A cloudrpc call is a message that contains parameters for RPC call. They are queued by message queues such as RabbitMQ, Amazon SQS.
The message contained by a cloudrpc call should conforms JSON-RPC 2.0. (http://www.jsonrpc.org/specification)

```json
{
    "jsonrpc": "2.0",
    "method": "DoSomething",
    "params": {
        "foo": "bar"
    },
    "id": 3
}
```

This cloudrpc call represents following function call.

```rb
doSomething(foo: "bar")
```

References
---

- JSON-RPC 2.0 Specification : http://www.jsonrpc.org/specification
