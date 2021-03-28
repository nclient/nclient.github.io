# Http client provider
By default, NClient uses [RestSharp](https://github.com/restsharp/RestSharp) client. 
If you want to use another one - implement your own `IHttpClientProvider` and pass it to `SetHttpClientProvider` method:

```ruby
public class MyHttpClientProvider : IHttpClientProvider
{
    public IHttpClient Create()
    {
        ...
    }
}

MyHttpClientProvider httpClientProvider = new MyHttpClientProvider();

IProductServiceClient client = NClientProvider
    .Use<IProductServiceClient>(host: "http://localhost:8080", httpClientProvider)
    .Build();
```
