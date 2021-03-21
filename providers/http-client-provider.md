# Http client provider
By default, NClient uses [RestSharp](https://github.com/restsharp/RestSharp) client. 
If you want to use another one - implement your own `IHttpClientProvider` and pass it to `SetHttpClientProvider` method:

```ruby
public class MyHttpClientProvider : IHttpClientProvider
{
    ...
}

MyHttpClientProvider httpClientProvider = new MyHttpClientProvider();

IProductServiceClient client = new ClientProvider()
    .Use<IProductServiceClient>(host: new Uri("http://localhost:8080"))
    .SetHttpClientProvider(httpClientProvider)
    .WithoutResiliencePolicy()
    .Build();
```
