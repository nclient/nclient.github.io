# Resilience
To make your client resilient to failures, you should specify resilience provider by using `WithResiliencePolicy` method during the client creation. 
By default, the provider that contains [Polly](https://github.com/App-vNext/Polly) library is used. So, you can create a policy using Polly library:

```ruby
IAsyncPolicy policy = Policy
    .Handle<Exception>()
    .WaitAndRetryAsync(maxRetryAttempts: 3, retryAttempt => TimeSpan.FromSeconds(Math.Pow(2, retryAttempt)));

IProductServiceClient client = NClientProvider
    .Use<IProductServiceClient>(host: "http://localhost:8080")
    .WithResiliencePolicy(policy)
    .Build();
```

But you can create your own implementation of `IResiliencePolicyProvider` and pass it to `WithResiliencePolicy` method.
