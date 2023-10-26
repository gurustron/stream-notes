## 26th

- [DistributedLock](https://github.com/madelson/DistributedLock):
  DistributedLock is a .NET library that provides robust and easy-to-use distributed mutexes, reader-writer locks, and semaphores based on a variety of underlying technologies.
- [Stryker.NET](https://github.com/stryker-mutator/stryker-net):
  Stryker offers mutation testing for your .NET Core and .NET Framework projects. It allows you to test your tests by temporarily inserting bugs in your source code.

## 19th

#### Test task. Multiplexing:

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
##### Simple version of the task:
1. You can use `IHighLevelNetworkAdapter`, i.e., delegate sending to the queue on `NetworkAdapter`.
2. You can omit `CancellationToken` handling in the `IRequestProcessor` methods.
3. You can omit `IRequestProcessor.StopAsync` implementation.
```c#
public sealed class SimpleRequestProcessor : IRequestProcessor
{
  private readonly IHighLevelNetworkAdapter _networkAdapter;

  public SimpleRequestProcessor(IHighLevelNetworkAdapter networkAdapter)
  {
    _networkAdapter = networkAdapter;
  }

  public Task StartAsync(CancellationToken cancellationToken)
  {
    // CancellationToken can be unused
    throw new NotImplementedException();
  }

  public Task StopAsync(CancellationToken cancellationToken)
  {
    // This method can be left unimplemented
    return Task.CompletedTask;
  }

  public Task<Response> SendAsync(Request request, CancellationToken cancellationToken)
  {
    // CancellationToken can be unused
    throw new NotImplementedException();
  }
}
```

##### Medium complexity:
1. You can use only `ILowLevelNetworkAdapter`.
2. You need to implement handling of `CancellationToken`.
3. You can omit `IRequestProcessor.StopAsync` implementation.

```c#
public sealed class MediumRequestProcessor : IRequestProcessor
{
  private readonly ILowLevelNetworkAdapter _networkAdapter;

  public MediumRequestProcessor(ILowLevelNetworkAdapter networkAdapter)
  {
    _networkAdapter = networkAdapter;
  }

  public Task StartAsync(CancellationToken cancellationToken)
  {
    throw new NotImplementedException();
  }

  public Task StopAsync(CancellationToken cancellationToken)
  {
    // This method can be left unimplemented
    return Task.CompletedTask;
  }

  public Task<Response> SendAsync(Request request, CancellationToken cancellationToken)
  {
    throw new NotImplementedException();
  }
}
```

##### Complex version of the task:

1. You can use only `ILowLevelNetworkAdapter`.
2. You need to implement handling of `CancellationToken`.
3. You need to implement `StopAsync`, which waits for responses for already sent requests (as long as the `CancellationToken` passed to `StopAsync` is not cancelled).
4. You need to implement a configurable timeout: if a response to a specific request is not received within the specified period of time, cancel the task returned from `SendAsync`. This should also handle the scenario where a response to a request never arrives; the global timeout should trigger to prevent memory leaks.

```c#
public sealed class ComplexRequestProcessor : IRequestProcessor
{
  private readonly ILowLevelNetworkAdapter _networkAdapter;
  private readonly TimeSpan _requestTimeout;

  public ComplexRequestProcessor(ILowLevelNetworkAdapter networkAdapter, TimeSpan requestTimeout)
  {
    if (requestTimeout <= TimeSpan.Zero)
      throw new ArgumentOutOfRangeException(nameof(requestTimeout));

    _networkAdapter = networkAdapter;
    _requestTimeout = requestTimeout;
  }

  public async Task StartAsync(CancellationToken cancellationToken)
  {
    throw new NotImplementedException();
  }

  public async Task StopAsync(CancellationToken cancellationToken)
  {
    throw new NotImplementedException();
  }

  public async Task<Response> SendAsync(Request request, CancellationToken cancellationToken)
  {
    throw new NotImplementedException();
  }
}
```

## 4th
- [Enterprise Integration Patterns](https://www.enterpriseintegrationpatterns.com/patterns/messaging/Messaging.html)
