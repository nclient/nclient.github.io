# Resilience provider
By default, NClient uses [Polly](https://github.com/App-vNext/Polly) for resilience policy. 
If you want to use another one - implement your own `IResilienceProvider` and pass it to `WithResiliencePolicy` method:

```ruby
public class MyResilienceProvider : IResilienceProvider
{
    public IResiliencePolicy Create()
    {
        ...
    }
}

MyResilienceProvider resilienceProvider = new MyResilienceProvider ();

IProductServiceClient client = NClientProvider
    .Use<IProductServiceClient>(host: "http://localhost:8080")
    .WithResiliencePolicy(resilienceProvider)
    .Build();
```
