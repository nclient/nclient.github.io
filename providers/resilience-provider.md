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

IProductServiceClient client = new ClientProvider()
    .Use<IProductServiceClient>(host: new Uri("http://localhost:8080"))
    .SetDefaultHttpClientProvider()
    .WithResiliencePolicy(resilienceProvider)
    .Build();
```
