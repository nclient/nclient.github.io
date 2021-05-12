# Resilience
To make your client resilient to failures, you should specify resilience provider by using `WithResiliencePolicy` method during the client creation. 
By default, the provider that contains [Polly](https://github.com/App-vNext/Polly) library is used. So, you can create a policy using Polly library:

```ruby
var policy = Policy
    .HandleResult<HttpResponse>(x => !x.IsSuccessful)
    .Or<Exception>()
    .WaitAndRetryAsync(maxRetryAttempts: 3, retryAttempt => TimeSpan.FromSeconds(Math.Pow(2, retryAttempt)));

IMyClient myClient = NClientProvider
    .Use<IMyClient>(host: "http://localhost:8080")
    .WithResiliencePolicy(policy)
    .Build();
```

But you can create your own implementation of `IResiliencePolicyProvider` and pass it to `WithResiliencePolicy` method.
