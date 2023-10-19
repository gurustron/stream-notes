## 4th
- [Enterprise Integration Patterns](https://www.enterpriseintegrationpatterns.com/patterns/messaging/Messaging.html)

## 19th
- Test task. Multiplexing:
  > Application communicates with remote service (RS) by sending requests and receiving responses. A single connection that handles requests and responses is established with the RS. Every request contains a unique Id (Guid) and this id is sent back with the corresponding response. Responses can be served in arbitrary order with arbitrary delays. We need to implement an interface abstracting this RS. `IRequestProcessor.SendAsync` should be thread safe. `ILowLevelNetworkAdapter` and `IHighLevelNetworkAdapter` interfaces are already implemented.
  ```c#
  public sealed record Request(Guid Id);
  public sealed record Response(Guid Id);
  // low-level adapter, ReadAsync and WriteAsync can be called concurrently 
  // can be considered and abstraction over half-duplex socket
  public interface ILowLevelNetworkAdapter
  {
    // reads next response, can not be called multuple times in parallel
    Task<Response> ReadAsync(CancellationToken cancellationToken);
  
    // sends request, can not be called multuple times in parallel
    Task WriteAsync(Request request, CancellationToken cancellationToken);
  }
  
  // high-level adapter with internal queue for messages, similar to RabbitMQ/Kafka clients
  public interface IHighLevelNetworkAdapter
  {
    // Tries to enque the request. Returns false if queue is full, request is not sent
    bool TryEnqueueWrite(Request request, CancellationToken cancellationToken);
  
    // Reads next response, can not be called multuple times in parallel
    Task<Response> ReadAsync(CancellationToken cancellationToken);
  }
  
  // TODO
  public interface IRequestProcessor
  {
    // Starts the processing, the returned Task should finish after initialization is complete
    // Is guaranteed to be called only once at app start 
    Task StartAsync(CancellationToken cancellationToken);
  
    // Gracefull shutdown (waits for all request completion)
    // Is guaranteed to be called only once at app shutdown 
    Task StopAsync(CancellationToken cancellationToken);
  
    // Executes the request, should be threadsafe
    // No guarantee is needed that request would not be send to server on cancellation, but Task should be cancelled.
    Task<Response> SendAsync(Request request, CancellationToken cancellationToken);
  }
  ```    
