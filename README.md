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
    "id": "5fb21945-d1cd-495e-9cb0-9d087125b471"
}
```

This cloudrpc call represents following function call (over the network).

```rb
doSomething(foo: "bar")
```

|Field  |Type  |Value                                |
|-------|------|-------------------------------------|
|jsonrpc|string|`"2.0"`                              |
|method |string|A method name to be called           |
|params |object|A parameter map to pass to the method|
|id     |string|A lowercased UUID v4 string          |

Interface
---

In Ruby,

```rb
cloudrpc :methodA do |params|
    processed_value_by_methodA = do_something params
    
    methodB processed_value_by_methodA
end

cloudrpc :methodB do |params|
    processed_value_by_methodB = do_something params
    
    methodC processed_value_by_methodB
end

cloudrpc :methodC do |params|
    # Final output of the WorkFlow
    puts params
end
```

Or in Java,

```java
public class SomeWorkFlow extends CloudRPC {
    @RPCMethod
    public void methodA(Map<String, Object> params) {
        // Do something...
    
        call("methodB", params);
    }
    
    @RPCMethod
    public void methodB(Map<String, Object> params) {
        // Do something...
    
        call("methodC", params);
    }
    
    @RPCMethod
    public void methodC(Map<String, Object> params) {
        // Do something...
    
        // Got final output!
    }
}

public class Application {
    public static void main(String[] ARGV) {
        SomeWorkFlow.run();
    }
}
```

These cloudrpc methods are invoked over the network. You can orchestrate a workflow cluster easily!

Since each instance is stateless, you can easily scaleup(down) the cluster. To indicate whether to scale the cluster, just count the messages in the message queue.

References
---

- JSON-RPC 2.0 Specification : http://www.jsonrpc.org/specification
